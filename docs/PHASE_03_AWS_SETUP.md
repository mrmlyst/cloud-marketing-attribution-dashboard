# Phase 3 – AWS Cloud Setup & Data Lake Configuration

## Overview
In this phase, cleaned CSV datasets were uploaded to Amazon S3 and manually registered in Amazon Athena as external tables for cloud-based SQL analytics.

---

## S3 Configuration

### Bucket Name
muiz-cloud-marketing-analytics-2026

### Folder Structure

project1_clean/
project1_clean_customers/
project1_clean_sessions/
project1_clean_events/
project1_clean_orders/
project1_clean_campaigns/
project1_clean_ad_spend/


Each dataset was placed in a separate S3 prefix to ensure proper external table mapping in Athena.

---

## Athena Setup

### Query Result Location

s3://muiz-cloud-marketing-analytics-2026/athena_results/


### Database Created

project1_marketing_db


### Table Creation Approach

Instead of using a Glue Crawler, external tables were manually created using SQL `CREATE EXTERNAL TABLE` statements.

Reason:
- Better schema control
- Clear mapping between S3 prefixes and tables
- Avoid multi-schema folder merge issues

---

## Tables Created

- customers_clean
- sessions_clean
- events_clean
- orders_clean
- campaigns_clean
- ad_spend_clean

Each table maps to its corresponding S3 folder location.

---

## Data Validation Performed

- Verified row counts using COUNT(*)
- Confirmed correct parsing of event_type values
- Debugged encoding issues by exporting CSV files as UTF-8
- Updated table LOCATION paths using ALTER TABLE

---

## Key Cloud Lessons

- Athena reads strictly from the defined S3 LOCATION.
- Proper S3 prefix structure is critical.
- CSV encoding must be UTF-8 for correct parsing.
- External tables enable serverless analytics without loading data into a warehouse.

---

## Screenshots (To Be Added)

- S3 bucket structure
- Athena CREATE TABLE statement
- COUNT(*) validation query
- Funnel event_type distribution query
