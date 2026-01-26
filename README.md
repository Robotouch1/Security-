# Regulated Azure Security Architecture Portfolio (NDA-Safe)

This repository demonstrates **senior-level Azure security architecture** for a **regulated enterprise environment**.

It reflects how security is **designed, governed, and operated** at scale — not tool demos or labs.

## Assumed Environment

- Regulated industry (aerospace / industrial)
- ~30–100 Azure subscriptions
- Centralized security & logging platform
- Separation between platform and workload ownership
- Identity-first (Zero Trust) security model

## How to Review This Repository

Start here:

1. **landing-zone-architecture/**
   - Defines scope, trust boundaries, blast radius, and governance

2. **architecture-decisions/**
   - Records security tradeoffs and architectural judgment

3. **detection-architecture/**
   - Explains how security signals are selected and interpreted

4. **security-evidence/**
   - Maps controls to verifiable evidence

Existing folders provide **control implementations**:
- `sentinel-alert/`
- `devops-change-control/`
- `devops-identity-trust/`

## Disclaimer

This repository is for demonstration purposes only.
No employer systems, tenants, or identifiers are used.
