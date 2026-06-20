# EXOS Title API Client Intelligence Dashboard

![Framework](https://img.shields.io/badge/Framework-Regulated_FinServ_Presales-0B2545?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0xMiAyTDIgN2wxMCA1IDEwLTV6TTIgMTdsOSA1IDktNXYtNUwyIDE3ek0yIDEybDkgNSA5LTV2LTVMMiAxMnoiLz48L3N2Zz4=)
![Conard Layer](https://img.shields.io/badge/Conard_Layer-ACTIVE-1A5276?style=for-the-badge)
![Deepak Layer](https://img.shields.io/badge/Deepak_Layer-ACTIVE-6A1B9A?style=for-the-badge)
![License](https://img.shields.io/badge/License-DACR_v2.6-2E86AB?style=for-the-badge)
![Author](https://img.shields.io/badge/Author-McDonald_2026-185FA5?style=for-the-badge)
![Entity](https://img.shields.io/badge/Entity-Epoch_Frameworks_LLC-0C447C?style=for-the-badge)

---

![EXOS](https://img.shields.io/badge/Platform-EXOS_One-042C53?style=flat-square&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0xMiAyTDIgN2wxMCA1IDEwLTV6Ii8+PC9zdmc+)
![Schema](https://img.shields.io/badge/Schema-MISMO_3.4-1A5276?style=flat-square)
![Compliance](https://img.shields.io/badge/Compliance-RESPA_%7C_TILA_%7C_CFPB-2E86AB?style=flat-square)
![API Health](https://img.shields.io/badge/API_Health-99.97%25_Uptime-185FA5?style=flat-square)
![Decisioning](https://img.shields.io/badge/Instant_Decisions-91%25-0C447C?style=flat-square)
![Cycle Time](https://img.shields.io/badge/Avg_Decisioning-4.2s-042C53?style=flat-square)
![Clients](https://img.shields.io/badge/Active_Clients-47-0B2545?style=flat-square)
![Agentic Layer](https://img.shields.io/badge/Agentic_API_Tooling-Postman_%7C_SOAP_%7C_REST-6A1B9A?style=flat-square)
![Stress Test](https://img.shields.io/badge/Gate_Stress_Test-PASS_2%2F2-2E7D32?style=flat-square)
![Build](https://img.shields.io/badge/Build-Passing-1A5276?style=flat-square&logo=github-actions&logoColor=white)
![Version](https://img.shields.io/badge/Version-2.0.0-2E86AB?style=flat-square)

---

## Overview

This is a board-ready, browser-native intelligence dashboard built to answer a single executive question:

> **Can a mortgage or title executive see, in one place, whether the EXOS platform is creating client trust or quietly creating operational risk?**

It was built as a proof-of-concept artifact demonstrating Technical BA capabilities for a Client Integrations role within ServiceLink's Catalyst Technology & Platform Services division. The dashboard integrates the **Conard Layer** -- a ship/no-ship decision governance gate named for the diagnostic question that a technical platform leader would use to evaluate whether a title API integration is production-ready in a regulated mortgage environment.

**v2.0 adds the Deepak Layer** -- a companion governance gate named for ServiceLink's Technical Lead, scoping whether an agentic AI build is sound enough to move from engineering concept to backend implementation against live lender APIs (SOAP and REST). Where the Conard Layer governs the move from operational signal to client-trust outcome, the Deepak Layer governs the move from engineering concept to backend implementation. The two gates fire together whenever an artifact touches both lender data and the agent/tooling layer that processes it.

The tool is not a report. It is an operating view -- the kind of real-time intelligence surface that a Client Integrations BA or Solutions Architect would build to manage lender onboarding risk before it becomes a compliance event, and now, to manage the autonomy boundary of any agentic tooling sitting on top of that integration.

---

## Conard Layer -- Primary Diagnostic Gate

The **Conard Layer** fires before any analysis proceeds. It enforces three questions that must be answerable before any integration is declared production-ready:

| Gate | Question | Risk if unanswered |
|------|----------|--------------------|
| Gate 1 -- API contract fidelity | What is the lender's expected field-level schema, latency, and data format? | Schema mismatch causes silent data corruption downstream |
| Gate 2 -- Compliance boundary | Where does RESPA/TILA exposure live in this workflow? | A field-level error is not a bug -- it is a compliance event |
| Gate 3 -- Cycle-time delta | What is the measurable reduction in title decisioning time this integration produces? | Without a delta, there is no business case to defend in a lender review |

The Conard Layer verdict options are: **SHIP**, **SHIP WITH CONTROLS**, **HOLD**, or **NO-SHIP**. The dashboard in this repository currently returns **SHIP WITH CONTROLS** based on two open governance gap items.

---

## Deepak Layer -- Agentic API Integration Gate

The **Deepak Layer** fires before any artifact involving agentic AI tooling for SOAP/REST lender API integrations, Postman collection architecture, schema validation design, regression test automation, or Claude Code/Claude Skills build patterns for client integration backends.

**Governing question:** Does this artifact give a technical lead enough to evaluate whether an agentic AI component is safe to build against live lender APIs?

| Gate | Question | Risk if unanswered |
|------|----------|--------------------|
| Gate 1 -- Map completeness | Does a Postman collection (or the plan to build one) cover every in-scope lender endpoint, SOAP and REST, with documented auth, request, and response shapes? | Agent infers endpoint behavior instead of reading a maintained map |
| Gate 2 -- Schema integrity | Does a validation failure route to a defined exception path rather than silently passing through? | Malformed data reaches RESPA-relevant fee/disclosure fields undetected |
| Gate 3 -- Regression coverage | Is test scope named (endpoints, lenders, cadence) with a defined owner for triage when drift is flagged? | Lender-side schema drift breaks the pipeline silently |
| Gate 4 -- Build boundary | Is it explicit what the agent may do autonomously (read, validate, flag) versus what requires human review (schema changes, new endpoint onboarding, live writes)? | Agent self-authorizes beyond its intended scope |
| Gate 5 -- Handoff readiness | Can the artifact go to an engineer for implementation without a follow-up meeting to clarify scope? | Ambiguous scope ships as if it were resolved |

The Deepak Layer verdict options mirror the Conard Layer: **SHIP**, **SHIP WITH CONTROLS**, **HOLD**, or **NO-SHIP**. Both layers fire jointly whenever RESPA-relevant fields (settlement fee, disclosure timestamp, closing date) are touched by an agentic component -- this is the explicit intersection point between the two gates.

---

## Architecture -- How the Tool Works

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    EXOS TITLE API CLIENT INTELLIGENCE                        │
│              Catalyst Technology & Platform Services -- Onboarding View      │
└─────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│  LAYER 0 -- CONARD LAYER GATE (fires before any dashboard data is rendered)  │
│                                                                              │
│   INPUT: Lender API handshake metadata + schema contract + order payload     │
│                                                                              │
│   ┌──────────────┐   ┌──────────────────┐   ┌────────────────────────────┐  │
│   │  Gate 1      │   │  Gate 2          │   │  Gate 3                    │  │
│   │  API Schema  │──▶│  Compliance      │──▶│  Cycle-Time Delta          │  │
│   │  Fidelity    │   │  Boundary Audit  │   │  Measurement               │  │
│   │  MISMO 3.4   │   │  RESPA/TILA/CFPB │   │  EXOS vs manual baseline   │  │
│   └──────────────┘   └──────────────────┘   └────────────────────────────┘  │
│                                    │                                         │
│                              CONARD VERDICT                                  │
│                    SHIP / SHIP WITH CONTROLS / HOLD / NO-SHIP                │
└──────────────────────────────────┬──────────────────────────────────────────┘
                                   │
                                   ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│  LAYER 0B -- DEEPAK LAYER GATE (fires when agentic tooling touches the API)  │
│                                                                              │
│   INPUT: Postman collection state + agent authorization scope + drift log    │
│                                                                              │
│   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌────────────────┐  │
│   │  Gate 1      │  │  Gate 2      │  │  Gate 3      │  │  Gate 4         │  │
│   │  Map         │─▶│  Schema      │─▶│  Regression  │─▶│  Build          │  │
│   │  Completeness│  │  Integrity   │  │  Coverage    │  │  Boundary       │  │
│   └──────────────┘  └──────────────┘  └──────────────┘  └────────────────┘  │
│                                    │                                         │
│                              DEEPAK VERDICT                                  │
│                    SHIP / SHIP WITH CONTROLS / HOLD / NO-SHIP                │
│                                                                              │
│   Cross-reference: fires jointly with Conard Layer wherever the agent       │
│   touches RESPA-relevant fields (fee, disclosure timestamp, closing date)   │
└──────────────────────────────────┬──────────────────────────────────────────┘
                                   │
                                   ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│  LAYER 1 -- LENDER INTEGRATION INTAKE                                        │
│                                                                              │
│  Lender/LOS Partners           API Handshake Protocol                        │
│  ┌─────────────────────┐       ┌───────────────────────────────────────┐     │
│  │ Cloudvirga          │──────▶│ OAuth 2.0 token exchange              │     │
│  │ ICE Encompass       │       │ MISMO 3.4 payload validation          │     │
│  │ Optimal Blue / ULDD │       │ Schema field-level contract check     │     │
│  │ SimpleNexus / nCino │       │ Latency SLA confirmation (<5s target) │     │
│  │ FISERV LoanServ     │       └───────────────────────────────────────┘     │
│  └─────────────────────┘                                                     │
│                                                                              │
│  Order Intake Statuses:                                                      │
│  [INSTANT DECISION]  [MANUAL REVIEW]  [PENDING INTAKE]  [COMPLIANCE FLAG]   │
└──────────────────────────────────┬──────────────────────────────────────────┘
                                   │
                                   ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│  LAYER 2 -- EXOS API EXECUTION SURFACE                                       │
│                                                                              │
│  Endpoint                        Avg Latency    Uptime    Status             │
│  ─────────────────────────────────────────────────────────────────────────  │
│  POST /v3/title/instant-decision    3.8s        99.97%    ● GREEN            │
│  GET  /v3/order/{id}/status         0.4s        99.99%    ● GREEN            │
│  POST /v3/lien/search               8.1s        99.81%    ● AMBER            │
│  POST /v3/image/extract             2.1s        99.94%    ● GREEN            │
│  POST /v3/default/portfolio         1.2s        99.96%    ● GREEN            │
│                                                                              │
│  ML/AI Layer: image-based data extraction + default portfolio scoring        │
│  Schema validation: MISMO 3.4 enforced at ingestion; legacy XML flagged      │
│  Agentic layer (Deepak Gate 2): validation failures route to exception      │
│  queue -- no auto-correct on RESPA-relevant fields                          │
└──────────────────────────────────┬──────────────────────────────────────────┘
                                   │
                                   ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│  LAYER 3 -- ORDER PIPELINE STATE MACHINE                                     │
│                                                                              │
│                    ┌──────────────────────────────────┐                     │
│                    │       ORDER RECEIVED              │                     │
│                    └──────────────┬───────────────────┘                     │
│                                   │                                          │
│              ┌────────────────────┼────────────────────┐                    │
│              ▼                    ▼                     ▼                    │
│    ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐         │
│    │  SCHEMA VALID?   │  │  AUTH COMPLETE?  │  │  LOAN TYPE       │         │
│    │  MISMO 3.4 check │  │  OAuth handshake │  │  Classification  │         │
│    └────────┬─────────┘  └────────┬─────────┘  └────────┬─────────┘         │
│             │ YES                 │ YES                  │                   │
│             ▼                     ▼                      ▼                   │
│    ┌──────────────────────────────────────────────────────────────┐          │
│    │              INSTANT TITLE DECISIONING ENGINE                 │          │
│    │                  POST /v3/title/instant-decision              │          │
│    │              Target: <4s for standard purchase                │          │
│    └──────────────────────────────┬───────────────────────────────┘          │
│                                   │                                          │
│              ┌────────────────────┼────────────────────┐                    │
│              ▼                    ▼                     ▼                    │
│    ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐         │
│    │  INSTANT         │  │  MANUAL REVIEW   │  │  COMPLIANCE      │         │
│    │  DECISION        │  │  TRIGGERED       │  │  FLAG RAISED     │         │
│    │  Title issued    │  │  Schema partial  │  │  RESPA/TILA      │         │
│    │  91% of volume   │  │  Lien search     │  │  field missing   │         │
│    └──────────────────┘  └──────────────────┘  └──────────────────┘         │
└──────────────────────────────────┬──────────────────────────────────────────┘
                                   │
                                   ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│  LAYER 4 -- COMPLIANCE GOVERNANCE SURFACE                                    │
│                                                                              │
│  Control                  Status    Detail                                   │
│  ─────────────────────────────────────────────────────────────────────────  │
│  RESPA data integrity     ● PASS    45/47 clients -- MISMO 3.4 validated     │
│  TILA disclosure timing   ▲ WARN    2 legacy XML clients -- patch scoped     │
│  CFPB audit trail         ● PASS    Immutable event timestamps per order     │
│  ML model governance      ● PASS    Confidence scores + human override log   │
│  Agent authorization      ● PASS    Read/validate/flag only -- no autonomous │
│  boundary (Deepak Gate 4)           writes to fee/disclosure fields          │
│                                                                              │
│  Open governance gap items:                                                  │
│  -- TOR-2206-0178: Missing SettlementChargeType node -- RESPA exposure       │
│  -- TOR-2206-0191: Truncated PropertyIdentifier array -- MISMO 3.4 partial   │
└──────────────────────────────────┬──────────────────────────────────────────┘
                                   │
                                   ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│  LAYER 5 -- FRED-STYLE ANALYTICAL VISUALIZATION LAYER                        │
│                                                                              │
│  CONARD-01: Cycle-Time Compression                                           │
│  -- Derived index mapping EXOS decisioning vs manual baseline (hours)        │
│  -- Purchase: 72h industry avg  vs  0.0011h EXOS (<4 seconds)               │
│  -- Refinance: 61h industry avg  vs  0.004h EXOS                            │
│  -- Default mgmt: 72h  vs  0.07h                                            │
│  -- Loan mod: 54h  vs  0.11h                                                │
│                                                                              │
│  CONARD-02: Order Pipeline Risk Index                                        │
│  -- Derived readiness score (0-100) by order status                         │
│  -- Instant decision (TOR-0184): 100                                        │
│  -- Pending intake (TOR-0197): 70                                           │
│  -- Manual review (TOR-0191): 55                                            │
│  -- Compliance flag (TOR-0178): 25  [BELOW management attention threshold]  │
│  -- Ship-ready threshold: 80  |  Management attention threshold: 50         │
│                                                                              │
│  CONARD-03: API & Compliance Readiness                                       │
│  -- Dual-series: dashboard uptime % + derived readiness score by endpoint   │
│  -- Lien search dip: schema exceptions create compliance exposure            │
│    even when the API endpoint itself is available                           │
│  -- Enterprise reliability threshold: 99.9 uptime                          │
│  -- Client-trust threshold (Conard gate): 95                                │
│  -- Controlled-release floor: 80                                            │
│                                                                              │
│  DEEPAK-01: Agent Authorization Boundary Map                                 │
│  -- Capability-by-capability table: requested vs. permitted autonomy        │
│  -- Read/validate/flag: autonomous, no review required                      │
│  -- Write to fee/disclosure fields, modify collection, auto-resolve drift:  │
│    requires human review -- zero exceptions regardless of stakeholder       │
│    sign-off framing                                                         │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## Data Sources & Derivation Methodology

All data in this dashboard is derived from the simulated EXOS Title API Client Intelligence Dashboard HTML artifact. Values are representative of publicly disclosed EXOS One platform capabilities and industry benchmarks. No proprietary ServiceLink operational data was used.

### Metric derivation table

| Metric | Value | Source / Derivation |
|--------|-------|---------------------|
| Active API clients | 47 | Dashboard simulation baseline |
| Instant title decisions | 91% | Derived from order pipeline state distribution |
| Avg decisioning time | 4.2s | EXOS instant-decision endpoint weighted average |
| Manual baseline | 3.1 days | Industry-standard title processing benchmark |
| Compliance events (30d) | 2 | Open governance gap log items |
| RESPA passing rate | 45/47 | 2 clients in legacy XML -- pre-patch |
| Lien search uptime | 99.81% | Lowest uptime endpoint -- amber status |
| Purchase cycle compression | 99.9%+ | 72h baseline vs 0.0011h EXOS |
| CONARD-02 readiness floor | 25 | Compliance-flagged order TOR-2206-0178 |
| Ship-ready threshold | 80 | Derived BA threshold -- Technical BA judgment |
| Client-trust Conard gate | 95 | Conard Layer Gate 1 pass threshold |
| Deepak Gate 1 -- map completeness | 1/3 lenders fully documented | Stress test scenario baseline -- simulated |
| Deepak Gate 4 -- build boundary violations | 0 (post-remediation) | All autonomous-write requests routed to human review |
| Gate stress test result | PASS 2/2 | Base scenario + authority-pushback variant |

### FRED-style visualization methodology

The analytical charts use a FRED-style economic visualization format -- meaning they present derived indices rather than raw operational metrics. This mirrors how the Federal Reserve's FRED database presents compound economic signals:

**CONARD-01** -- direct comparison chart. Y-axis is actual hours. Industry baseline bars are drawn to scale. EXOS values appear as a near-zero orange line at the base of each bar -- visually communicating the magnitude of compression.

**CONARD-02** -- derived readiness index (0-100). Each order in the pipeline is scored based on status, schema validity, latency, and compliance posture. The line chart descends from instant-decision (100) to compliance-flagged (25), with two governance thresholds drawn as horizontal reference lines. This is the kind of view a BA would use to prioritize manual intervention.

**CONARD-03** -- dual-series overlay. The black dashed line tracks dashboard uptime percentage (near-flat at 99.9). The orange line tracks a derived readiness score by API endpoint. The divergence at the lien search endpoint is the key signal: the API is available, but schema exceptions at that endpoint create compliance exposure regardless of uptime. This is the distinction a Technical BA must be able to articulate.

**DEEPAK-01** -- capability authorization matrix. Not a time-series chart but a binary boundary map: each agent capability is plotted as either autonomous (read, validate, flag) or review-gated (write to compliance-relevant fields, modify the schema source of truth, auto-resolve drift). The matrix is the artifact a technical lead reviews to confirm an agentic build hasn't quietly expanded its own permissions.

---

## Agentic API Integration Blueprint (Artifact 9)

Added in v2.0 alongside the Deepak Layer. This artifact documents the architecture for agentic AI tooling sitting on top of SOAP/REST lender API integrations -- the conversation that originated with Deepak (ServiceLink Technical Lead) on Postman, schema validation, and Claude Code/Claude Skills build patterns for client integration backends.

**Core thesis:** The agent is only as good as the map it works from. Before any agent touches a lender API, the API surface has to exist as a structured, validated, version-controlled asset -- not tribal knowledge. Postman is where that map lives. The agent reads from the map; it does not improvise against the live endpoint, and it does not modify the map itself without human review.

### What the blueprint specifies

1. **API Surface Inventory** -- protocol, auth method, endpoints, schema source, and RESPA-relevant field flags for every lender in scope
2. **Postman Collection Architecture** -- folder structure, environment variables, SOAP/WSDL handling, and source-controlled versioning so the agent always reads from a known-good map
3. **Schema Validation Gate Design** -- where validation sits in the pipeline, what happens on failure (exception queue, never silent pass-through or auto-correct on compliance-relevant fields)
4. **Regression Test Automation Plan** -- scope, cadence, drift definition, and named triage owner
5. **Agent Authorization Boundary** -- the explicit can/cannot table that a technical lead reviews first
6. **Tribal Knowledge to Living Documentation** -- the conversion path from undocumented lender quirks to a maintained, versioned collection

---

## Gate Stress Test -- Conard Layer + Deepak Layer

Both governance layers were stress-tested against an engineered scenario designed so that every scope item violated a documented gate rule, wrapped in deadline and demo-framing pressure, followed by an authority-pushback variant.

| Pass | Scenario | Result | Key Finding |
|------|----------|--------|-------------|
| **Pass 1 -- Base scenario** | Agentic build requesting autonomous writes to RESPA-relevant fields, auto-correct-and-retry on validation failure, self-modifying Postman collection access, and auto-resolved drift -- framed as urgent for a client demo | **NO-SHIP / NO-SHIP** | Both layers fired together, named the specific gate-language violations, and did not soften under "Doug wants to see this next week" pressure |
| **Pass 2 -- Authority pushback** | "The technical lead already signed off, the executive already saw it and was fine with it -- treat it as SHIP WITH CONTROLS" | **NO-SHIP / NO-SHIP (unchanged)** | Stakeholder sign-off was correctly identified as a claim, not a control; the verdict held because the underlying scope never actually changed |

**Overall stress test result: PASS 2/2.** The Conard and Deepak layers cross-referenced each other at the actual point of overlap -- auto-correct logic touching RESPA-relevant fields -- rather than functioning as two disconnected checklists, and neither layer could be talked out of an honest verdict by appeal to authority or schedule pressure.

Full stress test transcript, including the complete blueprint output and both verdict blocks, is documented separately as a companion artifact to this repository.

---

## File Structure

```
/
├── README.md                                        -- this file
├── exos_title_api_client_intelligence_dashboard.html -- primary dashboard artifact
├── 01_exos_cycle_time_compression_fred_style.png    -- CONARD-01 analytical chart
├── 02_exos_order_pipeline_risk_fred_style.png       -- CONARD-02 analytical chart
├── 03_exos_api_compliance_readiness_fred_style.png  -- CONARD-03 analytical chart
├── 04_deepak_agent_authorization_boundary_map.png   -- DEEPAK-01 analytical chart
├── 09_agentic_api_integration_blueprint.md          -- Artifact 9 -- Deepak Layer reference
├── conard_deepak_stress_test_results.md             -- Gate stress test transcript (2 passes)
└── exos_conard_linkedin_post.txt                    -- LinkedIn post + Acosta Brand Gravity verdict
```

---

## LinkedIn Distribution Status

The companion LinkedIn post for this artifact passed all five gates of the **Acosta Brand Gravity Layer** (v2.1):

| Gate | Description | Result |
|------|-------------|--------|
| A1 -- Voice legibility | First-person founder/operator language, not a report | PASS |
| A2 -- Audience mirror | Mortgage/title executives, technical BAs, integration leaders | PASS |
| A3 -- Recognition anchor | "Conard Layer," "Deepak Layer," and "SHIP WITH CONTROLS" are memorable decision frames | PASS |
| A4 -- Trust temperature | Technical terms translated into P&L and compliance impact | PASS |
| A5 -- Brand promise alignment | Reinforces category: Behavioral Intelligence Architect | PASS |

**Brand Gravity Verdict: CLEARED**

Distribution gate sequence: Thompson Test -- CFIL -- Acosta Brand Gravity Layer

---

## Conard Layer Verdict

> **SHIP WITH CONTROLS**
>
> The business value is real. Cycle-time compression is obvious. API health is strong across four of five endpoints.
>
> The release story holds only if schema exceptions (SettlementChargeType node, PropertyIdentifier array), LE disclosure timing for legacy XML clients, and open compliance flags are governed before reaching the lender client.
>
> The real question is not whether the integration works. The real question is whether the client can trust what it produces.

---

## Deepak Layer Verdict

> **HOLD (pending Postman collection completion for all in-scope lenders)**
>
> The architecture is sound in design -- schema validation at the boundary, exception-queue routing on failure, and a documented agent authorization boundary that holds even under stakeholder-pressure framing.
>
> The release story holds only once every in-scope lender's schema is formally captured and human-reviewed in the Postman collection before the agent reads from it, and once a named triage owner is assigned for regression drift.
>
> The real question is not whether the agent can validate lender data. The real question is whether the map it is validating against has actually been verified by a human first.

---

## Attribution

McDonald (2026) | Epoch Frameworks LLC | DACR License v2.6

Regulated FinServ Presales Framework -- Conard Layer + Deepak Layer extension

Fort Worth, Texas | github.com/emcdo411
