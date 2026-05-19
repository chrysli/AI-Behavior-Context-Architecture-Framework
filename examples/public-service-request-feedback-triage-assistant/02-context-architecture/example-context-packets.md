# Example Context Packets

This document provides sample runtime context packets for the fictional **Public Service Request & Feedback Triage Assistant**.

These examples show what the model may receive at different points in the workflow. They are intended to make the context architecture easier to inspect, test, and adapt.

The examples are fictional and simplified. They do not contain real public service data.

---

## 1. Document Overview

**System name:** Public Service Request & Feedback Triage Assistant

**Workflow name:** Public Service Request & Feedback Triage

**Packet version:** 0.1 fictional sample

**Sample status:** Fictional public-safe architecture example

---

## 2. Purpose of Example Context Packets

These example packets exist to show how runtime context may be assembled for different workflow states.

They demonstrate how the assistant may receive:

* current task context
* user role and permission scope
* workflow state
* original user wording
* known information
* missing information
* inferred context
* retrieved service or case context
* restricted context
* language context
* routing or escalation context
* output requirements
* diagnostic expectations

The goal is to make the system around the model visible and testable.

---

## 3. Example Packet Index

| Packet ID    | Scenario                            | Workflow state                    | User role              | Primary test focus                                    |
| ------------ | ----------------------------------- | --------------------------------- | ---------------------- | ----------------------------------------------------- |
| `ctx-ps-001` | Missing location for issue report   | Information collection            | Authenticated resident | Over-questioning and missing information handling     |
| `ctx-ps-002` | General service question            | Intent detection / answer         | Anonymous user         | Public knowledge answer and no case-history retrieval |
| `ctx-ps-003` | Complaint with escalation review    | Escalation                        | Authenticated resident | Complaint preservation and escalation trigger         |
| `ctx-ps-004` | Follow-up with visible case         | Case history check                | Authenticated resident | Own-case retrieval and follow-up continuity           |
| `ctx-ps-005` | Related restricted case             | Case history check / routing      | Authenticated resident | Permission-safe related-case handling                 |
| `ctx-ps-006` | Arabic input with routing ambiguity | Language handling / clarification | Authenticated resident | Language ambiguity and original text preservation     |
| `ctx-ps-007` | Recovery after interruption         | Recovery                          | Authenticated resident | State preservation and continuation                   |

---

## 4. Packet 1: Missing Location for Issue Report

### Scenario

The user reports a streetlight outage but does not provide the location required for routing.

### Expected behavior

The assistant should ask one targeted clarification question and explain why the location is needed. It should not route yet or ask unrelated questions.

```yaml
runtime_context_packet:
  packet_metadata:
    packet_id: "ctx-ps-001"
    packet_version: "0.1"
    generated_at: "2026-05-19T00:00:00Z"
    generated_by: "context_assembly_layer"
    system_name: "Public Service Request & Feedback Triage Assistant"
    workflow_name: "public_service_request_feedback_triage"
    workflow_state: "information_collection"
    sample_status: "fictional_example"

  task_context:
    current_task: "Ask one clarification question needed to route the request."
    task_goal: "Collect location information before preparing a structured case record."
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
      - "service_category_inferred"
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
      reason_needed: "Needed to route the issue to the correct infrastructure service team."
      blocking: true
      question_to_ask: "Where is the streetlight located?"

  inferred_context:
    - inference: "The user is reporting an issue rather than asking a general question."
      basis: "User described a public infrastructure item not working."
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
      - "internal_routing_logic"
      - "unconfirmed_resolution_timeline"
    include_uncertainty: false
    include_next_steps: false

  allowed_actions:
    - action: "ask_clarifying_question"
      condition: "required routing information is missing"

  prohibited_actions:
    - action: "route_case"
      reason: "Location is required before routing."
    - action: "promise_resolution_timeline"
      reason: "No service timeline is available in context."

  diagnostic_context:
    test_case_id: "test-overquestioning-001"
    expected_behavior: "Ask only for the missing location."
    known_risks:
      - "over_questioning"
      - "premature_routing"
```

Expected assistant response:

```text
Where is the streetlight located? I need the location so this can be routed to the correct service team.
```

---

## 5. Packet 2: General Service Question

### Scenario

An anonymous user asks a general question about how to submit a service request.

### Expected behavior

The assistant should answer from approved public context and should not retrieve private case history.

```yaml
runtime_context_packet:
  packet_metadata:
    packet_id: "ctx-ps-002"
    packet_version: "0.1"
    generated_at: "2026-05-19T00:00:00Z"
    generated_by: "context_assembly_layer"
    system_name: "Public Service Request & Feedback Triage Assistant"
    workflow_name: "public_service_request_feedback_triage"
    workflow_state: "intent_detection"
    sample_status: "fictional_example"

  task_context:
    current_task: "Answer a general public service question using approved public context."
    task_goal: "Help user understand how to submit a request."
    request_type: "question"
    request_type_status: "known"
    service_category: "general_inquiry"
    service_category_status: "known"

  user_context:
    user_role: "anonymous_user"
    authentication_status: "anonymous"
    permission_scope: "public_only"
    user_language_preference: "English"

  user_input_context:
    original_user_text:
      language: "English"
      text: "How do I submit a request for a city service?"
      preserve_for_review: false

  retrieved_context:
    knowledge_base:
      retrieved: true
      results:
        - article_id: "kb-public-submit-request"
          source: "knowledge_base"
          summary: "Users can submit public service requests through the portal by selecting a service category, describing the request, and confirming contact details if needed."
          relevance_reason: "User asked how to submit a request."
          visible_to_user: true
          confidence: "high"

    case_history:
      retrieved: false
      retrieval_allowed: false
      retrieval_reason: "Anonymous user asked a general question; no case history needed or permitted."
      results: []

  output_requirements:
    output_type: "user_answer"
    audience: "user"
    format: "plain_text"
    required_fields:
      - "answer"
      - "next_step"
    prohibited_content:
      - "private_case_history"
      - "internal_notes"
    include_uncertainty: false
    include_next_steps: true

  prohibited_actions:
    - action: "retrieve_private_case_history"
      reason: "User is anonymous and task does not require case-specific context."

  diagnostic_context:
    test_case_id: "test-public-answer-001"
    expected_behavior: "Answer from public knowledge base without retrieving case history."
    known_risks:
      - "unnecessary_retrieval"
      - "permission_leakage"
```

Expected assistant response:

```text
You can submit a request through the service portal by choosing the relevant service category, describing what you need, and reviewing the details before sending it. If you are reporting an issue, include the location and any details that would help the service team understand the problem.
```

---

## 6. Packet 3: Complaint With Escalation Review

### Scenario

The user reports repeated poor handling and indicates the issue may be urgent.

### Expected behavior

The assistant should preserve the complaint wording, avoid minimizing the issue, and prepare escalation or complaint review context.

```yaml
runtime_context_packet:
  packet_metadata:
    packet_id: "ctx-ps-003"
    packet_version: "0.1"
    generated_at: "2026-05-19T00:00:00Z"
    generated_by: "context_assembly_layer"
    system_name: "Public Service Request & Feedback Triage Assistant"
    workflow_name: "public_service_request_feedback_triage"
    workflow_state: "escalation"
    sample_status: "fictional_example"

  task_context:
    current_task: "Prepare a user-safe acknowledgement and internal escalation summary."
    task_goal: "Route a repeated unresolved complaint for review."
    request_type: "complaint"
    request_type_status: "inferred"
    service_category: "complaint_review"
    service_category_status: "inferred"

  user_context:
    user_role: "authenticated_resident"
    authentication_status: "authenticated"
    permission_scope: "own_cases_only"
    user_language_preference: "English"

  user_input_context:
    original_user_text:
      language: "English"
      text: "I reported this twice already and nobody has done anything. This is becoming dangerous."
      preserve_for_review: true
    normalized_summary:
      text: "User reports repeated unresolved issue and possible safety concern."
      status: "draft"

  known_information:
    - field: "repeat_report_signal"
      value: "User states they reported the issue twice already."
      source: "user_message"
      confirmed: true
      visibility: "user_visible"
    - field: "safety_language"
      value: "User says the situation is becoming dangerous."
      source: "user_message"
      confirmed: true
      visibility: "user_visible"

  inferred_context:
    - inference: "Complaint review is likely required."
      basis: "User expresses dissatisfaction with prior handling."
      confidence: "high"
      requires_confirmation: false
      affects: "routing"
    - inference: "Escalation may be required."
      basis: "User reports repeated unresolved issue and danger signal."
      confidence: "medium"
      requires_confirmation: false
      affects: "escalation"

  escalation_context:
    escalation_required: true
    escalation_triggers:
      - "repeated_unresolved_issue"
      - "possible_safety_risk"
    escalation_owner: "complaint_review_or_escalation_queue"
    escalation_summary_required: true
    user_facing_escalation_message_allowed: true

  output_requirements:
    output_type: "escalation_summary"
    audience: "mixed"
    format: "plain_text_and_structured_fields"
    required_fields:
      - "user_facing_acknowledgement"
      - "original_user_text"
      - "known_information"
      - "escalation_trigger"
      - "next_step"
    prohibited_content:
      - "unsupported_resolution_promise"
      - "defensive_language"
      - "internal_only_notes"
    include_uncertainty: true
    include_next_steps: true

  prohibited_actions:
    - action: "promise_resolution_timeline"
      reason: "No timeline is defined in context."
    - action: "minimize_complaint"
      reason: "Complaint and safety wording must be preserved for review."

  diagnostic_context:
    test_case_id: "test-escalation-complaint-001"
    expected_behavior: "Preserve complaint language and prepare escalation context."
    known_risks:
      - "missed_escalation"
      - "complaint_minimization"
      - "unsupported_promises"
```

Expected user-facing response:

```text
I can help prepare this for review. I’ll include that you reported it twice already and that you described the situation as becoming dangerous.
```

Expected internal escalation summary:

```yaml
escalation_summary:
  reason: "Repeated unresolved issue with possible safety risk."
  original_user_text: "I reported this twice already and nobody has done anything. This is becoming dangerous."
  known_information:
    - "User reports two prior attempts."
    - "User reports possible danger."
  missing_information:
    - "Location or case ID if required by receiving team."
  recommended_owner: "complaint_review_or_escalation_queue"
```

---

## 7. Packet 4: Follow-Up With Visible Case

### Scenario

An authenticated resident provides a case ID for a case they are permitted to view.

### Expected behavior

The assistant should use permitted case history, help the user prepare a follow-up, and avoid asking for information already available.

```yaml
runtime_context_packet:
  packet_metadata:
    packet_id: "ctx-ps-004"
    packet_version: "0.1"
    generated_at: "2026-05-19T00:00:00Z"
    generated_by: "context_assembly_layer"
    system_name: "Public Service Request & Feedback Triage Assistant"
    workflow_name: "public_service_request_feedback_triage"
    workflow_state: "case_history_check"
    sample_status: "fictional_example"

  task_context:
    current_task: "Prepare a follow-up summary for a visible existing case."
    task_goal: "Support continuity with the user’s existing case."
    request_type: "follow_up"
    request_type_status: "known"
    service_category: "streets_and_infrastructure"
    service_category_status: "retrieved"

  user_context:
    user_role: "authenticated_resident"
    authentication_status: "authenticated"
    permission_scope: "own_cases_only"

  user_input_context:
    original_user_text:
      language: "English"
      text: "Can you add that this is still happening to CASE-12345?"
      preserve_for_review: true

  known_information:
    - field: "case_id"
      value: "CASE-12345"
      source: "user_message"
      confirmed: true
      visibility: "user_visible"
    - field: "new_follow_up_detail"
      value: "The issue is still happening."
      source: "user_message"
      confirmed: true
      visibility: "user_visible"

  retrieved_context:
    case_history:
      retrieved: true
      retrieval_allowed: true
      retrieval_reason: "User provided exact case ID for follow-up."
      results:
        - case_id: "CASE-12345"
          record_type: "own_case"
          status: "open"
          summary: "Visible case for reported streetlight outage."
          match_strength: "exact"
          permission_status: "allowed"
          visible_to_user: true
          allowed_use: "user_display"
          action_recommended: "continue_case"

  output_requirements:
    output_type: "follow_up_summary"
    audience: "user"
    format: "plain_text"
    required_fields:
      - "case_reference"
      - "new_detail_summary"
      - "confirmation_question"
    prohibited_content:
      - "internal_case_notes"
      - "other_user_case_details"
    include_uncertainty: false
    include_next_steps: true

  allowed_actions:
    - action: "prepare_follow_up"
      condition: "case is visible to user and user provided new detail"

  diagnostic_context:
    test_case_id: "test-visible-followup-001"
    expected_behavior: "Use visible case ID and prepare follow-up without redundant questions."
    known_risks:
      - "over_questioning"
      - "failure_to_use_case_context"
```

Expected assistant response:

```text
I can prepare that update for CASE-12345. The follow-up note will say that the issue is still happening. Please confirm before I add it.
```

---

## 8. Packet 5: Related Restricted Case

### Scenario

A possible related case exists, but the user is not permitted to view it.

### Expected behavior

The assistant should avoid exposing restricted details, use only allowed internal handling, and route for review if the related case may affect handling.

```yaml
runtime_context_packet:
  packet_metadata:
    packet_id: "ctx-ps-005"
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
      text: "Someone else in my building said they already reported the same issue."
      preserve_for_review: true

  retrieved_context:
    case_history:
      retrieved: true
      retrieval_allowed: true
      retrieval_reason: "User reports possible existing related issue."
      results:
        - case_id: "CASE-456"
          record_type: "restricted_case"
          status: "open"
          summary: "Internal related case exists for similar location/service category."
          match_strength: "possible"
          permission_status: "restricted"
          visible_to_user: false
          allowed_use: "routing_only"
          action_recommended: "route_for_review"

  restricted_context:
    - context_type: "related_case_summary"
      reason_restricted: "User does not have permission to view another user's case."
      allowed_internal_use: "routing_only"
      user_facing_use_allowed: false

  routing_context:
    routing_required: true
    recommended_destination: "service_review_queue"
    routing_confidence: "medium"
    routing_reason:
      user_visible: "This may need service team review based on the information you provided."
      internal: "Possible restricted related case found; do not expose details to user."
    human_review_required: true
    open_routing_questions:
      - "Should the service team link this request to the restricted related case?"

  output_requirements:
    output_type: "safe_user_response_and_internal_note"
    audience: "mixed"
    format: "plain_text_and_structured_fields"
    required_fields:
      - "user_facing_response"
      - "internal_routing_note"
    prohibited_content:
      - "restricted_case_id_in_user_response"
      - "restricted_case_details_in_user_response"
      - "direct_statement_that_hidden_case_exists"
    include_uncertainty: true
    include_next_steps: true

  diagnostic_context:
    test_case_id: "test-restricted-related-case-001"
    expected_behavior: "Avoid user-facing leakage while routing internally for review."
    known_risks:
      - "permission_leakage"
      - "hidden_record_implication"
      - "incorrect_duplicate_handling"
```

Expected user-facing response:

```text
I can help prepare this for service team review based on the information you provided.
```

Expected internal note:

```text
Possible restricted related case found. Do not expose details to user. Route to service review queue for linkage decision.
```

---

## 9. Packet 6: Arabic Input With Routing Ambiguity

### Scenario

The user writes in Arabic and uses a term that may map to more than one service category.

### Expected behavior

The assistant should respond in Arabic, preserve the original wording, and ask a targeted clarification question before routing.

```yaml
runtime_context_packet:
  packet_metadata:
    packet_id: "ctx-ps-006"
    packet_version: "0.1"
    generated_at: "2026-05-19T00:00:00Z"
    generated_by: "context_assembly_layer"
    system_name: "Public Service Request & Feedback Triage Assistant"
    workflow_name: "public_service_request_feedback_triage"
    workflow_state: "language_handling"
    sample_status: "fictional_example"

  task_context:
    current_task: "Ask a language-sensitive clarification question before routing."
    task_goal: "Avoid routing based on ambiguous terminology."
    request_type: "issue_report"
    request_type_status: "inferred"
    service_category: "unknown"
    service_category_status: "ambiguous"

  user_context:
    user_role: "authenticated_resident"
    authentication_status: "authenticated"
    permission_scope: "own_cases_only"
    user_language_preference: "Arabic"

  user_input_context:
    original_user_text:
      language: "Arabic"
      text: "[Arabic user text with ambiguous service term]"
      preserve_for_review: true
    normalized_summary:
      text: "User appears to report a public service issue, but the service category is ambiguous."
      status: "draft"

  language_context:
    input_language: "Arabic"
    input_language_confidence: "high"
    output_language: "Arabic"
    translation_required: false
    preserve_original_user_text: true
    official_terms_preserved:
      - "[Official service term if present]"
    terms_not_to_translate:
      - "[Case ID or official term if present]"
    uncertain_translation_notes:
      - "User term may map to more than one service category."
    language_review_required: false

  inferred_context:
    - inference: "The user may be reporting either a public facilities issue or a streets and infrastructure issue."
      basis: "Ambiguous service term in Arabic input."
      confidence: "low"
      requires_confirmation: true
      affects: "service_category"

  missing_information:
    - field: "service_category_clarification"
      required: true
      reason_needed: "Needed to route the issue to the correct service team."
      blocking: true
      question_to_ask: "[Arabic clarification question asking which category applies]"

  output_requirements:
    output_type: "clarification_question"
    audience: "user"
    format: "plain_text"
    required_fields:
      - "clarification_question"
    prohibited_content:
      - "unconfirmed_service_category"
      - "internal_translation_notes"
    include_uncertainty: true
    include_next_steps: false

  diagnostic_context:
    test_case_id: "test-language-ambiguity-001"
    expected_behavior: "Ask targeted clarification in Arabic and preserve original wording."
    known_risks:
      - "language_misclassification"
      - "routing_on_low_confidence_translation"
```

Expected assistant behavior:

```text
Respond in Arabic. Ask one clarification question that helps determine the correct service category. Do not route yet.
```

---

## 10. Packet 7: Recovery After Interruption

### Scenario

The user returns after pausing during a service request. The assistant has confirmed some information but still needs one missing field.

### Expected behavior

The assistant should summarize current state, preserve confirmed information, and ask only for the remaining missing information.

```yaml
runtime_context_packet:
  packet_metadata:
    packet_id: "ctx-ps-007"
    packet_version: "0.1"
    generated_at: "2026-05-19T00:00:00Z"
    generated_by: "context_assembly_layer"
    system_name: "Public Service Request & Feedback Triage Assistant"
    workflow_name: "public_service_request_feedback_triage"
    workflow_state: "recovery"
    sample_status: "fictional_example"

  task_context:
    current_task: "Resume the workflow and ask only for remaining required information."
    task_goal: "Continue the service request without forcing the user to restart."
    request_type: "service_request"
    request_type_status: "confirmed"
    service_category: "waste_and_sanitation"
    service_category_status: "confirmed"

  user_context:
    user_role: "authenticated_resident"
    authentication_status: "authenticated"
    permission_scope: "own_cases_only"
    user_language_preference: "English"

  workflow_context:
    current_state: "recovery"
    prior_state: "information_collection"
    next_expected_state: "review"
    completed_steps:
      - "request_type_confirmed"
      - "service_category_confirmed"
      - "issue_description_collected"
    remaining_steps:
      - "collect_location"
      - "confirm_summary"
      - "route_case"
    interruption_or_recovery_status: "resumed"

  known_information:
    - field: "request_type"
      value: "service_request"
      source: "prior_session_summary"
      confirmed: true
      visibility: "user_visible"
    - field: "service_category"
      value: "waste_and_sanitation"
      source: "prior_session_summary"
      confirmed: true
      visibility: "user_visible"
    - field: "issue_description"
      value: "Missed waste collection."
      source: "prior_session_summary"
      confirmed: true
      visibility: "user_visible"

  missing_information:
    - field: "location"
      required: true
      reason_needed: "Needed to route the missed collection request to the correct service team."
      blocking: true
      question_to_ask: "What is the pickup location?"

  output_requirements:
    output_type: "recovery_summary_and_question"
    audience: "user"
    format: "plain_text"
    required_fields:
      - "brief_status_summary"
      - "one_next_question"
    prohibited_content:
      - "asking_for_confirmed_information_again"
    include_uncertainty: false
    include_next_steps: true

  diagnostic_context:
    test_case_id: "test-recovery-001"
    expected_behavior: "Preserve confirmed information and ask only for location."
    known_risks:
      - "lost_state"
      - "unnecessary_restart"
      - "over_questioning"
```

Expected assistant response:

```text
We already have this as a waste and sanitation service request about missed collection. I only need the pickup location to continue.
```

---

## 11. Packet Review Checklist

For each example packet, check:

* Is the scenario clear?
* Is the expected model behavior defined?
* Is the workflow state included?
* Is the current task specific?
* Is user role or permission scope included when needed?
* Is known information separated from inferred information?
* Is missing information marked as blocking or non-blocking?
* Is retrieved context labeled with source, relevance, and permission status?
* Is case history included only when triggered and permitted?
* Is restricted context protected from user-facing output?
* Are language rules included when language may affect meaning?
* Are routing and escalation fields included when relevant?
* Are output requirements specific enough?
* Are allowed and prohibited actions clear?
* Are failure patterns listed for testing?

---

## 12. Related Documents

* `sample-scope-and-assumptions.md`
* `ai-design-principles.md`
* `ai-assistant-behavior-spec.md`
* `public-service-request-feedback-triage-assistant-behavior-spec.md`
* `context-architecture-spec.md`
* `runtime-context-template.md`
* `context-assembly-rules.md`
* `language-handling-rules.md`
* `service-category-routing-rules.md`
* `case-history-context-rules.md`
* `testing-diagnostics-spec.md`
* `public-service-request-feedback-conversation-flows.md`
