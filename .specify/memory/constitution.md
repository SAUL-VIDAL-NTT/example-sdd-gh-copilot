# Banking Account Opening — Constitution

## Core Principles

### I. Security-First (NON-NEGOTIABLE)
All features must be designed with security as the primary concern.
- Every user input must be validated and sanitized before processing
- Authentication is mandatory for all account operations
- Sensitive data (identity documents, financial info) must be encrypted at rest and in transit
- No PII (Personally Identifiable Information) may be logged or exposed in error messages
- All API endpoints require authorization tokens; no anonymous access to financial operations

### II. Regulatory Compliance (NON-NEGOTIABLE)
The system must comply with applicable banking regulations at all times.
- **KYC (Know Your Customer)**: Identity verification is mandatory before account activation
- **AML (Anti-Money Laundering)**: Transaction monitoring must flag suspicious activity
- **PCI-DSS**: Payment card data must never be stored in plaintext
- **GDPR / Data Privacy**: Users must consent to data processing; right to erasure must be supported
- All compliance checks must be automated and auditable

### III. Spec-Driven Architecture (SDA)
Every feature starts from a spec, and architecture follows the spec.
- No code is written without an approved spec and plan
- Architecture decisions must be traceable to spec requirements
- The spec is the single source of truth; code must converge to the spec

### IV. Test-First Development (TDD)
- Tests are written before implementation
- Acceptance criteria from the spec become automated test cases
- Coverage must be ≥ 80% for all business logic
- Security and compliance scenarios must have explicit test coverage

### V. Auditability & Observability
- All account lifecycle events must be logged with timestamps and actor identity
- Audit trails are immutable and cannot be deleted
- System must support real-time monitoring of failed authentication attempts
- Errors must be user-friendly externally, detailed internally (never expose stack traces)

### VI. Simplicity & Incrementality
- Start with the minimum viable feature that satisfies the spec
- No premature optimization; no gold-plating
- Each SDLC phase produces a reviewable artifact before the next phase begins

## Security Requirements

- OAuth 2.0 / OpenID Connect for authentication
- Multi-Factor Authentication (MFA) required for account opening
- Session tokens expire after 15 minutes of inactivity
- Rate limiting on all authentication endpoints (max 5 attempts, then lockout)
- All external API calls must use mutual TLS (mTLS)

## Quality Gates (SDLC)

Each phase must produce an artifact and pass a review before proceeding:

| Phase | Artifact | Gate |
|-------|----------|------|
| Specify | `spec.md` | Business review + KYC/AML compliance check |
| Clarify | `clarification.md` | No open [NEEDS CLARIFICATION] items |
| Plan | `plan.md` | Architecture review + security design review |
| Checklist | `checklist.md` | All items addressed |
| Tasks | `tasks.md` | All tasks have acceptance criteria |
| Analyze | `analysis.md` | No critical gaps |
| Implement | `src/` | Code review + security scan |
| Converge | `convergence.md` | All tasks done, tests pass |

## Governance

- This constitution supersedes all other development practices
- Any exception to these principles requires written justification in the spec
- All agents must read and apply this constitution before generating artifacts
- Amendments require documentation, approval, and migration plan

**Version**: 1.0.0 | **Ratified**: 2026-07-01 | **Domain**: Banking / Digital Account Opening
