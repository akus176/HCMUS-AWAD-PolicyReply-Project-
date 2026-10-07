# PolicyReply PA#1 Proposal and Planning

**Repository:** https://github.com/akus176/HCMUS-AWAD-PolicyReply-Project-

## 1. Product, user, and problem

Linh is a support agent at a Vietnamese online cosmetics SME handling a refund request after delayed delivery. She searches scattered policy files, asks a manager in chat, and writes the reply manually. This is slow, inconsistent, and can create promises the policy does not allow.

PolicyReply is an authenticated support-operations web application for tickets, queues, assignment, priority, SLA, notes, approval requests, versioned policies, and audit history. Managers approve consequential replies.

## 2. LLM feature and cost when wrong

For a selected ticket and policy version, the LLM generates one customer-reply draft. Every refund, delivery, or eligibility claim must cite its supporting policy passage. An agent may edit the draft but must explicitly approve it before sending. The system never auto-sends.

A wrong draft may promise an invalid refund. The initial evaluation assumption is 150,000 VND loss per incorrect promise, plus customer-trust damage. The error is reversible before approval but harder to correct after sending. A case fails if a claim is uncited, cites the wrong policy version, or conflicts with the labelled evaluation answer.

## 3. Scope

**In scope:** authentication and RBAC; ticket workflow; queues and assignment; priority and SLA; notes; policy upload and versioning; policy search; cited LLM drafts; review-before-send; audit log; seed data; CI; deployment; monitoring; and an evaluation set.

**Out of scope:** Gmail, Zalo, Facebook, or CRM integration; automatic sending; customer portal; refund execution; billing; multilingual replies; advanced analytics; real customer data; and separate microservices.

## 4. Six checkpoints and ownership

| Checkpoint | Deliverable | Owner | Target |
|---|---|---|---|
| PA#1 | Proposal and repository | Tran Anh Khoa | 07 Oct 2026 |
| PA#2 | Rules, Docker, lint, tests, and merge-blocking CI | Pham Bao Huy | 15 Oct 2026 |
| PA#3 | Ticket/policy workflow and live Bedrock draft | To Thanh Long | 12 Nov 2026 |
| PA#4 | Labelled evaluation set, citation guardrail, review gate, red-team report | Pham Bao Huy | 19 Nov 2026 |
| PA#5 | Deployment, SLO dashboard, alert runbook, incident postmortem | To Thanh Long | 26 Nov 2026 |
| PA#6 | Individual explanation of code and decisions | All members | 17 Dec 2026 |

Dates after PA#1 are team planning targets and will be aligned if Classroom publishes different official deadlines.

## 5. Risks and mitigations

1. **Unsupported policy claims:** seed 20 policy-ticket pairs, require passage citations, reject uncited claims, and retain failures for PA#4.
2. **Scope expansion:** freeze v1 around policy-backed drafting and approval; defer external channels and automatic actions.

## 6. Technology and end-to-end delivery

| Choice | Justification |
|---|---|
| React + TypeScript | Typed multi-role interface for agents and managers. |
| Go + chi modular monolith | Clear, deployable API; one ingestion worker without premature microservices. |
| PostgreSQL + pgvector on RDS; S3 | Transactional workflow/audit data, policy retrieval, and durable source files. |
| Bedrock Converse + Nova Lite | One controlled LLM feature with logged usage; planning estimate about US$0.0005 per 6k-input/600-output draft at the published US reference rate. Verify the selected Region before deployment. |
| Docker, GitHub Actions, ECS Fargate, ALB | CI checks format, lint, tests, and migrations; staging smoke test; human-approved release; rollback image. |
| OpenTelemetry + CloudWatch | Observe 99.5% API availability, ticket-write p95 below 500 ms, citation failures, model errors, and token cost. |

## 7. Guardrails and acceptance criteria

Schema validation, policy-version citations, rejection of uncited claims, rate limits, least-privilege IAM, and explicit agent approval are mandatory.

- **Grounding:** every policy claim cites the selected policy version; uncited claims are rejected.
- **Human gate:** no reply is sent before explicit agent approval.
- **CI gate:** format, lint, tests, and migration checks pass before merge.
- **Operations:** API availability target is 99.5%; ticket-write p95 target is below 500 ms.
