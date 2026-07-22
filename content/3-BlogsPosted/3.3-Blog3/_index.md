---
title: "Blog 3"
date: 2026-07-7
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---


# PRIORITIZING AWS HEALTH ALERTS WITH AWS USER NOTIFICATIONS

This blog post introduces a streamlined solution to overcome "notification fatigue" for system engineers. By utilizing **AWS User Notifications** combined with available AWS CloudFormation templates, you can automate the filtering and classification of events from **AWS Health**. Instead of passively receiving every alert, your inbox will now prioritize critical incidents that require immediate attention, while less important maintenance information will be neatly batched.

**Key points to know:**

-	**Proactive Filtering:** The system uses Event rules to eliminate alerts from services you do not use. Only events belonging to your core architecture (such as Amazon RDS, Direct Connect, etc.) are allowed through. 
-	**Urgency-Based Routing:** The solution automatically divides notifications into two streams: 
    -	CRITICAL: Captures error events or scheduled changes. Notifications are sent immediately. 
    -	INFORMATIONAL: Catches standard events. 
-	**Built-in Batching:** This is the most valuable feature of AWS User Notifications. Instead of spamming individual emails, the system batches all INFORMATIONAL alerts over a specific time period (e.g., 5 minutes) into a single summary email, keeping the inbox clean. 
-	**Eliminates Code Maintenance Burden:** There is no need to write or maintain complex AWS Lambda functions to parse JSON content like in the old methods. Everything is handled directly at the service level (Managed Service). 
-	**Centralized & Multi-Account Management:** The entire configuration is monitored on a single Notification Center. The solution also supports easy scaling for dozens of member accounts through integration with AWS Organizations. 


**Image:**
![Blog3](/images/3-Blog/Blog3.png)

**Automated Deployment Guide via AWS CloudFormation:**

The principle of this solution is "Filter first, prioritize later". Installation is very fast using pre-configured CloudFormation templates. 

**Prerequisites:**
-	Permissions to deploy a CloudFormation stack and configure AWS User Notifications on the account. 
-	If deploying for the entire organization, access to the Management (Payer) account is required, along with the ID of the Organization root or Organizational Unit (OU). 

**Step 1: Choose deployment modes**

Download the **[CloudFormation template](https://github.com/aws-samples/sample-prioritize-aws-health-alerts-using-user-notifications)** from the original article and upload it to the AWS Console. You need to select 1 of the 4 DeploymentMode parameters suitable for your scale: 
-	Linked mode: The standard version, for a single account, using the default AWS email format. 
-	Payer mode: For the Enterprise level. Install once in the management account, applying alert filtering rules to all member accounts within AWS Organizations. 
-	Combined mode: For 1 account but combines EventBridge and Amazon SNS to automatically insert a [CRITICAL] or [INFORMATIONAL] tag into the email subject, helping to identify incidents faster. 
-	PayerCombined mode: Combines the scaling power of Payer and the email subject customization capabilities of Combined for the entire organization. 

**Step 2: Confirm email subscription**

-	After CloudFormation finishes running (CREATE_COMPLETE status), check the email inbox you provided. 
-	You must click the **Confirm subscription** link in the email. If you skip this step, AWS will not be able to push alerts to you. 

**Step 3: Verify the notification configurations**

-	Access the AWS User Notifications service on the console. 
-	Check the 2 newly created configurations: Health-Critical-Notifications (handles immediate errors) and Health-Informational-Notifications (batches secondary notifications). 
-	Review the list of monitored AWS services to ensure no critical services are missed (such as DynamoDB, ECS, etc.). 

**Step 4: Test the solution**

-	Go to **Amazon EventBridge** > Select **Send events**. 
-	Create a mock event with the source set to aws.health. 
-	Check your inbox: The Critical stream will pop up a notification immediately (with a red tag if using Option C/D), while the Informational stream will wait a full 5 minutes before compiling and sending. 

*Things to consider:*

Although this notification management solution is extremely effective, it is intentionally designed to be lightweight. Along with that simplicity comes a few trade-offs that everyone needs to understand before deployment: 

-	Email Format: By default, you cannot insert custom text (like [CRITICAL]) into the subject. The priority signal relies on the delivery method (standalone vs. batched). If this priority tag is mandatory, use the Combined mode (integrating EventBridge + SNS). 
-	Delivery Monitoring: The EventBridge + SNS stream (in the Combined option) has a built-in dead-letter queue (DLQ) and CloudWatch alarm, letting you know immediately if the notification delivery system itself fails. 
-	No Deduplication: Each event lifecycle (created, updated, resolved) will generate a notification. A single incident can generate 2–4 emails. You need to use AHA or a Lambda function if you want absolute deduplication. 
-	No Escalation Support: The system only "delivers mail" and does not track who has handled it. For on-call workflows, you should integrate the SNS stream directly into PagerDuty or OpsGenie. 
-	No Historical Storage: Notifications are pushed in real-time and then pass. To hold a post-incident review or create reports, you will need to pair this with the HEIDI solution or the CID Health Events Dashboard.


**[Link blog:](https://www.facebook.com/share/p/1EVZSYjUsa/)** https://www.facebook.com/share/p/1EVZSYjUsa/