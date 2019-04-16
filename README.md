## Project proposal

The goal of the project is to extract the health statistics for 500 cities in the US and  Census data for these cities. We will further load the health and census data for these cities to a database and clean them up.

### Data Sources

- Kaggle Dataset: CDC 500 Cities Dozens of Public Health Datapoints Reported by Residents of 500 US Cities (https://www.kaggle.com/cdc/500-cities)

 - Statistical data for the cities from Census API (https://api.census.gov)


### Database and output

We will be using MySQL to create a database titled “Health_Community_DB”. The Census data will be extracted through an API call. The data will be joined by FIPS codes on location.

