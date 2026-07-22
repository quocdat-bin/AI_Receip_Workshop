---
title: "Blog 1"
date: 2026-07-07
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# AMAZON RDS LOG ANALYSIS USING NATURAL LANGUAGE WITH KIRO AND MCP

This blog post introduces a solution that integrates the Kiro AI assistant and the MCP protocol into the Amazon RDS log analysis workflow, allowing you to query the system using natural language instead of writing complex command lines. This is a breakthrough approach that automates the troubleshooting process, reduces the burden on operations teams, and optimizes database security monitoring activities on the Cloud.

**Key points to know:**

-	Solving the "log reading nightmare": Amazon RDS generates a massive volume of logs (Error Logs, Audit Logs, Slow Query Logs, etc.). Manually tracing issues often requires skills in writing complex queries (like in CloudWatch Logs Insights) and is extremely time-consuming in emergency situations. 
-	How it works: The solution combines the log storage repository, Amazon CloudWatch, with the open-source MCP protocol (cloudwatch-mcp-server) and the Kiro AI assistant to transform dry data analysis into a conversation. 
-	Natural language queries: You do not need to memorize system syntax. Just issue a command (for example: "Filter out failed login attempts from the Audit Log in the last 2 hours"), and the system will automatically call the API, analyze it, and return the root cause in easy-to-understand text. 
-	Optimizing investigation time: Applying AI automates information extraction, significantly shortening incident response time and minimizing errors compared to humans having to read thousands of log lines manually. 
-	Practical application: It demonstrates the flexible and effective combination of available Cloud services with open-source protocols to create a much smarter and smoother system management workflow. 


This feature is especially useful when you have many applications running on the same IAM role but need different permission restrictions (for example: one pod only reads a specific S3 bucket, another pod only calls certain APIs).

**Image:**
![Blog1](/images/3-Blog/Blog1.png)

**Guide to Installing Kiro and CloudWatch MCP Server for RDS Log Analysis:**

To materialize the solution of turning logs into conversations, you need to establish a connection between Amazon RDS, CloudWatch, and the AI assistant via MCP (Model Context Protocol). Below are the basic implementation steps: 

**Prerequisites:**
-	AWS CLI is installed on your machine and an IAM User/Role is configured with Read-only access to Amazon CloudWatch Logs. 
-	Node.js (latest version) or Python is installed, depending on the environment you use to run Kiro. 
-	The Amazon RDS system has the log export feature enabled (Export logs) to Amazon CloudWatch. 

**Step 1: Enable Log exports from RDS to CloudWatch**
1.	Access the Amazon RDS Console and select the database instance you want to monitor. 
2.	Click on Modify. 
3.	Scroll down to the Log exports section and check the types of logs you need to analyze (e.g., Error log, Audit log, Slow query log). 
4.	Save the changes and wait for the instance to update. After this step, logs will begin to be pushed to CloudWatch Log Groups. 

**Step 2: Install the Kiro Assistant**

You can install the Kiro CLI via a package manager. 
Open the terminal and run the following command: 
-	npm install -g kiro-cli

After installation, ensure Kiro has recognized your configuration environment variables. 

**Step 3: Integrate the AWS CloudWatch MCP Server**

MCP (Model Context Protocol) is the bridge that helps AI understand the context from CloudWatch. 

1. Download and install the MCP server for CloudWatch from the AWS Labs open-source repository:

-	git clone https://github.com/awslabs/cloudwatch-mcp-server.git
-	cd cloudwatch-mcp-server
-	npm install

2. Launch the MCP server and configure the connection with Kiro. You will need to provide the regional domain (AWS Region) and AWS credentials so that MCP can call the GetLogEvents and StartQuery APIs from CloudWatch. 

**Step 4: Experience Natural Language Querying**

Once the Kiro -> MCP Server -> CloudWatch Logs connection flow is ready, you can open the Kiro command-line interface and start interacting. 
Try entering commands (prompts) like: 
-	"Check the Audit Log of the 'db-production' RDS instance and list the incorrect password login attempts in the last 24 hours." 

-	"Analyze the Slow Query Log from 10:00 AM to 11:00 AM today and point out which SQL query is causing the bottleneck." 
Kiro will automatically translate these requests into CloudWatch Logs Insights syntax, scan the data, and return a complete text report on the Root Cause Analysis. 


**Link Blog:**  https://www.facebook.com/share/p/1EALQQhdQz/
