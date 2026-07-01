---
name: "classify-pii-fields"
description: "Atomic task: given a list of data fields or a model definition, classifies each field as PII, Sensitive, or Public, and assigns the applicable GDPR retention period and encryption requirement. Outputs a data classification table ready to include in spec or architecture docs."
compatibility: "Banking SDLC Harness — Phases 1, 2, 4"
metadata:
  author: "banking-sdlc-harness"
  input: "List of field names or model definition"
  output: "Data classification table with GDPR retention and encryption rules"
---

## Task

Given the list of fields or model in `$ARGUMENTS`, classify each field and output a data classification table.

**Output only the table** — do not modify any spec or code file.

---

## Classification Rules

### PII (Personally Identifiable Information)
Fields that directly identify a natural person. Subject to GDPR Article 4.

Examples: full name, date of birth, national ID number, passport number, email address,
phone number, home address, IP address (when linked to identity), biometric data.

- **Retention**: 7 years (regulatory minimum for banking) or until account closure + 5 years
- **Encryption**: Required at rest and in transit
- **Erasure**: Not possible during active regulatory hold; possible for marketing/non-banking PII

### Sensitive (Financial / Operational)
Fields that are confidential but don't directly identify a person.

Examples: account number, balance, transaction history, credit score, source of funds,
employment status, monthly income, document images (without biometric link), risk score.

- **Retention**: 7 years post account closure
- **Encryption**: Required at rest and in transit
- **Erasure**: Not possible (regulatory hold)

### Public
Fields with no privacy risk.

Examples: product name, branch name, currency code, account type, application reference ID,
status label, created date (without actor link).

- **Retention**: 2 years or as needed
- **Encryption**: In transit only (HTTPS)
- **Erasure**: Possible

---

## Output Format

```markdown
# Data Classification: [Model/Feature Name]

| Field | Classification | GDPR Retention | Encrypted at Rest | Erasure Possible |
|-------|---------------|---------------|------------------|-----------------|
| fullName | 🔴 PII | 7 years | ✅ Yes | ❌ No (banking hold) |
| dateOfBirth | 🔴 PII | 7 years | ✅ Yes | ❌ No |
| accountNumber | 🟡 Sensitive | 7 years post-closure | ✅ Yes | ❌ No |
| productName | 🟢 Public | 2 years | ❌ No | ✅ Yes |

## Summary
- 🔴 PII fields: [count] — require encryption + access logging
- 🟡 Sensitive fields: [count] — require encryption
- 🟢 Public fields: [count] — standard handling

## Logging Warning
The following fields must NEVER appear in application logs:
- [list all PII and Sensitive fields]
```

---

## Example

**Input:** `$ARGUMENTS = "fullName, email, phoneNumber, dateOfBirth, nationalId, accountNumber, balance, productType, applicationStatus"`

**Output:**

```markdown
# Data Classification: Account Opening Application

| Field | Classification | GDPR Retention | Encrypted at Rest | Erasure Possible |
|-------|---------------|---------------|------------------|-----------------|
| fullName | 🔴 PII | 7 years | ✅ Yes | ❌ No |
| email | 🔴 PII | 7 years | ✅ Yes | ✅ Yes (if no active account) |
| phoneNumber | 🔴 PII | 7 years | ✅ Yes | ✅ Yes (if no active account) |
| dateOfBirth | 🔴 PII | 7 years | ✅ Yes | ❌ No |
| nationalId | 🔴 PII | 7 years | ✅ Yes | ❌ No |
| accountNumber | 🟡 Sensitive | 7 years post-closure | ✅ Yes | ❌ No |
| balance | 🟡 Sensitive | 7 years post-closure | ✅ Yes | ❌ No |
| productType | 🟢 Public | 2 years | ❌ No | ✅ Yes |
| applicationStatus | 🟢 Public | 2 years | ❌ No | ✅ Yes |

## Summary
- 🔴 PII fields: 5 — require encryption + access logging
- 🟡 Sensitive fields: 2 — require encryption
- 🟢 Public fields: 2 — standard handling

## Logging Warning
These fields must NEVER appear in application logs:
fullName, email, phoneNumber, dateOfBirth, nationalId, accountNumber, balance
```
