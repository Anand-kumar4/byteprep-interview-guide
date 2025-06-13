# Data Modeling & Warehousing

## 1. Star vs Snowflake Schema

**Question:** When should you use Star Schema vs Snowflake Schema in a data warehouse?

**Answer:**
- Use **Star Schema** when you prioritize performance and have denormalized dimensions.
- Use **Snowflake Schema** when space optimization, flexibility, or easier maintenance is important.

## 2. Slowly Changing Dimensions (SCD)

**Question:** How do you implement SCD Type 2 in a data warehouse?

**Answer:**
- Maintain historical records by adding `start_date`, `end_date`, and `is_current` columns.
- Use ETL tools (Glue, dbt, etc.) or SQL MERGE logic to detect changes and insert new rows.