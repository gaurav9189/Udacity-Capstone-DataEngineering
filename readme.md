# Project Capstone - Udacity

## US Immigration Data, Airport, US Demographics and Temprature ETL Pipeline

### Project Goal

This project augments the US I94 immigration data with further data such as US airport data, US demographics and temperature data to have a wider basis for analysis on the immigration data.

The environment created for the project is as follows:
- Spark version 3.0 - latest version https://www.apache.org/dyn/closer.lua/spark/spark-3.0.1/spark-3.0.1-bin-hadoop3.2.tgz
- Hadoop version 3.2 pre-bundled with Spark
- Pyspark v3 compatible with Spark version
- Library for reading SAS data with https://github.com/saurfang/spark-sas7bdat
- Python Anaconda - v1.9.12
All the python dependencies were installed for running this project.

An ETL pipeline feeds into the 'dwh'(datawarehouse) database within indigeneous Spark Metastore. This database has a fact and dimensions table necessary for studying Immigration behavior. The warehouse can be used as a BI Analytics backend for 


#### Files in the project:
* Capstone project notebook: Contains the main content, show clear implementation of the concepts around Data gathering, cleaning and loading into a conceptual model.

* Load_data_udacity: Should be used to load data from udacity worksapce into personal AWS S3 bucket, which can then be used for local workstation

* Readme: has same contents as above


## Data sources

### I94 Immigration Data
This data comes from the US National Tourism and Trade Office. A data dictionary is included in the workspace. [This](https://travel.trade.gov/research/reports/i94/historical/2016.html) is where the data comes from. There's a sample file so you can take a look at the data in csv format before reading it all in.


### World Temperature Data
 - The World Temperature dataset comes from Kaggle and represents global land temperatures by city. [here](https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data).

### U.S. City Demographic Data
This data comes from OpenSoft. You can read more about it [here](https://public.opendatasoft.com/explore/dataset/us-cities-demographics/export/).

### Airport Code Table
This is a simple table of airport codes and corresponding cities. It comes from [here](https://datahub.io/core/airport-codes#data).

## Complete Project Write Up

### Technologies for the project

I'm making use of Spark v3.0 native warehouse and a stand alone spark setup on a single node machine. Spark data frames are used for loading and manipulating data and written into a separate database created within the Warehouse of spark.
Amazon EMR can be utilised for easy scaling and highly distributed processing. Further down i'd recommend using Apache Airflow to help run ETL pipelines and generate dashboards



### Data Cleaning

* Cleaned date format from I94 data, fixed several data types issue
* Cleaned the aiport data and fixed the iso_region
* We need the IATA codes to join the data with other sources.
* Drop rows with missing IATA codes from I94 data.


### Table Definition

Immigrations data would store vital information for us, this data is augmented with airports data, demographics data. There are identifiers on all tables to allow sql joins and construct a star schema with a fact and rest as dimension tables. City and iata code are used for sql joins across the tables.



### ETL Pipelines

1. Data is read with Spark DF and written as tables
2. SQL Join on city to airports data.
3. Finally insert the data using cleaned spark DF as staging tables


### How often the data should be updated and why

The I94 data described immigration events aggregated on a monthly base. Thus, updating the data on a monthly base is recommended.

## FAQ: What would I do if...
 
* Write a description of how you would approach the problem differently under the following scenarios:
    1. The data was increased by 100x:
    
       Use Amazon EMR: It is an Apache Hadoop adaptation which can scale up to varying data needs and makes use of native Hadoop services like Spark, Hive etc. The storage can be both, S3 or HDFS for difference performance needs. Also i can suggest using Amazon Redshift for Datawarehousing needs if Spark warehouse is not sufficient.
       
    2. The data populates a dashboard that must be updated on a daily basis by 7am every day:
       
       In this scenario, Apache Airflow will be used to schedule and run data pipelines.
       
    3. The database needed to be accessed by 100+ people:
       
       In this scenario, we would move our analytics database into Amazon Redshift.

