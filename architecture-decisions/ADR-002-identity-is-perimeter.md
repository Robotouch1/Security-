# ADR-002: Identity as the Primary Security Perimeter

## Context

Traditional network perimeters are insufficient in cloud environments where:
- services are API-driven
- identities can operate across networks
- automation and pipelines require access

Security must be enforced based on **who** or **what** is acting, not where the request originates.

## Decision

Identity is treated as the **primary security control plane**.

All privileged actions are governed through:
- Entra ID identities
- Role-Based Access Control (RBAC)
- Scoped permissions
- Explicit approval paths for elevation

Network controls support identity enforcement but do not replace it.

## Alternatives Considered

### 1. Network-centric security model
Rejected due to:
- limited protection against credential misuse
- poor fit for DevOps automation
- inability to express intent

### 2. Shared administrative identities
Rejected due to:
- lack of accountability
- increased blast radius
- audit failure risk

## Consequences

### Positive
- Clear accountability for actions
- Fine-grained privilege control
- Alignment with Zero Trust principles
- Scalable across subscriptions

### Tradeoffs
- Requires strong identity governance
- Misconfigured RBAC can create silent risk

## Validation

- RBAC assignments reviewed centrally
- Identity actions logged via AzureActivity
- Privileged operations detectable in Sentinel
