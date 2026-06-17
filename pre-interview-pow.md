# Pre-Interview Proof of Work
## EXOS Title API Client Intelligence — BA Capability Brief
**Erwin Maurice McDonald | Epoch Frameworks LLC | Fort Worth, TX**
`github.com/emcdo411/api-client-intelligence` | `emcdo411.github.io/api-client-intelligence`

---

## Why This Document Exists

Before this interview, I built a public repository and live executive dashboard to demonstrate that I already understand ServiceLink's EXOS integration architecture, schema governance requirements, and compliance posture — not as a training exercise, but as a structured BA proof of work.

This brief explains what I built, what is real versus illustrative, and how it connects directly to the work a ServiceLink BA does every day.

---

## What I Built

A browser-native client intelligence dashboard and business requirements document framework modeled on ServiceLink's EXOS Close title API integration workflow. The repository includes:

| Artifact | What It Demonstrates |
|----------|----------------------|
| ServiceLink API BRD | Six-phase discovery guide (Org, API scope, Schema, Compliance, Error handling, Testing) mapped to development-ready BRD fields |
| Conard Layer Gate | Five-gate ship/no-ship framework governing whether API testing can be scheduled — Client Trust, Compliance, Cycle-Time, Failure-Mode, BA Acceptance |
| EXOS Architecture Brief | Mermaid system diagram mapping the dual REST and SOAP API surfaces, schema decision matrix, status code map, and compliance layer |
| Pipeline Dashboard | FRED-style executive visualization of order states, compliance posture, cycle-time compression, and API readiness scores |

---

## What Is Real and What Is Illustrative

I want to be direct about this because data integrity is central to what a BA in a regulated environment is responsible for.

**What is grounded in real standards:**
The regulatory framework is accurate. RESPA Section 8 referral fee restrictions, TILA disclosure timing windows, CFPB audit trail requirements, MISMO 3.4 schema validation, and Freddie Mac March 2026 model governance requirements are all drawn from public regulatory and industry documentation. The architecture logic — REST vs SOAP routing, IP whitelist as a day-one blocker, schema mismatch as the highest-frequency test failure cause, the mandatory field mapping requirement — reflects actual enterprise integration patterns documented in ServiceLink's public developer and partnership materials.

**What is illustrative:**
The operational metrics are constructed scenarios. The 47 active clients, the specific order IDs, the partner names in the pipeline table, and the compliance flag descriptions are designed to show what a production-grade monitoring layer looks like, not to represent live ServiceLink data. Any BA working in this space knows the difference between a governance model and a production report.

---

## How This Maps to the ServiceLink BA Role

| BA Core Requirement | What the Repo Demonstrates |
|--------------------|-----------------------------|
| Requirements documentation | Six-phase discovery guide produces development-ready BRD fields with named owners, formats, and dev actions |
| Schema and data mapping | MISMO 3.4 vs 2.x routing decision, mandatory field gap analysis, null handling strategy, custom field designation |
| Compliance awareness | RESPA, TILA, CFPB, and model governance gates are named, sequenced, and tied to specific BRD fields before sprint 1 |
| Failure mode documentation | Phase E maps 4xx handling, timeout thresholds, error surfacing, compliance hold routing, cancellation/rollback, and manual override — the broken path as clearly as the happy path |
| Acceptance criteria | Phase F defines sandbox success criteria, credential confirmation, test data type, UAT promotion gate, and sign-off owner before sprint planning begins |
| Release readiness governance | Conard Layer produces a SHIP / SHIP WITH CONTROLS / HOLD / NO-SHIP verdict — no API testing is scheduled without it |
| Stakeholder communication | Executive dashboard translates schema fidelity, compliance posture, and cycle-time compression into a format a VP of Client Integrations reads without a technical translator |

---

## What I Would Bring on Day One

Twenty-plus years in enterprise technology, including structured experience in requirements documentation, data architecture, and AI adoption governance across regulated environments. A working knowledge of ServiceLink's EXOS platform architecture built through independent research and public documentation review. A governance-first mindset: the artifact does not ship until the compliance gate, the schema map, and the acceptance criteria are documented and signed.

I built this repository because I wanted the hiring team to see the work before the conversation, not hear a description of it.

---

## Live Links

| Resource | URL |
|----------|-----|
| GitHub repository | `github.com/emcdo411/api-client-intelligence` |
| Live dashboard | `emcdo411.github.io/api-client-intelligence` |
| Pipeline view | `emcdo411.github.io/api-client-intelligence/#pipeline` |
| Architecture brief (markdown) | `/Outputs/service-link-api-brd.md` in repository |

---

*Erwin Maurice McDonald | Epoch Frameworks LLC | DACR License v2.6 | Fort Worth, TX*
*Built on the Regulated FinServ Presales Framework — Conard Layer extension.*
*This document is a BA working artifact. Compliance determinations in any production engagement must be reviewed by qualified legal counsel.*
