# Cornerstone Lending Partners — EXOS Close API Integration
## Discovery Session Output & Populated BRD

![Session Type](https://img.shields.io/badge/Session%20Type-BA%20Workflow%20Discovery-2D3436?style=flat-square)
![Framework](https://img.shields.io/badge/Framework-Regulated%20FinServ%20Presales%20v2.6-8E9FD5?style=flat-square)
![Conard Layer](https://img.shields.io/badge/Conard%20Layer-ACTIVE-1B2E5E?style=flat-square)
![Product](https://img.shields.io/badge/ServiceLink%20Product-EXOS%20Close-534AB7?style=flat-square)
![Date](https://img.shields.io/badge/Session%20Date-June%202026-2D3436?style=flat-square)
![Author](https://img.shields.io/badge/Framework%20Author-Erwin%20Maurice%20McDonald%20%7C%20Epoch%20Frameworks%20LLC-8E9FD5?style=flat-square)

---

> **What this document is.** This is the populated output of a structured BA discovery session for Cornerstone Lending Partners integrating with ServiceLink's EXOS Close API. Every answer maps to a BRD field. Every BRD field maps to a dev action. The Conard Layer verdict at the end determines whether API testing can be scheduled.

---

## Client Intake

![Client](https://img.shields.io/badge/Client-Cornerstone%20Lending%20Partners-1B2E5E?style=for-the-badge)
![Product](https://img.shields.io/badge/Product%20Line-EXOS%20Close-534AB7?style=for-the-badge)
![Mode](https://img.shields.io/badge/Integration%20Mode-Full%20Round--Trip-1D9E75?style=for-the-badge)

| Field | Value |
|---|---|
| Client Name | Cornerstone Lending Partners |
| ServiceLink Product | EXOS Close — closing scheduling, notary network, hybrid/remote/in-person closing orchestration |
| Integration Mode | Full round-trip — order initiation, status polling, and callback receipt |
| Target Sandbox Testing | July 28, 2026 |
| Target Production Go-Live | September 15, 2026 |

---

## Phase A — Client Environment and Org Context

![Phase](https://img.shields.io/badge/Phase%20A-Org%20%26%20Environment-1B2E5E?style=flat-square)
![Status](https://img.shields.io/badge/Status-COMPLETE-1D9E75?style=flat-square)

> **Purpose.** Understand who owns what, who approves what, and what is already built before the first line of integration code is written.

| # | Question | Answer | BRD Field |
|---|---|---|---|
| A1 | Who is the technical lead on the client side? | Marcus Webb, Senior Integration Engineer — marcus.webb@cornerstonelp.com — 214-555-0192 | BRD-ORG-01 |
| A2 | Who approves test environment credentials? | Dana Howell, VP of Technology Operations — SLA: 48 hours from request submission | BRD-ORG-02 |
| A3 | What is the client's current LOS / MSP? | ICE Encompass version 23.3 | BRD-ENV-01 |
| A4 | Is the client on-premise, cloud, or hybrid? | Cloud — Microsoft Azure, East US region | BRD-ENV-02 |
| A5 | What is the target go-live date? | Sandbox testing: July 28, 2026 — Production go-live: September 15, 2026 | BRD-SCHED-01 |
| A6 | Has this client integrated with any other title or closing API before? | Yes — Snapdocs closing platform (2023). Abandoned due to notary network coverage gaps in rural markets. | BRD-ENV-03 |

**BA Notes — Phase A:**
- ICE Encompass 23.3 confirms MISMO 3.4 compatibility — no translation layer expected at the schema level
- Azure East US region: confirm ServiceLink API gateway latency SLA is acceptable for East US routing
- Snapdocs failure context is a critical signal — notary network rural coverage must be explicitly tested in sandbox. Add to Phase F test case requirements
- Credential approver SLA is 48 hours — factor into sprint 1 kickoff timeline; submit credential request no later than July 14

---

## Phase B — ServiceLink Product Line and API Scope

![Phase](https://img.shields.io/badge/Phase%20B-API%20Scope%20%26%20Auth-1B2E5E?style=flat-square)
![Status](https://img.shields.io/badge/Status-COMPLETE-1D9E75?style=flat-square)

> **Purpose.** Lock down exactly which ServiceLink APIs are in scope and prevent scope creep post-kickoff. Every item defined here becomes a sprint boundary.

| # | Question | Answer | BRD Field |
|---|---|---|---|
| B1 | Which ServiceLink product is in scope? | EXOS Close — full closing orchestration including scheduling, notary assignment, and status callbacks | BRD-SCOPE-01 |
| B2 | Is the integration order initiation only, status polling only, or both? | Full round-trip — order initiation, status polling, and callback receipt | BRD-SCOPE-02 |
| B3 | What ServiceLink environments are needed? | Sandbox and Production. UAT handled within sandbox tier per Cornerstone IT policy | BRD-SCOPE-03 |
| B4 | What API version is the client being onboarded to? | EXOS Close API v3.2 — no deprecation notice received | BRD-API-01 |
| B5 | What authentication pattern does this endpoint use? | OAuth 2.0 — client credentials flow | BRD-API-02 |
| B6 | Are there IP whitelist requirements? | Yes — Cornerstone requires ServiceLink to whitelist three Azure NAT gateway IPs (to be provided during provisioning) | BRD-API-03 |
| B7 | What is the rate limit policy? | 120 calls per minute per credential set — confirmed with ServiceLink API ops | BRD-API-04 |

**BA Notes — Phase B:**
- Full round-trip requires both write and read auth scopes under the OAuth 2.0 client credentials flow — confirm scope strings with ServiceLink provisioning before sprint 1
- UAT inside sandbox tier is non-standard — document Cornerstone's IT policy rationale and confirm ServiceLink supports this configuration
- Three NAT gateway IPs must be submitted to ServiceLink before July 14 or sandbox calls will fail on day one
- 120 calls/minute: at full closing volume, model expected burst and confirm retry-with-backoff logic stays under the ceiling

---

## Phase C — Data Fields, Schema, and Mapping

![Phase](https://img.shields.io/badge/Phase%20C-Schema%20%26%20Field%20Mapping-1B2E5E?style=flat-square)
![Status](https://img.shields.io/badge/Status-COMPLETE%20WITH%20GAP%20FLAGGED-E67E22?style=flat-square)
![Risk Flag](https://img.shields.io/badge/Risk-ClosingAgentPreference%20MISSING-B91C1C?style=flat-square)

> **Purpose.** Every field the client sends must map to a ServiceLink-accepted value. Unmapped fields are the single largest cause of integration test failures.

| # | Question | Answer | BRD Field |
|---|---|---|---|
| C1 | What is the primary order object? | Loan number, property address, borrower name, closing type (remote/hybrid/in-person), preferred closing date window | BRD-SCHEMA-01 |
| C2 | What schema standard does the client LOS use? | MISMO 3.4 | BRD-SCHEMA-02 |
| C3 | Are there custom or non-standard fields? | Yes — `CLP_BranchCode` (internal P&L attribution field). Not in ServiceLink schema. | BRD-SCHEMA-03 |
| C4 | What are the mandatory fields in the ServiceLink payload? | All confirmed present in Encompass export **EXCEPT** `ClosingAgentPreference` — Cornerstone does not capture this field | BRD-SCHEMA-04 |
| C5 | How does the client handle null or missing values? | Cornerstone's LOS will send null for optional fields rather than omitting them from the payload | BRD-SCHEMA-05 |
| C6 | What status codes must trigger client-side actions? | `SCHEDULED`, `PENDING_NOTARY`, `NOTARY_ASSIGNED`, `COMPLETED`, `CANCELLED`, `EXCEPTION_HOLD` | BRD-SCHEMA-06 |

**BA Notes — Phase C:**

> **SCHEMA GAP — BLOCKING:** `ClosingAgentPreference` is a mandatory ServiceLink field. Cornerstone does not capture it in Encompass. Dev must decide before sprint 1: (a) default value strategy, (b) add field to Encompass workflow, or (c) confirm with ServiceLink whether this field can be omitted for EXOS-assigned notary workflows. **This gap must be resolved before sandbox testing is scheduled.**

- `CLP_BranchCode` designation decision required: pass-through, dropped, or error. Recommend dropped with logging — confirm with Marcus Webb
- Null-for-optional strategy: dev must test ServiceLink behavior when null is passed for optional fields vs. field omitted — these may produce different API responses
- All six status codes must have named client-side actions mapped in the BRD before sprint 1. See Section 4.3 for full map

**Status Code Action Map:**

| ServiceLink Status | Required Client Action |
|---|---|
| `SCHEDULED` | Update loan record in Encompass — trigger closing disclosure workflow |
| `PENDING_NOTARY` | Display status in Encompass plugin — no action required |
| `NOTARY_ASSIGNED` | Update Encompass + notify borrower via Cornerstone communication platform |
| `COMPLETED` | Close order in Encompass — trigger post-closing checklist |
| `CANCELLED` | Update Encompass — trigger manual closing coordinator review |
| `EXCEPTION_HOLD` | Trigger PagerDuty alert + route to Sandra Okafor (Compliance) + open ServiceNow ticket |

---

## Phase D — Compliance, Audit, and Regulatory Posture

![Phase](https://img.shields.io/badge/Phase%20D-Compliance%20%26%20Regulatory-1B2E5E?style=flat-square)
![Status](https://img.shields.io/badge/Status-COMPLETE%20WITH%20CONTROLS-E67E22?style=flat-square)
![RESPA](https://img.shields.io/badge/RESPA%20Section%208-IN%20SCOPE-B91C1C?style=flat-square)
![Model Gov](https://img.shields.io/badge/Freddie%20Mac%20AI%20Governance-APPLICABLE-B91C1C?style=flat-square)

> **Purpose.** The Conard Layer will not SHIP any integration that cannot demonstrate RESPA, TILA, and audit trail readiness. These answers are gathered before testing, not after.

| # | Question | Answer | BRD Field |
|---|---|---|---|
| D1 | Is this client subject to RESPA Section 8? | In scope — Cornerstone is a federally chartered lender. RESPA Section 8 applies to all title and closing referral arrangements | BRD-COMP-01 |
| D2 | What is the audit trail requirement? | 7 years — immutable log format — Azure Blob with WORM policy enabled | BRD-COMP-02 |
| D3 | Is there a model governance requirement? | Applicable — EXOS automated notary eligibility determination is in scope. Freddie Mac March 2026 AI governance requirements flagged by Cornerstone compliance team | BRD-COMP-03 |
| D4 | How is PII transmitted and protected? | Borrower name, property address, and loan number in payload — TLS 1.3 confirmed — SSN is NOT transmitted through the API | BRD-COMP-04 |
| D5 | Is there a regulatory SLA floor? | Yes — TILA requires closing disclosure delivery 3 business days before consummation. EXOS scheduling SLA must not create downstream CD timing violations | BRD-COMP-05 |

**BA Notes — Phase D:**

> **COMPLIANCE CONTROL REQUIRED:** Freddie Mac March 2026 AI governance requirements apply to the EXOS automated notary eligibility determination. Dev must confirm with ServiceLink whether EXOS eligibility decisions are subject to model documentation requirements under this framework. Cornerstone's compliance team must sign off before go-live — not at UAT.

- RESPA Section 8: data-sharing architecture and fee disclosure logic must be reviewed by Cornerstone legal before the integration spec is finalized. Do not proceed to sprint 1 without written legal sign-off
- Audit retention: Azure Blob WORM policy is compliant with 7-year immutability requirement. Dev must confirm API call logs are routed to the WORM-enabled container, not a standard Blob
- TLS 1.3 confirmed — exceeds the TLS 1.2 minimum. No field-level encryption required (SSN excluded from payload)
- TILA CD timing: EXOS scheduling windows must be validated against 3-business-day CD delivery requirement. A closing scheduled too close to consummation is a regulatory violation, not just a bad UX

---

## Phase E — Error Handling, Failure Modes, and Exception Workflow

![Phase](https://img.shields.io/badge/Phase%20E-Error%20Handling%20%26%20Failure%20Modes-1B2E5E?style=flat-square)
![Status](https://img.shields.io/badge/Status-COMPLETE-1D9E75?style=flat-square)

> **Purpose.** Dev needs the broken path documented as clearly as the happy path. Every failure mode not in the BRD becomes an incident ticket in production.

| # | Question | Answer | BRD Field |
|---|---|---|---|
| E1 | What happens on a 4xx error? | Retry with exponential backoff — max 3 retries — then alert to PagerDuty integration monitoring queue | BRD-ERR-01 |
| E2 | What is the maximum acceptable timeout? | 30 seconds — escalate to manual closing coordinator queue after timeout | BRD-ERR-02 |
| E3 | How are schema validation errors surfaced? | API response logged to Azure Monitor + UI alert in Encompass plugin + email to marcus.webb@cornerstonelp.com | BRD-ERR-03 |
| E4 | Who receives compliance-hold notifications? | Sandra Okafor, Closing Compliance Manager — s.okafor@cornerstonelp.com — email + ServiceNow ticket | BRD-ERR-04 |
| E5 | What is the cancellation / rollback process? | EXOS Cancel endpoint confirmed available — trigger: loan withdrawn or closing date change exceeding 72 hours — confirmation via `CANCELLED` status callback | BRD-ERR-05 |
| E6 | Is there a manual override workflow? | Yes — Cornerstone closing coordinator manually reassigns via EXOS portal — trigger: `EXCEPTION_HOLD` status or 3 failed retries — owner: Closing Operations team | BRD-ERR-06 |

**BA Notes — Phase E:**
- 30-second timeout at the boundary of sync vs. async design. If ServiceLink p95 response time exceeds 25 seconds under load, this integration requires async with webhook callback — confirm with ServiceLink before architecture is locked
- PagerDuty integration must be tested in sandbox — not assumed. Add PagerDuty alert trigger to Phase F test case F4
- Sandra Okafor is named — compliance hold notification gap is closed. Ensure ServiceNow ticket auto-creation is in the integration spec, not just the email
- Cancellation trigger at 72-hour closing date change: dev must confirm EXOS Cancel endpoint behavior when order is in `NOTARY_ASSIGNED` state vs. `PENDING_NOTARY` — behavior may differ
- Manual override via EXOS portal is a non-API path — confirm Cornerstone Closing Operations team has EXOS portal credentials and training before go-live

---

## Phase F — Testing, Acceptance, and Go-Live Readiness

![Phase](https://img.shields.io/badge/Phase%20F-Testing%20%26%20Go--Live%20Readiness-1B2E5E?style=flat-square)
![Status](https://img.shields.io/badge/Status-COMPLETE-1D9E75?style=flat-square)
![Credentials](https://img.shields.io/badge/Sandbox%20Credentials-CONFIRMED%20June%2010%202026-1D9E75?style=flat-square)

> **Purpose.** The Conard BA Acceptance Gate will not pass without signed, measurable success criteria. These are defined before sprint 1, not negotiated at the end of sprint 2.

| # | Question | Answer | BRD Field |
|---|---|---|---|
| F1 | What constitutes a successful sandbox test? | (1) Order submitted and `SCHEDULED` returned within 10 seconds; (2) `NOTARY_ASSIGNED` callback received within 60 seconds of scheduling; (3) `CANCELLED` confirmed via callback within 5 seconds of cancel call; (4) `EXCEPTION_HOLD` triggers PagerDuty alert within 2 minutes | BRD-TEST-01 |
| F2 | Who confirmed receipt of ServiceLink sandbox credentials? | Marcus Webb — confirmed via email — June 10, 2026 | BRD-TEST-02 |
| F3 | What is the agreed test data set? | Synthetic — ServiceLink-provided test case library plus 5 Cornerstone-generated synthetic loans | BRD-TEST-03 |
| F4 | What is the minimum test case coverage before UAT promotion? | Happy path (2 runs), notary assignment flow (2 runs), cancel/rollback (2 runs), `EXCEPTION_HOLD` error path (1 run), null field handling (1 run) — 8 test cases minimum | BRD-TEST-04 |
| F5 | Who signs off on UAT and what is their response SLA? | Dana Howell, VP of Technology Operations — 5 business days from test completion report submission | BRD-TEST-05 |
| F6 | Is there a production parallel-run period? | Yes — 30 days — monitoring owner: Marcus Webb — cutover trigger: zero `EXCEPTION_HOLD` events in final 7 days of parallel run | BRD-TEST-06 |

**BA Notes — Phase F:**
- Credentials confirmed June 10 — this is the single most important day-one blocker resolved. Document confirmation email in the project record
- Add rural market notary coverage test case to the minimum 8 — Snapdocs failure history makes this a client-specific risk scenario that must be validated before UAT promotion
- Dana Howell 5-business-day UAT SLA: with a September 15 go-live, UAT must be submitted no later than August 29 to allow SLA + buffer. Back-date the sprint plan from this date
- 30-day parallel run with Marcus Webb as monitoring owner: confirm Marcus has capacity and tooling (Azure Monitor + PagerDuty) set up before parallel run begins
- Cutover trigger (zero `EXCEPTION_HOLD` in final 7 days) is measurable and unambiguous — this is a well-defined gate

---

## Section 3 — Conard Layer Verdict

![Conard Layer](https://img.shields.io/badge/Conard%20Layer-GOVERNING%20GATE-1B2E5E?style=for-the-badge)

> **Governing question.** Can this integration safely move from operational signal to client-trust outcome?

### Gate Results

| Gate | Status | Evidence |
|---|---|---|
| 1 — Client Trust Gate | ![CONDITIONAL](https://img.shields.io/badge/CONDITIONAL-E67E22?style=flat-square) | POC named (Marcus Webb). Status code map complete (6 codes, all actions assigned). `ClosingAgentPreference` gap must be resolved — default value or field addition required before PASS. |
| 2 — Compliance Gate | ![CONDITIONAL](https://img.shields.io/badge/CONDITIONAL-E67E22?style=flat-square) | RESPA in scope — legal sign-off required before sprint 1. Freddie Mac AI governance applicable — ServiceLink model documentation must be confirmed. PII handling clean (TLS 1.3, no SSN). Audit retention confirmed (Azure WORM, 7 years). TILA SLA floor documented. |
| 3 — Cycle-Time Gate | ![CONDITIONAL](https://img.shields.io/badge/CONDITIONAL-E67E22?style=flat-square) | Sandbox success criteria include measurable timing targets (10-second scheduling, 60-second notary assignment). Baseline order placement time (pre-integration) not yet documented. Owner: Marcus Webb to provide baseline before UAT. |
| 4 — Failure-Mode Gate | ![PASS](https://img.shields.io/badge/PASS-1D9E75?style=flat-square) | All six Phase E fields complete. 4xx retry logic defined (exponential backoff, max 3). Timeout at 30 seconds with manual escalation. Compliance hold recipient named (Sandra Okafor). Cancellation endpoint confirmed. Manual override path documented (EXOS portal, Closing Operations). |
| 5 — BA Acceptance Gate | ![CONDITIONAL](https://img.shields.io/badge/CONDITIONAL-E67E22?style=flat-square) | Sandbox success criteria are measurable and specific. Credentials confirmed (Marcus Webb, June 10). UAT sign-off owner named (Dana Howell, 5-day SLA). `ClosingAgentPreference` gap and RESPA legal sign-off must be resolved before this gate reaches PASS. |

---

### Conard Verdict

![Verdict](https://img.shields.io/badge/Conard%20Verdict-SHIP%20WITH%20CONTROLS-E67E22?style=for-the-badge)

**Why:** Integration requirements are substantively complete and the failure-mode architecture is sound. Three controls must be executed and confirmed before API testing is scheduled — none of these are architectural blockers, but all three are compliance or schema risks that will surface as test failures or audit findings if deferred.

**Evidence:**
- **Client Trust:** POC named, status code map complete, EXOS portal access path confirmed. Gap: `ClosingAgentPreference` must be resolved
- **Compliance:** RESPA in scope with legal sign-off required. Freddie Mac AI governance applicable — model documentation pending. PII clean, audit retention confirmed, TILA SLA floor documented
- **Cycle-Time:** Sandbox timing criteria are measurable. Pre-integration baseline not yet documented — acceptable at this stage, required before UAT report
- **Failure-Mode:** All six error paths documented and complete. PASS
- **BA Acceptance:** Credentials confirmed. Acceptance criteria are specific and signed. Two open items block full PASS — see controls below

**Open Risks:**
- `ClosingAgentPreference` mandatory field gap — guaranteed sandbox test failure if unresolved
- RESPA Section 8 legal sign-off not yet obtained — sprint 1 cannot start without it
- Freddie Mac March 2026 AI governance applicability to EXOS notary eligibility not yet confirmed with ServiceLink

**Controls Required Before Scheduling API Testing:**

| # | Control | Owner | Resolution Gate |
|---|---|---|---|
| C1 | Resolve `ClosingAgentPreference` gap — default value, Encompass field addition, or ServiceLink waiver | Marcus Webb + ServiceLink API ops | Written decision documented in BRD-SCHEMA-04 before July 14 |
| C2 | Obtain RESPA Section 8 legal sign-off on integration architecture | Cornerstone Legal (to be named by Dana Howell) | Written sign-off in project record before sprint 1 kickoff |
| C3 | Confirm Freddie Mac AI governance requirements with ServiceLink — obtain model documentation or written exemption | BA + ServiceLink Compliance contact | Documented before UAT promotion — not a sprint 1 blocker but must not reach production without resolution |

**Next Gate:** BA resubmits Conard verdict after Controls C1 and C2 are resolved. If both are confirmed before July 14, testing may be scheduled for July 28 as planned.

**Verdict Owner:** _________________________ **Date:** _________________________

---

## Section 4 — Populated BRD Summary

![BRD](https://img.shields.io/badge/BRD%20Status-POPULATED%20FROM%20DISCOVERY-1B2E5E?style=for-the-badge)
![Mandatory Fields](https://img.shields.io/badge/Mandatory%20Fields-37%20Total-534AB7?style=flat-square)
![Gaps](https://img.shields.io/badge/Open%20Gaps-1%20BLOCKING%20%7C%202%20CONTROLLED-B91C1C?style=flat-square)

### 4.1 — Org and Environment

| BRD Field | Populated Value | Status |
|---|---|---|
| BRD-ORG-01: Client Technical POC | Marcus Webb — marcus.webb@cornerstonelp.com — 214-555-0192 | COMPLETE |
| BRD-ORG-02: Credential Approver | Dana Howell, VP Technology Operations — 48hr SLA | COMPLETE |
| BRD-ENV-01: Client LOS / MSP | ICE Encompass v23.3 | COMPLETE |
| BRD-ENV-02: Infrastructure Type | Cloud — Microsoft Azure East US | COMPLETE |
| BRD-ENV-03: Prior API Integrations | Snapdocs (2023) — abandoned, rural notary coverage gap | COMPLETE |
| BRD-SCHED-01: Target Go-Live | Sandbox July 28, 2026 — Production September 15, 2026 | COMPLETE |

### 4.2 — API Scope and Authentication

| BRD Field | Populated Value | Status |
|---|---|---|
| BRD-SCOPE-01: Product Line | EXOS Close — full closing orchestration | COMPLETE |
| BRD-SCOPE-02: Integration Mode | Full round-trip (initiation + polling + callbacks) | COMPLETE |
| BRD-SCOPE-03: Environments | Sandbox + Production (UAT within sandbox) | COMPLETE |
| BRD-API-01: API Version | EXOS Close API v3.2 — no deprecation notice | COMPLETE |
| BRD-API-02: Auth Pattern | OAuth 2.0 — client credentials flow | COMPLETE |
| BRD-API-03: IP Whitelist | 3 Azure NAT gateway IPs — to be provided at provisioning | COMPLETE |
| BRD-API-04: Rate Limits | 120 calls/minute per credential set | COMPLETE |

### 4.3 — Schema and Field Mapping

| BRD Field | Populated Value | Status |
|---|---|---|
| BRD-SCHEMA-01: Primary Order Object | Loan number, property address, borrower name, closing type, preferred date window | COMPLETE |
| BRD-SCHEMA-02: Source Schema Standard | MISMO 3.4 | COMPLETE |
| BRD-SCHEMA-03: Custom Fields | `CLP_BranchCode` — designation: DROP with logging | COMPLETE |
| BRD-SCHEMA-04: Mandatory Field Map | All fields present EXCEPT `ClosingAgentPreference` | **OPEN — BLOCKING** |
| BRD-SCHEMA-05: Null Handling Strategy | LOS sends null for optional fields — dev must test ServiceLink null vs. omit behavior | COMPLETE |
| BRD-SCHEMA-06: Status Code Map | 6 codes mapped — SCHEDULED, PENDING_NOTARY, NOTARY_ASSIGNED, COMPLETED, CANCELLED, EXCEPTION_HOLD | COMPLETE |

### 4.4 — Compliance and Regulatory

| BRD Field | Populated Value | Status |
|---|---|---|
| BRD-COMP-01: RESPA Scope | In scope — federally chartered lender — legal sign-off required | **OPEN — CONTROLLED** |
| BRD-COMP-02: Audit Retention | 7 years — Azure Blob WORM — immutable | COMPLETE |
| BRD-COMP-03: Model Governance | Applicable — Freddie Mac March 2026 — ServiceLink model docs pending | **OPEN — CONTROLLED** |
| BRD-COMP-04: PII Handling | Borrower name/address/loan number — TLS 1.3 — no SSN | COMPLETE |
| BRD-COMP-05: SLA Regulatory Floor | TILA — 3 business days CD delivery before consummation | COMPLETE |

### 4.5 — Error Handling and Failure Modes

| BRD Field | Populated Value | Status |
|---|---|---|
| BRD-ERR-01: 4xx Handling | Exponential backoff — max 3 retries — PagerDuty alert | COMPLETE |
| BRD-ERR-02: Timeout Threshold | 30 seconds — escalate to manual closing coordinator | COMPLETE |
| BRD-ERR-03: Schema Error Surfacing | Azure Monitor + Encompass plugin UI alert + email to Marcus Webb | COMPLETE |
| BRD-ERR-04: Compliance Hold Recipient | Sandra Okafor, Closing Compliance Manager — email + ServiceNow ticket | COMPLETE |
| BRD-ERR-05: Cancellation / Rollback | EXOS Cancel endpoint — trigger: withdrawal or 72hr date change — CANCELLED callback | COMPLETE |
| BRD-ERR-06: Manual Override Path | EXOS portal — trigger: EXCEPTION_HOLD or 3 failed retries — Closing Operations | COMPLETE |

### 4.6 — Testing and Acceptance Criteria

| BRD Field | Populated Value | Status |
|---|---|---|
| BRD-TEST-01: Sandbox Success Criteria | 4 measurable outcomes — timing-bound — see Phase F notes | COMPLETE |
| BRD-TEST-02: Credential Confirmation | Marcus Webb — confirmed June 10, 2026 | COMPLETE |
| BRD-TEST-03: Test Data Type | Synthetic — ServiceLink library + 5 Cornerstone synthetic loans | COMPLETE |
| BRD-TEST-04: UAT Promotion Gate | 8 test cases minimum — happy path, notary, cancel, exception, null | COMPLETE |
| BRD-TEST-05: UAT Sign-Off Owner | Dana Howell — 5 business day SLA | COMPLETE |
| BRD-TEST-06: Parallel Run Period | 30 days — Marcus Webb monitoring — zero EXCEPTION_HOLD in final 7 days | COMPLETE |

---

## Section 5 — Manager and Dev Team Summary

![For](https://img.shields.io/badge/For-Dev%20Team%20Lead%20%7C%20Manager-1B2E5E?style=for-the-badge)
![Verdict](https://img.shields.io/badge/Conard%20Verdict-SHIP%20WITH%20CONTROLS-E67E22?style=for-the-badge)

### What We Are Building

Cornerstone Lending Partners is integrating with ServiceLink's EXOS Close API (v3.2) to automate their closing scheduling and notary assignment workflow. The integration is full round-trip — Cornerstone's ICE Encompass LOS will initiate closing orders, receive real-time status callbacks, and trigger downstream borrower notifications and post-closing workflows without manual intervention. The integration runs on Azure East US using OAuth 2.0 and targets sandbox testing by July 28 and production go-live by September 15, 2026.

### Are We Ready to Schedule API Testing?

![Answer](https://img.shields.io/badge/Answer-NOT%20YET%20--%20Two%20Controls%20Must%20Close%20First-E67E22?style=flat-square)

**Do not schedule testing until the following two items are resolved:**

| # | Blocker | Owner | Must Resolve By |
|---|---|---|---|
| 1 | `ClosingAgentPreference` mandatory field gap — Encompass does not capture this field | Marcus Webb + ServiceLink API ops | July 14, 2026 |
| 2 | RESPA Section 8 legal sign-off on integration architecture | Cornerstone Legal (Dana Howell to assign) | Sprint 1 kickoff — before July 21 |

**Control C3** (Freddie Mac AI governance model documentation from ServiceLink) is not a sprint 1 blocker but must be resolved before UAT promotion. Add to sprint 2 backlog now.

### Next Actions

1. Marcus Webb submits three Azure NAT gateway IPs to ServiceLink for whitelist — no later than July 14
2. Dana Howell assigns a legal contact for RESPA Section 8 sign-off — no later than July 7
3. BA and Marcus Webb schedule `ClosingAgentPreference` resolution call with ServiceLink API ops — no later than July 10
4. BA resubmits Conard verdict after Controls C1 and C2 are confirmed — target July 15
5. Sprint 1 kickoff scheduled for July 21 — API sandbox testing target July 28 — contingent on Controls C1 and C2 closure

---

## Document Provenance

![Framework](https://img.shields.io/badge/Framework-Regulated%20FinServ%20Presales%20%7C%20Conard%20Layer%20%7C%20McDonald%20(2026)-8E9FD5?style=flat-square)
![IP](https://img.shields.io/badge/IP-Epoch%20Frameworks%20LLC%20%7C%20DACR%20License%20v2.6-2D3436?style=flat-square)
![Companion](https://img.shields.io/badge/Companion%20Frameworks-NorthStar%20v4.1%20%7C%20MOC%20v4.9-8E9FD5?style=flat-square)

**McDonald, E.M. (2026).** ServiceLink EXOS Close API Integration — Cornerstone Lending Partners Discovery Session Output. Epoch Frameworks LLC. DACR License v2.6. Fort Worth, TX.

*This document is a BA working artifact produced from a structured discovery session using test data. It does not constitute legal, compliance, or financial advice. All compliance determinations must be reviewed by qualified legal counsel before sprint execution.*
