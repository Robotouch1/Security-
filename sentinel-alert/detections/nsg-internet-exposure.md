## Detection Example: NSG Internet Exposure – Inbound Allow

### Detection Objective

Detect **security-relevant network exposure drift** where a Network Security Group is modified to allow inbound access from the Internet.

This detection is designed to surface:
- accidental exposure
- policy bypass attempts
- emergency changes that require review

### Architectural Context

This rule aligns with the landing zone design principles:
- Network exposure must be **explicit and intentional**
- Drift from expected posture is a security signal
- Detection is based on **control-plane telemetry**, not packet flow

### Signal Source

- AzureActivity logs
- Operation: `MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/SECURITYRULES/WRITE`

### Detection Logic (High Level)

The analytic rule triggers when:
- An inbound NSG rule is created or updated
- Source allows Internet (`0.0.0.0/0`, `Internet`, or `*`)
- Destination ports include administrative or unrestricted access
- Operation succeeds

### Outcome

- A Sentinel alert is generated
- The alert is promoted to an incident
- The incident contains:
  - Caller identity
  - NSG and resource group
  - Rule details
  - Timestamped evidence

### Evidence

- NSG rule creation: see `security-evidence/screenshots/05-nsg-risky-rules.png`
- Sentinel incident: see `security-evidence/screenshots/06-incident-events.png`

### Why This Matters

This detection demonstrates:
- Intent-aware monitoring
- Drift detection over static configuration checks
- Architecture-aligned alerting (high signal, low noise)

It is intentionally narrow and auditable.
