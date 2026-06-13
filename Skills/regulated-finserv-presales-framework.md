# Regulated FinServ Presales Framework

![Framework](https://img.shields.io/badge/Skill-Regulated_FinServ_Presales-0B2545?style=for-the-badge)
![Version](https://img.shields.io/badge/Version-2.0-1A5276?style=for-the-badge)
![Conard Layer](https://img.shields.io/badge/Conard_Layer-INTEGRATED-2E86AB?style=for-the-badge)
![License](https://img.shields.io/badge/License-DACR_v2.6-185FA5?style=for-the-badge)
![Entity](https://img.shields.io/badge/Entity-Epoch_Frameworks_LLC-0C447C?style=for-the-badge)

---

![Verticals](https://img.shields.io/badge/Verticals-Banking_%7C_Mortgage_%7C_Title_%7C_Insurance-042C53?style=flat-square)
![Platforms](https://img.shields.io/badge/Platforms-Snowflake_%7C_Fabric_%7C_Databricks_%7C_EXOS-0B2545?style=flat-square)
![Compliance](https://img.shields.io/badge/Compliance-SOX_%7C_BCBS_239_%7C_RESPA_%7C_TILA_%7C_CFPB-1A5276?style=flat-square)
![Gate](https://img.shields.io/badge/Ship_Gate-SHIP_%7C_SHIP_WITH_CONTROLS_%7C_HOLD-2E86AB?style=flat-square)
![Author](https://img.shields.io/badge/McDonald_2026-Epoch_Frameworks-185FA5?style=flat-square)

---

## What this skill does

This skill produces consulting-grade presales and technical BA artifacts for regulated enterprise data and platform modernization opportunities. It covers banks, credit unions, insurance carriers, mortgage servicers, title API platforms, and any data estate operating under SOX, BCBS 239, RESPA, TILA, CFPB, GDPR, CCPA, or equivalent regulatory frameworks.

**Core thesis:** Enterprise AI does not start with models. It starts with governed, accessible, trusted data platforms. In mortgage and title environments, that principle extends one level further -- a field-level schema error is not a bug. It is a compliance event.

**v2.0 adds:** The Conard Layer -- a ship/no-ship decision governance gate built specifically for mortgage fintech and title API client servicing environments. It fires before any presales analysis proceeds when the prospect operates in the ServiceLink / EXOS One / title API / mortgage servicing space.

---

## What changed in v2.0

### Conard Layer -- new primary gate for mortgage/title engagements

The Conard Layer was added as a named diagnostic gate that fires automatically when the engagement context involves any of the following:

- Title API client onboarding or integration
- Mortgage servicing platform assessment
- EXOS One / ServiceLink / Catalyst Technology & Platform Services
- Lender-to-servicer API handshake architecture
- MISMO 3.4 schema compliance
- Default portfolio management or loan modification workflows

When any of those signals are present, the skill runs the Conard Layer before producing any other artifact. The gate enforces three questions that must be answerable before any integration or recommendation is declared production-ready.

### New scenario client -- ServiceLink / EXOS One

The Northstar Commercial Bank scenario (v1.0) remains active for general banking and data modernization engagements. The EXOS Title API scenario is added as a second simulation environment for mortgage fintech and title API contexts.

### New artifact -- EXOS Title API Client Intelligence Dashboard

A live HTML dashboard artifact was added to the production artifact stack. It demonstrates a mock EXOS client onboarding intelligence view with API health monitoring, order pipeline state tracking, compliance posture scoring, and cycle-time compression visualization. It is designed to be opened in a browser during a ServiceLink interview or presales meeting.

---

## Conard Layer -- full specification

### Trigger conditions

The Conard Layer fires when the request involves:

```
title API integration          mortgage servicing platform
EXOS One / EXOS platform       ServiceLink / Catalyst Technology
MISMO 3.4 schema               lender API onboarding
RESPA/TILA compliance          default portfolio management
loan modification workflow     client trust / API governance question
ship/no-ship decision          integration readiness assessment
```

### Three gates -- always run in sequence

**Gate 1 -- API contract fidelity**

Ask and document: What is the lender's expected field-level schema, latency requirement, and data format? Is the client delivering a full MISMO 3.4 payload or a partial/legacy XML feed? What happens downstream when a required field is absent?

The risk if unanswered: Schema mismatch at the field level causes silent data corruption downstream. A missing node is not caught at the API layer -- it surfaces as a compliance event at the lender review stage.

**Gate 2 -- Compliance boundary**

Ask and document: Where does RESPA and TILA exposure live in this specific workflow? Which data fields carry regulatory weight? Is the LE disclosure window being triggered correctly for every loan type in the pipeline?

The risk if unanswered: In mortgage technology, a field-level error is not a software bug. It is a RESPA/TILA exposure point, a client-trust issue, and a manual remediation cost hiding inside the workflow. The compliance obligation does not belong to the API -- it belongs to the integration architecture.

**Gate 3 -- Cycle-time delta**

Ask and document: What is the measurable reduction in title decisioning time this integration produces compared to the manual baseline? What is the lender's current cycle time, and what is the EXOS target?

The risk if unanswered: Without a documented cycle-time delta, there is no business case to defend in a lender review. Speed is the primary value proposition of the EXOS platform -- if the integration cannot demonstrate compression, the deal has no anchor metric.

### Conard Layer verdicts

| Verdict | Meaning | When it applies |
|---------|---------|-----------------|
| SHIP | Integration is production-ready across all three gates | Schema valid, compliance clean, cycle-time documented |
| SHIP WITH CONTROLS | Business value is real but governance gaps must be resolved pre-launch | Open field-level flags, partial schema payloads, TILA timing patches in progress |
| HOLD | Integration is not ready for lender-facing deployment | Active RESPA exposure, API timeout on critical endpoints, schema contract unresolved |

The verdict is the final output of the Conard Layer. It precedes all other presales analysis. No recommendation, architecture, or POC scope is delivered until a Conard verdict is on record.

---

## Full artifact stack -- v2.0

### Artifacts inherited from v1.0 (general regulated finserv)

| # | Artifact | Trigger |
|---|----------|---------|
| 1 | Executive one-pager | Executive briefing or board-level summary requested |
| 2 | Discovery workshop guide | Discovery conversation in progress |
| 3 | Platform decision matrix | Platform recommendation needed (Snowflake / Fabric / Databricks) |
| 4 | Reference architecture pack | Architecture diagram or system design requested |
| 5 | Governance operating model | Governance, compliance, or data ownership question |
| 6 | Sample RFP/RFI response | Proposal writing or competitive response |
| 7 | POC-to-delivery handoff | POC scoping or delivery planning |
| 8 | Repeatable offering sheet | Packaging as a service offering |

### New artifacts added in v2.0 (mortgage fintech / title API)

| # | Artifact | Trigger |
|---|----------|---------|
| 9 | EXOS Title API Client Intelligence Dashboard | ServiceLink / EXOS / title API context -- browser-native HTML |
| 10 | Conard Layer diagnostic brief | Ship/no-ship assessment for any mortgage platform integration |
| 11 | FRED-style analytical visualization pack | Cycle-time compression, order pipeline risk, API compliance readiness charts |
| 12 | LinkedIn presales proof-of-work post | Acosta Brand Gravity cleared -- distribution-ready |

---

## Routing logic -- v2.0

```
REQUEST RECEIVED
       |
       v
Is the context mortgage / title / EXOS / ServiceLink / MISMO?
       |
      YES ----> RUN CONARD LAYER FIRST
       |             Gate 1: API schema fidelity
       |             Gate 2: Compliance boundary
       |             Gate 3: Cycle-time delta
       |             Verdict: SHIP / SHIP WITH CONTROLS / HOLD
       |                  |
       |                  v
       |         Route to Artifacts 9-12 (mortgage/title stack)
       |         Then layer in Artifacts 1-8 as needed
       |
       NO ----> Route directly to Artifacts 1-8 (general finserv stack)
                    Discovery conversation    --> Artifact 2
                    Platform recommendation  --> Artifact 3
                    Architecture diagram     --> Artifact 4
                    Governance question      --> Artifact 5
                    RFP/RFI writing          --> Artifact 6
                    POC scoping              --> Artifact 7
                    Offering packaging       --> Artifact 8
                    Executive briefing       --> Artifact 1
                    Full kit                 --> All artifacts in sequence
```

---

## Scenario environments

### Scenario A -- Northstar Commercial Bank (v1.0, general banking)

```
Client:           Northstar Commercial Bank
Segment:          Mid-market commercial bank, $8B AUM
Data Platform:    Legacy SQL Server 2016 DW (on-prem), ETL via SSIS
Pain Points:      Siloed customer/account/transaction/risk data
                  Manual regulatory reporting (DFAST, Call Report)
                  47 Power BI workspaces, no enterprise semantic layer
                  No data catalog, no lineage tracking
                  Failed MDM initiative (2021)
AI Interest:      Fraud analytics, customer 360, churn prediction
Platform Eval:    Snowflake (data team), Fabric (IT mandate), Databricks (data science)
Constraints:      3-year Azure EA, 18-month migration window, $2.1M budget
Regulatory Env:   OCC supervision, SOX reporting, BCBS 239 alignment in progress
CDO Contact:      David Reyes (fictional)
```

### Scenario B -- ServiceLink / EXOS One (v2.0, mortgage/title API)

```
Client:           ServiceLink -- Catalyst Technology & Platform Services
Platform:         EXOS One -- instant title decisioning, API integrations,
                  image-based data extraction, default portfolio AI/ML
Lender Partners:  Cloudvirga, ICE Encompass, Optimal Blue, SimpleNexus,
                  nCino, FISERV LoanServ
API Schema:       MISMO 3.4 (primary), Legacy XML (legacy clients -- flagged)
Regulatory Env:   RESPA, TILA, CFPB audit trail requirements
Active Metrics:   47 API clients, 91% instant decisions, 4.2s avg decisioning,
                  3.1-day manual baseline, 2 open compliance events
Conard Contact:   Douglas Conard -- VP, Catalyst Technology & Platform Services
Conard Verdict:   SHIP WITH CONTROLS (as of current dashboard state)
Open Items:       TOR-2206-0178: Missing SettlementChargeType node (RESPA)
                  TOR-2206-0191: Truncated PropertyIdentifier array (MISMO 3.4)
```

---

## Output standards -- v2.0

All artifacts produced by this skill must pass the following checks before delivery.

### Inherited quality gates (v1.0)

- Regulatory context is named (SOX, BCBS 239, OCC, FDIC, or applicable equivalent)
- Platform recommendation includes a "why not the others" clause
- Governance section has named roles (Data Owner, Data Steward, Data Custodian)
- POC scope has measurable success criteria
- Timeline is realistic -- no "2-week full migration" artifacts
- Differentiator language is specific -- "AI-ready" is not a differentiator

### New quality gates added in v2.0 (mortgage/title)

- Conard Layer verdict is on record before any recommendation is delivered
- Schema contract is specified -- MISMO 3.4 full payload vs partial vs legacy XML
- Compliance boundary is named -- RESPA, TILA, or CFPB exposure mapped to specific workflow steps
- Cycle-time delta is quantified -- EXOS decisioning time vs lender's documented manual baseline
- Open governance gap items are listed with remediation status before a SHIP verdict is issued
- API health status is current -- uptime and latency reported per endpoint, not aggregate

---

## Tone and positioning rules -- v2.0

The v1.0 positioning rules remain in force:

1. Data architecture first, AI second. Never lead with AI. Lead with trusted data.
2. Governance is the moat. In regulated industries, governance is not a workstream -- it is the product.
3. The POC is a sale. Treat POC scoping as a close, not a test.
4. Platform agnosticism with a point of view. Never refuse to recommend. Recommend with reasons, caveats, and exit ramps.
5. RFP responses win on specificity. Generic proposals lose. Named client context, named risks, named timelines win.

The following rules are added for v2.0 mortgage/title engagements:

6. A field-level error is a compliance event. Never frame schema exceptions as bugs or technical debt. Frame them as RESPA/TILA exposure points with a remediation cost and a client-trust consequence.
7. Speed is the EXOS value proposition -- own the delta. The cycle-time compression from multi-day manual processing to sub-5-second instant decisions is the primary business case. Every analysis must anchor to that number.
8. The Conard question is the north star. "Can you translate between what a lender's system needs from this API and what the EXOS data platform can produce -- and can you govern the gap between those two things in a regulated environment?" If the answer to that question is not visible in the artifact, the artifact is not done.
9. Lender trust is the product, not the outcome. In title API servicing, lender confidence is not a benefit of good integration work -- it is the deliverable. The dashboard, the governance log, and the compliance posture view all exist to make trust visible and measurable.

---

## Skill metadata

```
Skill name:       regulated-finserv-presales
Version:          2.0
Previous version: 1.0 (general banking / Northstar scenario only)
Author:           Erwin Maurice McDonald
Attribution:      McDonald (2026) | DACR License v2.6
Entity:           Epoch Frameworks LLC | EIN 41-5100223
Location:         Fort Worth, Texas
GitHub:           github.com/emcdo411
Added in v2.0:    Conard Layer gate
                  EXOS / ServiceLink scenario environment
                  Mortgage/title API routing logic
                  Artifacts 9-12 (dashboard, Conard brief, FRED charts, LinkedIn post)
                  6 new quality gates
                  4 new positioning rules
Trigger phrases:  title API, EXOS One, ServiceLink, Catalyst Technology,
                  MISMO 3.4, mortgage servicing, lender API, RESPA/TILA,
                  Conard Layer, ship/no-ship, client trust, instant title,
                  default portfolio, loan modification, lien search API,
                  run Conard gate, run presales brief, build discovery brief,
                  scope a finserv engagement, what would Conard ask
```

---

## Related skills

| Skill | Relationship |
|-------|-------------|
| EBT Framework v2.8 | Bias audit layer for vendor benchmarks cited as objective evidence in RFP responses |
| SEIM v3.1 | M&A and deal architecture intelligence -- applies when the prospect is mid-acquisition |
| AI Adoption Architect v6.5 | Human-side adoption dynamics when the lender organization is resistant to platform change |
| WCIS v5.7 | Geopolitical and regulatory risk context for GCC or international mortgage market engagements |
| Epoch x Salt GTM | Joint capability assessment when Salt Technologies engineering execution is part of the delivery model |
| NMIC AI Gap Analysis | Mortgage insurance vendor positioning -- Enact governance gap finding applies to competitive framing |
| Texas Contract Intelligence v4.0 | SOW and NDA generation for ServiceLink or any regulated finserv engagement |

---

*McDonald (2026) | Epoch Frameworks LLC | DACR License v2.6*
*Regulated FinServ Presales Framework v2.0 -- Conard Layer extension for mortgage fintech and title API client servicing*
