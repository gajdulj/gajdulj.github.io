---
layout: post
title:  "Databricks file pruning: the hidden performance secret"
date:   2025-07-24 09:00:00 +0100
categories: jekyll update
---

Does column order matter for the performance of your queries? 
I never expected it to until I used Databricks!

This is because of **file pruning** - an optimisation technique that is used by Spark. It allows the query engine to only look into files that match the expected criteria - based on column statistics like min/max values. If the filter condition is not met - the file is skipped, saving us time and money.

By default, only the first **32 columns** get skipped. So if you have a column that's frequently used for filters, make sure it's not at the end of a very wide table.

- 🐌 **Without optimisation**: Spark reads every file, even when the filter would eliminate most data
- ⚡ **With column reordering**: Moving the frequently-filtered column to the first 32 positions enables file pruning
- 📈 **Performance gain**: Queries can be 10x faster or more, depending on data distribution

P.S. The default can be changed with `spark.databricks.io.skipping.defaultNumIndexedCols` but I think it makes a lot of sense to put the most frequently queried columns at the beginning when designing the table.

# For another post

- Common Spark performance bottlenecks
- Data partitioning strategies