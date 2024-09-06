# IMDb Movie Data ETL Pipeline with Data Quality Monitoring

## Project Overview
This project aims to extract and process IMDb movie data using AWS services. The raw data is stored in an S3 bucket and is transformed through an ETL pipeline built using AWS Glue. The pipeline includes data quality checks to filter out invalid records. Valid records are stored in Redshift for further analysis, while invalid records are kept in a separate S3 location for review. The process is monitored using Amazon SNS and EventBridge for email notifications upon ETL job execution.

## Architecture Diagram
![ETL Pipeline Architecture](./path_to_diagram/architecture_diagram.png)

## Tech Stack
- **AWS S3**: Stores raw IMDb movie data (CSV format).
- **AWS Glue Crawler**: Crawls S3 and Redshift tables to infer schema and create data catalogs.
- **AWS Glue Visual ETL**: Extracts, transforms, and loads data while applying data quality checks.
- **Amazon Redshift**: Stores processed data for further analysis and reporting.
- **AWS SNS**: Sends email notifications upon ETL job completion.
- **AWS EventBridge**: Triggers SNS notifications on job success or failure.

---

## Project Steps

### 1. Data Storage in S3
- **Raw Data**: IMDb movie data in CSV format is uploaded to an S3 bucket.

### 2. Schema Detection Using Glue Crawler
- A Glue Crawler is created to scan the CSV file in S3.
- The crawler detects the schema and creates a corresponding data catalog for the S3 file.

### 3. Redshift Schema and Table Creation
- A Redshift cluster is set up.
- The Redshift schema and table are created to store the transformed data.

- **Creating a Glue Crawler for Redshift Table**
  : Another Glue Crawler is created to catalog the Redshift table. This allows the Redshift table’s metadata to be stored in the Glue Data Catalog, making it accessible for querying and future use
  
### 4. Data Extraction and Transformation in Glue Visual ETL
- **ETL Job Creation**: A Glue Visual ETL job is created to extract data from S3.
- **Data Quality Checks**: Additional columns are added to the dataset that mark rows as `Succeed` or `Failed` based on data quality checks.
- **Conditional Routing**: A Glue Conditional Router splits the data:
  - Records that pass the quality check are loaded into the Redshift table.
  - Records that fail the quality check are written back to a different S3 location for further analysis.

### 5. Data Load to Redshift
- The transformed data that passed validation is loaded into Redshift using the Glue ETL job.

### 6. Storing Invalid Data in S3
- Records that failed the quality checks are stored in a designated S3 bucket:

### 7. Monitoring and Notifications
- **SNS Notification**: Amazon SNS is used to send email notifications upon ETL job completion.
- **EventBridge**: EventBridge triggers the SNS topic upon completion of the Glue ETL job, sending an email notification about the job status (success or failure).

### 8. Email Notification on ETL Job Completion
- Upon each job run, an email notification is sent using the SNS topic. The email includes the job execution status (success or failure) and other relevant details.

## Conclusion
This ETL pipeline provides a robust and efficient solution for extracting, transforming, and validating IMDb movie data. Leveraging AWS services such as Glue, Redshift, SNS, and EventBridge, the pipeline ensures accurate data processing, quality monitoring, and real-time notifications.
