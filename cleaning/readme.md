### Data Cleaning Process
---

### 1. State mismatch and drop columns

**a. Resolve mismatches between state/state_code & address entries**  
Manual inspection showed the `address` column is correct, while split columns contain errors.

**b. Drop unnecessary columns**  
- `price_formatted` → redundant (duplicate of numeric price)  
- `open_house_date` → mostly null and not updated after scrape  

**Fixes:**
- Drop: `state_name`, `state_code`, `street`, `city`, `zipcode`
- Recreate by splitting `address` cleanly

- Drop: `price_formatted`, `open_house_date`

---

### 2. Remove non-single-family home types

- **Goal**
  - Keep only `home_type` rows that represent single-family home variations

- **Process**
  - Extract unique `home_type` values and their frequencies
  - Identify values that do not represent single-family homes
  - Remove rows with undesired `home_type` values
---

### 3. Clean missing values

- **Process**
  - Inspect Zillow dataset for missing value counts
  - Drop row with `zpid = 444753560` (new development)
  - Remove rows with `'Undisclosed'` street address
---

### 4. Zipcode cleaning & county matching

- **Zipcode standardization**
  - Identify zipcodes with fewer than 5 digits (missing leading 0s)
  - Impute leading 0s where necessary
  - Remove ZIP+4 formatting, keeping only first 5 digits

- **Zipcode → county mapping**
  - Use `pgeocode.Nominatim` for zipcode lookups
  - Create function to map zipcode → county
  - Store result in new column for downstream county-level joins
---

 ### 5. Clean state income tax dataset

- **Process**
  - Drop unnecessary columns
  - Strip symbols (e.g., `%`, `$`)
  - Rename columns for consistency
---

### 6. Clean property tax dataset

- **Process**
  - Strip symbols (`$`, `,`, `%`)
  - Standardize column naming conventions
  - Remove duplicate columns
  - Export cleaned DataFrames
---

### 7. Implement median household income data

- **Process**
  - Drop `stateFlagCode`
  - Standardize column names
  - Perform dtype conversions and aggregations
  - Merge datasets into main DataFrame
---

### 8. Manual corrections

- **Adjustments**
  - Fill or correct blank entries in:
    - `state_avg_median_income`
    - Tax bracket columns
    - Tax rate columns
  - Correct fallacious data identified during outlier inspection  
    - Example: large property recorded with unrealistically low sale price
