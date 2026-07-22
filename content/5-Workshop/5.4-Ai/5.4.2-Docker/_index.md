---
title : "Deploy Services with Docker & Vector Database (pgvector)"
date : 2026-07-07
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

### Step 1: Launch the PostgreSQL Container with pgvector Integrated
-	Run a Docker container to store 384-dimensional vectors on EC2
### Step 2: Run the Large-scale Batch Embedding Script (sync_bulk.py)
-	Use tmux to keep the process running in the background when the SSH session disconnects

![overview](/images/5-Workshop/5.4-Ai/13.jpg)
- The process automatically reads MySQL in batches of 1,000 dishes, encodes vectors at about ~32 dishes/second, and pushes them into pgvector.

### Step 3: Check the Number of Vector Records in the Database
-	Verify the number of records that have been loaded into pgvector:

![overview](/images/5-Workshop/5.4-Ai/13_2.jpg)
