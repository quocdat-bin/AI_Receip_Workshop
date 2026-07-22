---
title: "Event 1"
date: 2026-07-07
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---


# Report: “AWS FIRST CLOUD AI JOURNEY COMMUNITY DAY”

### Event Purpose

-   Update practical trends in Cloud Computing and methods for applying Generative AI (GenAI) within enterprise environments.
-   Share best practices for platform software architecture design, building multi-agent systems, and network infrastructure security.
-   Create a networking space for the IT community, inspiring and guiding the skill sets required for engineers in the AI era.


### Speaker List

-   **Nguyễn Gia Hưng** - Solutions Architect, AWS Vietnam (Founder FCAJ)
-   **Tinh Truong** - Platform Engineer, GoTymeX
-   **Anh Pham** - Cloud Consultant, G-AsiaPacific Vietnam
-   **Thinh Nguyen** - DevOps Engineer, FCAJ
-   **Uyển Lê, Thảo Nguyễn, Mai Nguyễn** - GenAI Engineers, VIB
-   **Duc Dao** - Solutions Architect, Cloud Kinetics
-   **Vy Lâm** - Senior Business Systems Analyst, VPBank


### Key Highlights

#### How the IT Job Market Is Shifting & the Mindset Engineers Now Need:
With AI steadily lowering the cost of shipping software, demand for brand-new products is climbing fast. In that reality, knowing the theory is no longer enough to prove anything; what counts is whether an engineer genuinely understands the business problem and can show up with a product that actually runs as evidence of their ability.

#### Giving AI the Context It Actually Needs:
A lot of hallucination traces back to overloading the model with information. Instead of pouring generic, internet-scraped documents into it (a pattern the speakers jokingly called the "Internet Builder" trap), engineers land much cleaner responses by tightening the scope and feeding in only the context that genuinely matters.

#### Day-to-Day Automation with Amazon Q (AI Agent):
A grounded demonstration of how a virtual assistant can process Excel files, build BI dashboards, and summarize meetings automatically, with everything wired together across office tools via MCP (Model Context Protocol).

#### Network Design & Security with Amazon CloudFront:
The Flat-rate pricing model was framed as protection against "bill shock", those sudden cost jumps that arrive after a traffic anomaly or a DDoS attack. By distributing load across AWS Point of Presence (PoP) edge locations and combining it with VPC Origin, CloudFront can connect directly into a private subnet and keep the backend entirely separated from the public internet.

#### Frontline Hackathon Lessons & Shipping a GenAI Product (UTM Morpo):
The team retold their 36-hour push to build a UI-generation tool on a Serverless foundation coordinated by three AI Agents working together. Their key breakthrough: rather than spending time and tokens regenerating the entire interface each round, they let users adjust individual components and CSS right on the screen. What came out of it was a set of costly risk lessons, keep a close eye on the token budget, steer clear of redundant code, and stay firmly focused on core features instead of trying to stuff in every idea.

#### Grappling with LLM Determinism:
A thorough breakdown of how LLMs really operate, including the awkward reality that outputs can still shift even with Temperature = 0, because of GPU hardware limits and provider-side inference optimizations. The recommended safeguards: run the same prompt multiple times and vote on the outcome, self-host the model, force JSON mode, and, most importantly, build downstream services flexible enough to absorb the occasional random misstep from the AI.

#### Building Multi-Agent Systems at Enterprise Scale:
A case study on coordinating multiple specialized AI agents to evaluate startup credit. Security and compliance sat at the heart of the design, tightening MCP attack surfaces, preventing Prompt Injection, maintaining audit trails, and rotating API keys on a set schedule.


### What Was Learned

- **Begin With the Business, Not the Technology:** 
The solutions that create the most value start from a genuine business problem and a genuine user experience. Whether that involves breaking a system into AI Agents or fully resolving a single core pain point (such as the on-screen UI editor instead of constant regeneration), going deep outperforms going wide.

- **Architecture & Information Security:** 
I came away with concrete methods for setting up secure gatekeeping using CloudFront, and I now view VPC Origin as a real step forward for isolating backend architecture from internet-facing threats.

- **Managing AI Risk:** 
Recognizing that LLMs are probabilistic stops you from placing blind faith in what they return. It's worth preparing in advance for the cases where AI hallucinates, reaches its token limit, or replies in a broken format, and having a fallback ready for each situation.


### Work Application

-   Multi-Agent design: break the system into dedicated agents (preference analysis, recipe lookup, nutrition calculation) so personalized recommendations arrive more accurately. 
-   Streamlined generation flow: when a user requests a change, modify only the affected component rather than regenerating the entire recipe, which saves a meaningful volume of tokens. 
-   Deterministic output: hold temperature low and enforce JSON mode so the returned data (dish names, ingredients, steps) always conforms to a fixed schema. 
-   Security & context: shield the database behind CloudFront and VPC Origin, and feed only the minimum context (just the ingredients the user currently has on hand) into each prompt to close off Prompt Injection. 


### Event Experience

Taking part in the **“AWS FIRST CLOUD AI JOURNEY COMMUNITY DAY”** proved to be truly rewarding; it gave me a well-rounded understanding of how AWS Cloud services and Generative AI can work together to build applications that are both secure and reliable. A handful of moments really stayed with me:

#### Learning From Experienced Speakers

-   Practitioners and engineers from AWS, VIB, GoTymeX, and Cloud Kinetics openly shared the kind of hands-on, real-world lessons that rarely appear in any documentation.
-   The concrete case studies, especially the 36-hour hackathon and the startup credit-scoring system, made it clear to me why Multi-Agent architectures handle tangled business workflows so much better than relying on a single model.


#### Hands-On Technical Insight

-   The session on LLM internals finally clarified why AI hallucinates, and how Temperature and JSON mode influence how consistent the output turns out to be.
-   Watching exactly how Amazon CloudFront and VPC Origin isolate a critical backend made network security feel concrete, protecting a system from both DDoS surges and runaway "bill shock" costs.
-   I also came away with a solid understanding of MCP (Model Context Protocol) and the security concerns that come with it, Prompt Injection and MCP Attack Vectors, once you allow AI to reach into your systems.

#### Key Takeaways
-   Breaking an AI pipeline into specialized agents reduces errors, scales more smoothly, and makes debugging considerably easier.
-   Never take AI output at face value. Add controls such as majority voting across several runs, and always design for the case where the model returns something broken.
-   Security stops being optional the moment GenAI reaches production. Walling off infrastructure with CloudFront/VPC Origin and rotating API keys are baseline compliance requirements, not extras.

#### Event Photos
* Add your event photos here

![Event 1](/images/4-Event/Event1_1.jpg)
![Event 1](/images/4-Event/Event1.jpg)