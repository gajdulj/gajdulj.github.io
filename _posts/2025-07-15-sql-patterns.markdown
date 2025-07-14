---
layout: post
title:  "SQL patterns that will level up your data engineering skills"
date:   2025-07-15 09:00:00 +0100
categories: jekyll update
---

## 1. Simplify queries with CTEs

- It's a smaller query defined on the top of the script that can be used in a larger query later on.
- It helps with readability and simplicity of SQL scripts.
- Abstracts away complex logic making larger queries more manageable.

**Example:**
We want to find tennis players with more than average number of aces.

| Player   | Aces |
|----------|------|
| Alcaraz  |  75  |
| Sinner   |  54  |
| Djokovic |  80  |

```sql
WITH avg_aces AS (
    SELECT AVG(aces) AS avg_val
    FROM players
)

SELECT player, aces
FROM players, avg_aces
WHERE aces > avg_val;

```

|  players | aces |
|----------|------|
|  Alcaraz |  75  |
| Djokovic |  80  |

tips:
- Keep CTEs small and single purpose.
- If we keep coming back to the same query, creating a view or a table may be a more efficient way to store the result.


## 2. Use window functions

- They allow us to perform calculations across rows of a table.
- Unlike aggregate functions, they don't reduce the number of rows
- Result in cleaner queries as we avoid excessive joins.
- Keep the granularity of individual rows while accessing summary-level insights.

**Example:**
We want to find how many more or less aces a player had compared to their previous match.

| player   | match_date | aces |
|----------|------------|------|
| Alcaraz  | 2024-07-01 |  50  |
| Alcaraz  | 2024-07-03 |  60  |
| Alcaraz  | 2024-07-05 |  55  |


```sql
SELECT player,
    match_date,
    aces,
    LAG(aces) OVER (
        PARTITION BY player ORDER BY match_date
    ) AS prev_match_aces
FROM player_stats;
```

| player   | match_date | aces | prev_match_aces |
|----------|------------|------|----------------|
| Alcaraz  | 2024-07-01 |  50  |      NULL      |
| Alcaraz  | 2024-07-03 |  60  |       50       |
| Alcaraz  | 2024-07-05 |  55  |       60       |
|----------|------------|------|----------------|


## 3. JOIN responsibly

- Understand the data relationships before the join (one to one, one to many, many to many)
- Don't assume keys are unique. Expect duplicates.
- Pick the right join type for the intended use case.

watch out for:
- Avoid unintended data duplication, always check before and after join unique counts.
