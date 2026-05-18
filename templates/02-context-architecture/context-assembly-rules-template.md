# Context Assembly Rules Template

Use this document to define how context is selected, filtered, prioritized, labeled, assembled, and passed into the model at runtime.

This file should make context assembly explicit enough that product, design, engineering, governance, and testing teams can review how the model’s operating environment is constructed.

---

## 1. Assembly Rules Overview

**System name:** `[Insert system/product/service name]`

**AI assistant name:** `[Insert assistant name]`

**Workflow name:** `[Insert workflow name]`

**Context architecture version:** `[Insert version]`

**Document owner:** `[Insert owner/team]`

**Last updated:** `[Insert date]`

---

## 2. Purpose of Context Assembly

Describe why context assembly rules are needed.

```text
These context assembly rules exist to...
```

Prompts:

* What context must the model receive to behave correctly?
* What context must be excluded or restricted?
* What context depends on workflow state?
* What context depends on user role or permission scope?
* What retrieval rules should be applied before model response?
* What labels must be added so the model can interpret context correctly?
* What should happen when context is missing, stale, conflicting, or restricted?

---

## 3. Assembly Goals

The context assembly process should:

* include only context relevant to the current task
* include enough context for correct workflow behavior
* exclude restricted or irrelevant context
* distinguish known, inferred, retrieved, missing, restricted, uncertain, stale, and conflicting context
* respect role, tenant, account, and permission boundaries
* apply workflow-state rules before response generation
* apply language rules before output generation
* support traceability and diagnostics
* make context inclusion and exclusion decisions reviewable

System-specific goals:

* `[Goal]`
* `[Goal]`
* `[Goal]`

---

## 4. Assembly Sequence

Define the standard sequence for building a runtime context packet.

Recommended sequence:

1. Identify current workflow and workflow state.
2. Identify current task and expected output.
3. Identify user role, permission scope, and tenant/account boundary.
4. Load always-required system context.
5. Load workflow-state context.
6. Add current user input.
7. Add known session context.
8. Identify missing information.
9. Identify possible inferred context.
10. Trigger retrieval if required and permitted.
11. Filter retrieved context by relevance, permissions, freshness, and source reliability.
12. Apply language handling rules.
13. Apply output requirements.
14. Apply escalation rules.
15. Exclude prohibited or irrelevant context.
16. Label context source, status, confidence, and visibility.
17. Assemble runtime context packet.
18. Run pre-response validation checks.
19. Pass packet to model.
20. Log packet metadata for diagnostics, where allowed.

System-specific sequence:

```text
[Insert custom assembly sequence if different from above.]
```

---

## 5. Required Context by Workflow State

Define what context must be included at each workflow state.

| Workflow state | Required context     | Optional context     | Excluded context     | Notes     |
| -------------- | -------------------- | -------------------- | -------------------- | --------- |
| `[State]`      | `[Required context]` | `[Optional context]` | `[Excluded context]` | `[Notes]` |
| `[State]`      | `[Required context]` | `[Optional context]` | `[Excluded context]` | `[Notes]` |
| `[State]`      | `[Required context]` | `[Optional context]` | `[Excluded context]` | `[Notes]` |

Example workflow states:

* start
* intent detection
* information collection
* clarification
* retrieval
* review
* confirmation
* submission
* routing
* escalation
* follow-up
* correction
* recovery

---

## 6. Always-Included Context

Define context that should always be included in runtime packets for this workflow.

| Context     | Source     | Reason included | Notes     |
| ----------- | ---------- | --------------- | --------- |
| `[Context]` | `[Source]` | `[Reason]`      | `[Notes]` |
| `[Context]` | `[Source]` | `[Reason]`      | `[Notes]` |
| `[Context]` | `[Source]` | `[Reason]`      | `[Notes]` |

Examples:

* current task
* workflow state
* output requirements
* assistant role boundaries
* user role or permission scope when relevant
* language rules when relevant
* prohibited actions
* escalation triggers when relevant

---

## 7. Conditionally Included Context

Define context included only under specific conditions.

| Context     | Include when  | Exclude when  | Notes     |
| ----------- | ------------- | ------------- | --------- |
| `[Context]` | `[Condition]` | `[Condition]` | `[Notes]` |
| `[Context]` | `[Condition]` | `[Condition]` | `[Notes]` |
| `[Context]` | `[Condition]` | `[Condition]` | `[Notes]` |

Examples:

* case history is included only when the user is authenticated and permitted
* prior session summary is included only when the user resumes a workflow
* policy context is included only when classification, eligibility, or routing depends on it
* escalation rules are included when the workflow state can trigger escalation
* diagnostic context is included only in test or evaluation environments

---

## 8. Excluded Context

Define context that should be excluded from the model packet or user-facing output.

| Context     | Exclusion type                                                                     | Reason     | Notes     |
| ----------- | ---------------------------------------------------------------------------------- | ---------- | --------- |
| `[Context]` | `[Do not retrieve / do not pass to model / do not expose to user / internal only]` | `[Reason]` | `[Notes]` |
| `[Context]` | `[Do not retrieve / do not pass to model / do not expose to user / internal only]` | `[Reason]` | `[Notes]` |
| `[Context]` | `[Do not retrieve / do not pass to model / do not expose to user / internal only]` | `[Reason]` | `[Notes]` |

Common exclusions:

* records outside the user’s permission scope
* tenant or account data from another organization
* internal notes not visible to the user
* security-sensitive operational details
* hidden scoring or risk signals not approved for exposure
* irrelevant retrieved content
* stale context when freshness is required
* sensitive personal data not required for the task

---

## 9. Context Labeling Rules

All context included in the packet should be labeled clearly enough for the model and reviewers to understand its source and status.

Required labels may include:

| Label               | Required?   | Meaning                                                                        | Example                                           |
| ------------------- | ----------- | ------------------------------------------------------------------------------ | ------------------------------------------------- |
| `source`            | Yes         | Where the context came from                                                    | `user_message`, `service_catalog`, `case_history` |
| `status`            | Yes         | Known, inferred, retrieved, missing, restricted, uncertain, stale, conflicting | `inferred`                                        |
| `confidence`        | Conditional | Confidence in the context or inference                                         | `low`, `medium`, `high`                           |
| `visibility`        | Conditional | Whether context can appear in user-facing output                               | `user_visible`, `internal_only`                   |
| `permission_status` | Conditional | Whether the user is allowed to access the context                              | `allowed`, `restricted`, `unknown`                |
| `timestamp`         | Conditional | When context was created, retrieved, or last updated                           | `2026-05-18T14:30:00Z`                            |
| `relevance_reason`  | Conditional | Why retrieved context was included                                             | `[Reason]`                                        |

System-specific labels:

| Label     | Required?                  | Meaning     | Example     |
| --------- | -------------------------- | ----------- | ----------- |
| `[Label]` | `[Yes / No / Conditional]` | `[Meaning]` | `[Example]` |

---

## 10. Context Priority Rules

Define how the assembly layer should prioritize context when multiple sources are available.

Recommended priority principles:

* Permission and boundary rules override convenience.
* Current workflow state overrides prior assumptions.
* Confirmed user input overrides unconfirmed inference.
* Authoritative system records may override outdated user-provided status information.
* Fresh policy or service rules may override older retrieved documents.
* Restricted context should not appear in user-facing output even when relevant.
* Retrieved context should not override workflow rules unless explicitly allowed.

Priority order:

1. `[Highest priority context/source]`
2. `[Next priority context/source]`
3. `[Next priority context/source]`
4. `[Lowest priority context/source]`

---

## 11. Conflict Handling Rules

Define how context conflicts should be handled.

| Conflict type                              | Example     | Assembly behavior | Model instruction |
| ------------------------------------------ | ----------- | ----------------- | ----------------- |
| User input conflicts with system record    | `[Example]` | `[Behavior]`      | `[Instruction]`   |
| Retrieved sources conflict                 | `[Example]` | `[Behavior]`      | `[Instruction]`   |
| Inference conflicts with known information | `[Example]` | `[Behavior]`      | `[Instruction]`   |
| Workflow state conflicts with user request | `[Example]` | `[Behavior]`      | `[Instruction]`   |
| Language interpretation conflicts          | `[Example]` | `[Behavior]`      | `[Instruction]`   |

Default conflict guidance:

* Do not silently resolve material conflicts.
* Label conflicting context explicitly.
* Ask for confirmation when the conflict affects user intent, routing, eligibility, risk, or output quality.
* Escalate when policy, permission, safety, or compliance interpretation is required.

---

## 12. Missing Context Rules

Define what should happen when required or useful context is missing.

| Missing context | Blocking?    | Assistant behavior                   | Fallback     |
| --------------- | ------------ | ------------------------------------ | ------------ |
| `[Context]`     | `[Yes / No]` | `[Ask / infer / proceed / escalate]` | `[Fallback]` |
| `[Context]`     | `[Yes / No]` | `[Ask / infer / proceed / escalate]` | `[Fallback]` |
| `[Context]`     | `[Yes / No]` | `[Ask / infer / proceed / escalate]` | `[Fallback]` |

Default guidance:

* If missing context blocks the workflow, ask for it or escalate.
* If missing context improves quality but does not block progress, proceed with a labeled limitation.
* If a safe inference is possible, label it as inference and confirm when material.
* Do not ask for missing information that does not affect the workflow outcome.

---

## 13. Inference Assembly Rules

Define when inferred context may be included in the packet.

Inference may be included when:

* it is based on user input, system state, or retrieved context
* it improves workflow continuity or reduces unnecessary questions
* it is labeled as inference
* confidence is indicated when useful
* confirmation is requested when the inference is material

Inference should be excluded or blocked when:

* it is speculative
* it affects eligibility, rights, urgency, safety, or escalation without confirmation
* it conflicts with known information
* it depends on restricted context that cannot be used for the current role
* it may introduce bias or unsupported assumptions

Inference format:

```yaml
inferred_context:
  - inference: "[Inference]"
    basis: "[Evidence or reason]"
    confidence: "[low | medium | high]"
    requires_confirmation: true
    affects: "[routing | eligibility | urgency | output quality | none]"
```

---

## 14. Retrieval Assembly Rules

Define how retrieved context is selected and filtered.

Retrieval should be triggered when:

* the workflow requires external records or knowledge
* the user asks about existing status, policy, service rules, records, or prior cases
* related records may affect the next step
* routing, eligibility, or escalation depends on stored information
* current context is insufficient and retrieval is allowed

Retrieval should not be triggered when:

* the user has not granted or does not have required access
* the workflow state does not require retrieval
* retrieval would cross tenant, account, or permission boundaries
* the task can be completed with available context
* retrieving additional context would increase risk of irrelevant or misleading output

Retrieved context must be filtered by:

* permission status
* relevance
* source reliability
* freshness
* workflow state
* language compatibility
* output visibility

Retrieved context format:

```yaml
retrieved_context:
  - source: "[Source]"
    source_type: "[database | document | knowledge_base | case_history | policy | service_catalog | other]"
    item_id: "[ID]"
    summary: "[Relevant summary]"
    relevance_reason: "[Why included]"
    permission_status: "[allowed | restricted | internal_only | unknown]"
    visible_to_user: true
    timestamp: "[Timestamp]"
    confidence: "[low | medium | high]"
```

---

## 15. Restricted Context Assembly Rules

Define how restricted context is handled.

Restricted context may be:

* excluded entirely
* included only for internal routing
* included only for safety escalation
* included only for reviewer-facing output
* summarized in a user-safe way
* replaced with a generic limitation message

Restricted context should never:

* appear in user-facing output unless explicitly allowed
* be revealed indirectly through comparison or explanation
* be used to make decisions outside defined authorization
* be mixed with visible context without labels
* be passed to the model if policy requires exclusion before model access

Restricted context format:

```yaml
restricted_context:
  - context_type: "[Context type]"
    reason_restricted: "[Reason]"
    allowed_internal_use: "[none | routing_only | safety_only | reviewer_only | other]"
    user_facing_use_allowed: false
```

---

## 16. Language Assembly Rules

Define how language context is assembled.

Language context should include:

* detected input language
* expected output language
* user language preference if known
* official terms that should not be translated
* whether translation is required
* whether original user wording should be preserved
* uncertain translation notes

Language context should be included when:

* the user input is not in the system default language
* the user switches languages mid-flow
* retrieved context is in a different language
* translation affects routing, eligibility, risk, or meaning
* official terms or names must be preserved

Language context format:

```yaml
language_context:
  input_language: "[Language]"
  output_language: "[Language]"
  translation_required: false
  preserve_original_user_text: true
  terms_not_to_translate:
    - "[Term]"
  uncertain_translation_notes:
    - "[Note]"
```

---

## 17. Output Assembly Rules

Define how output requirements are included in the packet.

Output requirements should include:

* output type
* audience
* required format
* required fields
* prohibited content
* whether uncertainty should be included
* whether next steps should be included

Output context should change by workflow state and audience.

Example:

```yaml
output_requirements:
  output_type: "structured_record"
  audience: "internal_team"
  format: "yaml"
  required_fields:
    - "request_type"
    - "summary"
    - "known_information"
    - "missing_information"
    - "routing_recommendation"
  prohibited_content:
    - "restricted user data not needed by receiving team"
  include_uncertainty: true
  include_next_steps: true
```

---

## 18. Escalation Assembly Rules

Define when escalation context is included.

Escalation context should be included when:

* escalation is possible in the current workflow state
* the user reports urgency, harm, outage, risk, or safety concern
* policy interpretation is required
* permission issues block progress
* conflicting context requires human review
* repeated failed attempts occur
* the system cannot safely proceed

Escalation context format:

```yaml
escalation_context:
  escalation_required: false
  escalation_triggers:
    - "[Trigger]"
  escalation_owner: "[Owner]"
  escalation_summary_required: false
```

---

## 19. Pre-Response Validation Checks

Before passing the packet to the model, the system should check:

* Is the current task defined?
* Is workflow state included?
* Are role and permission rules applied?
* Is restricted context excluded or labeled correctly?
* Is retrieved context relevant and permitted?
* Is inference labeled as inference?
* Is missing information marked as blocking or non-blocking?
* Are language rules included when needed?
* Are output requirements clear?
* Are prohibited actions included when needed?
* Are escalation conditions included when relevant?
* Is the packet small enough to avoid unnecessary noise?
* Is the packet complete enough to support expected behavior?

---

## 20. Assembly Logging and Traceability

Define what should be logged for diagnostics and review.

Log when allowed:

* packet ID
* workflow state
* user role or permission scope
* context sources included
* context sources excluded
* retrieval query or trigger
* reason for inclusion or exclusion
* conflict flags
* missing context flags
* escalation triggers
* output type requested
* model response ID or trace ID

Do not log:

* sensitive data without approval
* restricted content beyond retention rules
* unnecessary personal data
* full model inputs if prohibited by policy

Logging should support layer-level diagnosis without creating avoidable privacy or security risk.

---

## 21. Example Assembly Decision Table

| Situation                                   | Include     | Exclude     | Label      | Assistant behavior |
| ------------------------------------------- | ----------- | ----------- | ---------- | ------------------ |
| User starts new request                     | `[Context]` | `[Context]` | `[Labels]` | `[Behavior]`       |
| User resumes existing request               | `[Context]` | `[Context]` | `[Labels]` | `[Behavior]`       |
| User lacks permission for related record    | `[Context]` | `[Context]` | `[Labels]` | `[Behavior]`       |
| Retrieved context conflicts with user input | `[Context]` | `[Context]` | `[Labels]` | `[Behavior]`       |
| User switches language mid-flow             | `[Context]` | `[Context]` | `[Labels]` | `[Behavior]`       |
| Escalation trigger detected                 | `[Context]` | `[Context]` | `[Labels]` | `[Behavior]`       |

---

## 22. Assembly Failure Patterns to Test

Test for these failure patterns:

* required context is missing from the packet
* irrelevant context is included and distracts the model
* restricted context appears in user-facing output
* permission checks are applied after retrieval instead of before exposure
* inferred context is not labeled
* stale context is not marked as stale
* conflicting context is not flagged
* workflow state is wrong or missing
* output requirements are unclear
* language rules are missing when needed
* model asks for information already present in the packet
* model acts on context that should have been excluded
* packet includes too much context for the current task
* packet excludes context needed for correct behavior

---

## 23. Open Questions

List unresolved context assembly decisions.

| Question     | Why it matters | Owner     | Status                        |
| ------------ | -------------- | --------- | ----------------------------- |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |

---

## 24. Related Documents

Link to related architecture documents.

* `[AI Design Principles]`
* `[AI Assistant Behavior Spec]`
* `[Workflow Assistant Behavior Spec]`
* `[Context Architecture Spec]`
* `[Runtime Context Template]`
* `[Language Handling Rules]`
* `[Workflow Routing Rules]`
* `[Related Record Context Rules]`
* `[Testing & Diagnostics Spec]`
* `[Conversation Flows]`

---

## 25. Change Log

| Date     | Change          | Owner     |
| -------- | --------------- | --------- |
| `[Date]` | `[Change made]` | `[Owner]` |
