---
title : "Deploying the AI Chatbox & Artificial Intelligence (AI Services)"
date : 2026-07-07 
weight : 5
chapter : false
pre : " <b> 5.3. </b> "
---

#### Overview

This phase focuses on setting up the server infrastructure and integrating Artificial Intelligence (AI) technologies into the platform. The process ensures the system has enough resources to run its core services while successfully connecting to large language models to deliver intelligent search and natural conversation for users.

In this deployment step, we will carry out three main tasks to bring the AI engine into real operation:

- Prepare the AI Server (EC2) - Launch and configure an Amazon EC2 virtual server. Depending on resource requirements, the system will either use a dedicated instance for AI or set up a shared configuration to optimize cost and performance.

- Deploy services with Docker - Build and run independent containers for the FastAPI application. Traffic is clearly separated, with Port 8001 handling the search feature (/search) and Port 8002 handling the chat feature (/chat), following the architecture design diagram.

- Integrate the AI Engine - Build the logic code in FastAPI to connect the API out to external LLM services (such as Amazon Bedrock or the Google Gemini API). This is the core processing layer that lets the system perform Semantic Search and run the intelligent cooking-advisor Chatbot.
    
![overview](/images/5-Workshop/5.1-Workshop-overview/Workflow.jpg)


#### Contents

- [Prepare the AI Server (EC2) & Network Infrastructure (VPC, Security Group, Elastic IP)](5.4.1-EC2//)
- [Deploy Services with Docker & Vector Database (pgvector)](5.4.2-Docker/-ai/)
- [Integrate the AI Engine & Processing Logic (Semantic Search & Gemini Chatbot)](5.4.3-Chatbot/)
- [Test and Validate the AI Endpoints](5.4.4-CheckAi/)
