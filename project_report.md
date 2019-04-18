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

### Census public API data
**Source:**
[Census API](https://api.census.gov/data.html) 

**Steps for creating the Census demographic data set:**

1. Select which US Census survey and data set contains usable demographic data (age, race, education, poverty level, has health ins) and select api to get the data at the city level.
2. Make api call to the Census data site and convert the returned json object to a Pandas dataframe.
3. Set the data frame column names by converting the first row of the data frame to column names.
4. Create a dictionary to convert column names from Census data codes to meaningful names to make data easier to identify.
5. Convert all columns with population data to numeric values.
6. Add columns for number of males and females and store as a new column for a single number of people with health insurance
7. Contantenate the state FIPS code column with the city FIPS code column to match the format of the health data location field
8. There was not a single data field for the population over 65, all population fields with age ranges over 65 for males and females were     called from the Census site. All age range columns for males and females were added together to get a single population number of people over 65.
9. Very similarly, there was not a single field of number of people with education beyond high school. Multiple fields were called from the census site for numbers of people with different college degrees. These were added together in single column.
10. Created a new dataframe with discarding columns not needed after they were consolidated into other columns.
11. Divided all columns with demographic data my the total population column to covert them to percentages.


## Loading

We choose to load and store our data in MySQL database because the data are well organized, i.e., both datasets have predefined sets of columns. The datasets also have common keys, specifically city FIPS (Federal Information Processing Standard Publication) codes. These two factors make a relational database, such as MySQL, a perfect choice for our project, where we can realiably and easily join the datasets together.

We configure an engine and create a connection to MySQL Database with SQLalchemy queries. Next, we create a new empty database called 'city_health_demographics', where we load the Pandas dataframes with transformed Census and city health data as two separate tables ('city_demographics' and 'city_health') using *Dataframe.to_sql* command. Lastly, we merge the two tables into a single table 'city_health_demographics' on the city FIPS code by passing an sql query to the database with an *engine.excute* command.  The final database, containing all three tables, is exported and uploaded to the project files in Github, [here](data/).
