# Context Assembly Rules

This document defines how context is selected, filtered, prioritized, labeled, assembled, and passed into the model for the fictional **Public Service Request & Feedback Triage Assistant**.

The purpose is to make the assistant’s operating environment explicit before the model is asked to respond.

---

## 1. Assembly Rules Overview

**System name:** Public Service Request & Feedback Triage Assistant

**Workflow name:** Public Service Request & Feedback Triage

**Context architecture version:** 0.1 fictional sample

**Sample status:** Fictional public-safe architecture example

---

## 2. Purpose of Context Assembly

These context assembly rules exist to define what the model receives at each point in the public service request and feedback workflow.

The assembly layer should ensure that the assistant receives enough context to behave correctly while excluding restricted, irrelevant, stale, or unsupported context.

The assistant should not have to infer the whole operating environment from a user message alone.

---

## 3. Assembly Goals

The context assembly process should:

* include the current task and workflow state
* include user role and permission scope when they affect behavior
* include known user-provided information
* include missing required information when the workflow is incomplete
* include inferred context only when it is labeled and useful
* retrieve service catalog or case history only when triggered and permitted
* apply language rules before output generation
* apply routing and escalation rules when relevant
* exclude restricted or irrelevant context
* label context by source, status, confidence, and visibility
* support review and diagnostics when behavior fails

---

## 4. Assembly Sequence

Recommended runtime assembly sequence:

1. Identify current workflow state.
2. Identify current task and expected output.
3. Identify user role and authentication status.
4. Determine permission scope.
5. Add current user input and preserve original wording when needed.
6. Add available interface or form state.
7. Add known session context and confirmed prior information.
8. Identify missing information required for the current workflow state.
9. Identify safe inferences and label them.
10. Apply language detection and language handling rules.
11. Trigger service catalog retrieval if classification or routing requires it.
12. Trigger case history retrieval if follow-up, duplicate, repeated issue, or related-case handling requires it and permissions allow it.
13. Filter retrieved context by relevance, permission, freshness, source, and visibility.
14. Apply routing rules.
15. Apply escalation rules.
16. Define output requirements and prohibited content.
17. Exclude restricted, irrelevant, or unsupported context.
18. Assemble the runtime context packet.
19. Run pre-response validation checks.
20. Pass the packet to the model.
21. Log packet metadata for diagnostics when allowed.

---

## 5. Required Context by Workflow State

| Workflow state         | Required context                                                             | Optional context                                           | Excluded context                                                    | Notes                                                     |
| ---------------------- | ---------------------------------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------- | --------------------------------------------------------- |
| Start                  | User message, entry point, current task, output requirements                 | User role if known                                         | Case history unless triggered                                       | Assistant should orient and detect intent                 |
| Intent detection       | User wording, language context, request type signals                         | Interface context, service catalog if category is selected | Restricted case history                                             | Inference must be labeled if not confirmed                |
| Information collection | Known fields, missing fields, reason each missing field is needed            | Service category, prior session summary                    | Unrelated retrieved records                                         | Ask only for missing required information                 |
| Case history check     | User role, permission scope, case ID or related-record trigger               | Permitted own case history                                 | Other users’ case history, restricted records in user-facing output | Permission check happens before exposure                  |
| Review                 | User-facing summary, known info, inferred info, missing info, language notes | Retrieved public context                                   | Internal-only notes                                                 | User must review material restructuring before submission |
| Routing                | Request type, service category, known fields, routing rules, confidence      | Service catalog, related-case signal                       | Hidden records not allowed for receiving role                       | Routing reason should be reviewable                       |
| Escalation             | Escalation trigger, known info, missing info, urgency/risk signal, owner     | Restricted context only if allowed for escalation          | Normal routing output if escalation supersedes it                   | Escalation can override normal flow                       |
| Recovery               | Prior state, confirmed info, changed info, remaining steps                   | Prior session summary                                      | Stale assumptions not revalidated                                   | Preserve progress without forcing restart                 |

---

## 6. Always-Included Context

| Context                   | Source                       | Reason included                            | Notes                                    |
| ------------------------- | ---------------------------- | ------------------------------------------ | ---------------------------------------- |
| Current task              | Workflow orchestration layer | Defines what the model should do now       | Should be specific to the current turn   |
| Workflow state            | Workflow state manager       | Prevents generic assistant behavior        | Required for workflow-aware behavior     |
| Output requirements       | Context assembly layer       | Prevents open-ended or malformed responses | Include audience and format              |
| User input                | User message                 | Primary source of intent and details       | Preserve original wording when needed    |
| Assistant role boundaries | Behavior spec                | Prevents overreach                         | Include prohibited actions when relevant |

---

## 7. Conditionally Included Context

| Context               | Include when                                                          | Exclude when                                                           | Notes                                        |
| --------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------- | -------------------------------------------- |
| User role             | User is authenticated or role is known                                | Role is unknown and not needed                                         | Unknown users should be treated as anonymous |
| Permission scope      | Any retrieval, case history, or restricted context may be used        | No role-dependent context is needed                                    | Required before case history exposure        |
| Service catalog       | Category, routing, or public answer depends on official service info  | User is only providing free-form feedback and no routing is needed yet | Include official labels and source           |
| Case history          | User follows up, provides case ID, or related-case check is triggered | User is anonymous or permission is unknown                             | Must be permission-bound                     |
| Prior session summary | User resumes or recovery state is active                              | New request with no prior state                                        | Use only confirmed prior context             |
| Language context      | User language, translation, or terminology affects meaning            | Language has no effect on workflow output                              | Preserve original wording when needed        |
| Escalation rules      | Current state can trigger escalation                                  | Escalation cannot occur in current flow                                | Include owner and required summary fields    |
| Diagnostic context    | Running tests or evaluation                                           | Normal user-facing production flow                                     | Keep separate from user-facing output        |

---

## 8. Excluded Context

The following context should be excluded from the model packet or user-facing output unless explicitly permitted.

| Context                            | Exclusion type            | Reason                              | Notes                                               |
| ---------------------------------- | ------------------------- | ----------------------------------- | --------------------------------------------------- |
| Other users’ case history          | Do not retrieve or expose | Permission boundary                 | Never include in public user-facing packet          |
| Internal service notes             | Internal only             | Restricted operational context      | May be available to reviewer or service team only   |
| Restricted related-case details    | Do not expose to user     | Prevents indirect leakage           | May support routing only when allowed               |
| Stale service catalog entries      | Exclude or label stale    | Prevents outdated routing           | Retrieve current source when possible               |
| Hidden scoring or priority signals | Internal only             | Not approved for public explanation | Use only if policy allows                           |
| Diagnostic test metadata           | Test only                 | Not part of normal workflow         | Exclude from production packets                     |
| Irrelevant retrieved content       | Exclude                   | Reduces model distraction           | Retrieval should be filtered before packet assembly |
| Unnecessary personal data          | Exclude                   | Data minimization                   | Include only fields needed for workflow             |

---

## 9. Context Labeling Rules

All included context should be labeled with enough information for the assistant and reviewers to interpret it.

Required or conditional labels:

| Label               | Required?   | Meaning                                                                        | Sample value                                      |
| ------------------- | ----------- | ------------------------------------------------------------------------------ | ------------------------------------------------- |
| `source`            | Yes         | Where the context came from                                                    | `user_message`, `service_catalog`, `case_history` |
| `status`            | Yes         | Known, inferred, retrieved, missing, restricted, uncertain, stale, conflicting | `inferred`                                        |
| `confidence`        | Conditional | Confidence in classification, retrieval, match, or inference                   | `medium`                                          |
| `visibility`        | Conditional | Whether context can appear in user-facing output                               | `user_visible`, `internal_only`                   |
| `permission_status` | Conditional | Whether current user can access the context                                    | `allowed`, `restricted`, `unknown`                |
| `relevance_reason`  | Conditional | Why retrieved context was included                                             | `Needed for service routing`                      |
| `timestamp`         | Conditional | When source was created, updated, or retrieved                                 | `2026-05-19T00:00:00Z`                            |

---

## 10. Context Priority Rules

Priority order:

1. Permission and visibility rules
2. Current workflow state
3. Confirmed user-provided information
4. Current system records from approved sources
5. Service catalog and routing rules
6. Permitted case history
7. Language handling rules
8. Prior session summary
9. Labeled inference
10. Low-confidence or uncertain context

Priority guidance:

* Permission rules override convenience.
* Confirmed user input overrides inference.
* Workflow state determines what context matters now.
* Service catalog context should use official category labels.
* Related-case context should not override current user intent without review.
* Low-confidence inference should not drive routing or escalation alone.
* Restricted context should not appear in user-facing output.

---

## 11. Conflict Handling Rules

| Conflict type                              | Example                                                              | Assembly behavior                                    | Model instruction                     |
| ------------------------------------------ | -------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------- |
| User input conflicts with case status      | User says case is unresolved; permitted system status says closed    | Label conflict and include both if visible           | Ask clarification or route for review |
| Retrieved sources conflict                 | Service catalog and knowledge base imply different teams             | Include conflict flag and route for review           | Do not choose silently                |
| Inference conflicts with known info        | Assistant infers complaint, but user says they only want information | Prioritize user clarification                        | Confirm before classification         |
| Workflow state conflicts with user request | User asks to submit but required info is missing                     | Mark missing info as blocking                        | Ask targeted question                 |
| Language interpretation conflicts          | User term maps to multiple categories                                | Preserve original wording and include ambiguity note | Ask clarification                     |

Default rules:

* Do not silently resolve material conflicts.
* Label conflict explicitly.
* Ask the user when user input can resolve the conflict.
* Route to human review when policy, permission, or service ownership is unclear.

---

## 12. Missing Context Rules

| Missing context                       | Blocking?            | Assistant behavior                                         | Fallback                                                           |
| ------------------------------------- | -------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------ |
| User message                          | Yes                  | Cannot proceed                                             | Ask user to describe need                                          |
| Location for location-dependent issue | Yes                  | Ask one targeted question                                  | Do not route until location is provided                            |
| Request type                          | Conditional          | Infer if high confidence; confirm if ambiguous             | Ask clarification                                                  |
| Service category                      | Conditional          | Retrieve service catalog or infer with label               | Route for review if unresolved                                     |
| Case ID for follow-up                 | Conditional          | Ask if user wants case-specific follow-up                  | Treat as new request if user cannot provide ID and workflow allows |
| Permission status                     | Yes for case history | Do not expose case history                                 | Continue without case history or route for review                  |
| Language confidence                   | Conditional          | Ask clarification or preserve original wording             | Route for language review if material                              |
| Escalation owner                      | Conditional          | Flag escalation required and route to default review queue | Do not promise resolution                                          |

---

## 13. Inference Assembly Rules

Inference may be included when:

* it is grounded in user wording, workflow state, or retrieved context
* it helps reduce unnecessary questions
* it is labeled clearly
* confidence is included
* confirmation is requested when material

Inference should be blocked or treated cautiously when:

* it affects routing with medium or low confidence
* it affects escalation, urgency, eligibility, or user rights
* it conflicts with known information
* it depends on restricted context that cannot be used for the current role
* it would minimize complaint, harm, or urgency signals

Sample inference format:

```yaml
inferred_context:
  - inference: "The user is likely reporting an issue rather than asking a general question."
    basis: "User described something not working."
    confidence: "high"
    requires_confirmation: false
    affects: "request_type"
```

---

## 14. Retrieval Assembly Rules

Retrieval should be triggered when:

* the user asks a question answerable from approved knowledge base content
* service category or routing depends on service catalog context
* the user provides a case ID
* the user asks for follow-up on an existing case
* duplicate or related-case detection is required
* escalation owner or service team must be identified

Retrieval should not be triggered when:

* permission scope is unknown and the request involves case history
* the user is anonymous and asks for case-specific information
* retrieval would cross account, tenant, or ownership boundaries
* the workflow can proceed with current context
* retrieval would add irrelevant or misleading information

Retrieved context must be filtered by:

* permission status
* relevance
* source reliability
* freshness
* workflow state
* language compatibility
* output visibility

---

## 15. Restricted Context Assembly Rules

Restricted context may be:

* excluded entirely
* used only for internal routing
* used only for escalation
* used only for reviewer context
* represented through a safe limitation message

Restricted context should never:

* appear in public user-facing output unless explicitly allowed
* be revealed indirectly through comparisons or hints
* be mixed with user-visible context without labels
* drive user-facing claims without approved explanation
* be passed into a packet if policy requires pre-model exclusion

Sample restricted context format:

```yaml
restricted_context:
  - context_type: "internal_case_note"
    reason_restricted: "Visible only to assigned service team."
    allowed_internal_use: "reviewer_only"
    user_facing_use_allowed: false
```

---

## 16. Language Assembly Rules

Language context should be included when:

* the user writes in a supported non-default language
* the user switches languages mid-flow
* user wording may affect classification or routing
* official service terms should not be translated
* original wording should be preserved for review
* retrieved context is in a different language
* language ambiguity affects urgency, risk, or escalation

Language context should include:

```yaml
language_context:
  input_language: "[Detected language]"
  input_language_confidence: "[low | medium | high]"
  output_language: "[Expected language]"
  translation_required: false
  preserve_original_user_text: true
  official_terms_preserved:
    - "[Official term]"
  uncertain_translation_notes:
    - "[Note]"
  language_review_required: false
```

---

## 17. Routing Assembly Rules

Routing context should be included when:

* request type is known or sufficiently inferred
* service category is known or sufficiently inferred
* required routing information is available
* service catalog or routing rules identify a likely destination
* escalation does not supersede normal routing

Routing context should include:

```yaml
routing_context:
  routing_required: true
  recommended_destination: "[Service team / review queue / escalation queue]"
  routing_confidence: "[low | medium | high]"
  routing_reason:
    user_visible: "[Safe explanation]"
    internal: "[Internal reason if permitted]"
  human_review_required: false
  open_routing_questions:
    - "[Question]"
```

Routing should not occur when:

* required routing information is missing
* request type is too ambiguous
* permission status is unresolved for needed case context
* escalation is required
* service category conflict needs review

---

## 18. Escalation Assembly Rules

Escalation context should be included when:

* the user reports safety risk, harm, danger, or urgent disruption
* repeated unresolved issue is reported
* user disputes prior handling
* permission or policy conflict prevents normal handling
* language ambiguity affects urgency or risk
* retrieved context conflicts with user input in a material way
* system cannot safely continue

Escalation context format:

```yaml
escalation_context:
  escalation_required: true
  escalation_triggers:
    - "[Trigger]"
  escalation_owner: "[Team or role]"
  escalation_summary_required: true
```

If escalation is required, normal routing should pause unless the escalation path requires a routing destination.

---

## 19. Output Assembly Rules

Output requirements should include:

* output type
* audience
* format
* required fields
* prohibited content
* uncertainty handling
* next-step handling

Sample output requirements:

```yaml
output_requirements:
  output_type: "clarification_question"
  audience: "user"
  format: "plain_text"
  required_fields:
    - "one_question"
    - "brief_reason"
  prohibited_content:
    - "internal_routing_logic"
    - "restricted_case_details"
  include_uncertainty: false
  include_next_steps: false
```

Output context should change by workflow state.

| Workflow state         | Output type                                         |
| ---------------------- | --------------------------------------------------- |
| Intent detection       | Request type confirmation or clarification question |
| Information collection | Missing information question                        |
| Review                 | User-facing summary and confirmation prompt         |
| Routing                | Structured case record and routing recommendation   |
| Escalation             | Escalation summary and user-safe acknowledgement    |
| Recovery               | Recovery summary and next question                  |

---

## 20. Pre-Response Validation Checks

Before passing the packet to the model, the system should check:

* Is the current task defined?
* Is workflow state included?
* Are user role and permission scope included when needed?
* Is known information separated from inference?
* Is missing information marked as blocking or non-blocking?
* Is retrieved context relevant and permitted?
* Is restricted context excluded or labeled correctly?
* Are language rules included when needed?
* Are routing and escalation rules included when relevant?
* Are output requirements clear?
* Are prohibited actions clear?
* Is the packet focused enough for the current task?
* Is the packet complete enough to support expected behavior?

---

## 21. Assembly Logging and Traceability

Log when allowed:

* packet ID
* workflow state
* user role and permission scope
* request type and service category status
* context sources included
* context sources excluded
* retrieval triggers
* related-case check status
* permission flags
* language flags
* routing confidence
* escalation triggers
* output type requested
* model response ID or trace ID

Do not log:

* unnecessary personal data
* restricted case contents beyond approved retention rules
* internal notes in user-visible logs
* full model input if prohibited by policy

Logging should support diagnostics without creating avoidable privacy or security risk.

---

## 22. Example Assembly Decision Table

| Situation                     | Include                                                           | Exclude                             | Label                                   | Assistant behavior                        |
| ----------------------------- | ----------------------------------------------------------------- | ----------------------------------- | --------------------------------------- | ----------------------------------------- |
| User starts new issue report  | User message, workflow state, inferred request type, missing info | Case history unless triggered       | Inference and missing info              | Ask targeted question or prepare summary  |
| User provides case ID         | User role, permission scope, case retrieval trigger               | Records outside scope               | Retrieved context and permission status | Check permitted case history              |
| User is anonymous             | Public service info, current input                                | Case history                        | Role as anonymous                       | Provide general support or limited intake |
| Related restricted case found | Restricted context, allowed internal use, routing note            | User-facing case details            | Restricted and internal-only            | Avoid leakage; route for review           |
| User switches language        | Original text, language confidence, translation notes             | Unsupported translation assumptions | Language context                        | Clarify if meaning affects routing        |
| Escalation trigger detected   | Escalation context, known info, missing info                      | Normal routing output if superseded | Escalation required                     | Prepare escalation summary                |

---

## 23. Assembly Failure Patterns to Test

Test for these failure patterns:

* required context is missing from the packet
* assistant receives workflow state but ignores it
* packet includes irrelevant retrieved context
* restricted case history appears in user-facing output
* inference is unlabeled
* missing information is not marked as blocking
* language ambiguity is not included when material
* service catalog context is stale or missing
* routing happens without required fields
* escalation trigger is omitted
* output requirements are too vague
* assistant asks for information already present in packet
* packet excludes context needed for human review

---

## 24. Open Questions

| Question                                                                                     | Why it matters                         | Likely owner             | Status |
| -------------------------------------------------------------------------------------------- | -------------------------------------- | ------------------------ | ------ |
| Which fields are always required in runtime packets?                                         | Affects implementation and testing     | Engineering / product    | Open   |
| Which context must be excluded before model access rather than only from user-facing output? | Affects privacy and security           | Security / policy        | Open   |
| What logs are allowed for diagnostic traceability?                                           | Affects testing and governance         | Security / engineering   | Open   |
| What service catalog metadata is required for routing?                                       | Affects routing confidence             | Operations / engineering | Open   |
| What context should be available to reviewers but not users?                                 | Affects review outputs and permissions | Operations / policy      | Open   |

---

## 25. Related Documents

* `sample-scope-and-assumptions.md`
* `ai-design-principles.md`
* `ai-assistant-behavior-spec.md`
* `public-service-request-feedback-triage-assistant-behavior-spec.md`
* `context-architecture-spec.md`
* `runtime-context-template.md`
* `language-handling-rules.md`
* `service-category-routing-rules.md`
* `case-history-context-rules.md`
* `example-context-packets.md`
* `testing-diagnostics-spec.md`
* `public-service-request-feedback-conversation-flows.md`
