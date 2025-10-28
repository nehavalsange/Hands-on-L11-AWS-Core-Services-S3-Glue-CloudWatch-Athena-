# Hands-on-L11-AWS-Core-Services-S3-Glue-CloudWatch-Athena-
Hands-on L11: AWS Core Services (S3, Glue, CloudWatch, Athena)

This repository contains the necessary SQL scripts and results for the Hands-on L11 assignment, which involves utilizing AWS core services (S3, Glue, CloudWatch, and Athena) to create a serverless data catalog and query a large CSV dataset.

## Dataset

The data used for this hands-on is the "Amazon Sale Report" (though the prompt links to a different Kaggle dataset, the analysis is performed on the provided Amazon Sale Report.csv file). This dataset contains various e-commerce sales records, including order details, dates, sales amounts, product information, and shipping locations.

## Approach and Workflow
The objective was to set up a serverless data infrastructure on AWS and execute complex SQL queries for business intelligence. The workflow followed the steps outlined in the assignment:

**1. Configure Amazon S3 Buckets**
Two Amazon S3 buckets were created:
s3://handson11cloudbucket/raw/: Used to store the original Amazon Sale Report.csv file.
s3://handson11cloudbucket/processed/: Used by AWS Athena to store temporary data and query results.
The CSV file was uploaded to the raw data bucket.

**2. Create an IAM Role for Glue Crawler**
A new IAM role, named GlueCrawlerRole-handsonglueroles, was created. This role was configured with the following permissions to allow the AWS Glue crawler to access and catalog the data:
Trust Relationship: Set to allow the glue.amazonaws.com service to assume the role.
Permissions
AmazonS3FullAccess or a custom policy granting read/write access to both S3 buckets specified in Step 1.
AWSGlueServiceRole and AWSGlueConsoleFullAccess or a similar policy granting permissions needed for Glue operations, including writing to CloudWatch Logs.

**3. Create and Run a Glue Crawler**
A Glue crawler named ecom_sales_crawler was created in the Glue Studio console:
Data Source: S3, pointing to the s3://handson11cloudbucket/raw/ path.
IAM Role: The GlueCrawlerRole - handsonglueroles created in Step 2 was attached.
Output: The crawler was configured to create a new database named ecom_sales_db.
The crawler was run once. It successfully identified the CSV file, inferred the schema (including column names and data types), and created the amazon_sale_report table in the ecom_sales_db database.

**4. Monitor Crawler Status with CloudWatch**
The execution status of the Glue crawler was monitored in the AWS CloudWatch console. The logs confirmed that the crawler started, completed, and successfully added one table to the database.

**5. Query the Database using Athena**
The Athena query editor was used, targeting the output_db database and the amazon_sale_report table.


## Adaptation of SQL Queries
The original dataset (Amazon Sale Report.csv) lacks columns for Profit and Discount, which are essential for queries 2, 3, and 5.
Therefore, the queries were adapted to use the existing Amount column (for Sales/Revenue) and substitute missing concepts (e.g., using Category for Sub-Category). Additionally, all date conversions were explicitly handled using DATE_PARSE("Date", '%m-%d-%y') to match the format in the CSV file and satisfy Athena's requirements.
The specific table name in the queries is assumed to be "amazon_sale_report", which is the standard name Glue assigns to a file named amazon_sale_report.csv.


## Screenshots
Please insert the required screenshots below.

Screenshot 1: CloudWatch Logs for Glue Crawler

<img width="975" height="609" alt="image" src="https://github.com/user-attachments/assets/086e7fb4-0da5-4c56-ba25-37611502c5be" />

Screenshot 2: IAM Role Configuration

<img width="1919" height="1143" alt="image" src="https://github.com/user-attachments/assets/d488db9c-dc5f-43d6-953b-9705028d6cd0" />

Screenshot 3: S3 Buckets Configuration

<img width="1919" height="1131" alt="image" src="https://github.com/user-attachments/assets/9efac0d7-5442-4256-ae51-44fddb76c7a8" />



