# Interview Narrative – Regulated Azure Security Architecture

This repository represents how I approach **cloud security engineering with an architecture-first mindset** in regulated Azure environments.

It is intentionally NDA-safe and focuses on **design judgment, control intent, and evidence**, rather than reproducing employer-specific implementations.

---

## How to Read This Repository (Suggested Interview Flow)

When walking through this repository in an interview, I typically frame it in four layers:

1. Architectural intent
2. Preventive controls
3. Detective controls
4. Operational response

Each layer reinforces the others.

---

## 1. Architectural Intent

I start with the **landing zone and architectural decisions**, not tooling.

Key design choices:
- Separation of platform security and workload subscriptions
- Centralized, platform-owned logging
- Identity treated as the primary security perimeter
- Preventive guardrails preferred over reactive detection

These decisions are documented explicitly to show *why* certain controls exist and *why others were intentionally excluded*.

---

## 2. Preventive Controls First

Before detection, I focus on **making unsafe actions difficult or impossible**:

- Azure Policy deny controls
- DevOps approval gates for production changes
- Subscription and scope boundaries aligned to blast radius

Preventive controls reduce incident volume and simplify detection.

Detection is not used to compensate for missing guardrails.

---

## 3. Detection as an Architectural Control

Detection in this environment is designed around **intent-aware, control-plane signals**, not raw telemetry volume.

Key principles:
- Control-plane events represent human or automation intent
- Drift from expected state is more meaningful than static posture
- High-impact events deserve review; noise does not

The Sentinel detections in this repo focus on:
- Network exposure drift (NSG Internet access)
- Governance bypass (Policy exemptions)
- Identity privilege escalation (RBAC role assignments)

Each detection is:
- narrow in scope
- high signal
- backed by real Azure evidence

---

## 4. Evidence-Driven Validation

I intentionally include **evidence artifacts** to show that controls work in practice.

For each major control, the repository contains:
- the detection logic
- the triggering event
- the resulting Sentinel incident
- timestamps and correlation context

This mirrors how controls are validated for audits and internal assurance, not just how they are described in design documents.

---

## 5. Incident Response & Automation Philosophy

I treat SOAR and automation cautiously.

Automation is used for:
- enrichment
- tagging
- correlation
- evidence preservation

Automation is **not** used for:
- destructive containment
- access revocation without context
- decisions with business impact

High-risk actions remain human-approved, which aligns with regulated change-control expectations.

This philosophy is demonstrated in the incident playbook for privileged access escalation.

---

## What This Repository Is (and Is Not)

**This repository is:**
- an architecture demonstration
- a design and decision portfolio
- evidence of senior-level judgment

**This repository is not:**
- a collection of labs
- a service-by-service feature demo
- a reflection of any specific employer environment

---

## How This Reflects My Role

In my day-to-day work, I operate at the intersection of:
- security engineering
- cloud architecture
- governance and compliance

I focus on:
- designing controls that scale
- enabling engineering teams safely
- ensuring security decisions are explainable and auditable

This repository reflects how I think and work in real environments, even when details must remain confidential.

---

## Closing Thought

My goal is not to deploy the most security services.

My goal is to design **predictable, reviewable, and defensible security systems** that support regulated business operations without unnecessary friction.
