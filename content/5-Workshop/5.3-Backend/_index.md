---
title : "INITIALIZING THE DATABASE, DEPLOYING THE BACKEND, AND SETTING UP AMAZON COGNITO"
date : 2026-07-07 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---
#### Overview 
This phase focuses on building the storage, logic-processing, and security infrastructure for the application. The deployment process includes initializing the database on Amazon RDS, setting up the backend server on Amazon EC2, and configuring user management with Amazon Cognito. 
In this deployment step, we will implement three main modules to connect and operate the system:
- Initialize the Database (Amazon RDS) - Create an RDS MySQL instance placed in a Private Subnet. The system runs scripts to initialize the data tables (such as Users, Recipes, Collections) and imports the processed recipe and nutrition data into the database. 
- Deploy the Recipe Backend (EC2) - Launch an EC2 instance (Ubuntu Server) and configure a Python virtual environment to deploy the FastAPI application. This backend provides the Recipe APIs, connects securely to the RDS database, and is configured to run through Nginx (Reverse Proxy) and systemd to ensure continuous operation. 
- Set Up Amazon Cognito - Create a User Pool to establish a secure login and registration flow for users. The system uses JWT tokens to authenticate API requests and integrates with the backend through the /auth/me endpoint to sync users' identity information into the users table on Amazon RDS.  

#### Contents

- [Preparing the Database](5.3.1-Database/)
- [Creating and Configuring Amazon RDS MySQL](5.3.2-RDS/)
- [Building the Backend API with FastAPI](5.3.3-API/)
- [Deploying the FastAPI Backend to Amazon EC2](5.3.4-EC2/)
- [Configuring Amazon Cognito, User Authentication, and API Security](5.3.5-Cognito/)
