ADD Iteration 3 — AIDAP (AI-Powered Digital Assistant Platform)

SOFE3650 — Group 9

1. Architectural Drivers (Recap)

Business Goals:

Provide a reliable conversational assistant across university systems.

Reduce load on administrative staff by automating common queries.

Support 5000+ concurrent users.

Primary Quality Attributes:

Performance (Q1): Low-latency responses < 2 seconds.

Availability (Q2): 99.9% uptime for student/lecturer/admin usage.

Security & Privacy (Q3): Compliance with institutional policies, FERPA/PIPEDA-aligned data handling.

Usability (Q4): Conversational interface supporting text and voice interactions.

Modifiability (Q5): Ability to update intents, connectors, or data sources without service downtime.

Constraints:

Must integrate with LMS, Registration Systems, Calendar, Email via their APIs.

Cloud-native deployment.

Identity & authentication via university SSO.

Data must remain within institutional jurisdiction.

2. Selecting Module Decomposition (Iteration 3 Focus)

Iteration 1 & 2 defined the macro-architecture and the major subsystems.
Iteration 3 focuses on refining deeper internal modules based on key quality attributes.

Modules Refined This Iteration
Module	Why Refined?
NLU Pipeline	Performance + modifiability for model swaps
Context Manager	Personalization (history store), privacy
Integration Connectors	Reliability + recoverability
Conversation Experience Layer	Usability + availability
Monitoring & Logging Subsystem	Availability + maintainability

These were selected because they directly support the highest-priority scenarios in the ATAM utility tree.

3. Architectural Refinements & Tactics Applied
3.1 NLU Pipeline (Intent Classification + Entity Extraction)

Quality Attributes: Performance, Modifiability

Design Decisions:

Use a micro-pipeline composed of:
Preprocessor → Intent Classifier → Entity Extractor → Response Selector

Each stage runs in its own container for independent scaling.

Tactics Applied:

“Increase Computational Efficiency” → Caching results for repeated FAQ-type questions.

“Introduce Concurrency” → Use async inference for transformer-based NLU models.

“Anticipate Expected Changes” → Plug-in model registry allowing quick updates of intent models.

3.2 Context Manager (Session Memory + Personalization)

Quality Attributes: Security, Privacy, Modifiability

Design Decisions:

Sensitive context stored ephemerally with short TTLs.

Long-term preferences stored encrypted in a separate user-profile microservice.

Tactics Applied:

“Authenticate Users” & “Authorize Users” → integrated SSO tokens propagated through endpoints.

“Limit Exposure” → redact sensitive tokens before logs.

“Abstract Interfaces” → personalization APIs are versioned to allow updates without refactoring the assistant.

3.3 Integration Connectors (LMS, Registration, Calendar, Email)

Quality Attributes: Availability, Performance, Reliability

Design Decisions:

Each connector is separately deployed with retry logic and circuit breaker patterns.

Use a message queue (Kafka/RabbitMQ) for async tasks like reminders.

Tactics Applied:

“Detect Faults” → heartbeat checks with university API endpoints.

“Recover from Faults” → exponential backoff retry for LMS or registration outages.

“Limit Dependencies” → connectors isolated; failures do NOT break the entire assistant.

3.4 Conversation Experience Layer (Text + Voice UI)

Quality Attributes: Usability, Performance

Design Decisions:

Unified API response format supports text and optional voice synthesis.

UI clients (web/mobile/chatbot widget) consume WebSocket streams for real-time typing indicators + streaming responses.

Tactics Applied:

“Provide Meaningful Feedback” → Streaming responses start within 300–500ms.

“Maintain Responsiveness” → Cancel/interrupt conversation mid-response.

3.5 Monitoring, Logging, and Telemetry Subsystem

Quality Attributes: Availability, Maintainability

Design Decisions:

Distributed tracing using OpenTelemetry.

SLA monitoring for 99.9% uptime guarantee.

Tactics Applied:

“Monitor Health” → per-service metrics dashboard via Prometheus/Grafana.

“Track System State” → structured logs + correlation IDs.

“Prepare for Rollback” → blue-green deployment pipeline.

4. Updated Architecture Diagram (Text-Based)
                 ┌─────────────────────────────────────────────────┐
                 │        Conversational Experience Layer          │
                 │ (Web UI, Mobile UI, Voice UI, Chat Widget)      │
                 └───────────────────────────────┬─────────────────┘
                                                 │
                                      WebSocket / REST
                                                 │
                     ┌───────────────────────────┴─────────────────────────┐
                     │                API Gateway / SSO                     │
                     └───────────────┬─────────────────────────────────────┘
                                     │ Authenticated, normalized requests
                                     ▼
          ┌─────────────────────────────────────────────────────────────────────────┐
          │                    AIDAP Core Orchestration Engine                      │
          ├───────────────────────┬─────────────────┬──────────────────────────────┤
          │ NLU Pipeline          │ Context Manager │ Response Generator            │
          └───────────────────────┴─────────────────┴──────────────────────────────┘
                                     │
                                     ▼
                    ┌──────────────────────────────────────────────────┐
                    │            Integration Connectors                │
                    │ (LMS, Registration, Calendar, Email, Library)    │
                    └──────────────────────────────────────────────────┘

5. Final Output of Iteration 3

Iteration 3 explicitly refined modules using the top priority quality attributes:

Performance

Availability

Security/Privacy

Modifiability

Usability

All refinements are documented above and reflected in updated tactics.