# Software Design & Architecture (SOFE3650) - Final Project
**CRN 43510 - Group 9**  
**Tuesday, October 28, 2025**  
**AI Powered Digital Assistant Platform (AIDAP) for a conversational User Interface(UI) for University/College/School System Services**  
---  
**SOFE3650F25 Project Repository**   
---

AIDAP is a conversational assistant for students, lecturers, and administrators. It answers common academic questions, sends reminders, and integrates with university systems (LMS, registration, calendars, email). The assistant uses AI for intent understanding and for generating grounded responses.   
Stakeholders: Students, Lecturers, Administrators, System Maintainer, Data Source Systems.  

AIDAP provides conversational access to institutional data and services (R1), stores interaction history to personalize answers (R2), integrates live university systems (R3), supports text and voice (R4), runs cloud-native and scalable (R7), and protects user data under institutional policy (R8).

---

### Stakeholder Requirements

**Student (RS1 - RS14):** Ask any administrative and academic questions at any time, site preference change availablility, site accessibility on any platform, student diversity, etc.

**Lecturer (RL1 - RL8):** Course Material change/updates, student academic info viewabiity, course access/usage and data manageability, etc. 

**Admin (RA1 - RA7):** various system management authorities, monitoring ability throughout system, usage up to 5000 current users, legal/ethical abidance, etc.

**Maintainer (RM1 - RM7):** Support, log/record usage, monitor system, check for faults/errors, update/repair ability, etc. 

**Data Sources (RD1 - RD4):** data synchronization, API interoperability, retry/recovery for faults/errors, and data integrity and consistency.

---

### Deliverables and Due Dates

**Deliverable 1** — Requirements Analysis: Use case models, quality attributes, system constraints, architectural concerns, business case. Due *Oct 28*. 

**Deliverable 2** — ADD Iteration 1&2: Reference architecture, components, interfaces, deployment, domain-specific models. Due *Nov 16*. 

**Deliverable 3** — Design of a Use Case & ATAM: Include all 3 ADD iterations and an ATAM scenario analysis table. Due *Dec 6*. 

ATAM Evaluation – AIDAP: AI-Powered Digital Assistant Platform
1. Utility Tree
Top-Level Categories

Performance

Availability

Security/Privacy

Usability

Modifiability

Detailed Utility Tree
Quality Attribute	Scenario	Stimulus	Response	Priority (H/M/L)	Difficulty
Performance	P1	5000 users query simultaneously during course registration week	System responds within 2 seconds	H	M
	P2	User repeats a common FAQ	Cached result returned instantly	M	L
Availability	A1	LMS API goes down	Connector retries + circuit breaker; assistant still answers non-LMS queries	H	M
	A2	AIDAP container crashes	Auto-restart via orchestration	H	L
Security/Privacy	S1	Student asks academic history	Only authorized users access data using SSO tokens	H	M
	S2	Logs generated	Sensitive info redacted	M	L
Usability	U1	User speaks instead of typing	Voice input processed with real-time transcription	H	M
	U2	User expects type-ahead response	Streaming begins < 500ms	H	M
Modifiability	M1	New intent type introduced	Plug-in model registry accepts new model without full redeploy	M	M
	M2	Update connectors	Each connector is isolated microservice	M	L
2. ATAM Risk Assessment
Risk Summary Table
ID	Risk	Type	Explanation	Impact	Likelihood	Mitigation
R1	LMS downtime affects dependent queries	Sensitivity	The system heavily relies on LMS API	High	Medium	Circuit breakers + cached schedules
R2	NLU model latency spikes during peak load	Sensitivity	Deep models = compute heavy	High	Medium	Model caching + autoscaling
R3	Privacy risk: context manager storing sensitive history	Risk	Incorrect retention policy would violate compliance	High	Low	Short TTL, encryption, audit logs
R4	Integration connectors tightly coupled to external schema changes	Tradeoff	Data sources outside AIDAP control	Medium	Medium	Versioned API adapters
R5	Streaming UI overload at 5000 concurrent connections	Sensitivity	WebSockets require persistent connections	Medium	Medium	Horizontal scaling + load-balanced gateway
R6	Model updates breaking response format	Non-risk	Model registry isolates model behavior	Low	Low	Validation tests before rollout
R7	Queue backlog (for reminders) during high event load	Risk	Too many scheduled reminders → delayed delivery	Medium	Medium	Add consumer autoscaling
3. ATAM Summary

Primary Drivers: performance, availability, security, privacy

Architecture Strengths: microservices separation, connector isolation, model registry, conversation streaming

Key Sensitivity Points: NLU latency, dependence on LMS uptime, streaming concurrency

Key Tradeoffs: real-time responses vs compute cost; strong privacy constraints vs personalization richness

Final Conclusion: Architecture is robust for deployment, with clear mitigation strategies and supports the stakeholder needs.