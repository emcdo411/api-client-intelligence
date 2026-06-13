# ServiceLink API Integration
## Workflow Discovery Guide & Business Requirements Document

![Document Type](https://img.shields.io/badge/Document%20Type-BA%20Discovery%20%2B%20BRD-1B2E5E?style=for-the-badge)
![Conard Layer](https://img.shields.io/badge/Conard%20Layer-ACTIVE%20--%20Ship%20Gate%20Enforced-E67E22?style=for-the-badge)
![Framework](https://img.shields.io/badge/Framework-FinServ%20Presales%20v2.6-8E9FD5?style=for-the-badge)

![Author](https://img.shields.io/badge/Author-Erwin%20Maurice%20McDonald-2D3436?style=flat-square)
![Org](https://img.shields.io/badge/Org-Epoch%20Frameworks%20LLC-8E9FD5?style=flat-square)
![License](https://img.shields.io/badge/License-DACR%20v2.6-2D3436?style=flat-square)
![Confidential](https://img.shields.io/badge/Classification-CONFIDENTIAL%20--%20BA%20Working%20Document-B91C1C?style=flat-square)

---

> **How to use this document.** This is a reusable BA toolkit for every ServiceLink API integration engagement. Section 1 is the master prompt — run it once at session start. Section 2 is the discovery guide — walk through it phase by phase with the client. Section 3 is the Conard Layer gate — complete it after discovery. Section 4 is the BRD — fill it from discovery answers. Section 5 is the manager and dev summary — send it after the Conard verdict is issued. Do not skip sections. Do not schedule API testing before the Conard verdict is SHIP or SHIP WITH CONTROLS.

---

## Section 1 — Master Prompt

![Section](https://img.shields.io/badge/Section%201-Master%20Prompt-1B2E5E?style=flat-square)

Run this prompt verbatim at the start of every new client discovery session. It activates the discovery workflow, triggers the Conard Layer, and ensures your BRD output is development-ready before the first API test is scheduled.

---

> **COPY THIS PROMPT — Run at session start for every new ServiceLink API client**

```
You are acting as a 10x Senior Business Analyst embedded in a regulated mortgage
technology environment. Your framework is the Regulated FinServ Presales Framework
(Epoch Frameworks LLC, McDonald 2026, DACR License v2.6). The Conard Layer is active
for this entire session.

My client is integrating with ServiceLink's API suite, which includes EXOS Close,
title order intake, appraisal management, and default services workflows. I am the BA.
I need to:

1. Run a structured workflow discovery session to surface all integration requirements
2. Map every discovery answer to a concrete BRD field that development can act on
3. Run the Conard Layer gates (Client Trust, Compliance, Cycle-Time, Failure-Mode,
   BA Acceptance) and produce a SHIP / SHIP WITH CONTROLS / HOLD / NO-SHIP verdict
   before any API test is scheduled
4. Generate a final BRD summary formatted for my dev team lead and my manager
5. Flag any compliance, schema, or data-quality risk before it blocks the
   integration sprint

Start by asking me for the client name and the primary ServiceLink product line they
are integrating with. Then walk me through the discovery questions one phase at a time.
Do not skip a phase. After all discovery questions are answered, run the Conard Layer
and produce the BRD output block.

Conard Layer rule: the artifact does not ship -- meaning we do not schedule API
testing -- until the Conard verdict is SHIP or SHIP WITH CONTROLS with named controls,
owners, and a follow-up gate documented.
```

---

After pasting the master prompt, answer the intake question (client name and product line), then proceed through Sections 2 through 5 of this document in sequence.

---

## Section 2 — Step-by-Step Discovery Guide

![Section](https://img.shields.io/badge/Section%202-Discovery%20Guide-1B2E5E?style=flat-square)

Work through each phase in order. Do not skip ahead. Every answer feeds a field in the BRD (Section 4). The "Dev Field" column shows exactly which BRD row each answer populates.

---

### Phase A — Client Environment and Org Context

![Phase](https://img.shields.io/badge/Phase%20A-Org%20%26%20Environment-1B2E5E?style=flat-square)

> **Purpose.** Understand who owns what, who approves what, and what is already built.

| # | Discovery Question | Why It Matters to Dev | BRD Field |
|---|---|---|---|
| A1 | Who is the technical lead on the client side for this integration? | Primary escalation path for schema and auth issues | BRD-ORG-01: Client Technical POC |
| A2 | Who approves the test environment credentials on the client side? | Credential provisioning delays kill sprint timelines | BRD-ORG-02: Credential Approver |
| A3 | What is the client's current loan origination or servicing platform (LOS / MSP)? | Determines schema translation layer complexity | BRD-ENV-01: Client LOS / MSP |
| A4 | Is the client on-premise, cloud, or hybrid? Which cloud provider? | Affects API call routing, firewall rules, and latency SLA | BRD-ENV-02: Infrastructure Type |
| A5 | What is the client's target go-live date for API testing and production? | Back-dates the sprint schedule — cannot set milestones without this | BRD-SCHED-01: Target Go-Live |
| A6 | Has this client integrated with any other title or closing API before? | Prior integrations reveal known schema patterns and failure points | BRD-ENV-03: Prior API Integrations |

---

### Phase B — ServiceLink Product Line and API Scope

![Phase](https://img.shields.io/badge/Phase%20B-API%20Scope%20%26%20Auth-1B2E5E?style=flat-square)

> **Purpose.** Lock down exactly which ServiceLink APIs are in scope and prevent scope creep post-kickoff.

| # | Discovery Question | Why It Matters to Dev | BRD Field |
|---|---|---|---|
| B1 | Which ServiceLink product is in scope: EXOS Close, title order intake, appraisal / AMC, or default services? | Each product has a distinct API schema, auth pattern, and compliance layer | BRD-SCOPE-01: Product Line |
| B2 | Is the integration order initiation only, status polling only, or both (full round-trip)? | Read-only vs. write integrations have completely different auth and data-quality controls | BRD-SCOPE-02: Integration Mode |
| B3 | What ServiceLink environments are needed: sandbox, UAT, and production? | Each environment may require a separate credential set and IP whitelist | BRD-SCOPE-03: Environments |
| B4 | What API version is the client being onboarded to? Is there a deprecation timeline for prior versions? | Dev must target the correct endpoint version to avoid rework in sprint 2 | BRD-API-01: API Version |
| B5 | What authentication pattern does this API endpoint use: API key, OAuth 2.0, mTLS, or SAML? | Auth pattern drives the entire credential provisioning and secrets management design | BRD-API-02: Auth Pattern |
| B6 | Are there IP whitelist requirements on the ServiceLink side? | Missing whitelist entries block all API calls — a common day-one blocker | BRD-API-03: IP Whitelist |
| B7 | What is the rate limit policy (calls per minute / hour / day)? | Informs client retry logic and burst handling design | BRD-API-04: Rate Limits |

---

### Phase C — Data Fields, Schema, and Mapping

![Phase](https://img.shields.io/badge/Phase%20C-Schema%20%26%20Field%20Mapping-1B2E5E?style=flat-square)
![Risk](https://img.shields.io/badge/Highest%20Test%20Failure%20Risk-Unmapped%20Fields-B91C1C?style=flat-square)

> **Purpose.** Every field the client sends must map to a ServiceLink-accepted value. Unmapped fields are the single largest cause of integration test failures.

| # | Discovery Question | Why It Matters to Dev | BRD Field |
|---|---|---|---|
| C1 | What is the primary order object the client is sending — loan number, property address, borrower data, or all three? | Determines the required field set for the order intake payload | BRD-SCHEMA-01: Primary Order Object |
| C2 | Does the client's LOS use MISMO 3.4, MISMO 2.x, or a proprietary schema? | MISMO version determines whether a translation layer is needed | BRD-SCHEMA-02: Source Schema Standard |
| C3 | Are there any custom or non-standard fields the client currently sends to other vendors that they expect ServiceLink to accept? | Custom fields not in the ServiceLink schema must be handled as pass-through, dropped, or errored — dev must know which | BRD-SCHEMA-03: Custom Fields |
| C4 | What are the mandatory fields in the ServiceLink order payload for this product line? Walk through them now. | Every mandatory field gap is a guaranteed test failure | BRD-SCHEMA-04: Mandatory Field Map |
| C5 | How does the client handle null or missing values for required fields? | Null handling strategy must be defined before the first test call | BRD-SCHEMA-05: Null Handling Strategy |
| C6 | What status codes does ServiceLink return and which ones must trigger a client-side action? | Status code mapping is required for the client UI, reporting, and exception workflow | BRD-SCHEMA-06: Status Code Map |

---

### Phase D — Compliance, Audit, and Regulatory Posture

![Phase](https://img.shields.io/badge/Phase%20D-Compliance%20%26%20Regulatory-1B2E5E?style=flat-square)
![Rule](https://img.shields.io/badge/Rule-Gather%20compliance%20answers%20BEFORE%20testing%2C%20not%20after-B91C1C?style=flat-square)

> **Purpose.** The Conard Layer will not SHIP any integration that cannot demonstrate RESPA, TILA, and audit trail readiness. Gather this now, not after testing.

| # | Discovery Question | Why It Matters to Dev | BRD Field |
|---|---|---|---|
| D1 | Is this client subject to RESPA Section 8 referral fee restrictions that affect how the integration is structured? | RESPA scope determines permissible data-sharing architecture and fee disclosure logic | BRD-COMP-01: RESPA Scope |
| D2 | What is the audit trail requirement for orders placed through the API — how long must records be retained and in what format? | Audit log design must be in the BRD before dev writes the API call handler | BRD-COMP-02: Audit Retention |
| D3 | Does the client have a model risk or AI governance requirement that applies to any ServiceLink automated decision in this workflow? | Freddie Mac March 2026 governance requirements may apply to automated eligibility decisions via EXOS | BRD-COMP-03: Model Governance |
| D4 | How is PII (borrower name, SSN, address) transmitted and protected in the API payload? | PII in transit requires TLS 1.2+ minimum, and field-level encryption may be required | BRD-COMP-04: PII Handling |
| D5 | Is there a CFPB or state regulatory requirement that affects turnaround time SLA for this product type? | SLA violations in title or appraisal workflows can create borrower harm — must be in the BRD | BRD-COMP-05: SLA Regulatory Floor |

---

### Phase E — Error Handling, Failure Modes, and Exception Workflow

![Phase](https://img.shields.io/badge/Phase%20E-Error%20Handling%20%26%20Failure%20Modes-1B2E5E?style=flat-square)
![Rule](https://img.shields.io/badge/Rule-Document%20the%20broken%20path%20as%20clearly%20as%20the%20happy%20path-E67E22?style=flat-square)

> **Purpose.** Dev needs the broken path as much as the happy path. Every failure mode not documented here becomes an incident ticket.

| # | Discovery Question | Why It Matters to Dev | BRD Field |
|---|---|---|---|
| E1 | What happens when ServiceLink returns a 4xx error — does the client system retry, alert, queue, or fail? | Retry logic must be designed before test to prevent duplicate order submission | BRD-ERR-01: 4xx Handling |
| E2 | What is the maximum acceptable timeout for an API response before the client workflow must escalate to manual? | Timeout threshold determines the async vs. sync architecture decision | BRD-ERR-02: Timeout Threshold |
| E3 | How are schema validation errors surfaced to the client-side user — API response only, UI alert, email, or all three? | Error surfacing design affects both backend and frontend sprint scope | BRD-ERR-03: Schema Error Surfacing |
| E4 | Who on the client side receives compliance-hold notifications when ServiceLink puts an order on hold? | Notification routing is a BRD requirement — unnamed recipient = a compliance gap | BRD-ERR-04: Compliance Hold Recipient |
| E5 | What is the rollback or cancellation process if an order is submitted in error through the API? | Cancellation logic must be documented and tested before production — RESPA requires it | BRD-ERR-05: Cancellation / Rollback |
| E6 | Is there a manual override workflow for orders that fail automated processing? | Manual override path must be in the BRD or dev has no path for exception fulfillment | BRD-ERR-06: Manual Override Path |

---

### Phase F — Testing, Acceptance, and Go-Live Readiness

![Phase](https://img.shields.io/badge/Phase%20F-Testing%20%26%20Go--Live%20Readiness-1B2E5E?style=flat-square)
![Rule](https://img.shields.io/badge/Rule-Define%20acceptance%20criteria%20before%20sprint%201%2C%20not%20at%20the%20end%20of%20sprint%202-1D9E75?style=flat-square)

> **Purpose.** The Conard BA Acceptance Gate will not pass without confirmed success criteria. Define these before sprint 1, not at the end of sprint 2.

| # | Discovery Question | Why It Matters to Dev | BRD Field |
|---|---|---|---|
| F1 | What constitutes a successful sandbox test? Define in observable, measurable terms — not "it works." | Acceptance criteria signed off before sprint 1 prevents end-of-sprint disagreement | BRD-TEST-01: Sandbox Success Criteria |
| F2 | Who on the dev team has confirmed receipt of ServiceLink sandbox credentials? | Unconfirmed credentials are a day-one blocker — must be named in the BRD | BRD-TEST-02: Credential Confirmation |
| F3 | What is the agreed test data set — real data, synthetic data, or ServiceLink-provided test cases? | Test data type determines what the dev team can legally submit to sandbox | BRD-TEST-03: Test Data Type |
| F4 | What is the minimum test case coverage required before promoting to UAT? Name the scenarios. | Coverage gate prevents premature UAT promotion — a common source of regression | BRD-TEST-04: UAT Promotion Gate |
| F5 | Who signs off on UAT and what is their response SLA for defect review? | Unsigned UAT gates stall go-live — owner and SLA must be in the BRD | BRD-TEST-05: UAT Sign-Off Owner |
| F6 | Is there a production parallel-run period before full cutover? | Parallel-run period affects dev resource planning and monitoring setup | BRD-TEST-06: Parallel Run Period |

---

## Section 3 — Conard Layer Verdict

![Section](https://img.shields.io/badge/Section%203-Conard%20Layer%20Gate-1B2E5E?style=flat-square)
![Rule](https://img.shields.io/badge/Rule-Complete%20this%20AFTER%20all%20Phase%20A--F%20questions%20are%20answered-E67E22?style=flat-square)

Run this gate after all Phase A through F discovery questions are answered. The Conard Layer must produce a SHIP, SHIP WITH CONTROLS, HOLD, or NO-SHIP verdict before any API testing is scheduled. This section is the go / no-go gate for dev.

> **Governing Question:** Does this artifact help a mortgage/title technology executive decide whether the workflow can safely move from operational signal to client-trust outcome?

### Verdict Definitions

| Verdict | Meaning | Required Action |
|---|---|---|
| **SHIP** | Business value, data integrity, API readiness, compliance posture, and operational handoff are all clear | State why it ships and what evidence supports release |
| **SHIP WITH CONTROLS** | Value is clear, but one or more risks require explicit mitigation | Name the controls, owner, test, and follow-up gate |
| **HOLD** | Unresolved schema, compliance, operational, or client-trust risk | Name the blocking issue and minimum evidence needed to proceed |
| **NO-SHIP** | The recommendation could mislead clients, break compliance, or create borrower or lender harm | Stop the recommendation and propose a safer diagnostic step |

---

### Gate Results Table

Complete one row per gate. Status key: **PASS** / **CONDITIONAL** / **OPEN**. An OPEN gate blocks the verdict. A CONDITIONAL gate requires a named control and owner.

| Gate | Status | Evidence / Control Required |
|---|---|---|
| 1 — Client Trust Gate | `CONDITIONAL` | Discovery Phase A-B complete. Client-side POC named. Order status, decision timing, and exception handling must be documented in BRD-SCHEMA-06 before PASS. |
| 2 — Compliance Gate | `CONDITIONAL` | RESPA scope (BRD-COMP-01) and PII handling (BRD-COMP-04) must be answered. Audit retention (BRD-COMP-02) and SLA floor (BRD-COMP-05) required. Model governance (BRD-COMP-03) required if EXOS automated eligibility is in scope. |
| 3 — Cycle-Time Gate | `OPEN` | Client must confirm current order placement time (baseline) and target integration time. If cycle-time compression cannot be quantified, document why and name the owner who will provide the metric before UAT. |
| 4 — Failure-Mode Gate | `OPEN` | Phase E (Error Handling) must be complete. 4xx handling, timeout threshold, compliance hold recipient, and cancellation / rollback must all be documented. OPEN until all six Phase E fields are populated in the BRD. |
| 5 — BA Acceptance Gate | `OPEN` | Phase F (Testing) must be complete. Sandbox success criteria, credential confirmation, and UAT sign-off owner must be named. OPEN until all six Phase F fields are populated and signed. |

---

### Conard Verdict Block

Complete this block after all five gates are evaluated. Copy to the BRD cover and email to dev lead and manager.

```
Conard Layer Verdict:  [ SHIP ]  [ SHIP WITH CONTROLS ]  [ HOLD ]  [ NO-SHIP ]

Why: ___________________________________________________________________

Evidence:
  - Client Trust:    ___________________________________________________
  - Compliance:      ___________________________________________________
  - Cycle-Time:      ___________________________________________________
  - Failure-Mode:    ___________________________________________________
  - BA Acceptance:   ___________________________________________________

Open Risks:   __________________________________________________________

Next Gate:    __________________________________________________________

Verdict Owner: ________________________   Date: ________________________
```

---

## Section 4 — Business Requirements Document (BRD)

![Section](https://img.shields.io/badge/Section%204-BRD-1B2E5E?style=flat-square)
![Audience](https://img.shields.io/badge/Audience-Dev%20Team%20Lead%20%7C%20Manager-534AB7?style=flat-square)

This section is the deliverable for your dev team lead and manager. Every row maps to a Phase A-F discovery answer. "Dev Action" tells the developer exactly what to build or confirm for that field.

---

### 4.1 — Org and Environment

| BRD Field | Format | Required | Dev Action / Notes |
|---|---|---|---|
| BRD-ORG-01: Client Technical POC | Name + email + phone | MANDATORY | Primary escalation for schema, auth, and environment issues. Add to Jira / ADO contact field. |
| BRD-ORG-02: Credential Approver | Name + email + SLA | MANDATORY | Dev must confirm credential receipt from this person before sprint 1 kickoff. |
| BRD-ENV-01: Client LOS / MSP | Platform name + version | MANDATORY | Determines schema translation layer. If ICE Encompass or Empower, confirm MISMO 3.4 compatibility. |
| BRD-ENV-02: Infrastructure Type | Cloud / Hybrid / On-prem + Provider | MANDATORY | Affects firewall rules, VPN requirements, and API gateway routing. |
| BRD-ENV-03: Prior API Integrations | Vendor name + product + outcome | RECOMMENDED | Prior failure patterns are early warning for this integration. Document and share with dev. |
| BRD-SCHED-01: Target Go-Live | Date + milestone dependency | MANDATORY | Back-dates the sprint schedule. All testing gates must be confirmed achievable against this date. |

---

### 4.2 — API Scope and Authentication

| BRD Field | Format | Required | Dev Action / Notes |
|---|---|---|---|
| BRD-SCOPE-01: Product Line | EXOS Close / Title / AMC / Default | MANDATORY | Determines which API endpoint family and schema package to provision. |
| BRD-SCOPE-02: Integration Mode | Order initiation / Status polling / Full round-trip | MANDATORY | Full round-trip requires both write and read auth scopes. Confirm with ServiceLink provisioning. |
| BRD-SCOPE-03: Environments | Sandbox / UAT / Production | MANDATORY | Each environment may require a separate credential set. Confirm with ServiceLink API ops. |
| BRD-API-01: API Version | Version string + deprecation date | MANDATORY | Dev must target the confirmed version. Do not assume latest — confirm with ServiceLink. |
| BRD-API-02: Auth Pattern | API Key / OAuth 2.0 / mTLS / SAML | MANDATORY | Drives secrets management design. mTLS requires cert provisioning lead time — plan accordingly. |
| BRD-API-03: IP Whitelist | IP range(s) + submit-to contact | MANDATORY | Submit whitelist request before sprint 1 or all API calls will fail. Name the ServiceLink contact. |
| BRD-API-04: Rate Limits | Calls per minute / hour / day | MANDATORY | Dev must design retry-with-backoff logic against this limit. Document in API call handler spec. |

---

### 4.3 — Schema and Field Mapping

| BRD Field | Format | Required | Dev Action / Notes |
|---|---|---|---|
| BRD-SCHEMA-01: Primary Order Object | Field list from client LOS | MANDATORY | Attach the client LOS field export. Dev maps each field to the ServiceLink payload spec. |
| BRD-SCHEMA-02: Source Schema Standard | MISMO 3.4 / 2.x / Proprietary | MANDATORY | Non-MISMO schemas require a translation layer. Budget 1-2 sprints if translation is needed. |
| BRD-SCHEMA-03: Custom Fields | Field name + intended value + handling decision | MANDATORY | Custom fields not in ServiceLink schema must be designated as pass-through, dropped, or errored. |
| BRD-SCHEMA-04: Mandatory Field Map | ServiceLink required field list + client source field | MANDATORY | Attach the ServiceLink mandatory field spec. Map each required field to its client source. Flag any gaps. |
| BRD-SCHEMA-05: Null Handling Strategy | Per-field null policy | MANDATORY | Dev must know: does a null required field return a 4xx or silently drop? Test both. |
| BRD-SCHEMA-06: Status Code Map | ServiceLink status code + client action required | MANDATORY | Every ServiceLink status code must have a named client-side action. No unmapped codes at go-live. |

---

### 4.4 — Compliance and Regulatory

![Compliance Warning](https://img.shields.io/badge/Warning-Legal%20sign--off%20required%20before%20sprint%201-B91C1C?style=flat-square)

| BRD Field | Format | Required | Dev Action / Notes |
|---|---|---|---|
| BRD-COMP-01: RESPA Scope | In-scope / Out-of-scope + rationale | MANDATORY | RESPA Section 8 applies to most title referral workflows. Legal must confirm scope before sprint 1. |
| BRD-COMP-02: Audit Retention | Retention period + format + storage location | MANDATORY | Audit logs must be immutable. Dev must confirm log architecture meets retention requirement. |
| BRD-COMP-03: Model Governance | Applicable / Not applicable + framework name | CONDITIONAL | Required if EXOS automated eligibility decision is in scope. Freddie Mac March 2026 requirements apply. |
| BRD-COMP-04: PII Handling | Fields + encryption standard + transit protocol | MANDATORY | TLS 1.2+ minimum. Field-level encryption required if SSN or full DOB is in payload. |
| BRD-COMP-05: SLA Regulatory Floor | Product type + applicable regulation + turnaround requirement | MANDATORY | TILA, RESPA, and state law may impose turnaround SLAs. Dev must be aware of compliance ceiling. |

---

### 4.5 — Error Handling and Failure Modes

| BRD Field | Format | Required | Dev Action / Notes |
|---|---|---|---|
| BRD-ERR-01: 4xx Handling | Retry / Alert / Queue / Fail + retry interval | MANDATORY | Retry logic must include exponential backoff. Max retry count must be defined and tested. |
| BRD-ERR-02: Timeout Threshold | Seconds + escalation action | MANDATORY | Determines async vs. sync architecture. Sub-30-second threshold requires async with webhook callback. |
| BRD-ERR-03: Schema Error Surfacing | API response / UI alert / Email / All | MANDATORY | Multi-channel error surfacing affects both backend and frontend sprint scope. Confirm before design. |
| BRD-ERR-04: Compliance Hold Recipient | Name + role + notification channel | MANDATORY | Unnamed recipient is a compliance gap. Must be a named person, not a generic queue. |
| BRD-ERR-05: Cancellation / Rollback | Cancel endpoint + conditions + confirmation mechanism | MANDATORY | RESPA requires a documented cancellation path. Dev must test cancellation before go-live. |
| BRD-ERR-06: Manual Override Path | Trigger conditions + manual workflow + owner | MANDATORY | Automated failure without a manual path creates a stuck order. Must be documented and tested. |

---

### 4.6 — Testing and Acceptance Criteria

| BRD Field | Format | Required | Dev Action / Notes |
|---|---|---|---|
| BRD-TEST-01: Sandbox Success Criteria | Observable outcomes + pass/fail threshold | MANDATORY | Must be signed by both BA and dev lead before sprint 1. "It works" is not a criterion. |
| BRD-TEST-02: Credential Confirmation | Name + confirmation date + environment | MANDATORY | Dev lead must sign confirmation. Unconfirmed credentials block sprint 1 start. |
| BRD-TEST-03: Test Data Type | Real / Synthetic / ServiceLink-provided | MANDATORY | Synthetic data must cover all mandatory fields and at least two error scenarios. |
| BRD-TEST-04: UAT Promotion Gate | Minimum test case list + coverage % | MANDATORY | Coverage gate prevents premature UAT promotion. Minimum: happy path + 3 error scenarios. |
| BRD-TEST-05: UAT Sign-Off Owner | Name + role + response SLA | MANDATORY | Named owner required. SLA must be documented or UAT stage has no completion gate. |
| BRD-TEST-06: Parallel Run Period | Duration + monitoring owner + cutover trigger | CONDITIONAL | Required for lenders with high transaction volume. Parallel run period must be in sprint plan. |

---

## Section 5 — Manager and Dev Team Summary

![Section](https://img.shields.io/badge/Section%205-Manager%20%26%20Dev%20Summary-1B2E5E?style=flat-square)
![Audience](https://img.shields.io/badge/Audience-Dev%20Team%20Lead%20%7C%20Reporting%20Manager-534AB7?style=flat-square)

Populate this section after Sections 2 and 3 are complete. This is the version you send to your manager and the dev team lead. It answers three questions: what are we building, are we ready to test, and what is blocking us.

---

**Client:** ___________________________________ **ServiceLink Product:** ___________________________________

**Target Go-Live:** ___________________________ **Conard Verdict:** `[ SHIP ]` `[ SHIP WITH CONTROLS ]` `[ HOLD ]` `[ NO-SHIP ]`

---

### What We Are Building

*Describe the integration scope in one paragraph — product line, integration mode, environments, and what the client will be able to do when the integration is live. Write as if your manager has not read the BRD.*

_______________________________________________________________________________

_______________________________________________________________________________

_______________________________________________________________________________

---

### Are We Ready to Schedule API Testing?

> The answer is determined by the Conard Verdict above. **SHIP or SHIP WITH CONTROLS** = schedule testing. **HOLD or NO-SHIP** = do not schedule until blocking items are resolved.

### Open Blockers

*Must be resolved before testing is scheduled.*

| # | Blocker Description | Owner | Target Resolution Date |
|---|---|---|---|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |

---

### Next Actions

1. BA completes all OPEN BRD fields and returns to manager for review
2. Dev lead confirms sandbox credential receipt (BRD-TEST-02) in writing
3. Conard verdict is re-run after all OPEN gates are resolved
4. API testing is scheduled only after SHIP or SHIP WITH CONTROLS verdict is signed
5. Controls and open risks from the Conard block are added to the sprint backlog as acceptance criteria

---

## Document Provenance

![Framework](https://img.shields.io/badge/Frameworks-Regulated%20FinServ%20Presales%20%7C%20Conard%20Layer%20%7C%20McDonald%20(2026)-8E9FD5?style=flat-square)
![IP](https://img.shields.io/badge/IP-Epoch%20Frameworks%20LLC%20%7C%20DACR%20License%20v2.6-2D3436?style=flat-square)
![Companion](https://img.shields.io/badge/Companion%20Frameworks-NorthStar%20v4.1%20%7C%20MOC%20v4.9-8E9FD5?style=flat-square)
![Sources](https://img.shields.io/badge/Regulatory%20Anchors-RESPA%20%7C%20TILA%20%7C%20CFPB%20%7C%20Freddie%20Mac%20March%202026-2D3436?style=flat-square)

**Frameworks applied in this document:**
- Regulated FinServ Presales Framework — Conard Layer mortgage / title API shipping gate
- NorthStar Intelligence News v4.1 — source credibility and claim verification
- Marketing Ops Catalyst v4.9 (MOC) — AI adoption diagnosis and AWSS benchmarking

**McDonald, E.M. (2026).** ServiceLink API Integration — Workflow Discovery Guide and Business Requirements Document. Epoch Frameworks LLC. DACR License v2.6. Fort Worth, TX.

---

*This document is a BA working artifact. It does not constitute legal, compliance, or financial advice. Compliance determinations must be reviewed by qualified legal counsel before sprint execution. No API testing should be scheduled before a Conard Layer SHIP or SHIP WITH CONTROLS verdict is issued and documented.*
