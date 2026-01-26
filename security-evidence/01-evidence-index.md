# Security Evidence Index (NDA-Safe)

This index maps security control objectives to **verifiable evidence** produced by the Azure environment backing this portfolio.

> Evidence is intentionally minimal, redactable, and focused on proving control behavior.

## Environment (High-Level)

- Platform Security Subscription: `sub-platform-security`
- Workload Subscription: `sub-prod-workload`
- Central Log Analytics Workspace: `law-central-security` (platform-owned)

## Evidence Table

| Control Objective | Evidence Artifact | What It Proves | Where |
|---|---|---|---|
| Centralized logging ownership | Screenshot: LAW workspace overview | Logs are platform-owned, not workload-owned | Azure Portal |
| Control-plane telemetry available | Screenshot: Logs query showing `AzureActivity` rows | Control-plane actions are observable | LAW → Logs |
| Preventive guardrails work | Screenshot: Policy **deny** or pipeline failure caused by deny | Unsafe changes are blocked | Azure Policy / DevOps |
| Drift / exposure is detectable | Screenshot: Sentinel alert firing on NSG public exposure or RBAC change | Security-relevant drift becomes a signal | Sentinel |
| Guardrail bypass visibility | Policy exemption creation | Preventive controls cannot be silently bypassed | ./screenshots/07-policy-exemption-created.png |
| Governance drift detection | Sentinel incident for policy exemption | Exemptions generate reviewable security incidents | ./screenshots/08-policy-exemption-incident.png |

## Redaction Rules

- Blur subscription IDs, tenant IDs, user emails
- Avoid public IPs (or blur)
- Keep timestamps visible (proves recency)
