# DS644-MLBA-Final-Project

## Overview

This project aims to use methods learned through the course of BU Spring DS 444/644 to predict home prices using [Feb. 2026 Zillow Data](https://www.kaggle.com/datasets/hmashiqurrahman/zillow-real-estate-listings-top-20-us-states) found on Kaggle, posted by user HM Ashiqur Rahman. 

The goals of this project are two-fold:

1) Determine which model type provides the best predictions on the Zillow dataset for predicting home value based on a set of provided features
2) Display understanding of the strengths and weaknesses of the various machine learning models taught over the course of the semester, which include but are not limited to: linear regressions, trees, Lasso, Ridge, Boosting, Neural Nets, and more.

## Data 

The dataset was sourced from Kaggle since our team was given free reign to choose any dataset for this project work.

A few points our team considered when selecting a dataset to work with:

* Avoiding synthetic data: while not explicitly forbidden by class rules for this project, we felt like we preferred to work with data that reflect a real world problem for a better learning experience.
* Enough features to work with: Professor Zervas required a minimum of 10 columns so there would be enough features to work with. We wanted to have as many as possible so we could have enough interesting routes for feature engineering, which lead to the concatenation of state-related tax information to improve feature density, resulting in a final dataset with 43 unique features used for predictions after encoding categorical variables.
* Enough rows to work with: there was no explicity requirement for number of rows but we wanted to use a dataset with enough to have confidence in the generalizability of our models, which lead us to prefer this dataset with 2,000+ rows over others we found with 300 or fewer.

Given the text heavy nature of the data, an extensive cleaning and EDA process was necessary to get the data both in workable condition, as well as to properly attach the corresponding state tax information to each row. This was handled in the ```cleaning``` folder of this repository.

The README found at ```cleaning/readme.md``` contains a detailed breakdown of the dataset issues and steps taken to address them during the cleaning process. 

The cleaning process was primarily overseen by team members Brendan Mcmanus, Madison Woo, and Nikhita Singh.

## Modeling

Models for this project were selected from various machine learning models that were introduced over the course of the semester.

These range from as simple as an intercept-only Linear Regression to a small Neural Network model built in PyTorch.

The models trained for this project are found in the following notebooks, along with the respective team member who worked on each model:

```
models/bagging_and_boosting.ipynb (Muhannad Alsahaf)
models/deeplearning.ipynb (Gregory Knapp)
models/intercept_only.ipynb (Nikhita Singh)
models/lasso_ridge_model.ipynb (Nikhita Singh)
models/iterative_v2.ipynb i.e. Trees/Random Forests (Brenden Mcmanus)
```

Additionally, a master script containing all models contributed to the project is found in the following notebook:

```models/models_IN_PROGRESS.ipynb```

Which can be run from start to finish to recreate our project results.

## Running project files

A requirements.txt file has been included with the repository to install all necessary dependencies for running all relevant project scripts and files. This can be install simply by running ```pip install -r requirements.txt``` from terminal from the root folder of this project repository.

Once dependencies are installed, cleaning or modeling scripts can be executed.

The finalized clean dataset has been included with the repository in the following location:

```data/dataset_final.csv```

This csv file is the final output of our main cleaning script ```cleaning/cleaning_COMPLETE.ipynb```, and has been included so interested parties are not required to run our cleaning scripts.

Since the dataset is included in the repository, interested parties can either run each model notebook individually or use the master script ```models/models_IN_PROGRESS.ipynb``` to run a single notebook and view all outputs at once.

For a complete recreation of our work, the following steps are recommended

1) Load all dependencies from ```requirements.txt```
2) Run all cells in ```cleaning/cleaning_COMPLETE.ipynb``` to recreate our complete dataset ```data/dataset_final.csv```
3) Run individual model notebooks found in the subfolder ```modeling``` OR run the single notebook ```modeling/models_IN_PROGRESS.ipynb``` to view results.

**Note that due to the inclusion of a PyTorch neural network model, execution of the complete model master notebook may take upwards for 40 minutes if run on a non-CUDA enabled device. The script will check if CUDA is available on the current device and use it if possible, resulting in training times of approximately 7-8 minutes, rather than 20+ minutes.**

## Project Results

While model results are available through execution of each individual notebook or by viewing the saved outputs from our execution of each notebook, we have compiled our results into a single PowerPoint presentation that was presented to the Boston University Spring 2026 CDS 444/644 class on Thursday, April 30th, 2026. 

A link to the complete presentation slides which contains graphs and analysis of our outputs is available [here](https://docs.google.com/presentation/d/1LLNs1QdPA0MznS0nkqLJ2wAdfumKaNa5vtrJYjlx6kQ/edit?usp=sharing)

## Special Thanks

Team 5 members would like to thank Professor Giorgos Zervas for constant guidance through the completion of this project, on top of excellent lectures throughout the semester to teach us about each model and the strengths and weaknesses of each.