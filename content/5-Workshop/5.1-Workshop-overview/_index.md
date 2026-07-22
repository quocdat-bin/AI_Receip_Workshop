---
title : "Introduction"
date : 2026-07-07 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Introducing AI Recipe Finder 

In busy modern life, planning daily meals from leftover ingredients often takes a lot of time and easily leads to food waste. On top of that, manually tracking nutrition metrics (Calories, Protein, Fat, etc.) makes it hard for users to maintain a healthy diet. To fully address these problems, the project aims to build **AI Recipe Finder** – an intelligent dish-suggestion platform on AWS. The system provides a semantic search tool that identifies dishes from the input ingredients, integrates an AI Chatbot that acts as a virtual assistant guiding cooking step by step, and automatically analyzes and calculates nutrition. This platform not only helps individual users save time and prevent waste, but also serves as a real-world technical use-case demonstrating the effective combination of Cloud Computing and Generative AI.

#### Workshop Overview
In this workshop, the entire system is designed following a secure cloud architecture, with clearly separated service layers and optimization for Serverless/Containerized environments. The data flow begins when a user accesses the web interface (a React SPA), which is hosted and deployed automatically (CI/CD) through **AWS Amplify**. Login and identity verification are strictly and securely managed by **Amazon Cognito**. After authentication, instead of exposing the backend to the public internet, requests from the Frontend are routed through a **VPC Link** bridge into the private internal network (VPC). Within this secure network, the **EC2 Recipe API** receives requests, processes the system logic, and looks up structured data (user information, recipes, history) from the relational database **Amazon RDS (MySQL)**. In particular, for tasks that require Artificial Intelligence, requests are handed off to the **EC2 AI Server** (running independently via **Docker** containers). This server is flexibly designed to communicate securely over HTTPS with the **Google Gemini API**, taking full advantage of the LLM's natural-language-processing power and function-calling capability to operate the Semantic Search tool and the AI Chatbot, ensuring accurate, fast responses and easy cost control during deployment.

![overview](/images/5-Workshop/5.1-Workshop-overview/workflow.jpg)

**Basic Security and Access Management**

To ensure maximum safety for the data and the system according to AWS standards, the project's architecture strictly applies the core security measures below:
-	Access management with IAM Roles: The system strictly follows the Principle of Least Privilege and absolutely never uses hard-coded access keys in the source code. Instead, the EC2 servers (Recipe API and AI Server) are assigned IAM Roles with individually designed policies. These Roles grant only exactly the permissions the instances need to write system logs to CloudWatch or interact safely with other AWS services.
-	Hiding the Backend from the Public Internet with a VPC Link: The security highlight of this architecture is fully isolating the Backend system and Database from the public network environment. The core resources, including the EC2 Recipe API, the EC2 AI Server, and the Amazon RDS database, are all placed deep inside Private Subnets of the virtual network (VPC) and have no Public IP address at all. To allow the Frontend interface to communicate with the Backend, the system uses a VPC Link integrated with Amazon API Gateway. The VPC Link acts as a private, secure tunnel, allowing API Gateway to route valid request flows directly into the internal network without having to expose any endpoint or open any port to the internet. Thanks to this design, the server system and database are comprehensively protected from vulnerability scanning and direct external attacks.


