# Siarch — Full-Stack Feature Engineering Prompt Pipeline

**Stop shipping fragile AI boilerplate. Start shipping production-grade features.**

Most LLM coding prompts output surface-level snippets—missing strict input validation, zero idempotency, N+1 queries, and no rollback plan. This repository shares two core prompts (free and complete) from the **50-Step Full-Stack Feature Engineering OS**: taking a raw feature spec all the way to an OWASP-audited, merge-ready Pull Request.

Optimized for **Claude Sonnet, GPT, and Gemini Pro** (works directly in Cursor, Windsurf, VS Code, or web chat).

---

## Real-World Output Samples (Stripe Metered Billing Engine)

These outputs were generated directly using **Prompt #1** and **Prompt #5** below:

![Technical Specification Preview](./assets/01_prompt_1_technical_spec_preview.png)
![Pre-Flight Security Audit Verdict](./assets/02_prompt_5_security_audit_verdict.png)
![Production Pull Request Output](./assets/03_prompt_5_production_pr_cover.png)

---

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
