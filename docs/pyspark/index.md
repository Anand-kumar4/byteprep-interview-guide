# PySpark Coding Questions

Below are some medium-complexity PySpark problems commonly asked in data engineering interviews.

---

### 1. Group Customers by First Purchase Category

**Problem:**  
Given a dataset of customer purchases, write a PySpark program to find the first product category each customer purchased.

**Input Schema:**  
`customer_id`, `purchase_date`, `category`

**Expected Output:**  
`customer_id`, `first_category`

```python
import pyspark.sql.functions as F
from pyspark.sql import Window
from pyspark.sql.functions import row_number

window_spec = Window.partitionBy("customer_id").orderBy("purchase_date")
df_with_rank = df.withColumn("rank", row_number().over(window_spec))
first_purchase = df_with_rank.filter(F.col("rank") == 1).select("customer_id", "category").withColumnRenamed("category", "first_category")
```

---

### 2. Rolling 7-Day Aggregation of Page Views

**Problem:**  
Given a dataset of web page views, compute the rolling 7-day total views per user.

**Input Schema:**  
`user_id`, `view_date`, `page_url`

**Expected Output:**  
`user_id`, `view_date`, `rolling_7_day_views`

```python
from pyspark.sql import functions as F
from pyspark.sql.window import Window

df = df.withColumn("view_date", F.to_date("view_date"))
df = df.withColumn("view_date_ts", F.col("view_date").cast("timestamp").cast("long"))
window_spec = Window.partitionBy("user_id").orderBy("view_date_ts").rangeBetween(-6 * 86400, 0)
daily_views = df.groupBy("user_id", "view_date").agg(F.count("*").alias("daily_views"))
rolling_views = daily_views.withColumn("rolling_7_day_views", F.sum("daily_views").over(window_spec))
```

---

### 3. Detect Session Start and End for User Activity

**Problem:**  
Given a timestamped list of user actions, detect sessions for each user. A new session starts if the gap between two actions is more than 30 minutes.

**Input Schema:**  
`user_id`, `timestamp`, `event_type`

**Expected Output:**  
`user_id`, `session_id`, `session_start`, `session_end`

```python
from pyspark.sql import functions as F
from pyspark.sql.window import Window

df = df.withColumn("timestamp", F.to_timestamp("timestamp"))
window_spec = Window.partitionBy("user_id").orderBy("timestamp")
df = df.withColumn("prev_timestamp", F.lag("timestamp").over(window_spec))
df = df.withColumn("session_gap", F.unix_timestamp("timestamp") - F.unix_timestamp("prev_timestamp"))
df = df.withColumn("is_new_session", (F.col("session_gap") > 1800) | F.col("session_gap").isNull())
window_spec_session = window_spec.rowsBetween(Window.unboundedPreceding, 0)
df = df.withColumn("session_id", F.sum(F.col("is_new_session").cast("int")).over(window_spec_session))

session_boundaries = df.groupBy("user_id", "session_id").agg(
    F.min("timestamp").alias("session_start"),
    F.max("timestamp").alias("session_end")
)
```

---

### 4. Identify Top N Products Per Category by Revenue

**Problem:**  
Given a dataset of product sales, calculate revenue per product and return top 3 products in each category.

**Input Schema:**  
`product_id`, `category`, `unit_price`, `quantity`

**Expected Output:**  
`category`, `product_id`, `revenue`

```python
from pyspark.sql import functions as F
from pyspark.sql.window import Window

df = df.withColumn("revenue", F.col("unit_price") * F.col("quantity"))
window_spec = Window.partitionBy("category").orderBy(F.desc("revenue"))
df = df.withColumn("rank", F.row_number().over(window_spec))
top_n_products = df.filter(F.col("rank") <= 3).select("category", "product_id", "revenue")
```

> Note: You can also use `dense_rank()` or `rank()` depending on how you want to treat ties in revenue.
