# End-To-End ETL Data Analysis
This project aims at analyzing and minimizing trip fare refunds along with a focus on trip fare dispute in an attempt to minimize the loss of money from taxi companies through such refunds. This benefits the taxi companies in minimizing losses while improving customer satisfaction and issues that lead to fare disputes.

## Data Overview
The data primed for the analysis involves two seperate tables, one with the yellow taxi data across the state of New York in the month of October in the year 2024, and the other being location details where pickup and dropoff are done. The tables are joined to provide a more complete representation of the yellow taxi data concerned. The dataset contains pickup locations, dropoff locations, payment methods, fare amounts and other addages, trip distances and passenger counts for each trip in the whole month.

## Access to Files
Due to GitHub's file size limitations, certain large files are stored on Google Drive. The files can be accessed through the link below:<br>
📂 [Project Files](https://drive.google.com/drive/folders/1Sv_NJrqnMWRalsk0DOqzobpEwt4CdA4Y?usp=drive_link)

## Project Steps
1. **File Type Conversion**
   - The original data is in the form of a parquet file. Hence the file needs to be converted to `.csv` format to ease the process of joining the file with the taxi zones data which is in the `.csv` format.
   - Such a task is accomplished through the use of the Pandas function within Python using the `pd.read_parquet()` and `.to_csv()` functions.
2. **Uploading the files to an Amazon S3 bucket**
   - A general purpose bucket is created within Amazon S3 named `taxi.data.holding`.
   - Both `.csv` files are uploaded to the bucket for storage.
3. **Assigning IAM Roles for the bucket**
   - IAM Roles are assigned pertaining to the bucket to offer specific permissions with credentials that last for short durations.
   - In this case, a role is assigned for full access of S3 services pertaining to the bucket which is named 'taxi.data.role'. This step helps with seamless integration of the bucket to the Snowflake data warehousing tool.
4. **Setting the Environment on Snowflake**
   - A warehouse and within it, a SQL worksheet, is created to facilitate SQL-based queries to help `**Extract**` the data from the bucket.
5. **Setting up Storage Integration on Snowflake**
   - This step is executed to allow for seamless connection to the S3 bucket with the IAM role in tow. The integration object is named `TAXI_Integration`.
6. **Creating the Database and Schema**
   - A database named `TAXI` and a corresponding schema named `taxi_data` is made to facilitate the handling of both tables to be extracted from the bucket.
7. **Creating Empty Tables for Storage**
   - Tables are created to place and store the imported data within Snowflake.
8. **Extracting Data through a Stage**
   - Stages are created, while referencing the S3 bucket, for the specific purpose of successfully integrating the data within Snowflake after the successful creation of the integration object beforehand.
   - The data is extracted from the bucket, leveraging the integration object, upon the table, onto the empty tables created.
9. **Transforming Data Within Snowflake**
   - SQL is leveraged to help ready the data for effective reporting using Power BI. The techniques involved data validation, data cleaning, feature engineering and table joins.
   - A primary left join is executed between taxi data and taxi zones data to effectively record pickup locations.
   - A secondary left join is executed between the joined table and taxi zones to effective record dropoff locations.
   - Columns are renamed to effectively communicate the difference between pickup and dropoff locations.
   - Two new features based on the hour of pickup and dropoff are engineered based on existing data.
   - Anomalies are checked with suspected columns and are cleaned/modified for a sensical representation of the data at hand.
   - Rate Code and Payment Method columns are converted based on the character correlating to the numbers listed in the original data.
   - Records with no passengers, no trip distance and no fare amount are dropped.
   - Certain records with anomalies, especially records with no fare amount, are flagged but not dropped, to be addressed seperately for reporting.
   - A new table is created with the intention of reordering the columns to resemble the original data, before dropping all previous tables within the database.
10. **Loading To Power BI**
   - The data is loaded to Power BI with the use of the `Get Data` option under the `Home` tab in Power BI through the use of Snowflake account credentials which involves `Warehouse Name`.
   - A broad-to-specific approach is adopted as basic visualizations are crafted to detail trip frequency analysis and location-based statistics analysis before diving deeper into distance/fare specific analysis before reaching the main area of analysis to answer the problem statement which involves refunded trip fares and fare disputes.

## Insights and Inferences
1. The average number of taxi trips chartered per second recorded throughout the month of October 2025 was 1.26 per second while the number of trips throughout the whole month crosses 3.3 million. Such values can attributed to the sheer population density and facilities within the state.
2. The number of trips hired by a particular hour-mark sees a steady incline from 7am before peaking at 6pm, including both morning and evening rush hours, with an overall increase of 181.62% in trip hires. Since the time period involves both rush hours, it does make sense to see a steady increase across the time period as the citizens are mostly active across this time period with relations of work obligations, sight seeing, etc.
3. Thursday has the number of trip hires at 580,376 followed by Wednesday at 570,906 while the lowest number of taxi trip hires were seen on Monday at 378,144.
4. The most frequent borough with regards to pickup and dropoffs is Manhattan amounting to over 89% prevalance with both pickup and dropoff, as it dwarfs all other boroughs. This can be explained through both the population density and various accessible amenities with the city areas in Manhattan, while boroughs like Queens serves more as a residential district with a lesser need in frequent trip hires compared to Manhattan.
5. It is found that a total of 17654 trips are affiliated to outside NYC. The reasoning can be attributed to people having their destinations outside NYC pertaining to the closest airports to the destination being in NYC, tourists leaving NYC to explore a newer destination, etc.
