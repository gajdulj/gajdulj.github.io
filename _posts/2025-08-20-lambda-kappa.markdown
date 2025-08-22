---
layout: post
title:  "Lambda vs Kappa architecture"
date:   2025-08-20 09:00:00 +0100
categories: jekyll update
---

What are the common data processing architectures and what are the pros and cons of each?


Lambda
Starts with messaging layer- where stream data is queued for processing.
Often uses Kafka as a messaging layer.
Then it's split to:
- batch layer- handles data in files or other batch processes
- streaming layer (also known as speed layer)- can be consumed instantly with an API

Both layers then land in a serving layer (often data warehouse) where analytics processes can access the data.

Advantages:
- Can use batch processing
- Can use streaming only when needed
- Enables fast access when needed but keep the reliability of batch
- Batch process can do validation and cleaning

Disadvantages:
- Complexity of the architecture: need to maintain two separate processes
- Timing of data more difficult to manage as some are daily and some real time

Kappa
Message layer coordinates everything.
Everything gets passed through speed layer as received and landing in a serving layer.

Advantages:
- Fast
- Simpler architecture with fewer parts

Disadvantages:
- Difficult to achieve: when files used to transfer data
- Legacy systems can't stream

If all sources can stream data- Kappa is perfect.
If you have batch sources- start with Lambda and try to add streaming.
