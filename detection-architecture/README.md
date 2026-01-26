# Detection Architecture & Philosophy

This folder documents **how detection is designed** in this environment — not just which alerts exist.

The goal is to demonstrate **intent-aware, architecture-aligned detection engineering** suitable for regulated Azure environments.

Detection here is treated as a **control layer**, not a reactive afterthought.

---

## Detection Design Principles

### 1. Control-Plane First

All detections are built on **Azure control-plane telemetry** (AzureActivity).

Rationale:
- Control-plane actions represent *intent*
- They occur before or at the moment of impact
- They are auditable and deterministic
- They do not rely on packet inspection or agent deployment

This approach provides consistent coverage across subscriptions and services.

---

### 2. Drift Over Static Posture

Detection focuses on **drift from intended state**, not static misconfiguration.

Examples:
- Network Security Group rules changing to allow Internet access
- Azure Policy exemptions being created
- Subscription-level RBAC assignments being modified

Rationale:
- Drift indicates human or automation intent
- Static posture checks alone miss transient or emergency changes
- Drift is often what auditors and investigators care about most

---

### 3. High-Signal, Low-Noise by Design

Only **high-confidence, high-impact events** generate alerts.

This repository intentionally avoids:
- noisy posture alerts
- informational-only signals
- detections without clear response or review value

Each detection answers:
- *Why does this matter?*
- *What decision should follow?*
- *Who should review it?*

---

### 4. Architecture-Aligned Scope

Detection scope mirrors **architectural boundaries**:

- Subscription-level RBAC changes are treated as high severity
- Platform-owned logging ensures detections cannot be disabled by workloads
- Governance bypasses are surfaced regardless of workload context

This ensures detection reinforces, rather than contradicts, the landing zone design.

---

### 5. Detective Controls Complement Preventive Guardrails

Preventive controls (policy deny, approvals) are preferred wherever possible.

Detections exist to:
- surface attempted bypasses
- identify emergency or exceptional changes
- preserve audit and investigation evidence

Detection is not a substitute for guardrails — it is a **verification layer**.

---

## Implemented Detection Categories

### Network Exposure Drift
- NSG inbound rules allowing Internet access
- Detects unintended or emergency exposure
- Surfaces blast-radius expansion

### Governance Bypass
- Azure Policy exemption creation or deletion
- Detects intentional deviations from enforced guardrails
- Supports audit transparency

### Identity Privilege Escalation
- Subscription-level RBAC role assignment changes
- Detects privilege grants and removals
- Preserves accountability for high-impact access changes

Each detection is documented with:
- design intent
- signal source
- architectural rationale
- linked evidence

---

## Evidence-Driven Validation

Every detection in this repository is backed by:
- real AzureActivity telemetry
- a Sentinel analytic rule
- a generated alert and incident
- timestamped, reviewable evidence

Evidence artifacts are indexed in:
`/security-evidence/00-evidence-index.md`

---

## What Is Intentionally Not Detected

The following are intentionally excluded to maintain signal quality:
- low-risk configuration noise
- service-specific posture recommendations
- alerts without clear ownership or response path

This restraint is deliberate and reflects real-world SOC and audit priorities.

---

## Outcome

This detection architecture ensures:
- security-relevant changes are visible
- privileged actions are accountable
- governance deviations cannot occur silently
- detection aligns with system design, not tooling availability
