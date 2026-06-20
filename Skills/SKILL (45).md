---
name: regulated-finserv-presales
description: Regulated FinServ Presales and Technical BA framework for enterprise data, AI, mortgage, title, and compliance modernization. Use for discovery, RFP/RFI, architecture, governance, POC, platform selection, Snowflake/Fabric/Databricks/Lakehouse, EXOS/title API, mortgage servicing, lender workflows, client intelligence dashboards, order pipelines, compliance flags, API handoffs, technical BA artifacts, and ship/no-ship readiness verdicts using the Conard Layer. Also covers agentic AI tooling for SOAP/REST lender API integrations -- Postman collection architecture, agent-readable API maps, schema validation gates, automated regression testing, and Claude Code/Claude Skills build patterns for client integration backends -- using the Deepak Layer.
---
# Regulated FinServ Presales Framework
**Converged Data & AI Presales Framework for Regulated Enterprises**
*Snowflake - Microsoft Fabric - Databricks - Lakehouse - Governed AI Readiness*

McDonald / Epoch Frameworks LLC - DACR License v2.6

---
## FRAMEWORK OVERVIEW

This skill produces consulting-grade presales artifacts for regulated enterprise data modernization opportunities - banks, credit unions, insurance carriers, healthcare payors, and any data estate subject to SOX, BCBS 239, GDPR, CCPA, or equivalent regulation.

**Core thesis:** Enterprise AI does not start with models. It starts with governed, accessible, trusted data platforms.

**Fictional reference client:** Northstar Commercial Bank - legacy SQL Server DW, siloed customer/account/transaction data, manual regulatory reporting, Power BI sprawl, no enterprise catalog, evaluating Snowflake / Fabric / Databricks.

---
## ARTIFACTS PRODUCED

This skill generates **eight production-grade presales artifacts**. Reference files contain the full content frameworks.

| # | Artifact | Reference File |
|--|--|--|
| 1 | Executive One-Pager | `references/01-exec-one-pager.md` |
| 2 | Discovery Workshop Guide | `references/02-discovery-guide.md` |
| 3 | Platform Decision Matrix | `references/03-platform-matrix.md` |
| 4 | Reference Architecture Pack | `references/04-architecture-pack.md` |
| 5 | Governance Operating Model | `references/05-governance-model.md` |
| 6 | Sample RFP/RFI Response | `references/06-rfp-response.md` |
| 7 | POC-to-Delivery Handoff | `references/07-poc-handoff.md` |
| 8 | Repeatable Offering Sheet | `references/08-offering-sheet.md` |
| 9 | Agentic API Integration Blueprint | `references/09-agentic-api-blueprint.md` |

---
## CONARD LAYER -- MORTGAGE / TITLE API SHIPPING GATE

The Conard Layer fires before any presales diagnostic, technical BA artifact, dashboard, requirements brief, or delivery recommendation when the prospect, client, or workflow involves:

- EXOS, ServiceLink, title, appraisal, closing, settlement, valuation, lender-partner, or mortgage servicing workflows
- Title API integrations, order intake, fulfillment orchestration, API handshakes, schema mapping, vendor handoff, or exception queues
- RESPA, TILA, CFPB, UDAAP, fair lending, audit trail, model governance, compliance flags, borrower-impacting automation, or field-level data quality issues
- Client intelligence dashboards, order pipeline views, operational readiness views, technical BA requirements, acceptance criteria, or release-readiness decisions

**Governing question:** Does this artifact help Doug Conard, or an equivalent mortgage/title technology executive, decide whether the workflow can safely move from operational signal to client-trust outcome?

The Conard Layer must produce a verdict before the artifact is considered complete:

| Verdict | Meaning | Required Action |
|--|--|--|
| **SHIP** | Business value, data integrity, API readiness, compliance posture, and operational handoff are clear enough to proceed. | State why it ships and what evidence supports release. |
| **SHIP WITH CONTROLS** | Value is clear, but one or more risks require explicit mitigation. | Name the controls, owner, test, and follow-up gate. |
| **HOLD** | The artifact exposes unresolved schema, compliance, operational, or client-trust risk. | Name the blocking issue and the minimum evidence needed to proceed. |
| **NO-SHIP** | The recommendation could mislead clients, break compliance posture, or create borrower/lender harm. | Stop the recommendation and propose a safer diagnostic step. |

### Conard Gates

Run these gates in order:

1. **Client Trust Gate** -- Does the artifact make the lender/title client more confident in order status, decision timing, and exception handling?
2. **Compliance Gate** -- Are RESPA, TILA, CFPB audit trail, field-level data integrity, model governance, and borrower-impacting controls named where relevant?
3. **Cycle-Time Gate** -- Does the artifact quantify how the workflow compresses days to seconds, or explain why it cannot yet be quantified?
4. **Failure-Mode Gate** -- Does the artifact show happy path and broken path: instant decision, schema mismatch, pending API handshake, compliance hold, manual review, or escalation?
5. **BA Acceptance Gate** -- Are requirements, data fields, API dependencies, exception states, success criteria, and release-readiness evidence concrete enough for delivery?

### Mandatory Conard Output Block

When this layer fires, end with:

```
Conard Layer Verdict: [SHIP / SHIP WITH CONTROLS / HOLD / NO-SHIP]
Why: [one executive sentence]
Evidence: [3-5 bullets tied to client trust, compliance, cycle time, failure modes, BA acceptance]
Open Risks: [bullets or "None material"]
Next Gate: [what must happen before release, demo, client delivery, or proposal submission]
```

If the user asks for a technical BA artifact, never omit this verdict.

---
## DEEPAK LAYER -- AGENTIC API INTEGRATION GATE

The Deepak Layer fires before any artifact involving agentic AI tooling for SOAP/REST lender API integrations, Postman collection architecture, schema validation design, regression test automation, or Claude Code/Claude Skills build patterns for client integration backends.

Named for Deepak, Technical Lead at ServiceLink. Where the Conard Layer governs whether a workflow is safe to move from operational signal to client-trust outcome, the Deepak Layer governs whether an agentic build is sound enough to move from engineering concept to backend implementation. Run both layers when an artifact touches lender data AND the agent/tooling layer that processes it.

**Governing question:** Does this artifact give Deepak, or an equivalent technical lead, enough to evaluate whether an agentic AI component is safe to build against live lender APIs?

### The Four Pillars

**1. Postman Collection as Agent Map**
A Postman collection is the structured, machine-readable inventory of every lender API call in scope: endpoint, method, auth, request shape, expected response, documented error states. This collection is the reference surface an agent works from -- it should never infer endpoint behavior, it should read it from a maintained collection. For SOAP, this means the WSDL contract is mirrored into the collection alongside REST entries so the agent has one consistent map across both protocols.

**2. Schema Validation Gate**
Before any lender payload (SOAP XML against WSDL/XSD, REST JSON against schema) reaches downstream pipeline logic, an agentic skill validates it against the documented schema from the Postman collection. Malformed or out-of-spec data is caught at the API boundary, not after it has already propagated into RESPA-relevant fee, disclosure, or timing fields. This is where the Deepak Layer and Conard Layer intersect directly -- a schema failure at this gate is a compliance event, not just a data quality ticket.

**3. Automated Regression Testing**
Postman test suites, chained and scheduled, continuously verify lender integrations still behave as contracted. An agentic layer on top doesn't just run the suite -- it flags drift (a lender silently changing a field type, a new required field, a deprecated endpoint) before that drift becomes a production incident. Regression coverage should be scoped explicitly: which endpoints, which lenders, what cadence, what counts as a pass.

**4. Documented, Repeatable Asset (Tribal Knowledge to Living Documentation)**
Manual API exploration -- the "I think that lender's endpoint behaves this way" knowledge sitting in someone's head or a stale ticket -- gets converted into a maintained Postman collection plus agent skill definitions. This is the artifact a technical BA owns: not the code, but the documented, versioned map of what the integration does, validated continuously, that both humans and agents read from the same source of truth.

### Deepak Gates

Run these gates in order:

1. **Map Completeness Gate** -- Does the Postman collection (or the plan to build one) cover every in-scope lender endpoint, both SOAP and REST, with documented auth, request, and response shapes?
2. **Schema Integrity Gate** -- Is there an explicit validation step before lender data reaches downstream logic, and does a validation failure route to a defined exception path rather than silently passing through?
3. **Regression Coverage Gate** -- Is test scope named (which endpoints, which lenders, what cadence) with a clear definition of drift and a clear owner for triage when drift is flagged?
4. **Build Boundary Gate** -- Is it explicit what the agent is authorized to do autonomously (read, validate, flag) versus what requires human review before acting (schema changes, new endpoint onboarding, anything touching live lender writes)?
5. **Handoff Gate** -- Can this artifact be handed to Deepak or another engineer to implement without requiring a follow-up meeting to clarify scope?

### Mandatory Deepak Output Block

When this layer fires, end with:

```
Deepak Layer Verdict: [SHIP / SHIP WITH CONTROLS / HOLD / NO-SHIP]
Why: [one technical-lead-facing sentence]
Evidence: [3-5 bullets tied to map completeness, schema integrity, regression coverage, build boundary, handoff readiness]
Open Risks: [bullets or "None material"]
Next Gate: [what must happen before this moves from concept to backend implementation]
```

If the user asks for an agentic API tooling artifact, never omit this verdict.

---
## ROUTING LOGIC

**Mortgage/title/API/EXOS/technical BA readiness** -> Fire the Conard Layer first, then route to the closest artifact.

**Agentic AI tooling for lender APIs (Postman, schema validation, regression automation, Claude Code/Skills builds)** -> Fire the Deepak Layer first, then route to `09-agentic-api-blueprint.md`. If the artifact also touches client-facing lender data or compliance posture, fire the Conard Layer as well.

Read the request and route to the correct reference file. For compound requests (e.g., "build me the full kit"), produce all nine artifacts in sequence.

**Discovery conversation in progress** - `02-discovery-guide.md`
**Platform recommendation needed** - `03-platform-matrix.md`
**Architecture diagram requested** - `04-architecture-pack.md`
**Governance/compliance question** - `05-governance-model.md`
**RFP/RFI or proposal writing** - `06-rfp-response.md`
**POC scoping or delivery planning** - `07-poc-handoff.md`
**Packaging as a service offering** - `08-offering-sheet.md`
**Executive briefing or one-pager** - `01-exec-one-pager.md`
**Agentic API tooling / Postman / agent skill build for lender integrations** - `09-agentic-api-blueprint.md`
**Full kit / build it all** - Read all reference files, produce full artifact stack

---
## OUTPUT STANDARDS

- All outputs use consulting IP tone: precise, evidence-grounded, no filler
- Financial services terminology throughout: BCBS 239, SOX, data lineage, reconciliation controls, model risk, regulatory capital
- Platform recommendations always include a **rationale layer** - not just "Snowflake wins" but why given the client's constraint profile
- Governance section always ties to **named regulatory requirements**, not generic best practices
- POC scope always includes **success criteria** - observable, measurable, signed off before work begins
- Diagrams described in structured text notation (Mermaid-compatible where applicable) unless HTML rendering is available

---
## NORTHSTAR COMMERCIAL BANK - SCENARIO BRIEF

Use this profile when the user wants to run a demo, simulate a discovery call, or illustrate any artifact.

```
Client:           Northstar Commercial Bank
Segment:          Mid-market commercial bank, $8B AUM
Data Platform:    Legacy SQL Server 2016 DW (on-prem), ETL via SSIS
Pain Points:
  - Siloed customer, account, transaction, and risk data
  - Regulatory reporting (DFAST, Call Report) is manual/Excel-dependent
  - 47 Power BI workspaces with no enterprise semantic layer
  - No data catalog, no lineage tracking
  - Failed MDM initiative (2021)
AI Interest:      Fraud analytics, customer 360, churn prediction
Platform Eval:    Snowflake (preferred by data team), Fabric (IT mandate),
                  Databricks (data science team request)
Constraints:      3-year Azure EA, 18-month migration window, $2.1M budget
Regulatory Env:   OCC supervision, SOX reporting, BCBS 239 alignment in progress
```

---
## QUALITY GATES

Before delivering any artifact, verify:

- [ ] If mortgage/title/API/EXOS/technical BA context is present, Conard Layer verdict is included
- [ ] If agentic AI tooling for lender APIs is present, Deepak Layer verdict is included
- [ ] Regulatory context named (SOX, BCBS 239, OCC, FDIC, or applicable)
- [ ] Platform recommendation includes a "why not the others" clause
- [ ] Governance section has named roles (Data Owner, Data Steward, Data Custodian)
- [ ] POC scope has measurable success criteria
- [ ] Timeline is realistic (no "2-week full migration" artifacts)
- [ ] Differentiator language is specific, not generic ("AI-ready" is not a differentiator)
- [ ] Ship/no-ship logic is explicit when the output may influence release, demo, proposal, client delivery, or operational handoff

---
## TONE AND POSITIONING RULES

1. **Data architecture first, AI second.** Never lead with AI. Lead with trusted data.
2. **Governance is the moat.** In regulated industries, governance isn't a workstream - it's the product.
3. **The POC is a sale.** Treat POC scoping as a close, not a test.
4. **Platform agnosticism with a point of view.** Never refuse to recommend. Recommend with reasons, caveats, and exit ramps.
5. **RFP responses win on specificity.** Generic proposals lose. Named client context, named risks, named timelines win.
