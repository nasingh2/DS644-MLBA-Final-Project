# Data Cleaning

## Manual Inspection of zillow dataframe revealed inaccuracies between the address column and 'state_name' & 'state_code' columns

**Examples Include:**\
13371070	FL	Florida	1511 S Corona Street, Denver, CO 80210\
21928004	PA	Pennsylvania	1704 Butterfield Trl, Choctaw, OK 73020\
333915491	IL	Illinois	25689 N 140th Ln, Surprise, AZ 85387\
61420798	IL	Illinois	3132 Benefit Rd, Chesapeake, VA 23322\
20021372	IL	Illinois	13114 Addison St, Sherman Oaks, CA 91423\
20009320	IL	Illinois	6032 Goodland Ave, North Hollywood, CA 91606\
113686664	IL	Illinois	N2298 State Highway 35, Stoddard, WI 54658\
72272555	IL	Illinois	259 Dunbarton Ln. #Shaftesbury Greens, Conway, SC 29526

## To correct this:
### 1. Drop problematic columns (also drops price_formatted and open_house_date)
```
python

# dropping address related cols since many have missing information
# dropping price formatted since '$' is not useful for analysis
# dropping open_house_date since most rows are NaN

drop_cols = ['state_code', 'state_name', 'street', 'city', 'zipcode', 'price_formatted', 'open_house_date']

df.drop(columns=drop_cols, inplace=True)

display(df.head())
```
### 2. Create a map for State names -> abbreviations
### 3. Split full address (which contains the correct state) into deliminated parts
```
python

address_parts = df['address'].str.split(', ', expand =True)

df['street_add'] = address_parts[0]
df['city'] = address_parts[1]
df['state_zipcode'] = address_parts[2]

state_zip_split = df['state_zipcode'].str.split(' ', expand =True)
df['state_code'] = state_zip_split[0]
df['zipcode'] = state_zip_split[1]

df['state_name'] = df['state_code'].map(state_map)
```

### At this point, address parts should be corrected.
### 4. Save the updated df for additional cleaning

## Removal of home types that are not usable for analysis
### 1. Check counts by home_type
`df['home_type'].value_counts()`


home_type\
SINGLE_FAMILY    2108\
CONDO              30\
TOWNHOUSE          25\
MANUFACTURED       23\
MULTI_FAMILY       19\
LOT                 9\
Name: count, dtype: int64

### 2. Removal of desired home_types
```
python

df = (
    df[~df['home_type']
       .isin(['LOT', 'MANUFACTURED', 'MULTI_FAMILY']
        )
    ])

# confirms selected home_type rows are gone
df['home_type'].value_counts()
```

home_type\
SINGLE_FAMILY    2108\
CONDO              30\
TOWNHOUSE          25\
Name: count, dtype: int64

**Value count confirms proper removal of rows**

## 4 rows missing lat/lon &  1 row missing state_zipcode, state_code, zipcode, and state_name

### 1. 'display(df[df['zipcode'].isna()])' located one property with no zipcode or state.  This was a new construction with numerous homes listed, not an actual address.
### 2. Drop symptomatic row
```
python

df.drop(
    df[
        df['zpid'] == 444753560].index, inplace=True
    )
```
**Removal of zpid = 444753560 successfully removed missing state_zipcode, state_code, zipcode, and state_name entries**
### 3. Manual inspection of 4 missing lat/lon rows.  Manual inspection found a 3 of the rows as unusable, and one that requires manual correction or geocoding.

#### Properties with undisclosed locations were extremely high value properties, most likely no location was given for privacy and safety concerns.

* **455353390	(undisclosed Address), Seattle, WA 98112**
* **460335453	Undisclosed, Canyon Creek, MT 59633**
* **460335454	Undisclosed, Darby, MT 59829**

Requires manual correction for lat/lon:
* **460321976	9033 Chapel Heights Rd, Auburn, AL 36830**

### 4. Removal of undisclosed properties.

```
python

drop_list = [455353390, 460335453, 460335454]

df = df[~df['zpid'].isin(drop_list)]
```
