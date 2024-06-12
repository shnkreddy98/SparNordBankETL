# Spar Nord Bank (ETL)

## Introduction

Welcome to the Spar Nord Bank ATM Transactions Analysis Project! This project demonstrates the development of a batch ETL pipeline using widely adopted tools and technologies such as Apache Sqoop, Apache PySpark, Amazon S3, and Amazon Redshift. The objective of this project is to extract, transform, and load (ETL) transactional data from a MySQL RDS database into Amazon Redshift, followed by performing analytical queries to derive valuable insights.

### Use Case

Spar Nord Bank needs to optimize its ATM refilling strategy by analyzing ATM usage patterns, particularly withdrawals, and their influencing factors such as weather, time, and location. Additionally, the bank seeks to gain insights into ATM failures and transaction behaviors to enhance overall ATM management and customer service.

### Project Workflow

The project involves the following steps:

1. **Data Extraction**: Extract transactional data from MySQL RDS using Apache Sqoop and load it into HDFS.
2. **Data Transformation**: Utilize Apache PySpark to transform the extracted data into a format compatible with the target schema, which includes creating dimension and fact tables.
3. **Data Loading**: Load the transformed data from HDFS to Amazon S3.
4. **Redshift Setup**: Create a Redshift cluster, define the schema, and load the data from S3 to Redshift tables.
5. **Data Analysis**: Perform analytical queries on the loaded data to answer business questions and derive insights.

### Tools and Technologies

- **Apache Sqoop**: For data ingestion from MySQL RDS to HDFS.
- **Apache PySpark**: For data transformation and creating dimension and fact tables.
- **Amazon S3**: For storing the transformed data before loading it into Redshift.
- **Amazon Redshift**: For setting up a data warehouse and running analytical queries.

## Data

The dataset used in this project was sourced from Kaggle and contains detailed ATM transactional data along with weather information at the time of the transactions from around 113 ATMs across Denmark for the year 2017. The dataset comprises approximately 2.5 million records and includes various fields such as transaction date and time, ATM status, ATM details, weather conditions, transaction details, and more.

[Link to Kaggle Dataset](https://www.kaggle.com/datasets/sparnord/danish-atm-transactions)

## Data Model

![Data Model](images/data_modelling.png)

The data model consists of four dimension tables and one fact table:

1. **ATM Dimension**: Contains ATM-related data including ATM ID, manufacturer, and location reference.
2. **Location Dimension**: Contains location data such as city, street name, street number, zip code, latitude, and longitude.
3. **Date Dimension**: Contains time-related data including timestamp, year, month, day, hour, and weekday.
4. **Card Type Dimension**: Contains information about different card types used in transactions.
5. **Transaction Fact**: Contains numerical and transactional data such as currency, service type, transaction amount, message codes, and weather information.

## Approach

![Approach Diagram](images/Approach.png)

The approach for this project is divided into the following stages:

### 1. Data Extraction

Using Sqoop, data is extracted from the MySQL RDS and loaded into HDFS on an EC2 instance.

### 2. Data Transformation

PySpark is used to:

- Define the input schema using StructType to ensure correct data types.
- Read and verify the data from HDFS.
- Create and clean dimension tables by removing duplicates and ensuring proper primary keys.
- Create the transaction fact table by joining with the dimension tables and cleaning the data.

### 3. Data Loading

The cleaned and transformed data is written to an Amazon S3 bucket, with separate folders for each dimension and fact table.

### 4. Redshift Setup and Data Loading

A Redshift cluster is created, and tables are defined according to the target schema. Data is then copied from the S3 bucket into the respective Redshift tables.

### 5. Data Analysis

Analytical queries are executed on the Redshift cluster to answer the following business questions:

1. Top 10 ATMs with the most inactive transactions.
2. Number of ATM failures corresponding to different weather conditions.
3. Top 10 ATMs with the highest number of transactions throughout the year.
4. Monthly count of inactive transactions.
5. Top 10 ATMs with the highest total amount withdrawn.
6. Number of failed transactions across various card types.
7. Top 10 transaction records ordered by ATM number, manufacturer, location, weekend flag, and transaction count.
8. Most active day for each ATM in the location "Vejgaard".

## Conclusion

This project provides a comprehensive ETL pipeline to process and analyze ATM transactional data, helping Spar Nord Bank optimize ATM management and gain valuable insights into ATM usage patterns. The approach demonstrates effective use of modern data engineering tools and cloud services to solve real-world business problems.
