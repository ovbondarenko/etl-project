# ETL Project Report

**Project team:** Brian E Reyes, David Chen, Steve Bogdan, Olesya Bondarenko

## Summary

In this project we extract socio-economical statistical data from [Census API](https://api.census.gov/data.html) and public health data for 500 US cities (available on [Kaggle](https://www.kaggle.com/cdc/500-cities)). We transform the data to a more readable format using Pandas package and load it to a MySQL database via SQLalchemy queries. The cityhealth_db.sql database file is available for download in the [data](data) folder.

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

We choose to load and store our data in MySQL database because the data are well organized, i.e., both datasets have predefined sets of columns. The datasets also have common keys, specifically city FIPS (Federal Information Processing Standard Publication) codes. These two factors make a relational database, such as MySQL, a perfect choice for our project, where we can realiably and easily join the datasets together.

We configure an engine and create a connection to MySQL Database with SQLalchemy queries. Next, we create a new empty database called 'city_health_demographics', where we load the Pandas dataframes with transformed Census and city health data as two separate tables ('city_demographics' and 'city_health') using *Dataframe.to_sql* command. Lastly, we merge the two tables into a single table 'city_health_demographics' on the city FIPS code by passing an sql query to the database with an *engine.excute* command.  The final database, containing all three tables, is exported and uploaded to the project files in Github, [here](data/).
