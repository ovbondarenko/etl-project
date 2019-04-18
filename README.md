# ETL Project Description

In this project we collect socio-economical statistical data, provided by [Census via API calls](https://api.census.gov/data.html) and public health data, collected by Centers for Disease Control and Prevention, National Center for Chronic Disease Prevention and Health Promotion, Division of Population Health for 500 US cities and available on [Kaggle](https://www.kaggle.com/cdc/500-cities). We transforme the data in Pandas and load it to MySQL database tables using the SQLalchemy package. Finally, the two datasets are merged into a single table ('city_health_demographics' table) through the sql inner join query. The cityhealth_db.sql database file is available for download in the [data](data) folder.

We built the ETL process in the [Jupyter Notebook](main.ipynb).

The original project proposal can be found [here](project_proposal.md).

The final project report can be found [here](project_report.md).

## Required Python packages and other dependencies:

- Pandas
- SQLalchemy
- Requests
- config.py file with your census api key (census_api_key) and MySQL password (mysql_pw)

## Authors:

- Brian Edward Reyes
- David Chen
- Steve Bogdan
- Olesya Bondarenko

