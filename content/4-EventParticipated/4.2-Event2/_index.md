---
title: "Event 2"
date: 2026-07-07
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Report: “FCAJ COMMUNITY DAY - DATA DRIVEN, AI RISEN”

### Event Purpose

-   Provide a practical perspective on the AI wave shaping the future of businesses.
-   Guide how to integrate modern solutions such as Voice AI, DevOps automation, and AI in Human Resources (HR).
-   Share methods for securing internal connections when deploying AI Agent systems (via MCP protocol) on the AWS cloud.


### Speaker List

-   **Steve Tran** - CTO/Founder, CloudThinker
-   **Trung Vu** - CEO, Revve AI
-   **Nghi Danh** - AI Engineer, Renova Cloud
-   **Kiet Tran** - AI Engineer, AWS Student Builder Group
-   **Bao Phan & Nguyen Nguyen** - Cloud Engineers, Cloud Kinetics
-   **Truong Tran & Anh Dang** - AI Solution Sales, Noventiq
-   **Toan Nguyen** - AWS Security Builder


### Key Highlights

#### A Shift in Career Direction:
Companies are winding down hiring for entry-level roles and turning instead to AI or to Senior staff who are highly skilled at operating AI. For IT students, building up hands-on experience in a real enterprise environment as soon as possible has become a must rather than an option.

#### DevOps Automation & Cloud Infrastructure:
A DevOps Agent tool was introduced that helps engineers carry out Root Cause Analysis, cutting the process from hours down to just minutes by learning the system topology on its own and exporting data over secure protocols. That said, AI only makes recommendations; humans remain the final gate for actually executing decisions.

#### Voice AI Solutions for Vietnamese:
Addressing the shortage of Vietnamese data (a Low-resource language). Rather than relying on the conventional Speech-to-Speech model, the experts presented a 3-stage separated approach: Speech-to-Text -> LLM logic processing -> Text-to-Speech. This method lets businesses tightly govern what the AI says, detect gender and regional accents, and integrate Tool Calling effectively.

#### AI Applied to Human Resources (HR):
Tackling personal bias and the loss of strong candidates. Amazon Q is connected directly to workplace platforms to scan CVs in bulk, generate JDs automatically, compare candidate capabilities, and report expected salaries without being capped by token processing limits.

#### Securing Private MCP Connections:
So that Amazon Q can reach internal databases without pushing data onto the Public Internet, the proposed design places the MCP Server inside the VPC's Private Subnet, routes traffic through VPC Endpoints, and encrypts certificates with AWS Certificate Manager (ACM), guaranteeing full security compliance.


### Lessons & Practical Experience

#### Practical Experience
-   Getting to see remarkably impressive live demos up close, from a Voice Agent handling MacBook inquiries through a switchboard, to a DevOps AI pinpointing DDoS-triggered system errors within moments, and Amazon Q analyzing CVs and scoring candidates in real time.

#### Key Takeaways
-   AI itself doesn't take people's jobs, but those who know how to work with AI will replace those who don't.
-   When building AI Agents for enterprises, security comes first. Data Pipelines have to be set up to encryption standards, particularly when the interaction runs over an internal Model Context Protocol (MCP).
-   For complex systems, it's important to adopt Multi-Agent architectures rather than Single Agents in order to cut down "hallucinations", optimize Context Windows, and make Role-Based Access Control (RBAC) straightforward to implement.


### Work Application

-   **Upgrade the Backend for the AI Recipe Finder:** Apply the VPC Connection and Private MCP Server patterns shared at the event so that API call requests from the recipe recommendation system to the Database (which stores customer allergy/preference data) stay fully private and never leak onto the Internet.
-   **Automated Testing and Operations:** Trial DevOps AI Agent toolkits that automatically analyze logs whenever the application reports an error (for example, a dish image failing to load), thereby shortening incident response time.
-   **Voice AI Application (Additional Idea):** Down the line, integrate a Voice AI model (Speech -> Text -> LLM finding the recipe -> Text -> Speech) into the AI Recipe Finder, so users can cook while asking and answering recipe questions with a virtual assistant entirely in Vietnamese, without ever touching the screen.


#### Event Photos

![Event 2](/images/4-Event/3.jpg)