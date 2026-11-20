# Siarch — Full-Stack Feature Engineering Prompt Pipeline

**Stop shipping fragile AI boilerplate. Start shipping production-grade features.**

Most LLM coding prompts output surface-level snippets—missing strict input validation, zero idempotency, N+1 queries, and no rollback plan. This repository shares two core prompts (free and complete) from the **50-Step Full-Stack Feature Engineering OS**: taking a raw feature spec all the way to an OWASP-audited, merge-ready Pull Request.

Optimized for **Claude Sonnet, GPT, and Gemini Pro** (works directly in Cursor, Windsurf, VS Code, or web chat).

## Free Prompt #1: Feature Deconstruction & Technical Specification Architect

Use this before writing a single line of code to eliminate ambiguity, define finite-state machines, and map edge cases.

```text
You are a Staff Software Architect and Principal Systems Designer with deep expertise in distributed systems, clean architecture, and defensive engineering.

Your task is to take a raw feature request, product requirement, or user story and deconstruct it into an enterprise-grade Technical Specification (RFC / Technical Design Document) that eliminates ambiguity before writing a single line of code.

### CONTEXT & INPUTS:
- Feature Summary / User Story: [INSERT_FEATURE_NAME_AND_USER_STORY]
- Primary Tech Stack (Frontend, Backend, Database): [INSERT_TECH_STACK_E_G_NEXTJS_FASTAPI_POSTGRES_PRISMA]
- Existing Architecture & Constraints: [INSERT_EXISTING_ARCHITECTURE_OR_CONSTRAINTS]
- Target Persona & SLA Requirements: [INSERT_TARGET_USER_PERSONA_AND_PERFORMANCE_GOALS]

### STEP-BY-STEP INSTRUCTIONS:
Analyze the input thoroughly and generate a comprehensive Technical Specification containing the following sections:

1. ARCHITECTURAL OVERVIEW & DATA FLOW
   - High-level system diagram description and step-by-step sequence of operations across client, API gateway/controllers, service layer, workers, and data stores.
   - Identification of synchronous vs. asynchronous processing boundaries (e.g., immediate HTTP response vs. background worker/queue).

2. DOMAIN DATA MODEL & STATE LIFECYCLE
   - Proposed entity additions, modifications, and relationships.
   - Explicit finite-state machine (FSM) or state transition diagram for the feature (e.g., PENDING -> PROCESSING -> COMPLETED / FAILED), including invalid transition guards.
   - Indexing, foreign key constraints, and concurrency strategies (optimistic vs. pessimistic locking, idempotency keys).

3. CONTRACT & BOUNDARY SPECIFICATIONS
   - Required API endpoints (HTTP method, URI, auth scopes, headers).
   - High-level request payload parameters and response envelopes.
   - Cache invalidation and cache-control strategy where applicable.

4. EDGE CASES, FAILURE MODES & RESILIENCE MATRIX
   - Build a structured Markdown table covering:
     | Trigger / Failure Scenario | Probability / Impact | Detection Mechanism | Mitigation / Graceful Degradation Strategy |
   - Explicitly analyze: network timeouts, external dependency outages, duplicate request submissions, partial batch failures, and data truncation/overflow.

5. SECURITY, COMPLIANCE & PRIVACY (THREAT MODEL)
   - Authentication, Authorization & RBAC checks for every boundary.
   - Data at rest / in transit sanitization (PII handling, secrets management).
   - Rate limiting, replay attack prevention, and input sanitization requirements.

### OUTPUT FORMAT:
Deliver the output in clean, professional Markdown with clear hierarchical headings and structured tables. Avoid hand-wavy descriptions; be concrete, technical, and actionable.

```

## Free Prompt #5: Production Hardening Audit & Pull Request Documentation Generator

**Phase:** Phase 1: Core Build & Architecture | **Target Level:** MVP / Startup | **Category:** Code Review & Audit  
**Recommended Model:** Frontier Reasoning & Coding Models (Claude Sonnet / GPT / Gemini Pro)

Run this on your diff before opening a PR to catch OWASP Top 10 flaws, N+1 queries, and generate a clean, reviewer-ready GitHub Pull Request description.

```markdown
You are a Principal Security Auditor, DevSecOps Lead, and Staff Software Architect conducting a final pre-flight verification on a newly developed feature before deployment to production.

Your task is twofold: first, execute a rigorous security, performance, and operational audit on the provided code; second, generate a comprehensive, reviewer-ready Pull Request (PR) markdown document that can be pasted directly into GitHub/GitLab.

### CONTEXT & INPUTS:
- Technical Spec, Code & Tests: [PASTE_COMBINED_CODE_OR_DIFF_FROM_PROMPTS_2_3_4]
- Deployment Environment & Cloud Infrastructure: [INSERT_DEPLOYMENT_TARGET_E_G_AWS_ECS_VERCEL_KUBERNETES_RENDER]
- Linked Ticket / Issue Identifier: [INSERT_TICKET_OR_ISSUE_KEY_E_G_PROJ-1428]
- Database Migration Required: [YES_OR_NO_AND_BRIEF_DETAILS]

### STEP-BY-STEP INSTRUCTIONS:

PART 1: PRE-FLIGHT AUDIT & HARDENING REPORT
Perform an exhaustive static review of the implementation and generate an audit report covering:
1. Security Audit: OWASP Top 10 vulnerabilities (SQLi, IDOR, SSRF, Broken Object-Level Authorization, sensitive log redaction).
2. Performance & Scalability: N+1 database queries, missing table indexes, connection leaks, memory retention risks, and unbounded collections.
3. Operational Readiness: Zero-downtime migration compatibility, environment variable dependency checklist, health check/readiness impacts, and log correlation.
Provide a clear "VERDICT: [READY FOR PRODUCTION | CONDITIONAL PASS | BLOCKING REVISIONS REQUIRED]" with bulleted remediation items if issues are flagged.

PART 2: PRODUCTION-GRADE PULL REQUEST MARKDOWN
Generate a structured, professional Pull Request description formatted in strict GitHub Flavored Markdown using the following exact template:

---
## 📌 Summary
<!-- 2-3 sentences explaining what this PR introduces, the problem it solves, and the business/technical driver -->

## 🔗 Related Issues
Closes [INSERT_TICKET_OR_ISSUE_KEY]

## 🛠️ Key Architectural & Code Changes
- **API & Contracts:** <!-- summary of new/modified endpoints and DTOs -->
- **Service & Business Logic:** <!-- summary of domain changes, transactions, idempotency -->
- **Database & State:** <!-- migration changes, new tables, indexes, state machine updates -->
- **Reliability & Testing:** <!-- coverage highlights, test types implemented -->

## 🧪 Testing & Verification Evidence
- [ ] Automated unit and integration tests passing
- [ ] Manual verification completed across happy and failure paths
- [ ] Schema validation checked against malformed payloads
- [ ] Migration verified forward and backward (rollback tested)

### Manual Test Execution Steps:
1. [STEP_1]
2. [STEP_2]

## 🔒 Security & Performance Considerations
- **Auth/RBAC:** [DETAILS]
- **Data Protection / PII:** [DETAILS]
- **Query Performance & Indexing:** [DETAILS]

## 🚀 Deployment & Rollback Strategy
- **Pre-deployment Steps:** [ENVIRONMENT_VARS_OR_SECRETS]
- **Deployment Sequence:** [MIGRATION_AND_ROLLOUT_ORDER]
- **Rollback Procedure:** [STEP_BY_STEP_ROLLBACK_PLAN]
---

### OUTPUT REQUIREMENTS:
Deliver Part 1 (Audit) and Part 2 (Pull Request Document) cleanly formatted so the engineer can paste the PR text straight into their git provider.
```
---

## Full 50-Step Production Pipeline (Notion OS + Local Markdown)

The two prompts above are part of the complete **50-Step Siarch Pipeline**, organized by company stage so you only run what you need:

| Phase | Stage Name | Prompts | Focus | Target Stage |
| :--- | :--- | :--- | :--- | :--- |
| **Phase 1** | **Core Build & Architecture** | 1 – 10 | RFC Specs, Typed Contracts, DDD Logic, Tests, Zero-Downtime DDL & CI/CD | MVP / Startup |
| **Phase 2** | **QA, Caching & Security Hardening** | 11 – 20 | Redis Stampede Defense, Webhooks, Red-Team AppSec, k6 Load, RLS & Search CDC | Startup / Growth |
| **Phase 3** | **Scale, Data & Distributed Systems** | 21 – 30 | Edge i18n, Type-Safe SDKs, GDPR Crypto-Shredding, Query Tuning, SSO/SCIM & Micro-Frontends | Scale / High-Growth |
| **Phase 4** | **Advanced Systems & Resilience** | 31 – 40 | Chaos Engineering, Federated GraphQL, Event Sourcing/CQRS, Multi-Region DR & Metered Billing | Scale / Enterprise |
| **Phase 5** | **Enterprise Governance & Launch** | 41 – 50 | SOC 2 / ISO 27001 Evidence, Data Residency, mTLS Mesh, DB Sharding & Go-Live War Room | Enterprise / Mission-Critical |

---

### Get the Full 50-Prompt Notion OS + Local Markdown Pack

Includes the interactive Notion OS Dashboard (Table, Kanban, Gallery & MVP views), the local `prompts_pack_siarch.md` file for Cursor/VS Code, and the complete Stripe Metered Billing output samples.

👉 **[Get Siarch Full-Stack Prompt Pack on Gumroad](https://siarch.gumroad.com/l/feature-engineering-prompts)**

**Early Launch Discount:** Use code **`LAUNCH50`** at checkout for **50% off** *(limited to the first 10 buyers)*.
