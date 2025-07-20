Azure Data Factory Project: Daily File Ingestion and Processing
Project Summary
This project demonstrates the use of Azure Data Factory (ADF) to design a daily data ingestion pipeline that fetches structured CSV files from Azure Data Lake Storage (ADLS) and loads them into corresponding SQL tables after applying necessary transformations.

The solution supports three types of input files, each with specific formatting and loading logic, and performs a truncate-and-load operation on a daily schedule.

Goals
Read multiple types of CSV files from a data lake

Use filename patterns to drive conditional logic

Extract and transform date information from filenames

Load the data into SQL tables with daily overwrite (truncate)

Automate the entire process using scheduled triggers

File Types and Logic
CUST_MSTR_yyyyMMdd.csv

Extracts the date portion from the filename.

Converts the extracted date into yyyy-MM-dd format.

Adds a new column named date with this value.

Loads data into the CUST_MSTR SQL table.

master_child_export-yyyyMMdd.csv

Extracts the date from the filename.

Adds two new columns:

date: yyyy-MM-dd

dateKey: yyyyMMdd

Loads data into the master_child SQL table.

H_ECOM_ORDER.csv

Loads directly into the H_ECOM_Orders table without modification.

Pipeline Details
Name: p_load_files_daily

Components Used:

Get Metadata to retrieve file list from a folder.

ForEach loop to iterate through file list.

If Condition blocks to match file names.

Copy Data activities to read and write files.

Processing Mode: Truncate the SQL table before each load.

Datasets Configuration
Three delimited text datasets are used:

Dataset Name	File Name Pattern	Description
ds_blob_CUST_MSTR	CUST_MSTR_*.csv	Customer master data files
ds_blob_master_child	master_child_export_*.csv	Master-child relationship files
ds_blob_H_ECOM_ORDER	H_ECOM_ORDER_*.csv	E-commerce order data

All datasets use:

Linked Service: ls_adls2

File format: CSV (UTF-8, comma separated)

Escape characters and quotes are appropriately configured

Parameters Used
The source dataset fields in each activity are parameterized:

folderPath: assigned dynamically at runtime

fileName: taken from file list during iteration

These are passed using ADF expressions:

text
Copy
Edit
@pipeline().parameters.folderPath
@pipeline().parameters.fileName
Trigger
A daily trigger has been added to automate pipeline execution.

The pipeline runs once every day without manual input.

Technology Stack
Azure Data Factory (ADF)

Azure Data Lake Storage Gen2

Azure SQL Database

Parameterization and expressions

Scheduled triggers in ADF

Status
Pipeline has been deployed and successfully tested.

Data from all three file types is being ingested as expected.

Scheduled trigger is active and functioning properly.

