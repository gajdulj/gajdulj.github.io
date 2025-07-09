---
layout: post
title:  "Spark: Performance Optimisation Cheat Sheet ✨"
date:   2025-07-09 09:00:00 +0100
categories: jekyll update
---

## 🔍 Top 3 Spark Performance Issues

### 1. ⚖️ Skew 
- Skew is an uneven distribution of data across partitions.
- In a dataset where one value occurs more frequently than others, this can lead to uneven distribution of tasks across the workers.
- It can lead to slow tasks (parallelism not used enough), or the task can timeout and fail.

**Solution:**
- Enable adaptive query execution that automatically detects and handles skew.
- Use salting to distribute data evenly. By creating an artificial column, you can split the frequently occurring values into multiple partitions.
- Filter out and process skewed values separately.

### 2. 🔄 Shuffle
- costly data movement across the cluster
- Happens when you call an operation that requires an exchange of data between the nodes (e.g., `groupby` and `join`).

**Solution:**
- Filter early.
- Use broadcast join when possible (when one of the tables is small enough, you can send it to all the nodes for instant retrieval).
- Use partitioning to reduce the amount of data that needs to be shuffled.
- Use window functions instead of joins when possible.
- Use bucket join.

### 3. 💾 Spill
- Happens when the data doesn't fit in memory of a executor.
- memory limits → disk usage -> slow tasks

**Solution:**
- Select only the columns you need.
- Filter early.
- Reduce cardinality (the number of unique values in a column).
- Increase memory per core.

---

## ⚠️ Common Issues

### 1. 🗂️ Too many/ too few partitions 

- Too many small files cause issues with reading and metadata management.
- Few large files result in poor parallelism.
- Be cautious when designing your data ingestion process.
- Setting the wrong partition can result in too many small files and affect query performance.
- ✅ Use auto-compaction (e.g., when using Delta tables).
- On Databricks, you can use the `OPTIMIZE` command to compact small files into larger files and improve query performance.

### 2. Setting the Wrong Partition
- Best to partition on columns with low to moderate cardinality (dozens to thousands of values)
- To pick the right column for partitioning, consider the consumption patterns, including most frequently used filters, joins, and aggregations.

### 3. Following Spark Anti-Patterns

- Iterating over rows. Is a no-no as it can't be parallelised.
- Using Python UDFs when not needed. When possible, use Spark built-in functions.
- Calling `collect()` or `.toPandas()` on big datasets—defeats the purpose of using Spark.

### 4. Not Using Caching When Needed

- Due to Spark's lazy evaluation, whenever you call an action, the data is cleared from memory.
- To avoid expensive recomputation after you call an action, cache the DataFrame if you plan to use it multiple times; otherwise, it will be recomputed every time.
