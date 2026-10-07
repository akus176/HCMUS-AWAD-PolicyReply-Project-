# PolicyReply PA#1 Proposal and Planning

**Repository:** https://github.com/akus176/HCMUS-AWAD-PolicyReply-Project-

## Product user and problem

Linh is a support agent at a Vietnamese online cosmetics SME handling a refund request after delayed delivery. She searches scattered policy files, asks a manager in chat, and writes the reply manually. This is slow, inconsistent, and can create promises the policy does not allow.

PolicyReply is an authenticated support-operations web application where agents manage tickets, queues, assignment, priority, SLA, notes, and approval requests. Managers version policies and approve consequential replies. An audit timeline records ticket changes, selected policy version, draft, review, and send decision.

## One LLM feature and cost when wrong

For a selected ticket and policy version, PolicyReply generates a draft customer reply. Every refund, delivery, or eligibility claim must cite the exact policy passage that supports it. The agent may edit the draft, but must explicitly approve it before it can be sent. The system never auto-sends.

A wrong draft can promise a refund that is not allowed. In the initial evaluation data, an incorrect promise may cost the seller 150,000 VND per case and mislead the customer. It is reversible before approval; after sending, it requires a manual correction and may damage trust. A case fails when a policy claim has no citation, cites the wrong policy version, or conflicts with the labelled evaluation answer.

## Semester scope

**In:** email/password authentication and RBAC; tickets, queues, assignment, priority, SLA indicator, notes, approval state, policy upload/versioning, policy search, LLM draft with citations, review-before-send, audit log, seed data, CI, deployment, monitoring, and an evaluation set.

**Out:** Gmail/Zalo/Facebook/CRM integration; automatic sending; customer portal; refund execution; billing; multilingual replies; advanced analytics; real customer data; and separate microservices.

## Six checkpoints

| Checkpoint | Result | Owner | Target |
|---|---|---|---|
| PA#1 | Proposal and repository | Tran Anh Khoa | 07 Oct 2026 |
| PA#2 | Rules file, Docker, lint, tests, merge-blocking CI | Pham Bao Huy | 15 Oct 2026 |
| PA#3 | Ticket/policy workflow and live Bedrock draft | To Thanh Long | 12 Nov 2026 |
| PA#4 | Labelled eval set, citation guardrail, review gate, red-team report | Pham Bao Huy | 19 Nov 2026 |
| PA#5 | Deployment, SLO dashboard, alert runbook, incident postmortem | To Thanh Long | 26 Nov 2026 |
| PA#6 | Individual explanation of own code and decisions | All team members | 17 Dec 2026 |

Targets after PA#1 are team planning dates and will be aligned if Classroom publishes a different official deadline.

## Risks and mitigations

1. **Unsupported policy claims make the LLM feature unsafe.** Start with 20 seeded policy-ticket pairs, require citations, reject uncited claims, and collect failed cases for PA#4.
2. **Scope becomes a generic helpdesk.** Freeze v1 around policy-backed drafting; defer every external channel and automatic action.

## Technology choices

| Choice | Why it is appropriate |
|---|---|
| React + TypeScript | Typed multi-role screens for agents and managers. |
| Go + chi modular monolith | Keeps the project deployable and understandable; a worker handles ingestion without premature microservices. |
| PostgreSQL + pgvector on RDS; S3 | One transactional store for workflow/audit data and policy retrieval; object storage for source files. |
| Bedrock Converse + Nova Lite | One controlled LLM feature with logged usage; planning estimate for 6k input + 600 output tokens is about US$0.0005 per draft at the published US reference rate. |
| Docker, GitHub Actions, ECS Fargate, ALB | CI validates format/lint/tests/migrations before merge; staging smoke test and human-approved production release with rollback. |
| OpenTelemetry + CloudWatch | Track 99.5% API availability, ticket-write p95 below 500 ms, citation failures, Bedrock errors, and token cost. |

## Guardrails and acceptance

Input/output schema validation, policy-version citations, rejection of uncited claims, rate limits, least-privilege IAM, and agent approval before send.

- Every policy claim cites a passage in the selected policy version; uncited claims are rejected.
- No reply is sent until the assigned agent records explicit approval.
- Format, lint, tests, and migration checks must pass before merge.
- API availability target is 99.5%; ticket-write p95 target is below 500 ms.
