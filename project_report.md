# ETL Project Report

**Project team:** Brian E Reyes, David Chen, Steve Bogdan, Olesya Bondarenko

## Summary

In this project we extract socio-economic data from [Census API](https://api.census.gov/data.html) by city and public health data for 500 US cities (available on [Kaggle](https://www.kaggle.com/cdc/500-cities)). We transform the data to a more readable format using Pandas package and load it to a MySQL database via SQLalchemy queries. The cityhealth_db.sql database file can be found in the [data](data) folder.

## Extraction+Transformation

### CDC public health survey data
**Source:**
CDC 500 Cities Kaggle Dataset (https://www.kaggle.com/cdc/500-cities). his data was collected by Centers for Disease Control and Prevention, National Center for Chronic Disease Prevention and Health Promotion, Division of Population Health

**Process:**
First, we downloaded the original public health dataset for 500 US citiesfrom Kaggle and saved as a .csv file. In Jupyter Notebook, we transform the raw dataset with 117 columns to a smaller dataset with 31 columns and update the column names for a better readability using Pandas At this point the dataset is ready to be loaded to the database.

Here is the dictionary of the column names between the original CDC 500 dataset and our final CDC dataset:
columns={"StateAbbr": "State", "PlaceName": "City", "PlaceFIPS": "PlaceFIPS",
        "ACCESS2_AdjPrev": "ACCESS2", "ARTHRITIS_AdjPrev" :"ARTHRITIS", "BINGE_AdjPrev" :"BINGE",
        "BPHIGH_AdjPrev" :"BPHIGH", "BPMED_AdjPrev" :"BPMED", "CANCER_AdjPrev" :"CANCER", "CASTHMA_AdjPrev" :"CASTHMA",
        "CHD_AdjPrev" :"CHD", "CHECKUP_AdjPrev" :"CHECKUP", "CHOLSCREEN_AdjPrev" :"CHOLSCREEN",
        "COLON_SCREEN_AdjPrev" :"COLON_SCREEN", "COPD_AdjPrev" :"COPD", "COREM_AdjPrev" :"COREM",
        "COREW_AdjPrev" :"COREW", "CSMOKING_AdjPrev" :"CSMOKING", "DENTAL_AdjPrev" :"DENTAL",
        "DIABETES_AdjPrev" :"DIABETES", "HIGHCHOL_AdjPrev" :"HIGHCHOL", "KIDNEY_AdjPrev" :"KIDNEY",
        "LPA_AdjPrev" :"LPA", "MAMMOUSE_AdjPrev" :"MAMMOUSE", "MHLTH_AdjPrev" :"MHLTH",
        "OBESITY_AdjPrev" :"OBESITY", "PAPTEST_AdjPrev" :"PAPTEST", "PHLTH_AdjPrev" :"PHLTH",
        "SLEEP_AdjPrev" :"SLEEP", "STROKE_AdjPrev" :"STROKE", "TEETHLOST_AdjPrev" :"TEETHLOST"

### Census public API data
**Source:**
[Census API](https://api.census.gov/data.html) 

**Steps for creating the Census demographic data set:**

1. Select which US Census survey and data set contains usable demographic data (age, race, education, poverty level, has health ins) and select api to get the data at the city level.
2. Make api call to the Census data site and convert the returned json object to a Pandas dataframe.
3. Set the data frame column names by converting the first row of the data frame to column names.
4. Create a dictionary to convert column names from Census data codes to meaningful names to make data easier to identify.
    Here is the dictionary for all column names in the raw census data file ([data/census_data_by_city_raw.csv](data/census_data_by_city_raw.csv))
    column_names = {"B01003_001E":"total_pop", "B01001_020E":"male_65_66", "B01001_021E":"male_67_69",
                    "B01001_022E":"male_70_74", "B01001_023E":"male_75_79", "B01001_024E":"male_80_84",
                    "B01001_025E":"male_over_85", "B01001_044E":"female_65_66", "B01001_045E":"female_67_69",
                    "B01001_046E":"female_70_74", "B01001_047E":"female_75_79", "B01001_048E":"female_80_84",
                    "B01001_049E":"female_over_85", "B02001_002E":"white_pop", "B02001_003E":"black_pop",
                    "B02001_004E":"native_amer_pop", "B02001_005E":"asian_pop", "B02001_006E":"pac_island_pop",
                    "B02001_007E":"other_race_pop", "B01001I_001E":"hispanic_pop", "B15003_002E":"no_high_school",
                    "B15003_017E":"high_school_grad", "B15003_022E":"bachelor_deg", "B15003_023E":"master_deg", 
                    "B15003_025E":"doctorate_deg", "B17001_002E":"below_poverty", "B27001_002E":"male_w_health_ins",
                    "B27001_030E":"female_w_health_ins", "place":"city_FIPS"}

5. Convert all columns with population data to numeric values.
6. Add columns for number of males and females and store as a new column for a single number of people with health insurance
7. Contantenate the state FIPS code column with the city FIPS code column to match the format of the health data location field
8. There was not a single data field for the population over 65, all population fields with age ranges over 65 for males and females were     called from the Census site. All age range columns for males and females were added together to get a single population number of people over 65.
9. Very similarly, there was not a single field of number of people with education beyond high school. Multiple fields were called from the census site for numbers of people with different college degrees. These were added together in single column.
10. Created a new dataframe with discarding columns not needed after they were consolidated into other columns.
11. Divided all columns with demographic data my the total population column to covert them to percentages.


## Loading

We choose to load and store our data in MySQL database because the data are well organized, i.e., both datasets have predefined sets of columns. The datasets also have common keys, specifically city FIPS (Federal Information Processing Standard Publication) codes. These two factors make a relational database, such as MySQL, a perfect choice for our project, where we can realiably and easily join the datasets together.

We configure an engine and create a connection to MySQL Database with SQLalchemy queries. Next, we create a new empty database called 'city_health_demographics', where we load the Pandas dataframes with transformed Census and city health data as two separate tables ('city_demographics' and 'city_health') using *Dataframe.to_sql* command. Lastly, we merge the two tables into a single table 'city_health_demographics' on the city FIPS code by passing an sql query to the database with an *engine.excute* command.  The final database, containing all three tables, can be recreated by copying the .csv files with the raw data found in the [data](data) folder and running the last part of main.ipynb file, titled "Database creation and data upload".

