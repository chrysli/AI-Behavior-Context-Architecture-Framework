# Runtime Context Template

This document defines the sample runtime context packet structure for the fictional **Public Service Request & Feedback Triage Assistant**.

A runtime context packet describes what the model needs to know at a specific moment in the workflow to behave correctly.

This sample is written in YAML-style structure for readability. A real implementation could adapt this into JSON, XML, Markdown, typed objects, database records, or orchestration-layer payloads.

---

## 1. Runtime Context Purpose

The runtime context packet exists to give the assistant the task-specific, workflow-specific, role-aware, permission-aware, and language-aware context needed to support public service request and feedback triage.

The packet should help the assistant:

* understand the current workflow state
* distinguish known, missing, inferred, retrieved, restricted, and uncertain context
* ask only necessary questions
* preserve user intent
* avoid permission leakage
* prepare structured outputs
* route or escalate when appropriate
* support human review and diagnostics

---

## 2. Baseline Runtime Context Packet

```yaml
runtime_context_packet:
  packet_metadata:
    packet_id: "[unique_packet_id]"
    packet_version: "0.1"
    generated_at: "[timestamp]"
    generated_by: "context_assembly_layer"
    system_name: "Public Service Request & Feedback Triage Assistant"
    workflow_name: "public_service_request_feedback_triage"
    workflow_state: "[start | intent_detection | information_collection | case_history_check | review | routing | escalation | recovery]"
    sample_status: "fictional_example"

  task_context:
    current_task: "[What the assistant should do now]"
    task_goal: "[Expected workflow outcome]"
    request_type: "[question | complaint | service_request | issue_report | feedback | follow_up | unknown]"
    request_type_status: "[known | inferred | ambiguous | unknown]"
    service_category: "[Category if known]"
    service_category_status: "[known | inferred | ambiguous | unknown]"

  user_context:
    user_role: "[anonymous_user | authenticated_resident | authenticated_business_owner | internal_service_team_member | reviewer | administrator]"
    authentication_status: "[anonymous | authenticated | unknown]"
    permission_scope: "[public_only | own_cases_only | business_cases | assigned_cases | review_queue | admin_config]"
    user_language_preference: "[Language if known]"
    user_region_or_jurisdiction: "[Optional / if relevant]"

  workflow_context:
    current_state: "[Current workflow state]"
    prior_state: "[Previous workflow state if relevant]"
    next_expected_state: "[Expected next state]"
    completed_steps:
      - "[Completed step]"
    remaining_steps:
      - "[Remaining step]"
    interruption_or_recovery_status: "[none | interrupted | resumed | corrected | changed_direction]"

  user_input_context:
    original_user_text:
      language: "[Detected language]"
      text: "[Original user wording]"
      preserve_for_review: true
    normalized_summary:
      text: "[Assistant/system summary if available]"
      status: "[draft | confirmed | needs_review]"
    attachments:
      - attachment_type: "[image | document | screenshot | other]"
        included_in_context: false
        notes: "[Notes]"

  known_information:
    - field: "[Field name]"
      value: "[Known value]"
      source: "[user_message | interface_state | account_context | confirmed_record | service_catalog]"
      confirmed: true
      visibility: "[user_visible | internal_only | restricted]"
      notes: "[Optional notes]"

  missing_information:
    - field: "[Missing field]"
      required: true
      reason_needed: "[Why this matters]"
      blocking: true
      question_to_ask: "[Targeted question]"

  inferred_context:
    - inference: "[Inference]"
      basis: "[User wording / workflow state / retrieved context]"
      confidence: "[low | medium | high]"
      requires_confirmation: true
      affects: "[request_type | service_category | routing | escalation | output_quality | none]"

  retrieved_context:
    service_catalog:
      retrieved: false
      results:
        - service_category: "[Category]"
          source: "service_catalog"
          summary: "[Relevant service catalog detail]"
          relevance_reason: "[Why included]"
          permission_status: "allowed"
          visible_to_user: true
          confidence: "[low | medium | high]"

    knowledge_base:
      retrieved: false
      results:
        - article_id: "[Article ID]"
          source: "knowledge_base"
          summary: "[Relevant answer or guidance]"
          relevance_reason: "[Why included]"
          visible_to_user: true
          confidence: "[low | medium | high]"

    case_history:
      retrieved: false
      retrieval_allowed: false
      retrieval_reason: "[Why checked or not checked]"
      results:
        - case_id: "[Case ID]"
          record_type: "[own_case | related_case | duplicate_case | restricted_case]"
          status: "[open | pending | closed | escalated | unknown]"
          summary: "[Visible or internal summary depending on permission]"
          match_strength: "[exact | strong | possible | weak]"
          permission_status: "[allowed | restricted | internal_only | unknown]"
          visible_to_user: false
          allowed_use: "[user_display | routing_only | reviewer_context | escalation_only | none]"

  restricted_context:
    - context_type: "[Restricted context type]"
      reason_restricted: "[Reason]"
      allowed_internal_use: "[none | routing_only | escalation_only | reviewer_only]"
      user_facing_use_allowed: false

  language_context:
    input_language: "[Detected language]"
    input_language_confidence: "[low | medium | high]"
    output_language: "[Expected output language]"
    translation_required: false
    preserve_original_user_text: true
    official_terms_preserved:
      - "[Official term]"
    terms_not_to_translate:
      - "[Term]"
    uncertain_translation_notes:
      - "[Note]"
    language_review_required: false

  routing_context:
    routing_required: false
    recommended_destination: "[Service team / review queue / escalation queue / self-service answer flow]"
    routing_confidence: "[low | medium | high]"
    routing_reason:
      user_visible: "[Safe explanation]"
      internal: "[Internal reason if permitted]"
    human_review_required: false
    open_routing_questions:
      - "[Question]"

  escalation_context:
    escalation_required: false
    escalation_triggers:
      - "[Trigger if present]"
    escalation_owner: "[Team or role]"
    escalation_summary_required: false
    user_facing_escalation_message_allowed: true

  output_requirements:
    output_type: "[clarification_question | user_answer | user_review_summary | structured_case_record | routing_recommendation | escalation_summary | recovery_summary]"
    audience: "[user | internal_service_team | reviewer | system | mixed]"
    format: "[plain_text | markdown | yaml | json | structured_fields]"
    required_fields:
      - "[Field]"
    prohibited_content:
      - "[Content that must not appear]"
    include_uncertainty: true
    include_next_steps: true

  allowed_actions:
    - action: "[Allowed action]"
      condition: "[Condition]"

  prohibited_actions:
    - action: "[Prohibited action]"
      reason: "[Reason]"

  diagnostic_context:
    test_case_id: "[Optional]"
    expected_behavior: "[Optional]"
    known_risks:
      - "[Risk]"
    failure_patterns_to_watch:
      - "[Failure pattern]"
```

---

## 3. Required Packet Sections by Workflow State

| Workflow state         | Required sections                                                                                                  | Notes                                                           |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------- |
| Start                  | `packet_metadata`, `task_context`, `user_context`, `workflow_context`, `user_input_context`, `output_requirements` | Used to orient the assistant and identify intent                |
| Intent detection       | `user_input_context`, `known_information`, `inferred_context`, `language_context`, `output_requirements`           | Request type may be inferred but may require confirmation       |
| Information collection | `known_information`, `missing_information`, `workflow_context`, `output_requirements`                              | Assistant should ask only for missing required information      |
| Case history check     | `user_context`, `retrieved_context.case_history`, `restricted_context`, `output_requirements`                      | Must respect permissions and visibility rules                   |
| Review                 | `known_information`, `inferred_context`, `missing_information`, `language_context`, `output_requirements`          | User-facing review summary should exclude restricted context    |
| Routing                | `task_context`, `routing_context`, `retrieved_context.service_catalog`, `known_information`, `output_requirements` | Routing reason should be reviewable                             |
| Escalation             | `escalation_context`, `known_information`, `missing_information`, `output_requirements`                            | Normal routing may stop when escalation is required             |
| Recovery               | `workflow_context`, `known_information`, `missing_information`, `output_requirements`                              | Preserve confirmed information and ask only for remaining needs |

---

## 4. Minimal Packet Example: Clarification Needed

```yaml
runtime_context_packet:
  packet_metadata:
    packet_id: "ctx-sample-clarification-001"
    packet_version: "0.1"
    generated_at: "2026-05-19T00:00:00Z"
    generated_by: "context_assembly_layer"
    system_name: "Public Service Request & Feedback Triage Assistant"
    workflow_name: "public_service_request_feedback_triage"
    workflow_state: "information_collection"
    sample_status: "fictional_example"

  task_context:
    current_task: "Ask one clarification question needed to route the request."
    task_goal: "Collect missing location information before preparing a case record."
    request_type: "issue_report"
    request_type_status: "inferred"
    service_category: "streets_and_infrastructure"
    service_category_status: "inferred"

  user_context:
    user_role: "authenticated_resident"
    authentication_status: "authenticated"
    permission_scope: "own_cases_only"
    user_language_preference: "English"

  workflow_context:
    current_state: "information_collection"
    prior_state: "intent_detection"
    next_expected_state: "review"
    completed_steps:
      - "initial_description_collected"
      - "request_type_inferred"
    remaining_steps:
      - "collect_location"
      - "confirm_summary"
      - "route_case"
    interruption_or_recovery_status: "none"

  user_input_context:
    original_user_text:
      language: "English"
      text: "The streetlight near my building has been out for three nights."
      preserve_for_review: true
    normalized_summary:
      text: "User reports a streetlight outage near their building."
      status: "draft"

  known_information:
    - field: "issue_description"
      value: "Streetlight near user’s building has been out for three nights."
      source: "user_message"
      confirmed: true
      visibility: "user_visible"

  missing_information:
    - field: "location"
      required: true
      reason_needed: "Needed to route the issue to the correct service team."
      blocking: true
      question_to_ask: "Where is the streetlight located?"

  inferred_context:
    - inference: "The request is likely an issue report."
      basis: "User described something not working over multiple nights."
      confidence: "high"
      requires_confirmation: false
      affects: "request_type"

  output_requirements:
    output_type: "clarification_question"
    audience: "user"
    format: "plain_text"
    required_fields:
      - "one_question"
      - "brief_reason"
    prohibited_content:
      - "internal routing logic"
      - "unconfirmed service promise"
    include_uncertainty: false
    include_next_steps: false

  allowed_actions:
    - action: "ask_clarifying_question"
      condition: "required routing information is missing"

  prohibited_actions:
    - action: "route_case"
      reason: "Required location information is missing."
    - action: "promise_resolution_timeline"
      reason: "No service timeline is provided in context."
```

Expected assistant response:

```text
Where is the streetlight located? I need the location so this can be routed to the correct service team.
```

---

## 5. Minimal Packet Example: Restricted Related Case

```yaml
runtime_context_packet:
  packet_metadata:
    packet_id: "ctx-sample-restricted-case-001"
    packet_version: "0.1"
    generated_at: "2026-05-19T00:00:00Z"
    generated_by: "context_assembly_layer"
    system_name: "Public Service Request & Feedback Triage Assistant"
    workflow_name: "public_service_request_feedback_triage"
    workflow_state: "case_history_check"
    sample_status: "fictional_example"

  task_context:
    current_task: "Prepare safe user-facing response and internal routing note."
    task_goal: "Avoid exposing restricted related-case details while supporting review."
    request_type: "follow_up"
    request_type_status: "inferred"
    service_category: "streets_and_infrastructure"
    service_category_status: "inferred"

  user_context:
    user_role: "authenticated_resident"
    authentication_status: "authenticated"
    permission_scope: "own_cases_only"

  user_input_context:
    original_user_text:
      language: "English"
      text: "I already reported this last week and nothing has happened."
      preserve_for_review: true

  retrieved_context:
    case_history:
      retrieved: true
      retrieval_allowed: true
      retrieval_reason: "User appears to be following up on a repeated issue."
      results:
        - case_id: "case-456"
          record_type: "related_case"
          status: "open"
          summary: "Internal related case exists for similar location/service category."
          match_strength: "possible"
          permission_status: "restricted"
          visible_to_user: false
          allowed_use: "routing_only"

  restricted_context:
    - context_type: "related_case_summary"
      reason_restricted: "User does not have permission to view this related record."
      allowed_internal_use: "routing_only"
      user_facing_use_allowed: false

  routing_context:
    routing_required: true
    recommended_destination: "service_review_queue"
    routing_confidence: "medium"
    routing_reason:
      user_visible: "This may need review because you indicated this is a repeated issue."
      internal: "Possible restricted related case found; route for service team review."
    human_review_required: true
    open_routing_questions:
      - "Can the service team confirm whether this should be linked to the existing case?"

  output_requirements:
    output_type: "safe_user_response"
    audience: "mixed"
    format: "plain_text_and_internal_note"
    required_fields:
      - "user_facing_response"
      - "internal_routing_note"
    prohibited_content:
      - "restricted case ID"
      - "restricted case details"
      - "language implying hidden record existence beyond allowed explanation"
    include_uncertainty: true
    include_next_steps: true
```

Expected user-facing response:

```text
I can help prepare this as a follow-up for review. I’ll include that you reported the issue last week and that it appears unresolved.
```

Expected internal note:

```text
Possible related restricted case found. Do not expose details to user. Route to service review queue for confirmation and linkage decision.
```

---

## 6. Packet Review Checklist

Before using a runtime context packet, check:

* Is the current workflow state included?
* Is the current task specific enough?
* Is the user role included when permissions matter?
* Is missing information marked as blocking or non-blocking?
* Is inferred context labeled as inference?
* Is retrieved context labeled by source, permission, and visibility?
* Is restricted context excluded from user-facing output?
* Are language rules included when needed?
* Are routing and escalation fields included when relevant?
* Are output requirements specific enough to prevent generic responses?
* Are prohibited actions clear?
* Can a reviewer diagnose which context shaped the assistant behavior?

---

## 7. Related Documents

* `sample-scope-and-assumptions.md`
* `ai-design-principles.md`
* `ai-assistant-behavior-spec.md`
* `public-service-request-feedback-triage-assistant-behavior-spec.md`
* `context-architecture-spec.md`
* `context-assembly-rules.md`
* `language-handling-rules.md`
* `service-category-routing-rules.md`
* `case-history-context-rules.md`
* `example-context-packets.md`
* `testing-diagnostics-spec.md`
* `public-service-request-feedback-conversation-flows.md`
