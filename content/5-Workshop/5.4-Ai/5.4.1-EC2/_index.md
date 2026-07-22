---
title : "Prepare the AI Server (EC2) & Network Infrastructure (VPC, Security Group, Elastic IP)"
date : 2026-07-07
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---
### **Step 1: Launch the EC2 Server (AI Server)**
1.	Open the AWS Management Console, go to the EC2 Dashboard, and choose Launch Instance.
2.	Name the instance embed (dedicated to Vector encoding and AI Services tasks).
3.	Under Quick Start, select the Ubuntu Server 24.04 LTS (HVM) operating system, 64-bit (x86) architecture.
4.	Under Configure storage, set the Root Volume to 30 GiB gp3 (3000 IOPS) to provide enough space for the AI model and the Vector database.

![overview](/images/5-Workshop/5.4-Ai/1.jpg)
![overview](/images/5-Workshop/5.4-Ai/2.jpg)
![overview](/images/5-Workshop/5.4-Ai/3.jpg)

### **Step 2: Configure the Security Group Firewall**
-	Create or customize the Security Group attached to the embed instance with the following Inbound Rules:
    -	Port 22 (SSH): Open the SSH admin connection from your personal IP.
    -	Port 8001 (Custom TCP): Open the port for the FastAPI /search service (Semantic Search).
    -	Port 8002 (Custom TCP): Open the port for the FastAPI /chat service (AI Chatbot).

![overview](/images/5-Workshop/5.4-Ai/4.jpg)
![overview](/images/5-Workshop/5.4-Ai/5.jpg)

### **Step 3: Allocate & Associate an Elastic IP (Static IP)**
1.	Open the AWS VPC Dashboard → Elastic IP addresses → click Allocate Elastic IP address.
2.	Choose the Network border group ap-southeast-1 (Singapore) and click Allocate.
3.	Select the newly allocated IP address (for example: 54.254.77.58), click Associate Elastic IP address, and link it to the EC2 instance embed (i-018c3a28a2d9e3e5c).

![overview](/images/5-Workshop/5.4-Ai/6.jpg)
![overview](/images/5-Workshop/5.4-Ai/7.jpg)

### **Step 4: SSH Admin Connection & Checking the Source Code on the Server**
-	Open a Terminal/PowerShell on your local machine and connect via SSH using the Private Key:
![overview](/images/5-Workshop/5.4-Ai/8.jpg)

- Check the project directory with the command ls -la
![overview](/images/5-Workshop/5.4-Ai/9.jpg)
- The files have already been added in advance

### **Step 5: Infrastructure Cost Optimization Scenario (Instance Resizing Strategy)**
1.	Batch Embedding phase (heavy load): Use a high-spec instance (Compute-Optimized) to quickly compute vectors for hundreds of thousands of recipes.
2.	Production phase (light load): After finishing loading the vector data:
-	In the AWS Console: Select the embed instance → Instance state → Stop instance.
-	Click Actions → Instance settings → Change instance type → switch to t3.medium (2 vCPU, 4GB RAM).
-	Click Start instance to bring the server back online at an optimized cost.

![overview](/images/5-Workshop/5.4-Ai/10.jpg)
![overview](/images/5-Workshop/5.4-Ai/11.jpg)
![overview](/images/5-Workshop/5.4-Ai/12.jpg)
