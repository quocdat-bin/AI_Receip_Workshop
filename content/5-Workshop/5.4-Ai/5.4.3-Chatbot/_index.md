---
title : "Integrate the AI Engine & Processing Logic (Semantic Search & Gemini Chatbot)"
date : 2026-07-07
weight : 3
chapter : false
pre : " <b> 5.3.3 </b> "
---

### Step 1: Local Multilingual Embedding Engine (embeddings.py)
1.	Use the sentence-transformers model (MiniLM, 384 dimensions) running directly on EC2.
2.	The model supports Vietnamese natively, allowing users to enter ingredients in Vietnamese (for example: "thịt lợn, trứng, cà chua") without needing translation.
### Step 2: Integrate the Nutrition Chatbot (chat.py)
-	Integrate the Google Gemini API combined with an automatic Function Calling (Tool Calling) mechanism:
    -	lookup_nutrition: Look up calorie, protein, carb, and fat information from MySQL.
    -	search_recipes: Suggest suitable recipes based on the context of the question.
![overview](/images/5-Workshop/5.4-Ai/14.jpg)

### Step 3: API Gateway Routing (ai-recipe-finder-api)
-	Create Routes on AWS API Gateway pointing to the EC2 AI Server:
    -	POST /search → Points to [http://54.254.77.58:8001/search](http://54.254.77.58:8001/search)
    -	POST /chat → Points to [http://54.254.77.58:8002/chat](http://54.254.77.58:8002/chat)
    -	GET /health → Check the server status.

![overview](/images/5-Workshop/5.4-Ai/15.jpg)

![overview](/images/5-Workshop/5.4-Ai/16.jpg)

- Configuring Routes on AWS API Gateway.

### Step 4: Deploy Background Services with systemd (auto-start & self-recovery)
-	Register the two FastAPI services as systemd services on EC2 so they start with the machine and restart automatically on failure:
    -	ai-search.service — runs Uvicorn on port 8001 (/search).
    -	ai-chat.service — runs Uvicorn on port 8002 (/chat).
sudo systemctl enable --now ai-search ai-chat
*Note:* pgvector runs inside a Docker container; the two FastAPI services run directly via systemd, making them lightweight and self-recovering after the machine reboots.


![overview](/images/5-Workshop/5.4-Ai/17.jpg)
