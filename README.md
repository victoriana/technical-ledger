# Technical Ledger

An active engineering ledger centralizing my professional background, core technical competencies, and enterprise impact.

I am a Senior Systems & Infrastructure Engineer with a focus on shipping reliable, scalable systems. I write the code myself, manage the deployment pipelines, and own what runs in production. My work spans full-stack development, cloud infrastructure, and modern AI applications, always with an emphasis on stability, security, and measurable impact. I hold an M.S. in Computer Science from the University of Georgia and an electrical engineering degree from Georgia Tech, with a Certificate in Computer Engineering. Alongside my infrastructure and AI work, I'm applying that hardware-meets-software background to a self-directed Rust and edge-computing project.

**Currently:** Open to W2 roles (contract or direct hire). Building out `lit-flow-flash` into a measurable, self-improving agent loop, with acro-spin as a longer-running background project.

**Featured Work:**
- **[NYL Agentic POC](./NYL_POC_SUMMARY.md):** LangGraph + Claude (Bedrock) workflow over Databricks data.
- **[Lit Flow Flash](https://github.com/victoriana/lit-flow-flash):** An AI writing trainer (read → write → AI critique) with a public test harness. In progress: evolving it into an agentic loop (generate → score → critique → revise) with an evaluation set, baseline comparison, and guardrails.
- **[Acro-spin](https://github.com/victoriana/acro-spin):** A self-directed design and build plan for an agentic coach driven by IMU movement data, developed simulation first: synthetic data, kinematic metrics, and a LangGraph agent loop are validated in software before any hardware is used. Active as a background project with no fixed timeline. Supporting scaffolding: [ros2-rust-workspace](https://github.com/victoriana/ros2-rust-workspace) (Rust + ROS 2 in a devcontainer). See the [project plan](./ACRO-SPIN-PROJECT-PLAN.md).

Below is a **One-Page Resume** summarizing my career history and system architecture credentials.
```
RAVI BASAWA, US citizen
Senior Systems & Infrastructure Engineer | Enterprise DevOps & Data Pipelines
Atlanta, GA (open to hybrid or remote) · rbasawa1@gmail.com · 678-851-2365
github.com/victoriana · linkedin.com/in/ravibasawa

PROFESSIONAL SUMMARY
Senior engineer with deep Linux, AWS cloud, Java/Python, CI/CD, and Databricks/data-pipeline experience at enterprise scale, now focused on production LLMOps and agentic AI on Bedrock. I write the code, deploy via CI/CD, and own production.

CORE SKILLS
LLMOps/GenAI: AWS Bedrock (Claude), agentic AI, LangChain/LangGraph, RAG, prompt engineering, NL-to-SQL, model governance & evaluation
MLOps: SageMaker (Pipelines, Endpoints, Model Registry), model life-cycle management, CI/CD for ML, endpoint monitoring (CloudWatch)
Cloud/IaC: AWS: Lambda, Step Functions, API Gateway, S3, EC2, EKS, Redshift, CloudWatch, IAM/PKI; Docker; Terraform
Data Platforms: Databricks (lakehouse/notebook workflows), Redshift, Oracle, Cassandra (NoSQL), Tableau, MuleSoft
CI/CD/Automation: Jenkins, GitHub, Conda; automated QA/performance & stress harnesses for 5+ concurrent teams
Languages: Python (advanced), Java (Spring Framework), C/C++, SQL, Bash

RECENT & RELEVANT EXPERIENCE
NEW YORK LIFE INSURANCE — Senior Associate, Tech Data AI Ventures (TDAV) Atlanta, GA  — 2014 – 2026
LLMOps & Agentic AI (AWS Bedrock / SageMaker)
• Pioneered enterprise LLMOps on AWS SageMaker and Bedrock; built agentic Claude applications with LangChain/LangGraph orchestration, version governance, and evaluation gates for production release.
• Delivered NL-to-SQL assistant over Redshift correspondence warehouse (multi-million records), enabling self-service AI-driven business reporting.
• Established model/artifact lifecycle practices — registry, versioning, endpoint deployment, and CloudWatch-based monitoring — aligned with enterprise security.
AWS Cloud Migration & Serverless Architecture
• Migrated Java pre/post-processing pipelines from on-prem Linux to AWS via Lambda, Step Functions, API Gateway, and S3 event triggers — zero production incidents across all migration waves.
• Replaced legacy EDM Hadoop/Tableau reporting with Amazon Redshift, creating a scalable cloud data foundation for AI agent integration.
Platform, API & ETL Engineering
• Built production RESTful/SOAP APIs in Java/Spring and Python microservices; developed ETL pipelines for mailing intake and e-delivery.
• Authored Python automation adopted into active BAU cloud delivery; maintained containerized (Docker/EKS) workloads supporting multiple concurrent releases.
CI/CD, Testing & Technical Leadership
• Designed Jenkins-based CI/CD plus performance/stress-testing frameworks serving 5+ concurrent teams; pair-programmed with and upskilled engineers and contractors on automation, Git workflows, and AWS.
• Remediated critical security vulnerabilities including FTP/SFTP exposure; enforced PKI-based access across pipelines; led credential-management remediation under TDLC governance.

EARLIER CAREER — AT&T, AirSage, Accenture | Big Data, Cloud & Enterprise Software
Technical Architect and R&D/Product roles across telecom, retail, and consulting: architected a multi-tenant Apache Cassandra NoSQL platform at carrier scale for AT&T; built "FastCache," an in-memory geo-fencing engine delivering sub-second spatial queries at AirSage; led U.S. C/C++/UNIX capability and a predictive-analytics center for enterprise clients at Accenture. Full details available on request.

EDUCATION
M.S., Computer Science — University of Georgia
B.E., Electrical Engineering — Georgia Institute of Technology
```
