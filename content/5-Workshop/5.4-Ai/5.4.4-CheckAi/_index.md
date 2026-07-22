---
title : "Test and Validate the AI Endpoints"
date : 2026-07-07
weight : 4
chapter : false
pre : " <b> 5.3.4 </b> "
---

### Step 1: Test Locally on EC2 with curl
1.	Test the Search service:
- curl localhost:8001/health   →   {"status":"ok","recipes_indexed":191000}
2.	Test the Chat service:
- curl localhost:8002/health   →   {"status":"ok","mode":"mysql"}

![overview](/images/5-Workshop/5.4-Ai/18.jpg)

### Step 2: Test via API Gateway (public HTTPS)
-	Use Postman or PowerShell to call the HTTPS endpoint through API Gateway:
GET https://lis0c4qiz0.execute-api.ap-southeast-1.amazonaws.com/search/health
→ {"status":"ok","recipes_indexed":191000}
![overview](/images/5-Workshop/5.4-Ai/19.jpg)
