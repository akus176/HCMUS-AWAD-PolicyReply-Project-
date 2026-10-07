# PolicyReply

PolicyReply is a support-operations web application for Vietnamese SMEs. It helps support agents prepare policy-grounded customer replies for refund, delivery, and eligibility tickets.

## PA#1 proposal

- Course: Advanced Web Application Development
- Team:
  - 23120131 - Pham Bao Huy
  - 23120135 - Tran Anh Khoa
  - 23120143 - To Thanh Long
- Proposal: [docs/PA1_PROPOSAL.md](docs/PA1_PROPOSAL.md)
- AI-use record: [AI-LOG.md](AI-LOG.md)

## User problem

Linh, a support agent, searches scattered policy documents, asks a manager in chat, and writes replies manually. This is slow, inconsistent, and can produce promises that do not match the current policy.

## Product and single LLM feature

The application manages tickets, queues, assignment, priority, SLA, notes, approval requests, versioned policies, and an audit timeline.

For a selected ticket and policy version, the LLM generates a draft customer reply. Every refund, delivery, or eligibility claim must cite the supporting policy passage. The agent reviews and explicitly approves the draft before it is sent. PolicyReply never auto-sends.

## v1 boundaries

In: authentication and RBAC; ticket workflow; versioned policies; policy search; cited LLM draft; review-before-send; audit log; seeded data; CI; deployment; monitoring; and an evaluation set.

Out: external Gmail/Zalo/Facebook/CRM integrations; automatic sending; customer portal; refund execution; billing; multilingual replies; advanced analytics; production customer data; and separate microservices.

## Planned architecture

React and TypeScript frontend, Go and chi modular-monolith API, PostgreSQL with pgvector, S3 policy storage, Amazon Bedrock Converse with Amazon Nova Lite, Docker, GitHub Actions, ECS Fargate, Application Load Balancer, OpenTelemetry, and CloudWatch.

The team will first establish the PA#2 harness before application implementation: Docker development environment, linting, tests, migration checks, and merge-blocking CI.