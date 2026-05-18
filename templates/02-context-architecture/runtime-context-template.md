# Runtime Context Template

Use this document to define the structure of the runtime context packet passed to the model at a specific moment in the workflow.

A runtime context packet should describe what the model needs to know now to behave correctly. It should separate known information, inferred context, retrieved context, restricted context, workflow state, role, language rules, output requirements, uncertainty, and escalation conditions.

This template can be adapted into YAML, JSON, XML, Markdown, or another structured format depending on implementation needs.

---

## 1. Runtime Packet Overview

**System name:** `[Insert system/product/service name]`

**AI assistant name:** `[Insert assistant name]`

**Workflow name:** `[Insert workflow name]`

**Runtime packet version:** `[Insert version]`

**Document owner:** `[Insert owner/team]`

**Last updated:** `[Insert date]`

---

## 2. Purpose of the Runtime Context Packet

Describe what this packet is designed to do.

```text
This runtime context packet exists to provide the model with the task-specific, role-specific, workflow-specific, and permission-aware context needed to behave correctly at the current moment in the workflow.
```

Prompts:

* What behavior does this packet support?
* What workflow state does it represent?
* What context must always be included?
* What context is conditional?
* What context must be excluded?
* What output does the packet prepare the model to produce?

---

## 3. Runtime Context Packet Structure

Use this structure as the baseline runtime packet.

```yaml
runtime_context_packet:
  packet_metadata:
    packet_id: "[Unique packet ID]"
    packet_version: "[Version]"
    generated_at: "[Timestamp]"
    generated_by: "[System / service / orchestration layer]"
    workflow_name: "[Workflow name]"
    workflow_state: "[Current workflow state]"
    model_target: "[Model or model class if known]"

  task_context:
    current_task: "[What the model is being asked to do now]"
    task_goal: "[Expected workflow outcome]"
    task_stage: "[Stage within the workflow]"
    user_intent: "[Known or inferred user intent]"
    intent_status: "[known | inferred | ambiguous | unknown]"

  user_context:
    user_role: "[Role]"
    user_permission_scope: "[Permission scope]"
    user_language_preference: "[Language preference if known]"
    user_location_or_region: "[Optional / if relevant]"
    user_account_or_tenant_id: "[Optional / if permitted]"

  workflow_context:
    current_state: "[Workflow state]"
    prior_state: "[Previous workflow state if relevant]"
    next_expected_state: "[Expected next state]"
    completed_steps:
      - "[Completed step]"
    remaining_steps:
      - "[Remaining step]"
    interruption_or_recovery_status: "[none | interrupted | resumed | corrected | changed_direction]"

  known_information:
    - field: "[Field name]"
      value: "[Known value]"
      source: "[User / system / confirmed record]"
      confirmed: true
      notes: "[Optional notes]"

  missing_information:
    - field: "[Missing field]"
      required: true
      reason_needed: "[Why this information matters]"
      blocking: false
      question_to_ask: "[Question if needed]"

  inferred_context:
    - inference: "[Inference]"
      basis: "[What the inference is based on]"
      confidence: "[low | medium | high]"
      requires_confirmation: false
      affects: "[routing | eligibility | urgency | output quality | none]"

  retrieved_context:
    - source: "[Source name]"
      source_type: "[database | document | knowledge_base | case_history | policy | service_catalog | other]"
      retrieved_item_id: "[Record or item ID]"
      summary: "[Relevant summary]"
      relevance_reason: "[Why included]"
      timestamp: "[Source or retrieval timestamp]"
      confidence: "[low | medium | high]"
      visible_to_user: true
      permission_status: "[allowed | restricted | internal_only | unknown]"

  restricted_context:
    - context_type: "[Restricted context type]"
      reason_restricted: "[Reason]"
      allowed_internal_use: "[none | routing_only | safety_only | reviewer_only | other]"
      user_facing_use_allowed: false

  language_context:
    input_language: "[Detected or selected language]"
    output_language: "[Expected output language]"
    translation_required: false
    preserve_original_user_text: true
    terms_not_to_translate:
      - "[Term]"
    uncertain_translation_notes:
      - "[Note]"

  output_requirements:
    output_type: "[answer | clarification_question | summary | structured_record | routing_recommendation | escalation_note | other]"
    audience: "[user | internal_team | system | reviewer]"
    format: "[plain_text | markdown | json | yaml | structured_fields | other]"
    required_fields:
      - "[Field]"
    prohibited_content:
      - "[Content type]"
    include_uncertainty: true
    include_next_steps: true

  allowed_actions:
    - action: "[Allowed action]"
      condition: "[Condition]"

  prohibited_actions:
    - action: "[Prohibited action]"
      reason: "[Reason]"

  escalation_context:
    escalation_required: false
    escalation_triggers:
      - "[Trigger if present]"
    escalation_owner: "[Human role / team / system]"
    escalation_summary_required: false

  diagnostic_context:
    test_case_id: "[Optional test case ID]"
    expected_behavior: "[Expected behavior if used for testing]"
    known_risks:
      - "[Risk]"
    failure_patterns_to_watch:
      - "[Failure pattern]"
```

---

## 4. Required Packet Sections

Define which sections must be included for this workflow.

| Packet section        | Required?   | When required                                                                    | Notes                                                       |
| --------------------- | ----------- | -------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `packet_metadata`     | Yes         | Always                                                                           | Identifies packet version, workflow, and generation context |
| `task_context`        | Yes         | Always                                                                           | Defines what the model is being asked to do                 |
| `user_context`        | Conditional | Required when behavior depends on role, permission, language, or account context | `[Notes]`                                                   |
| `workflow_context`    | Yes         | Always for workflow-aware systems                                                | Defines state and continuity                                |
| `known_information`   | Conditional | Required when user/system facts affect output                                    | `[Notes]`                                                   |
| `missing_information` | Conditional | Required when workflow may be incomplete                                         | `[Notes]`                                                   |
| `inferred_context`    | Conditional | Required when inference affects behavior or output                               | `[Notes]`                                                   |
| `retrieved_context`   | Conditional | Required when retrieval is triggered                                             | `[Notes]`                                                   |
| `restricted_context`  | Conditional | Required when restrictions affect reasoning or output                            | `[Notes]`                                                   |
| `language_context`    | Conditional | Required for multilingual or language-sensitive workflows                        | `[Notes]`                                                   |
| `output_requirements` | Yes         | Always                                                                           | Defines what the model should produce                       |
| `allowed_actions`     | Conditional | Required when assistant can take or recommend actions                            | `[Notes]`                                                   |
| `prohibited_actions`  | Conditional | Required when constraints affect behavior                                        | `[Notes]`                                                   |
| `escalation_context`  | Conditional | Required when escalation is possible                                             | `[Notes]`                                                   |
| `diagnostic_context`  | Conditional | Required for testing and evaluation                                              | `[Notes]`                                                   |

---

## 5. Packet Metadata

Packet metadata should help teams trace which context was assembled, when, and for what purpose.

Required metadata:

* packet ID
* packet version
* generated timestamp
* workflow name
* workflow state
* context assembly source or service

Optional metadata:

* model target
* environment
* tenant/account ID if allowed
* test case ID
* session ID
* trace ID

Example:

```yaml
packet_metadata:
  packet_id: "ctx-00001"
  packet_version: "0.1"
  generated_at: "2026-05-18T14:30:00Z"
  generated_by: "context-orchestration-service"
  workflow_name: "[Workflow name]"
  workflow_state: "clarification"
  model_target: "reasoning_model"
```

---

## 6. Task Context

Task context defines what the model is being asked to do now.

The task should be specific to the current moment, not a general assistant mission.

Examples:

```yaml
task_context:
  current_task: "Ask one clarification question needed to route the request."
  task_goal: "Collect missing location information before creating a case record."
  task_stage: "missing_information_clarification"
  user_intent: "Report a recurring service issue."
  intent_status: "inferred"
```

Task context should answer:

* What should the model do in this turn?
* What is the workflow outcome?
* What stage is the interaction in?
* Is user intent known, inferred, ambiguous, or unknown?

---

## 7. User Context

User context should include only information needed for behavior, permissions, language, routing, or output.

Examples:

```yaml
user_context:
  user_role: "resident"
  user_permission_scope: "own_cases_only"
  user_language_preference: "English"
  user_location_or_region: "[Region if relevant]"
  user_account_or_tenant_id: "[Tenant ID if permitted]"
```

Guidance:

* Include role when behavior or visibility depends on role.
* Include tenant or account context only when permitted and necessary.
* Avoid including unnecessary personal data.
* Do not include sensitive user attributes unless required and approved.

---

## 8. Workflow Context

Workflow context defines where the user is in the process and what has already happened.

Examples:

```yaml
workflow_context:
  current_state: "information_collection"
  prior_state: "intent_detection"
  next_expected_state: "user_review"
  completed_steps:
    - "request_type_detected"
    - "initial_description_collected"
  remaining_steps:
    - "collect_location"
    - "confirm_summary"
    - "route_case"
  interruption_or_recovery_status: "none"
```

Workflow context should prevent the assistant from behaving like each turn is a new conversation.

---

## 9. Known Information

Known information should include facts that are explicitly provided, confirmed, or system-known.

Example:

```yaml
known_information:
  - field: "request_type"
    value: "service_issue"
    source: "user_message"
    confirmed: false
    notes: "Detected from user wording; user has not explicitly selected category."
```

Guidance:

* Label source.
* Mark whether confirmed.
* Preserve user-provided wording when important.
* Avoid overwriting known information with inference.

---

## 10. Missing Information

Missing information should include only information that matters for the workflow outcome.

Example:

```yaml
missing_information:
  - field: "location"
    required: true
    reason_needed: "Needed to route the issue to the correct service team."
    blocking: true
    question_to_ask: "Where is this issue happening?"
```

Guidance:

* Ask only for information that affects routing, eligibility, urgency, safety, output quality, or completion.
* Mark whether missing information blocks progress.
* Include the reason the information is needed.
* Include the question to ask when useful.

---

## 11. Inferred Context

Inferred context should be labeled clearly.

Example:

```yaml
inferred_context:
  - inference: "The user is reporting an issue rather than asking a general question."
    basis: "User described something not working and requested help."
    confidence: "medium"
    requires_confirmation: true
    affects: "routing"
```

Guidance:

* Inference should not be treated as confirmed fact.
* Confirm material inferences when they affect routing, eligibility, risk, escalation, or output quality.
* Include the basis for the inference when reviewability matters.

---

## 12. Retrieved Context

Retrieved context should be included only when relevant, permitted, and labeled.

Example:

```yaml
retrieved_context:
  - source: "service_catalog"
    source_type: "service_catalog"
    retrieved_item_id: "svc-123"
    summary: "Streetlight maintenance requests are handled by the municipal infrastructure team."
    relevance_reason: "User appears to be reporting a streetlight outage."
    timestamp: "2026-05-18T14:20:00Z"
    confidence: "high"
    visible_to_user: true
    permission_status: "allowed"
```

Guidance:

* Include source and source type.
* Include relevance reason.
* Include permission status.
* Include timestamp when freshness matters.
* Do not include retrieved context just because it exists.

---

## 13. Restricted Context

Restricted context should be explicitly labeled and protected.

Example:

```yaml
restricted_context:
  - context_type: "internal_case_notes"
    reason_restricted: "Visible only to service team members."
    allowed_internal_use: "routing_only"
    user_facing_use_allowed: false
```

Guidance:

* Restricted context should not appear in user-facing output unless explicitly allowed.
* Avoid indirect leakage through explanations, comparisons, summaries, or reasoning traces.
* If restricted context influences internal routing, define how that influence can be represented safely.

---

## 14. Language Context

Language context defines how input and output language should be handled.

Example:

```yaml
language_context:
  input_language: "Arabic"
  output_language: "Arabic"
  translation_required: false
  preserve_original_user_text: true
  terms_not_to_translate:
    - "[Official service name]"
  uncertain_translation_notes:
    - "User used informal wording that may map to multiple service categories."
```

Guidance:

* Preserve original wording when it affects review, routing, legal meaning, or service resolution.
* Do not translate official terms that must remain unchanged.
* Flag uncertain translations when they affect workflow decisions.
* Define behavior when retrieved context is in a different language from the user input.

---

## 15. Output Requirements

Output requirements should tell the model exactly what kind of response to produce.

Example:

```yaml
output_requirements:
  output_type: "clarification_question"
  audience: "user"
  format: "plain_text"
  required_fields:
    - "one_question"
    - "brief_reason"
  prohibited_content:
    - "internal routing logic"
    - "restricted case history"
  include_uncertainty: false
  include_next_steps: true
```

Guidance:

* Define audience.
* Define format.
* Define required fields.
* Define prohibited content.
* Separate user-facing output from internal structured output when needed.

---

## 16. Allowed and Prohibited Actions

Define what the assistant can and cannot do in the current context.

Example:

```yaml
allowed_actions:
  - action: "ask_clarifying_question"
    condition: "required routing information is missing"
  - action: "prepare_structured_record"
    condition: "minimum required information is present"

prohibited_actions:
  - action: "submit_case_without_confirmation"
    reason: "User must review the structured case before submission."
  - action: "show_internal_case_notes"
    reason: "Restricted to service team members."
```

Guidance:

* Allowed actions should be tied to conditions.
* Prohibited actions should include reasons.
* Irreversible or externally visible actions usually require confirmation.

---

## 17. Escalation Context

Escalation context defines whether escalation is required or possible.

Example:

```yaml
escalation_context:
  escalation_required: true
  escalation_triggers:
    - "safety_risk_reported"
  escalation_owner: "service_operations_team"
  escalation_summary_required: true
```

Guidance:

* Escalation triggers should be defined before runtime.
* If escalation is required, the model should not continue as if it can resolve the issue alone.
* Escalation summaries should include known information, missing information, urgency, and next step.

---

## 18. Diagnostic Context

Diagnostic context is optional but useful during testing and evaluation.

Example:

```yaml
diagnostic_context:
  test_case_id: "tc-clarification-001"
  expected_behavior: "Assistant asks only for missing location information."
  known_risks:
    - "over-questioning"
    - "incorrect routing"
  failure_patterns_to_watch:
    - "asks for information already provided"
    - "routes before required information is collected"
```

Guidance:

* Include diagnostic context in test environments.
* Keep diagnostic context out of production packets unless needed for monitoring.
* Use diagnostic context to trace whether failure came from prompt, retrieval, packet assembly, workflow state, permissions, output schema, or model behavior.

---

## 19. Minimal Runtime Packet

Use this smaller version when the workflow is simple or when prototyping.

```yaml
runtime_context_packet:
  task:
    current_task: "[Task]"
    workflow_state: "[State]"
  user:
    role: "[Role]"
    permission_scope: "[Scope]"
  context:
    known:
      - "[Known information]"
    missing:
      - "[Missing information]"
    inferred:
      - "[Inference, labeled as inference]"
    retrieved:
      - "[Retrieved context with source]"
    restricted:
      - "[Restricted context not for user-facing output]"
  language:
    input_language: "[Language]"
    output_language: "[Language]"
  output:
    type: "[Output type]"
    format: "[Format]"
  constraints:
    allowed_actions:
      - "[Allowed action]"
    prohibited_actions:
      - "[Prohibited action]"
    escalation_required: false
```

---

## 20. Runtime Packet Review Checklist

Before using a runtime packet, check:

* Is the current task clear?
* Is the workflow state included?
* Is user role or permission scope included if needed?
* Is known information separated from inference?
* Is retrieved context labeled with source and permission status?
* Is restricted context protected from user-facing output?
* Is missing information listed only when it matters?
* Are language rules included when relevant?
* Is the required output type clear?
* Are prohibited actions listed?
* Are escalation conditions included when relevant?
* Can failures be traced back to packet contents?

---

## 21. Open Questions

List unresolved runtime packet decisions.

| Question     | Why it matters | Owner     | Status                        |
| ------------ | -------------- | --------- | ----------------------------- |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |

---

## 22. Related Documents

Link to related architecture documents.

* `[AI Design Principles]`
* `[AI Assistant Behavior Spec]`
* `[Workflow Assistant Behavior Spec]`
* `[Context Architecture Spec]`
* `[Context Assembly Rules]`
* `[Language Handling Rules]`
* `[Workflow Routing Rules]`
* `[Related Record Context Rules]`
* `[Testing & Diagnostics Spec]`
* `[Conversation Flows]`

---

## 23. Change Log

| Date     | Change          | Owner     |
| -------- | --------------- | --------- |
| `[Date]` | `[Change made]` | `[Owner]` |
