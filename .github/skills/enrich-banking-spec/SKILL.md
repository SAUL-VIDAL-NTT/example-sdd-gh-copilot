---
name: "enrich-banking-spec"
description: "Atomic task: takes a raw banking feature description and outputs an enriched version with KYC/AML/GDPR compliance context, actor definitions, application state machine, and measurable success criteria — ready to pass directly to /speckit.specify."
compatibility: "Banking SDLC Harness — Phase 1: Requirements"
metadata:
  author: "banking-sdlc-harness"
  input: "Raw feature description"
  output: "Enriched feature description for /speckit.specify"
---

## Task

Given the raw feature description in `$ARGUMENTS`, produce an enriched version ready for `/speckit.specify`.

**Do not write the spec** — only output the enriched description text.

---

## Enrichment Rules

### 1. Identify the Banking Product and Add Regulations

| Product | Required Regulations |
|---------|---------------------|
| Savings / Current Account | KYC, AML, GDPR |
| Credit Card | KYC, AML, PCI-DSS, GDPR |
| Loan / Mortgage | KYC, AML, GDPR, local credit regulations |
| Transfer / Payment | AML, PCI-DSS |
| Onboarding / Identity | KYC, GDPR, biometric data rules |

### 2. Add Compliance Requirements
Always include:
- **KYC**: identity document validation, liveness check, sanctions screening (OFAC/UN), PEP detection
- **AML**: risk scoring, source-of-funds declaration, CTR/SAR triggers if applicable
- **GDPR**: versioned consent collection, data retention rules, right to erasure scope
- **Audit**: immutable event log for all lifecycle state transitions

### 3. Define Key Actors
- **Applicant/Customer** — the end user
- **Compliance Officer** — reviews escalated/flagged cases
- **External KYC Provider** — third-party identity verification API
- **Core Banking System** — downstream account provisioning

### 4. Add Application State Machine
```
DRAFT → SUBMITTED → KYC_PENDING → APPROVED / REJECTED → ACTIVE
                                       ↓
                                   ESCALATED (PEP/AML flag)
```

### 5. Add Measurable Success Criteria (technology-agnostic)
- Application completed in < 10 minutes
- KYC result returned in < 30 seconds
- Zero PII exposed in logs or error messages
- 100% of applications produce a complete audit trail
- Compliance officer notified within 5 minutes of any AML flag

---

## Output Format

Return only the enriched description as plain text:

```
[Feature purpose in 1-2 sentences]

The feature must support:
- [functional requirements]
- [KYC compliance requirement]
- [AML compliance requirement]
- [GDPR compliance requirement]
- [audit trail requirement]

Key actors: [list]
Application states: [state machine]
Success criteria: [measurable list]
```

---

## Example

**Input:** `$ARGUMENTS = "quiero que los clientes abran una cuenta de ahorros online"`

**Output:**
```
Build a digital savings account opening feature for retail banking customers.

The feature must support:
- Multi-step application: personal info, contact details, employment, source of funds
- Real-time KYC: document upload + liveness check via external provider
- AML screening: sanctions lists (OFAC, UN), PEP detection, risk scoring
- GDPR-compliant versioned consent collection
- Rejection flow with reason communicated to applicant (no internal codes exposed)
- Compliance officer notification for all escalated applications
- Immutable audit trail for every state transition with actor + timestamp

Key actors: Applicant, Compliance Officer, External KYC Provider, Core Banking System
Application states: DRAFT → SUBMITTED → KYC_PENDING → APPROVED/REJECTED → ACTIVE → ESCALATED
Success criteria:
- Customer completes application in < 10 minutes
- KYC result returned in < 30 seconds
- Zero PII in logs or error messages
- 100% of applications have a complete audit trail
- Compliance officer alerted within 5 minutes of AML flag
```
