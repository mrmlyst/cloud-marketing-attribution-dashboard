Objective

Establish a structured cloud data lake architecture in Amazon S3 and manually configure Amazon Athena external tables to enable serverless SQL analytics.

This phase focused on:

Cloud storage organization

Clean vs raw data separation

Manual table creation (no Glue crawler)

Debugging encoding and schema errors

Preparing data for analytical modeling

Amazon S3 Data Lake Architecture
Bucket Created
muiz-cloud-marketing-analytics-2026

Folder Structure Designed
project1_raw/
project1_clean/
project1_qs/

Explanation
Folder	Purpose
project1_raw	Original unmodified CSV files
project1_clean	Excel-cleaned, UTF-8 standardized datasets
project1_qs	Optimized Parquet outputs for BI (CTAS tables)

This separation ensures:

Data lineage traceability

Clean audit trail

Clear ETL boundaries

BI performance optimization



Manual Table Creation in Athena

Instead of using AWS Glue Crawlers, tables were manually created to ensure:

Explicit schema control

Correct data types

Avoidance of crawler misclassification

Better debugging transparency


Database Created
CREATE DATABASE project1_marketing_db;


External Table Pattern Used

Example (sessions_clean):

CREATE EXTERNAL TABLE project1_marketing_db.sessions_clean (
    session_id STRING,
    customer_id STRING,
    session_date STRING,
    channel STRING
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (
    'separatorChar' = ',',
    'quoteChar' = '"'
)
LOCATION 's3://muiz-cloud-marketing-analytics-2026/project1_clean/project1_clean_sessions/';

Each table pointed directly to a clean S3 prefix.


Debugging & Data Quality Issues Solved

During Athena modeling, several real-world issues occurred.

Issue 1 — Corrupted Encoding in events_clean

Symptom:

event_type displayed strange characters

Root Cause:

Non-UTF8 encoding in CSV

Fix:

Re-exported cleaned CSV as UTF-8 in Excel

Re-uploaded to S3

Re-exported cleaned CSV as UTF-8 in Excel

Re-uploaded to S3

Issue 2 — TABLE_NOT_FOUND Error

Error:

Table does not exist in awsdatacatalog

Root Cause:

Table created in different database

Incorrect S3 location path

Fix:

Verified correct database

Recreated table pointing to correct S3 prefix


Issue 3 — BAD_DATA Parsing Error

Error:

Error parsing column 'customer_id'

Root Cause:

Empty string values in numeric columns

Fix:

Re-exported CSV ensuring NULLs instead of empty strings

Updated table properties where necessary


Issue 4 — Date Parsing Failures

Error:

INVALID_CAST_ARGUMENT: Value cannot be cast to date

Root Cause:

session_date format inconsistencies

Fix:

Used REGEXP_EXTRACT + DATE_PARSE in SQL views

Example:

CAST(
  date_parse(
    regexp_extract(session_date, '^[0-9]{1,2}/[0-9]{1,2}/[0-9]{4}', 0),
    '%c/%e/%Y'
  ) AS DATE
)

This ensured consistent date casting for analysis.


Creation of Analytical Views

After validating base tables, views were created to serve as semantic models.

Views included:

v_channel_funnel

v_roas_performance

v_high_value_customers

v_campaign_lift

v_campaign_lift_summary

Purpose:

Separate transformation logic from visualization

Centralize metric definitions

Improve BI consistency


Parquet Optimization for QuickSight

To prevent SPICE import errors and improve performance, a CTAS (Create Table As Select) strategy was used.

Example:

CREATE TABLE project1_marketing_db.qs_campaign_lift
WITH (
  format = 'PARQUET',
  external_location = 's3://muiz-cloud-marketing-analytics-2026/project1_qs/campaign_lift/'
) AS
SELECT ...

Benefits:

Faster BI performance

Reduced data scanned

Cleaner schema inference

Elimination of complex view parsing errors


Athena Results Location Configuration

Athena query results were configured to store in S3:

s3://muiz-cloud-marketing-analytics-2026/athena_results/

This ensures:

Reproducibility

Auditing

Proper QuickSight integration


Outcomes of Phase 2

By the end of this phase:

✔ Structured cloud data lake established
✔ External tables manually configured
✔ Data quality issues resolved
✔ Views created for funnel & ROAS analysis
✔ Parquet optimization implemented
✔ Query result storage configured
✔ BI-ready data prepared

This phase transformed raw marketing CSV data into a structured, query-ready cloud analytics environment.


Skills Demonstrated in Phase 2

Cloud Data Lake Architecture

Schema Design in Athena

SQL Debugging

Encoding & NULL Handling

Serverless Analytics

Data Modeling Best Practices

Parquet Optimization via CTAS

Query Engine Troubleshooting
