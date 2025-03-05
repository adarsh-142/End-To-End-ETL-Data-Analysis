# Cloud-based Static ETL Taxi Data Analytics Pipeline with Power BI
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
3. **


