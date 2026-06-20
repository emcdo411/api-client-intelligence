# Artifact 9: Agentic API Integration Blueprint

McDonald / Epoch Frameworks LLC - DACR License v2.6
Governed by the Deepak Layer (see SKILL.md)

---

## PURPOSE

This artifact turns a conversation like "how could we use Claude Code and Claude Skills to handle SOAP/REST lender API tasks" into a concrete, handoff-ready technical plan. It is built for the moment a technical lead (Deepak, or equivalent) and a technical BA are scoping whether and how to put agentic tooling on top of existing lender integrations.

**Core thesis:** The agent is only as good as the map it works from. Before any agent touches a lender API, the API surface has to exist as a structured, validated, version-controlled asset -- not tribal knowledge. Postman is where that map lives. The agent reads from the map; it does not improvise against the live endpoint.

---

## STRUCTURE

### Section 1: API Surface Inventory

List every lender integration in scope. For each:

| Field | Capture |
|--|--|
| Lender / Partner | Name |
| Protocol | SOAP / REST |
| Auth Method | OAuth2, API key, mutual TLS, etc. |
| Endpoints in Scope | List, with method (GET/POST/PUT for REST; operation name for SOAP) |
| Payload Direction | Inbound (lender → ServiceLink) / Outbound / Bidirectional |
| Schema Source | WSDL/XSD location for SOAP; JSON Schema/OpenAPI spec for REST |
| Current Documentation State | Documented in Postman / Partially documented / Tribal knowledge only |
| RESPA-Relevant Fields | Fee fields, disclosure timestamps, settlement data -- flag for Conard Layer cross-reference |

### Section 2: Postman Collection Architecture

Describe the collection structure, not just "we'll use Postman":

- **Folder structure**: one folder per lender, sub-folders by function (order intake, status, fee disclosure, document delivery, etc.)
- **Environment variables**: per-lender auth tokens, base URLs, separated dev/staging/prod
- **Request-level documentation**: each request annotated with expected payload shape, required fields, known quirks
- **SOAP handling**: WSDL imported and mirrored as raw XML request templates with envelope structure documented inline, since Postman's native SOAP support is weaker than REST
- **Versioning**: collection stored in source control (not just Postman's cloud sync), so changes are reviewable and the agent always reads from a known-good version

### Section 3: Schema Validation Gate Design

Specify where validation sits in the pipeline:

```
Lender API Response
       |
       v
[ Schema Validation Agent ]  <- reads schema from Postman collection / WSDL / XSD
       |
   pass?  ---- NO ----> [ Exception Queue ] -> human review / Conard compliance flag
       |
      YES
       |
       v
Downstream Pipeline (order processing, fee calc, disclosure trigger)
```

Name explicitly:
- What library/approach performs validation (XML schema validation for SOAP, JSON Schema validation for REST)
- What happens on failure (route to exception queue, alert, do NOT silently pass through or drop)
- Who owns triage on the exception queue

### Section 4: Regression Test Automation Plan

| Element | Specification |
|--|--|
| Scope | Which lender endpoints get automated coverage first (start with highest-volume or highest-compliance-risk) |
| Cadence | Scheduled run frequency (e.g., nightly, pre-deploy) |
| Assertions | What counts as pass/fail -- response shape, required fields present, response time threshold, status code |
| Drift Definition | What constitutes meaningful drift vs. noise (a lender adding an optional field is not the same severity as a required field disappearing) |
| Alert Routing | Who gets notified, what channel, what SLA for triage |
| Agent Role | Does the agent just run and report, or does it also propose the fix (e.g., flag that the collection needs updating because the lender changed something legitimately) |

### Section 5: Agent Authorization Boundary

This is the section a technical lead will scrutinize most closely. Be explicit:

**Agent CAN do autonomously:**
- Read the Postman collection / schema definitions
- Validate incoming payloads against documented schema
- Run regression test suites on schedule
- Flag drift, anomalies, or validation failures
- Generate documentation updates as drafts for human review

**Agent CANNOT do without human review:**
- Modify the live Postman collection / schema source of truth
- Onboard a new lender endpoint into production validation
- Make any write call to a lender's live API
- Auto-resolve a schema validation failure by altering downstream data
- Change RESPA-relevant field mappings without compliance sign-off

### Section 6: From Tribal Knowledge to Living Documentation

Describe the conversion path for existing undocumented integrations:

1. Inventory current manual knowledge (who knows what about each lender's quirks)
2. Build the Postman collection from that knowledge, validated against live test calls
3. Annotate edge cases directly in the collection (not in a separate doc that will drift)
4. Stand up the schema validation gate against the newly documented schema
5. Layer regression automation on top once the collection is stable
6. Treat the collection as the single source of truth going forward -- code and agent skills reference it, they don't duplicate it

---

## DEEPAK LAYER VERDICT (MANDATORY)

Every Agentic API Integration Blueprint ends with:

```
Deepak Layer Verdict: [SHIP / SHIP WITH CONTROLS / HOLD / NO-SHIP]
Why: [one technical-lead-facing sentence]
Evidence: [3-5 bullets tied to map completeness, schema integrity, regression coverage, build boundary, handoff readiness]
Open Risks: [bullets or "None material"]
Next Gate: [what must happen before this moves from concept to backend implementation]
```

If any RESPA-relevant field is in scope, cross-reference the Conard Layer compliance gate as well.

---

## TONE NOTES FOR THIS ARTIFACT

- This is an engineering-facing artifact. Lead with architecture, not business value framing.
- Specificity wins. "We'll validate schemas" is not an artifact. The validation flow diagram and failure-routing logic is.
- Never claim the agent will be fully autonomous on lender-facing writes. The authorization boundary section is what earns trust from a technical lead -- it shows you understand where the real risk sits.
- This artifact is meant to be handed directly to an engineer. If it requires a follow-up meeting to clarify scope, it has not met the Handoff Gate.
