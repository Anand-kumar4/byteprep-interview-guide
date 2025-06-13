
---
title: AWS Interview Questions
description: Explore real-world AWS scenario-based questions covering Glue, S3, Lambda, and best practices for data engineering interviews.
---



## AWS Scenario-Based Questions

### 1. AWS Glue Scenario

**Question:** You have a daily batch job that ingests CSV files from an S3 bucket, performs transformations, and writes to a Snowflake data warehouse. Occasionally, the schema of incoming files changes (e.g., new columns are added). How do you handle schema evolution in AWS Glue?

**Answer:**
- Use AWS Glue DynamicFrame with `mergeSchema=True` to handle schema evolution.
- Create a Glue Crawler that runs before the ETL job to update the Data Catalog.
- In PySpark code, dynamically reference columns using `df.columns` and use conditional logic (`if 'new_col' in df.columns`) for optional fields.
- Update the Snowflake destination table with new columns and use `MERGE` logic to maintain idempotency.

---

### 2. Amazon S3 Scenario

**Question:** You are storing large amounts of data in S3 that are accessed infrequently but must be retrieved within a few minutes when needed. You also want to reduce costs. Which storage class should you use and how will you automate the transition?

**Answer:**
- Use **S3 Intelligent-Tiering** or **S3 Standard-IA (Infrequent Access)** based on access patterns.
- Set up an S3 Lifecycle policy to transition objects to the desired storage class after a specific number of days (e.g., 30 days).
- Enable object-level logging and monitoring via CloudWatch to refine lifecycle rules over time.

---

### 3. AWS Lambda Scenario

**Question:** You have an S3 bucket that receives real-time uploads of images. You want to resize and store them in another bucket as thumbnails. How do you implement this using AWS Lambda?

**Answer:**
- Configure the source S3 bucket to trigger a Lambda function on `s3:ObjectCreated` events.
- The Lambda function (using Python or Node.js) reads the image using `boto3`, processes it using `Pillow` or `Sharp`, and writes the thumbnail to a destination S3 bucket.
- Ensure the Lambda has appropriate IAM permissions for both source and destination buckets.
- Monitor the Lambda executions via CloudWatch for errors and duration.