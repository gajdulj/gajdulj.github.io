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
- Most expensive operation in Spark.
- Costly data movement across the cluster
- Happens when you call an operation that requires an exchange of data between the nodes (e.g., `groupby` and `join`).

**Solution:**
- Filter early.
- Use broadcast join when possible (when one of the tables is small enough, you can send it to all the nodes for instant retrieval).
- Use partitioning to reduce the amount of data that needs to be shuffled.
- Use window functions instead of groupby & joins when possible.

### 3. 💾 Spill
- Happens when the data doesn't fit in memory of a executor.
- Memory limits → disk usage -> slow tasks

**Solution:**
- Select only the columns you need, filter early.
- Reduce cardinality (the number of unique values in a column).
- Increase memory per core.

---

## ⚠️ Common Pitfalls

### 1. 🗂️ Too many/ too few partitions 
- Too many small files cause issues with file reads and metadata management, negatively impacting query performance.
- Few large files result in poor parallelism.
- ✅ Use auto-compaction (e.g., when using Delta tables).
- On Databricks, you can use the `OPTIMIZE` command to compact small files into larger files and improve query performance.

### 2. Setting the wrong partition
- Best to partition on columns with low to moderate cardinality (dozens to thousands of values)
- When designing the data ingestion process, work from the end goal in mind.
- To pick the right column for partitioning, consider the consumption patterns, including most frequently used filters, joins, and aggregations.

### 3. Following Spark anti-patterns

- Iterating over rows. Is a no-no as it can't be parallelised.
- Using Python UDFs when not needed. When possible, use Spark built-in functions.
- Calling `collect()` or `.toPandas()` on big datasets—defeats the purpose of using Spark.

### 4. Not using caching when you should

- Due to Spark's lazy evaluation, whenever you call an action, the data is cleared from memory.
- To avoid expensive recomputation after you call an action, cache the DataFrame if you plan to use it multiple times; otherwise, it will be recomputed every time.
