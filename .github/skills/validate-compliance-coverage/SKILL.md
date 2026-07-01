---
name: "validate-compliance-coverage"
description: "Atomic task: given a spec.md or plan.md content, checks whether KYC, AML, PCI-DSS, and GDPR requirements are covered. Outputs a pass/fail checklist with specific gaps identified. Use this after /speckit.specify or /speckit.plan to verify banking compliance before proceeding."
compatibility: "Banking SDLC Harness — Phases 1, 2, 5"
metadata:
  author: "banking-sdlc-harness"
  input: "Content of spec.md or plan.md"
  output: "Compliance coverage checklist with pass/fail per regulation"
---

## Task

Read the file path or content provided in `$ARGUMENTS` (spec.md or plan.md) and evaluate compliance coverage.

**Output only the checklist** — do not rewrite the spec or plan.

---

## Evaluation Checklist

For each item below, mark ✅ PASS or ❌ FAIL and quote the evidence (or note its absence).

### KYC (Know Your Customer)
- [ ] Identity document validation is specified
- [ ] Liveness check / biometric verification is included
- [ ] Sanctions list screening (OFAC, UN) is mentioned
- [ ] PEP (Politically Exposed Person) detection is covered
- [ ] KYC failure/rejection flow is defined
- [ ] Maximum KYC response time is specified (target: < 30s)

### AML (Anti-Money Laundering)
- [ ] Risk scoring or risk assessment is mentioned
- [ ] Source-of-funds declaration is required
- [ ] Escalation path for AML flags is defined
- [ ] Compliance officer notification is specified
- [ ] CTR/SAR triggers are addressed (if deposit amounts are in scope)

### PCI-DSS (if payment data is in scope)
- [ ] No plaintext storage of card/payment data
- [ ] Payment data fields are identified and marked sensitive
- [ ] Tokenization or vault reference is mentioned

### GDPR / Data Privacy
- [ ] Consent collection is specified (versioned + timestamped)
- [ ] Data retention periods are defined
- [ ] Right to erasure scope is addressed
- [ ] PII fields are identified
- [ ] No PII in logs requirement is stated

### Audit Trail
- [ ] All lifecycle state transitions are logged
- [ ] Log entries include: actor, timestamp, action, result
- [ ] Audit trail is defined as immutable (append-only)

---

## Output Format

```markdown
# Compliance Coverage Report

**File reviewed**: [path/name]
**Date**: [today]
**Overall**: ✅ PASS / ⚠️ PARTIAL / ❌ FAIL

## KYC
| Check | Status | Evidence |
|-------|--------|---------|
| Document validation | ✅ / ❌ | "quote from file" or "NOT FOUND" |
...

## AML
...

## GDPR
...

## Audit Trail
...

## Gaps to Address
- [List each ❌ FAIL item with specific recommendation]
```

---

## Scoring

| Result | Criteria |
|--------|---------|
| ✅ PASS | All mandatory items covered (KYC + AML + GDPR + Audit) |
| ⚠️ PARTIAL | ≥ 80% covered, gaps are non-critical |
| ❌ FAIL | Any KYC or Audit item missing, or > 2 AML gaps |

A **FAIL** result must block progression to the next SDLC phase.
