Architectural Concerns

Project: Smart Scheduler (AI-assisted student scheduling)
Version: v1.0
Date: 2025-10-27

Architectural concerns are grouped by quality attribute. Each item lists the Concern, Why it matters, Decisions / Tactics, and Measurable Checks so the team can verify we met the intent.

1) Availability & Reliability

Concern: Users must access schedules reliably—especially near deadlines/exams.

Why: Downtime directly causes missed deadlines and user churn.

Decisions / Tactics:

Multi-AZ DB replica set; stateless API behind ALB with health checks.

Graceful degradation (read-only dashboards if queue/DB degraded).

Retry with exponential backoff; circuit breakers between API ↔ DB/Redis.

Measurable Checks: Monthly uptime ≥ 99.5%; MTTR < 30 min; error budget policy in place.

2) Performance & Scalability

Concern: Keep responses fast during start-of-term spikes.

Why: Slow UI increases abandonment; calendar APIs have quotas.

Decisions / Tactics:

Redis caching for dashboard aggregates; pagination on lists.

Asynchronous jobs for heavy “suggested plan” generation.

DB: targeted compound indexes on userId+date ranges; avoid N+1 queries.

Measurable Checks: p95 < 400 ms for reads; < 700 ms for writes/enqueues; sustain 1k concurrent users at <60% CPU.

3) Security & Privacy

Concern: Protect personal calendars and class data.

Why: Legal/ethical requirements (PIPEDA), user trust, brand risk.

Decisions / Tactics:

OAuth 2.0 with least-privilege scopes; short-lived tokens + refresh.

Secrets in AWS Secrets Manager; TLS 1.2+; AES-256 at rest (KMS).

Audit trails for privileged actions; WAF & rate limits.

Measurable Checks: Quarterly OWASP ASVS L1 review; zero critical vulns; security headers in all responses.

4) Modifiability (Changeability)

Concern: Add new calendar providers and rules without major rewrites.

Why: Roadmap demands frequent integration changes.

Decisions / Tactics:

Ports & Adapters for CalendarPort; provider plugins behind an interface.

Feature flags (LaunchDarkly-style) for safe rollouts.

Contract tests per adapter; API versioning for public endpoints.

Measurable Checks: New provider shipped with ≤ 2 days lead time; ≤ 300 LoC changes outside adapter.

5) Usability & Accessibility

Concern: Students must capture tasks quickly on mobile and web.

Why: First-run success drives activation and retention.

Decisions / Tactics:

Offline-first task capture; keyboard shortcuts; semantic HTML/ARIA.

Clear conflict badges and one-tap resolution.

Measurable Checks: SUS ≥ 75; median time to create & see a scheduled task ≤ 60 s.

6) Data Integrity & Consistency

Concern: Avoid double-booking and duplicate events across syncs.

Why: Integrity errors erode trust immediately.

Decisions / Tactics:

Idempotency keys for writes; optimistic concurrency (ETags).

Conflict detection rules centralized; eventual consistency notes in UI.

Measurable Checks: Duplicates < 0.1% of writes; reconciliation jobs clear drift < 15 min.

7) Integration & Interoperability

Concern: Robust sync with Google/Outlook and timetable imports (ICS/CSV).

Why: External APIs are failure-prone and quota-limited.

Decisions / Tactics:

Webhooks where available; scheduled backfills; token refresh handling.

Backpressure on queues when providers throttle (429).

Measurable Checks: Sync success rate ≥ 99%; failed jobs retried ≤ 3 times with dead-letter visibility.

8) Observability

Concern: Diagnose incidents and AI scheduling issues quickly.

Why: Faster triage reduces MTTR and churn.

Decisions / Tactics:

Structured logs with correlation IDs; metrics (RQPS, latency, queue depth).

Tracing for API ↔ worker ↔ provider calls; dashboards and SLO alerts.

Measurable Checks: Time-to-detect (TTD) < 5 min via alerts; logs searchable by userId & requestId.

9) Deployability & Operability

Concern: Safe releases and quick rollbacks without downtime.

Why: Small team; mistakes will happen.

Decisions / Tactics:

Blue/green deploys; DB migrations are forward-only with fallbacks.

Infra as Code (Terraform); GitHub Actions CI/CD with smoke tests.

Measurable Checks: Change failure rate < 10%; rollback < 10 min.

10) Testability

Concern: Confidence to change code rapidly.

Why: Frequent iterations on rules and adapters.

Decisions / Tactics:

Test pyramid (unit > contract > integration > e2e).

Provider simulators/mocks; deterministic time with clock abstraction.

Measurable Checks: Backend coverage ≥ 70%; flake rate in CI < 1%.

11) Portability

Concern: Ability to move between MongoDB Atlas and AWS DocumentDB.

Why: Cost and residency options.

Decisions / Tactics:

Use compatible features subset; avoid provider-specific extensions.

Migration scripts and seed data standardized.

Measurable Checks: DR runbook validated quarterly; trial restore in alt-env < 2h.

12) Compliance & Governance

Concern: Meet PIPEDA and GDPR-style user rights.

Why: Legal liability and trust.

Decisions / Tactics:

Data export/delete endpoints; retention policies; DPIA checklist.

Measurable Checks: Export within 30 s for typical accounts; deletion completed within 30 days.

13) Cost Efficiency

Concern: Keep cloud spend within student-project budget.

Why: Constraint from stakeholders.

Decisions / Tactics:

Autoscaling with conservative max; scheduled scale-down for off-hours.

Cache hot reads to reduce DB load; cold-start friendly containers.

Measurable Checks: Pre-launch cloud bill ≤ CAD $300/mo average.

14) Backup & Disaster Recovery

Concern: Recover from data loss or regional issues.

Why: Calendars and tasks are critical personal data.

Decisions / Tactics:

Daily snapshots + PITR; periodic restore drills.

Runbooks for region failover; infra recreated from Terraform.

Measurable Checks: RPO ≤ 24h; RTO ≤ 2h; successful quarterly restore tests.

15) AI/Heuristics Scheduling

Concern: “Suggested plan” must be explainable and non-blocking.

Why: Trust and predictability.

Decisions / Tactics:

Heuristic rules first (duration, deadline, priority, energy levels); job queue.

Show rationale in UI (“Placed here due to free 1h slot before class”).

Measurable Checks: User accepts ≥ 60% of suggestions; generation job p95 < 10 s.

Open Risks & Questions

Quota limits from Google/Microsoft during peak—need early monitoring of 429s and potential user messaging.

Data residency exceptions for third-party telemetry—confirm endpoints hosted in Canada or anonymize.

Timetable import variance—sample real ICS/CSV files from multiple faculties to harden parsers.

Mobile offline conflicts—define precedence rules and conflict UI copy.