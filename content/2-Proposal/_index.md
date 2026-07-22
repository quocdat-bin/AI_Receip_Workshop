---
title: "Proposal"
date: 2026-07-07
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# AI Recipe Finder
## Smart AI-Powered Recipe Recommendation System on the AWS Platform

### 1. Introduction
AI Recipe Finder is a website designed to help users find dishes based on available ingredients. The system uses AI to suggest recipes, assist with cooking via a chatbot, and analyze nutritional value based on standard data. The entire system is deployed on the Amazon Web Services (AWS) platform to ensure scalability, stability, and ease of practical deployment.

### 2. Problem Statement
### What’s the Problem?
Users frequently struggle to decide on a daily menu based on available or leftover ingredients in their kitchen, leading to food waste. Searching for suitable recipes on traditional tools takes a lot of time and often does not exactly match the actual ingredients. Additionally, self-tracking and calculating nutritional indicators (Calories, Protein, Fat, Carbohydrates) for each meal is very manual and complex. Current applications on the market often lack smart interaction and do not have nutritional consulting systems or flexible real-time cooking guidance.

### The Solution
The AI Recipe Finder platform is fully deployed on the AWS Cloud model to completely resolve the above issues. The platform uses smart AI search technology (Semantic Search) to analyze user-inputted ingredients and provide accurate recipe suggestions. The system integrates Amazon Bedrock to operate an AI Chatbot, acting as a virtual assistant to support step-by-step cooking instructions and advise on ingredient substitutions. The Backend API (running on EC2 or App Runner) connects with the USDA database to automatically analyze detailed Calories, Protein, Fat, and Carbohydrates for each dish. The entire interface is provided via AWS Amplify, combined with an Amazon RDS database, creating a centralized, stable, and easily scalable architecture.

### Benefits 
The solution provides a powerful tool that helps individual users save time, optimize food usage to avoid waste, and easily maintain a healthy diet thanks to automated nutritional data. Technically, the platform creates a complete foundational product that demonstrates the practical application and effectiveness of Generative AI combined with Cloud Computing. At the same time, the flexible architecture of AWS helps build a solid foundation so the team can easily expand with advanced features such as personalized meal planning, ingredient recognition via images, or aiming for commercialization of the system in the future.

### 3. Solution Architecture
The platform applies a cloud-based architecture on AWS to manage and suggest thousands of cooking recipes based on the user's ingredients. Requests from the web interface are received and processed by the Backend API (running on EC2 or App Runner), looking up data from Amazon RDS and images from Amazon S3. Advanced tasks such as semantic search and the nutritional consulting Chatbot are processed directly by Amazon Bedrock, while AWS Amplify provides a smooth and stable user interface.

![AI Recipe Finder](/images/2-Proposal/Proposal1.jpg)

### AWS Services Used
-	AWS Amplify: Hosts and deploys the web interface (React + Tailwind CSS). 
-	Amazon Cognito: Securely handles login, identity verification, and user access management. 
-	Amazon EC2: Operates the Backend API (FastAPI/Node.js) to process system logic. 
-	Amazon RDS: Acts as a relational database storing user information, search history, and recipes. 
-	Amazon S3: Stores static resources, primarily images of dishes. 
-	Amazon Bedrock: Provides AI models (LLMs) to operate the Semantic Search feature and the cooking consultation AI Chatbot. 
-	AWS CloudWatch & Budgets: Monitors system logs, tracks performance, and alerts on costs. 


### Component Design
-	Web Interface: AWS Amplify hosts the Frontend application, providing a dashboard for users to input ingredients, view recipes, and chat with the AI. 
-	Authentication and User Management: Amazon Cognito handles registration, login, and identity security, ensuring safe access to personalized features (such as saving favorite dishes and search history). 
-	Central Processing: The Backend API receives requests from the interface, coordinates the logic to look up recipes from Food.com, and calls analysis services. 
-	Data Storage: Structured data (dish information, accounts) is stored in Amazon RDS; unstructured data (dish images) is stored in Amazon S3. 
-	AI Engine: Amazon Bedrock processes natural language, analyzes input ingredients to find suitable dishes, and responds to cooking-related questions via the Chatbot. 
-	Nutritional Analysis: The backend connects to the USDA FoodData Central database to extract and calculate Calories, Protein, Fat, and Carbohydrates for each suggestion. 


### 4. Main Features
#### System implementation features: 
**1. Smart Search and Suggestion Tools (AI-Powered)**
-	Search by ingredients: Allows users to input available ingredients so the system can retrieve corresponding recipes from the database. 
-	AI recipe suggestions: Applies Semantic Search to understand context and provide accurate dish suggestions even when users input generic keywords. 
-	Dish selection wheel: A random dish selection feature that helps solve the "what to eat today" problem based on preferences or available ingredients. 
-	Nutritional analysis: Automatically calculates and displays detailed indicators (Calories, Protein, Fat, Carbohydrates) for each dish based on standard USDA data. 

**2. AI Virtual Assistant and Cooking Support Tools**
-	Cooking support Chatbot: An AI assistant (operated by Amazon Bedrock) guides step-by-step cooking and answers recipe questions in real time. 
-	Ingredient substitution suggestions: The AI assistant automatically proposes suitable alternative ingredients when users lack items, while maintaining the dish's original flavor. 
-	Portion adjustment: The system automatically recalculates the exact quantity of ingredients when users change the number of servings. 
-	Shopping list: An automatic feature that creates a grocery list from the user's selected recipes, making ingredient shopping management easy. 

**3. Personalized User Experience**
-	Registration / Login: A secure identity verification system via Amazon Cognito, helping synchronize user data. 
-	Save favorite dishes and Recipe Collection: Allows users to save their favorite dishes and create their own "Recipe Collections" based on personal themes (e.g., vegetarian, diet, weekend parties). 
-	Save search history: Tracks and stores recent searches to optimize the access experience. 
-	Personal notes: A feature allowing users to attach private notes (e.g., adjusting saltiness/sweetness) to each saved recipe. 

**4. Diary and Sharing**
-	Cooking Diary: A place for users to keep track of their cooking journey, upload actual finished product images, and rate the success of the dish. 
-	Sharing: Integrates the ability to share recipes, finished dishes, or shopping lists with friends and family via social media platforms or messaging. 

### 5. Timeline & Milestones
**Project Timeline**
- Preparation Phase:
    -	Month 1: Study and learn about AWS, and plan the project deployment. 
- Development and Deployment Phase:
    -	Month 2: Proceed with designing the AWS infrastructure (Amplify, EC2, RDS, S3), build the database from data sources (Food.com, USDA FoodData Central), and develop the basic Frontend interface. 
    -	Month 3: Integrate Amazon Bedrock, build the AI search feature (Semantic Search), the cooking support Chatbot, and complete the personalized features (Recipe Collection, Cooking Diary). Conduct system testing. 
- Post-Deployment Phase:
    -	Monitor performance, optimize AWS costs, and research expanding advanced features (such as ingredient recognition via images) over the following month. 
### 6. Budget Estimation
### Infrastructure Costs
- 	AWS Amplify: $1.00/month (Hosting React frontend, serving basic interface requests). 
-	Amazon EC2: $15.00/month (Operating 1 small-scale instance/service for the Backend API). 
-	Amazon RDS: $15.00/month (db.t3.micro/db.t4g.micro configuration to store dish and user data). 
-	Amazon Bedrock: $25.00/month (Estimated processing of AI requests for Semantic Search and Chatbot). 
-	Amazon Cognito: $0.00/month (Using AWS Free Tier supporting up to 50,000 MAUs). 
-	Amazon S3: $0.50/month (Storing dish images and static data). 
-	AWS CloudWatch & Budgets: $1.00/month (Basic system log recording and setting cost limit alerts). 
-	Data Transfer: $2.00/month (Bandwidth for data transmission between services and users). 

Total: ~$59.50/month, ~$178.50/3 months (testing/demo).
- Other Costs: $10.00 – $12.00 one-time (Optional domain name for the system, or $0 if using the default subdomain provided by AWS Amplify). 
### 7. Risk Assessment
#### Risk Matrix
-	AI cost overrun (Amazon Bedrock): High impact, medium probability. 
-	Bandwidth overload (Data Transfer): Medium impact, medium probability. 
-	External data connection error (USDA Database): High impact, low probability. 

#### Mitigation Strategies
-	AI Costs: Apply a Cache mechanism (temporary storage) for search results and common questions, and limit the number of requests. 
-	Bandwidth: Optimize image sizes before uploading to S3, and closely monitor costs via AWS Budgets. 
-	Data: Synchronize and pre-store a portion of basic nutritional data in Amazon RDS to reduce reliance on external APIs. 
#### Contingency Plans
-	Switch to traditional keyword search (Full-text search on the MySQL database) if the Bedrock AI system encounters issues or reaches the budget threshold. 
-	Use Infrastructure as Code (like AWS CloudFormation) to easily destroy all resources when not needed and quickly restore the system configuration when required for a demo. 


