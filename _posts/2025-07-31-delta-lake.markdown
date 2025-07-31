---
layout: post
title:  "Quick intro to delta lake"
date:   2025-07-31 09:00:00 +0100
categories: jekyll update
---

Delta lake is a table format that fits between storage and compute.
It enables object storage to behave like a database-like table.

Benefits:
- Storage and compute are decoupled and can be scaled independently.
- Tracks the changes to the data (deltas) and allows for easy rollback.
- Performance optimisations like collecting statistics on columns.
- Supports ACID transactions.
- Can enforce schema on writes and simple schema evolution.
- Works with both batch and streaming.

Things to watch out for:
- Requires maintenance that need to be thought of when designing the pipelines. This includes `VACUUM` to delete the table backups that can take up a lot of storage and `OPTIMIZE` to reduce the number of files. Not having this may be okay to begin with but will eventually pile up as tech debt.
- Partitioning needs to be managed manually on write. Unless you use something like liquid clustering in Databricks which can help to simplify the maintenance.
- Monitor _delta_log growth and table versions. It's possible to accidentally create a lot of versions that will take up 100 or 1000 times the storage of the current version of the table. Example for this is when you do an upsert that uses bad merge conditions, such as checking on equality of columns that will be different for every run- such as load_timestamp or updated_at.
