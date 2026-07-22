---
title: "Workshop"
date: 2026-07-07
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


# THE AI RECIPE FINDER PROJECT ON THE AWS PLATFORM

#### Overview

**AI Recipe Finder** is an intelligent web platform built entirely on AWS cloud infrastructure. The project directly addresses the problem of food waste and the difficulty of planning daily meals by analyzing the ingredients a user already has, then using AI to provide accurate dish suggestions and comprehensive support throughout the cooking process.

Today, users often spend a lot of time thinking up menus, easily waste leftover food in the kitchen, struggle to calculate nutrition manually, and lack smart interactive tools while cooking.

To solve this, the platform applies Generative AI combined with Cloud Computing to create a closed-loop ecosystem: from recipe suggestions via Semantic Search, to chatting with a virtual assistant (Chatbot) for cooking guidance, to automatically analyzing detailed nutrition (Calories, Protein, Fat, Carbs) based on standard USDA data. The system offers standout features powered by specific AWS services:

-	Smart Search & Suggestions - Applies AI models from Amazon Bedrock to analyze the input ingredients and provide suitable recipes through Semantic Search, integrating a "dish-picking wheel" to quickly answer the question "what to eat today".
-	AI Cooking Assistant - Runs a Chatbot through Amazon Bedrock to answer questions in real time and automatically suggest substitute ingredients when you run short.
-	Nutrition & Shopping Management - Uses a Backend API (running on EC2/App Runner) to automatically recalculate quantities when the number of servings changes, display detailed nutrients from the USDA, and automatically generate a shopping list.
-	Personalized Space - Secured and stored by Amazon Cognito, AWS Amplify, and Amazon S3/RDS, supporting saving favorite recipe collections, keeping a "Cooking Diary" to store photos of finished dishes, and sharing them with friends.


#### Contents

1. [Introduction](5.1-Workshop-overview/)
2. [Deploying the Core Infrastructure & Backend](5.3-Backend/)
3. [Deploying the AI Chatbox & Artificial Intelligence (AI Services)](5.4-Ai/)
4. [Deploying the Frontend & Authentication (Frontend & Auth)](5.5-Fronend//)
5. [Cleaning Up Resources](5.6-Cleanup/)
6. [Project Documentation](5.7-ProjectDocument/)
