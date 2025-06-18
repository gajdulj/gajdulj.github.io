---
layout: post
title:  "3 core tools every data engineer should master (and why)"
date:   2025-06-18 09:00:00 +0100
categories: jekyll update
---

Despite the growing number of tools and frameworks, the fundamentals of data engineering technologies are quite static.
Here are couple of areas that are worth focusing on from the start as they come up in almost all projects.


# 📌 Must have

1. **SQL** - a language for interacting with databases. Must have for querying and manipulating data.  
Used in all modern data platforms.

   Useful commands:
   - `SELECT column_name FROM table_name` - simple query to get data from a table
   - `WHERE` - filter the data
   - `ORDER BY` - sort the data, to answer questions like "what is the most popular product?"
   - `GROUP BY` - group the data, ex. for calculating user monthly spend
   - `JOIN` - join together two tables by a common column
   - `WINDOW FUNCTIONS` - aggregations over subsets of rows, without changing the granularity of the data

   Knowing the above should cover 90% of SQL use cases.

1. **Python** - a general purpose programming language that excels in data.  
It simplifies the complexity and makes it easier to focus on the business logic.

   Opens the door to many applications, including big data and AI.  
   It's power is in a super wide range of open source libraries and frameworks.

   You can use it for anything data related: transforming data, visualising it, training machine learning models, bu not only. It can also be used in web development and automation so its definitely worth learning.

   libraries: 
   pandas, matplotlib, sci-kit learn, etc.

1. **Linux** - an entry point into running scripts and automation.  
Has a very steep learning curve, knowing the basics will make you more productive.

   Useful commands:
   - `ls` - list files and directories
   - `cd` - change directory
   - `mkdir` - make directory
   - `rm` - remove file
   - `cp` - copy file
   - `mv` - move file
   - `cat` - print file content
   - `chmod` - change file permissions

---

# ✨ Nice to have

1. **git** - a way to go for code versioning. Helps with staying sane over code changes and face the consequences of accidentally deleting your work. (we have all been there). Helps to keep track of changes introduced in our applications and make the quality of code easier to maintain through the process of Pull Requests.

1. **Orchestration** - a way to automate the execution of tasks and manage the dependencies between them.  
   For simpler use cases you can use cron jobs that are built in linux tool for simple scheduling.  
   For bigger workflows, requiring more complex dependencies, you will need something like Airflow or Dagster.  
   Must have for any data platform.

1. **Cloud** - when you want to step away from your local laptop and have your data applications running live reliably, you need to deploy it on some kind of server. A natural choice is public cloud providers like AWS, GCP or Azure because buying and stacking hardware is not cool anymore.

   Not all data engineering roles interact with it directly, I found that operations heavy and data platform jobs tend to need it the most.


1. **CI/CD & infrastructure as code** - a way to automate the deployment of your code. Be able to reliably tell that your code is working as expected before it goes live. These are **MUST HAVE** considerations for enterprise scale applications but if you are just getting started, I wouldn't worry about them at the start.
**This is rather a controversial take, but I have seen startup projects fail because they followed enterprise best practices too early before identifying the right problems to solve.**


1. **Big Data** - Must have when data is too big to fit into memory of a single machine. Although it has a lot of use cases, a lot of companies don't really need it due to the scale of their data.

   This is a broad topic so expect separate posts on this.
example: 
Apache Spark.


Is that all? No!
There are thousands of other frameworks and technologies that will come up in your projects! 
But mastering the fundamental tools that are not going anywhere soon seems the best time investment at the start. They keep coming up and not going anywhere.


**TLDR, this is how I would prioritise learning the tools if I started my journey again:**

![Data Tiering](/assets\/images\/tiering.png)
