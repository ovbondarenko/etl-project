# ETL Project Report

## Summary

In this project we extract socio-economical statistical data from [Census API](https://api.census.gov/data.html) and public health data for 500 US cities (available on [Kaggle](https://www.kaggle.com/cdc/500-cities)). We transform the data to a more readable format using Pandas package and loaded it to MySQL database via SQLalchemy queries. The cityhealth_db.sql database file is available for download in the [data](data) folder.

## Extraction+Transformation

### CDC public health survey data
**Source:**
CDC 500 Cities Kaggle Dataset (https://www.kaggle.com/cdc/500-cities). his data was collected by Centers for Disease Control and Prevention, National Center for Chronic Disease Prevention and Health Promotion, Division of Population Health

**Process**
First, we downloaded the original public health dataset for 500 US citiesfrom Kaggle and saved as a .csv file. In Jupyter Notebook, we transform the raw dataset with 117 columns to a smaller dataset with 31 columns and update the column names for a better readability using Pandas At this point the dataset is ready to be loaded to the database.

### Census public API data
**Source:**
[Census API](https://api.census.gov/data.html) 

**Process**


## Loading

With SQLalchemy queries we configure an engine and create a connection to MySQL. Next, we create a new empty database called 'city_health_demographics', where we load the transformed Census and city health data as two separate tables ('city_demographics' and 'city_health'). Lastly, we merge the two tables into a single table 'city_health_demographics' on the city FIPS (Federal Information processing Standard Publication) code. The final database is exported and uploaded to the project files in Github, [here](data/cityhealth_db.sql).