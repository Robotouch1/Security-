# Incident Playbook: Privileged Access Escalation (RBAC)

## Purpose

This playbook defines how the organization responds to **privileged access escalation events** detected through Azure RBAC role assignment changes.

The objective is to:
- contain risk quickly
- preserve evidence
- avoid over-automation
- maintain audit readiness

This playbook is designed for **regulated Azure environments** where identity is the primary security control plane.

---

## Entry Conditions

This playbook is triggered when:

- A Sentinel incident is created by the analytic rule:
  **RBAC Privilege Escalation – Subscription Role Assignment Change**
- The role assignment scope is **subscription-level**
- The operation succeeded (`ROLEASSIGNMENTS/WRITE` or `DELETE`)

---

## Initial Classification

Upon incident creation, classify the event into one of the following categories:

### 1. Authorized / Expected
Examples:
- Break-glass access by on-call administrator
- Pre-approved change window
- Automation identity executing documented deployment

Action:
- Document justification
- Close incident with rationale
- Retain evidence for audit

---

### 2. Suspicious but Unconfirmed
Examples:
- Human identity granting elevated role
- Change outside normal change window
- Justification unclear or missing

Action:
- Escalate to security reviewer
- Temporarily restrict further privilege changes if necessary
- Await confirmation before containment

---

### 3. High Risk / Unauthorized
Examples:
- Privileged role granted unexpectedly
- Unknown identity or service principal
- Change conflicts with governance policy

Action:
- Initiate containment steps immediately
- Notify security leadership
- Preserve forensic evidence

---

## Automated SOAR Actions (Safe by Design)

The following actions **may be automated immediately** upon incident creation:

- Enrich incident with:
  - role name
  - scope
  - caller identity
  - correlation ID
- Tag incident as:
  - `PrivilegeEscalation`
  - `Identity`
- Link related AzureActivity events
- Create investigation task checklist

> Automation is intentionally **non-destructive** at this stage.

---

## Human-in-the-Loop Decision Points

The following actions **require human approval**:

- Removing RBAC role assignments
- Revoking active sessions
- Disabling user or service principal
- Initiating tenant-wide access restrictions

Rationale:
- Prevents disruption to legitimate operations
- Preserves separation of duties
- Aligns with regulated change control requirements

---

## Containment Actions (If Required)

Once unauthorized access is confirmed:

1. Remove unauthorized role assignment
2. Rotate affected credentials (if applicable)
3. Review recent activity by the identity
4. Restrict further privilege changes temporarily
5. Notify governance and compliance stakeholders

All actions must be logged and timestamped.

---

## Evidence Preservation

The following evidence must be retained:

- Sentinel incident record
- AzureActivity logs related to role assignment
- Caller identity details
- Correlation IDs
- Any approval or justification records

Evidence is indexed under:
`security-evidence/00-evidence-index.md`

---

## Post-Incident Review

After resolution:

- Determine why the escalation occurred
- Identify gaps in preventive controls
- Assess whether additional guardrails are required
- Update detection logic if necessary
- Document lessons learned

---

## Design Intent

This playbook is intentionally:

- Conservative with automation
- Explicit about decision authority
- Focused on audit and accountability
- Aligned with identity-as-perimeter strategy

It reflects real-world security operations in regulated cloud environments.
