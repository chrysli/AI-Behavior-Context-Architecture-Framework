# Context Architecture Spec

This document defines the context architecture for the fictional **Public Service Request & Feedback Triage Assistant**.

The goal is to define what context the assistant needs, where that context comes from, when it should be included, how it should be labeled, and how it should affect behavior inside the public service request and feedback workflow.

---

## 1. Context Architecture Overview

**System name:** Public Service Request & Feedback Triage Assistant

**Workflow name:** Public Service Request & Feedback Triage

**Primary users:** Anonymous users, authenticated residents, authenticated business owners, internal service teams, reviewers, and administrators

**Sample status:** Fictional public-safe architecture example

---

## 2. Purpose of Context Architecture

This context architecture exists to help the assistant behave correctly inside a public service intake workflow.

The assistant needs context to determine:

* what the user is trying to do
* where the user is in the workflow
* what information is already known
* what information is missing
* what can be safely inferred
* what can be retrieved
* what the user is allowed to see
* what language rules apply
* what output should be produced
* whether the case should be routed, reviewed, or escalated

The assistant should not rely only on the latest user message when workflow state, user role, service context, case history, or permission boundaries affect the correct behavior.

---

## 3. Context Architecture Goals

The context architecture should:

* provide enough context for the assistant to support the current workflow state
* reduce unnecessary user questions
* preserve user intent and original wording when needed
* separate known, inferred, retrieved, missing, uncertain, and restricted context
* prevent restricted case history or internal notes from leaking into user-facing output
* support multilingual input and terminology preservation
* prepare structured outputs for service teams or reviewers
* make routing and escalation decisions reviewable
* support diagnostics when the assistant behaves incorrectly

---

## 4. Context Types

| Context type           | Description                                                        | Example                                                                | Notes                                                              |
| ---------------------- | ------------------------------------------------------------------ | ---------------------------------------------------------------------- | ------------------------------------------------------------------ |
| Known context          | Information explicitly provided or confirmed by the user or system | User says a streetlight is out on a specific road                      | Treated as available, but may still need confirmation if ambiguous |
| Inferred context       | Information derived from user wording or workflow state            | User likely reports an issue rather than asks a question               | Must be labeled and confirmed when material                        |
| Retrieved context      | Information pulled from approved sources                           | Service catalog category, permitted case status, knowledge base answer | Must include source and permission status                          |
| Workflow-state context | Current point in the workflow                                      | Intent detection, clarification, routing, escalation, recovery         | Drives assistant behavior                                          |
| Role context           | User type and permission scope                                     | Anonymous user, resident, business owner, reviewer                     | Determines visibility and allowed actions                          |
| Case history context   | Prior or related records                                           | Existing case ID, visible prior case, restricted related case          | Permission-bound                                                   |
| Language context       | Input/output language, translation, terminology, original wording  | User writes in Arabic; output should remain Arabic                     | Must preserve meaning and official terms                           |
| Output context         | Required output type and audience                                  | Clarification question, structured case record, escalation summary     | Prevents generic responses                                         |
| Escalation context     | Triggers or conditions requiring handoff                           | Urgency, safety risk, repeated unresolved issue                        | May override normal routing                                        |
| Diagnostic context     | Test or review metadata                                            | Test case ID, expected behavior, failure pattern                       | Used for evaluation, not normal user interaction                   |

---

## 5. Context Sources

| Source                  | Context provided                                               | Access method                          | Notes                                       |
| ----------------------- | -------------------------------------------------------------- | -------------------------------------- | ------------------------------------------- |
| User message            | Request description, language, intent signals, urgency signals | Conversation input                     | Preserve original wording when needed       |
| Interface or form state | Selected category, completed fields, entry point               | UI/system state                        | Avoid asking for visible information again  |
| User account profile    | Role, authentication status, permission scope                  | Account system                         | Include only what is needed                 |
| Service catalog         | Service categories, responsible teams, basic routing rules     | Approved system source                 | Source of official category labels          |
| Case management system  | Own case history, case status, related records                 | Permission-bound retrieval             | Must respect visibility rules               |
| Knowledge base          | Public or internal answers                                     | Retrieval                              | Use only relevant approved content          |
| Prior session summary   | Completed steps and confirmed information                      | Session state                          | Supports recovery after interruption        |
| Language handling layer | Input language, output language, translation notes             | Detection/translation service or model | Must flag uncertainty when material         |
| Escalation rules        | Escalation triggers and owners                                 | Workflow rules                         | Used when normal triage should stop         |
| Testing harness         | Test case and expected behavior                                | Evaluation environment                 | Not normally included in production packets |

---

## 6. Context Availability

| Context              | Availability                         | Missing-context behavior                                                     |
| -------------------- | ------------------------------------ | ---------------------------------------------------------------------------- |
| Current user message | Always required                      | Cannot proceed without user input                                            |
| Workflow state       | Required for workflow-aware behavior | Use start state if no prior state exists                                     |
| User role            | Conditional                          | Treat as anonymous if not authenticated                                      |
| Permission scope     | Conditional                          | Do not retrieve or expose case history if unknown                            |
| Service catalog      | Conditional                          | Ask clarification or route for review if needed and unavailable              |
| Case history         | Conditional                          | Proceed without case history unless follow-up or duplicate check requires it |
| Language context     | Conditional                          | Detect from input; ask or flag if uncertain and material                     |
| Escalation rules     | Required when escalation can occur   | Use conservative human-review fallback if unavailable                        |
| Output requirements  | Required                             | Do not ask model for open-ended response when structured output is needed    |

---

## 7. Context Status Labels

The assistant should receive context with labels that identify source, status, confidence, and visibility.

Suggested labels:

| Label         | Meaning                          | Example                                            |
| ------------- | -------------------------------- | -------------------------------------------------- |
| `known`       | Explicitly provided or confirmed | User provided location                             |
| `inferred`    | Derived but not confirmed        | Assistant infers this is an issue report           |
| `retrieved`   | Pulled from approved source      | Service catalog match                              |
| `missing`     | Needed but unavailable           | Location missing for infrastructure issue          |
| `restricted`  | Not visible to the current user  | Internal case notes                                |
| `uncertain`   | Low-confidence or ambiguous      | Translation may map to multiple service categories |
| `conflicting` | Sources disagree                 | User says case is unresolved, system shows closed  |
| `stale`       | May be outdated                  | Old service status or prior case update            |

---

## 8. Context Priority Rules

Recommended priority order:

1. Permission and visibility rules
2. Current workflow state
3. Confirmed user input
4. Current system records from approved sources
5. Fresh service catalog or policy context
6. Permitted case history
7. Prior session summary
8. Inferred context
9. Low-confidence or uncertain context

Priority rules:

* Permission rules override convenience.
* Confirmed user input should not be overwritten by inference.
* Current workflow state should override outdated prior session assumptions.
* Retrieved context should be evaluated by source, freshness, permission, and relevance.
* Restricted context should not appear in user-facing output unless explicitly allowed.
* Low-confidence inference should not drive routing or escalation without confirmation or review.

---

## 9. Context Boundary Rules

| Context or data type            | Boundary                              | Allowed use                                   | Prohibited use                          |
| ------------------------------- | ------------------------------------- | --------------------------------------------- | --------------------------------------- |
| Other users’ case history       | Restricted                            | None for public user-facing flow              | Display, summarize, imply existence     |
| Internal service notes          | Internal only                         | Reviewer or service-team context if permitted | Public user-facing output               |
| User’s own case history         | Role-bound                            | Follow-up support when authenticated          | Anonymous access                        |
| Service catalog                 | Public or internal depending on field | Routing and explanation                       | Inventing categories outside catalog    |
| Hidden risk or priority signals | Internal only                         | Escalation or review if permitted             | User-facing explanation unless approved |
| Language translation notes      | Conditional                           | Review and clarification                      | Treating uncertain translation as fact  |
| Diagnostic test metadata        | Test only                             | Evaluation                                    | Production user-facing output           |

---

## 10. Retrieval Context Rules

Retrieval may be used for:

* public knowledge base answers
* service catalog lookup
* permitted case history
* related-case checks
* routing rules
* escalation owner lookup

Retrieval should be blocked or limited when:

* the user is anonymous and asks for case-specific information
* permission status is unknown
* the record belongs to another user or tenant
* retrieved content is stale, irrelevant, or low confidence
* retrieved content would expose restricted information

Retrieved context should include:

* source
* source type
* timestamp or freshness signal when relevant
* relevance reason
* permission status
* visibility status
* confidence or match strength

---

## 11. Workflow-State Context Rules

| Workflow state         | Context needed                                          | Assistant behavior                                  |
| ---------------------- | ------------------------------------------------------- | --------------------------------------------------- |
| Start                  | User message, entry point, user role if known           | Identify likely intent and request type             |
| Intent detection       | User wording, request type signals, language context    | Classify or ask targeted clarification              |
| Information collection | Known fields, missing required fields, service category | Ask only for required missing information           |
| Case history check     | Case ID, authenticated role, permission scope           | Retrieve only permitted case context                |
| Review                 | Structured summary, known/inferred/missing fields       | Ask user to confirm before submission or routing    |
| Routing                | Request type, service category, location, routing rules | Prepare routing recommendation or review flag       |
| Escalation             | Escalation trigger, known info, risk/urgency signal     | Stop normal handling and prepare escalation summary |
| Recovery               | Prior state, confirmed fields, remaining steps          | Resume without forcing restart                      |

---

## 12. Role Context Rules

| Role                         | Context available                                                | Context restricted                            | Output differences                 |
| ---------------------------- | ---------------------------------------------------------------- | --------------------------------------------- | ---------------------------------- |
| Anonymous user               | Public knowledge base, general service catalog, current message  | Case history, account-specific records        | General guidance or limited intake |
| Authenticated resident       | Own case history, own submitted requests, public service catalog | Other users’ cases, internal notes            | Can follow up on own cases         |
| Authenticated business owner | Authorized business cases, business service categories           | Unauthorized business records, internal notes | Business-specific request handling |
| Internal service team member | Assigned routed cases, permitted internal summaries              | Cases outside scope                           | Internal structured outputs        |
| Reviewer                     | Review queue, diagnostic notes if permitted, open questions      | Restricted data outside review scope          | Review-focused output              |
| Administrator                | Configuration and service taxonomy context                       | Still bounded by policy                       | Rule management and diagnostics    |

---

## 13. Language Context Rules

Language context should be included when:

* the user writes in a supported non-default language
* the user switches languages mid-flow
* user wording may affect classification or routing
* official service names or IDs must be preserved
* translation may affect escalation, urgency, or complaint handling
* internal summaries require a different language from user-facing responses

Language context should include:

* input language
* output language
* original user wording when needed
* translated summary when needed
* official terms not to translate
* uncertainty notes
* review flag if language ambiguity affects outcome

---

## 14. Output Context Rules

| Output type            | Audience                     | Required context                                               | Restricted context                            |
| ---------------------- | ---------------------------- | -------------------------------------------------------------- | --------------------------------------------- |
| Clarification question | User                         | Missing required field, reason needed                          | Internal routing logic                        |
| User-facing summary    | User                         | Known user-provided info, confirmed inference                  | Internal notes, restricted case history       |
| Structured case record | Internal service team/system | Known, missing, inferred, retrieved, routing, language context | Only permitted internal context               |
| Routing recommendation | Internal team/system         | Request type, category, location, routing rules, confidence    | Hidden records not allowed for receiving team |
| Escalation summary     | Human escalation owner       | Known info, risk signal, missing info, urgency                 | Restricted data outside escalation scope      |
| Recovery summary       | User                         | Prior state, confirmed fields, remaining steps                 | Internal-only notes                           |

---

## 15. Context Assembly Overview

High-level assembly sequence:

1. Identify current workflow state.
2. Identify user role and permission scope.
3. Add current user input and interface context.
4. Add known session context.
5. Identify missing required fields.
6. Add inference with labels when useful.
7. Trigger retrieval only when workflow state requires it and permissions allow it.
8. Filter retrieved context by relevance, freshness, source, and visibility.
9. Apply language rules.
10. Apply routing or escalation rules.
11. Define output requirements.
12. Exclude restricted, irrelevant, or unsupported context.
13. Assemble the runtime context packet.
14. Validate that the packet supports the expected assistant behavior.

---

## 16. Context Failure Patterns

The system should test for these context-related failures:

* assistant asks for information already available
* assistant lacks workflow state and responds generically
* assistant treats inference as confirmed fact
* assistant retrieves case history without permission
* assistant exposes restricted record details
* assistant uses stale service catalog information
* assistant ignores language ambiguity
* assistant loses original user wording when review needs it
* assistant routes based on weak related-case signal
* assistant omits required output fields
* assistant fails to surface missing information for human review
* assistant continues normal routing when escalation context is present

---

## 17. Diagnostic Questions

Use these questions when reviewing or testing this context architecture:

* Does the assistant know the current workflow state?
* Does the assistant know what information is known, missing, inferred, retrieved, restricted, or uncertain?
* Does the runtime packet include only context relevant to the current task?
* Are permission rules applied before user-facing output?
* Is related case history included only when permitted?
* Are language rules included when they affect meaning?
* Does the output requirement match the workflow state?
* Can a reviewer understand why routing or escalation was recommended?
* Can a failure be traced to context assembly, retrieval, permissions, workflow state, or model behavior?

---

## 18. Open Questions

| Question                                                   | Why it matters                                   | Likely owner                        | Status |
| ---------------------------------------------------------- | ------------------------------------------------ | ----------------------------------- | ------ |
| What exact service categories exist?                       | Required for routing and service catalog context | Service operations                  | Open   |
| Which user roles can view which case fields?               | Required for permission-aware output             | Security / policy / product         | Open   |
| What case history fields can be used for internal routing? | Affects restricted context rules                 | Operations / security               | Open   |
| What languages are supported for full intake?              | Affects language context and testing             | Product / localization              | Open   |
| Which outputs must be structured for backend systems?      | Affects runtime packet and output requirements   | Engineering                         | Open   |
| What logging is permitted for diagnostics?                 | Affects traceability and privacy                 | Engineering / security / governance | Open   |

---

## 19. Related Documents

* `sample-scope-and-assumptions.md`
* `ai-design-principles.md`
* `ai-assistant-behavior-spec.md`
* `public-service-request-feedback-triage-assistant-behavior-spec.md`
* `runtime-context-template.md`
* `context-assembly-rules.md`
* `language-handling-rules.md`
* `service-category-routing-rules.md`
* `case-history-context-rules.md`
* `example-context-packets.md`
* `testing-diagnostics-spec.md`
* `public-service-request-feedback-conversation-flows.md`
