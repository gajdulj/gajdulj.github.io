---
layout: post
title:  "Quick intro to delta lake"
date:   2025-07-31 09:00:00 +0100
categories: jekyll update
---
🔺 **What is delta lake?**
Delta lake is a table format that fits between storage and compute.
It enables object storage to behave like a database-like table.
Contrary to traditional databases, with delta lake the storage and compute are decoupled and can be scaled independently.

✅ ** Delta Table Benefits:**
- Tracks the changes to the data (deltas) and allows for easy rollback.
- Performance optimisations like collecting statistics on columns.
- Supports ACID transactions.
- Can enforce schema on writes and simple schema evolution.
- Works with both batch and streaming.

**How does it work?**
- Data is stored as Parquet files, while metadata and transaction history are tracked using log files stored in a hidden _delta_log/ directory inside the table directory.

The _delta_log/ contains:

- JSON log files: Each file represents a single atomic commit and contains metadata about added/removed files and schema changes.
- Checkpoint Parquet files: Periodically written to speed up reads, summarizing state up to a point (instead of scanning many JSON files).

When querying or writing to a Delta table:

- Delta reads the latest checkpoint + newer JSON logs to reconstruct the current table state.
- Changes (inserts, updates, deletes) are recorded as new log entries and new Parquet files — old files are not mutated, enabling versioning.
- This structure allows versioned reads, rollback to previous versions, and concurrent writes with isolation.

⚠️ **Things to watch out for:**
- Requires maintenance that need to be thought of when designing the pipelines. This includes `VACUUM` to delete the table backups that can take up a lot of storage and `OPTIMIZE` to reduce the number of files. Not having this may be okay to begin with but will eventually pile up as tech debt.
- Partitioning needs to be managed manually on write and needs to be well thought of. Liquid clustering in Databricks can help to simplify the challenge of choosing the right partition key and updating it as the data evolves.
- Monitor _delta_log growth and table versions. It's possible to accidentally create a lot of versions that will take up 100 or 1000 times the storage of the current version of the table. Example for this is when you do an upsert that uses bad merge conditions, such as checking on equality of columns that will be different for every run- such as load_timestamp or updated_at.

**References:**
- https://delta.io/
- https://docs.databricks.com/aws/en/delta/
- https://docs.google.com/document/d/1FWR3odjOw4v4-hjFy_hVaNdxHVs4WuK1asfB6M6XEMw/edit?usp=drivesdk