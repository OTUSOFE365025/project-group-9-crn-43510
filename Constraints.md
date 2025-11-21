System Constraints

Project: Smart Scheduler (AI-assisted student scheduling)
Version: v1.0
Date: 2025-10-27

1) Technical Constraints

Target Clients: Web (React) and Mobile (React Native).

Backend Runtime: Node.js 20, Express.

Database: MongoDB 7 (document model), single logical primary with replica set; Redis for sessions/cache only.

APIs: REST over HTTPS; JSON only in v1 (no GraphQL, no gRPC).

Time & Locale: All timestamps stored in UTC (ISO-8601); locale/formatting handled client-side.

Accessibility: UI must meet WCAG 2.1 AA.

3rd-Party Integrations: Google Calendar and Microsoft Outlook via OAuth 2.0 (authorization-code flow).

Background Work: Queue/worker for sync and plan generation; max job runtime 60s.

File Formats Supported: ICS and CSV imports for timetables; max file size 5 MB.

2) Regulatory, Privacy & Security Constraints

Compliance: PIPEDA (Canada) baseline; GDPR-style data-subject rights (export & delete).

Data Residency: Primary data stored in Canada region.

Encryption: TLS 1.2+ in transit; AES-256 at rest using cloud KMS.

Secrets: Managed by AWS Secrets Manager; never committed to source.

Auth: Email/password with strong policy + optional MFA; OAuth scopes limited to least privilege.

Retention: Logs 30 days (prod), 7 days (staging); user data deletion within 30 days of request.

3) Operational Constraints

Hosting: AWS (ECS Fargate for API, S3 + CloudFront for web, managed MongoDB/DocumentDB).

Availability: ≥ 99.5% monthly uptime (prod).

Latency Target: p95 < 400 ms for read APIs; p95 < 700 ms for write/plan-generation enqueue.

Observability: Centralized logs, request IDs, basic APM, uptime probes, alerting on SLO breaches.

Backups & DR: Automated daily snapshots; restore point objective ≤ 24h; recovery time ≤ 2h.

Rate Limits: Per-user API ceiling to protect upstream calendar quotas; backoff on 429/5xx.

4) Process & Organization Constraints

Release Cadence: Bi-weekly sprints; at least one production release per month.

CI/CD: GitHub Actions; all PRs require 1+ review and passing tests.

Testing Minimums: Backend unit coverage ≥ 70%; frontend ≥ 60%; smoke tests must pass pre-deploy.

Issue Tracking: GitHub Issues/Projects; P1 on-call during 9–5 ET with 15-minute ack target.

5) Economic & Schedule Constraints

Cloud Budget (pre-launch): ≤ CAD $300/month average.

v1 Feature Freeze: Nov 30; soft launch Dec 15.

Team Size: ≤ 4 active contributors; no dedicated DBA/SRE in v1.

6) Scope Constraints (v1 Exclusions)

No in-app chat or group calendars.

No Apple Calendar integration.

No paid subscriptions or billing.

No GraphQL API.

No data sharing with third parties beyond listed providers.

7) Data & Integrity Constraints

Consistency: Event writes must be idempotent (idempotency keys) to avoid duplicates.

Conflict Rules: Single source of truth is the app’s plan; external changes reconciled via webhook/periodic backfill.

PII Minimization: Store only identifiers & tokens necessary for sync; never store calendar body text.

8) Device & Network Constraints

Lowest Supported: iOS 16 / Android 10; modern evergreen browsers (last 2 versions).

Offline: Task capture must work offline; sync when online.

Payload Size: API responses ≤ 256 KB p95; gzip/brotli enabled.

9) Assumptions (treated as soft constraints)

Students are willing to grant OAuth calendar access.

Universities can export schedules as ICS/CSV.

Peak load occurs around term start/exam periods and is bursty.