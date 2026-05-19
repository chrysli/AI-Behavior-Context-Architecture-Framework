# Example Context Packets Template

Use this document to provide example runtime context packets for specific workflow states, user roles, scenarios, and test cases.

Example packets make the context architecture easier to review because they show what the model would actually receive at runtime.

These examples should be realistic enough to test the architecture, but safe enough to avoid exposing sensitive, private, restricted, or production data.

---

## 1. Document Overview

**System name:** `[Insert system/product/service name]`

**AI assistant name:** `[Insert assistant name]`

**Workflow name:** `[Insert workflow name]`

**Packet template version:** `[Insert version]`

**Document owner:** `[Insert owner/team]`

**Last updated:** `[Insert date]`

---

## 2. Purpose of Example Context Packets

Describe why example packets are included.

```text
These example context packets exist to...
```

Prompts:

* What workflow states need example packets?
* What user roles should be represented?
* What routing, retrieval, permission, language, or escalation scenarios should be tested?
* What failure patterns should these examples help diagnose?
* What assumptions are intentionally included for review?

---

## 3. How to Use These Examples

Use example context packets to:

* review what the model receives at runtime
* check whether the packet includes enough context for expected behavior
* check whether irrelevant or restricted context has been excluded
* validate whether context labels are clear
* test workflow-state behavior
* test role and permission boundaries
* test language handling rules
* test retrieval and related-record handling
* test structured output requirements
* diagnose failures by layer

These examples should be used with:

* `[Runtime Context Template]`
* `[Context Architecture Spec]`
* `[Context Assembly Rules]`
* `[Language Handling Rules]`
* `[Workflow Routing Rules]`
* `[Related Record Context Rules]`
* `[Testing & Diagnostics Spec]`

---

## 4. Example Packet Index

List all example packets in this document.

| Packet ID     | Scenario     | Workflow state | User role | Primary test focus |
| ------------- | ------------ | -------------- | --------- | ------------------ |
| `[packet-id]` | `[Scenario]` | `[State]`      | `[Role]`  | `[Focus]`          |
| `[packet-id]` | `[Scenario]` | `[State]`      | `[Role]`  | `[Focus]`          |
| `[packet-id]` | `[Scenario]` | `[State]`      | `[Role]`  | `[Focus]`          |

Suggested packet examples:

* new request with complete information
* new request with missing required information
* ambiguous request type
* low-confidence routing
* related record found and visible
* related record found but restricted
* user follows up on existing record
* user switches language mid-flow
* escalation trigger detected
* retrieved context conflicts with user input
* user lacks permission for requested information
* recovery after interruption

---

## 5. Example Packet Format

Use this format for each example.

```yaml
example_packet:
  packet_id: "[Example packet ID]"
  scenario: "[Scenario name]"
  purpose: "[What this packet is meant to demonstrate or test]"
  expected_model_behavior: "[Expected behavior]"
  failure_patterns_to_watch:
    - "[Failure pattern]"

  runtime_context_packet:
    packet_metadata:
      packet_id: "[Runtime packet ID]"
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
        visibility: "[user_visible | internal_only | restricted]"
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
        affects: "[routing | eligibility | urgency | output_quality | none]"

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

    related_record_context:
      checked: false
      check_trigger: "[Trigger if checked]"
      results: []
      no_match_reason: "[Reason if checked and no meaningful match found]"

    restricted_context:
      - context_type: "[Restricted context type]"
        reason_restricted: "[Reason]"
        allowed_internal_use: "[none | routing_only | safety_only | reviewer_only | other]"
        user_facing_use_allowed: false

    language_context:
      input_language: "[Detected or selected language]"
      input_language_confidence: "[low | medium | high]"
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

## 6. Example Packet 1: New Request With Complete Information

Use this example to show a straightforward packet where the assistant has enough context to prepare the expected output.

```yaml
example_packet:
  packet_id: "example-001"
  scenario: "New request with complete information"
  purpose: "Demonstrate a normal workflow path with enough context to proceed."
  expected_model_behavior: "Prepare the required output without asking unnecessary questions."
  failure_patterns_to_watch:
    - "asks for information already provided"
    - "ignores workflow state"
    - "produces malformed structured output"

  runtime_context_packet:
    packet_metadata:
      packet_id: "ctx-example-001"
      packet_version: "[Version]"
      generated_at: "[Timestamp]"
      generated_by: "[Context assembly layer]"
      workflow_name: "[Workflow name]"
      workflow_state: "[State]"
      model_target: "[Model target]"

    task_context:
      current_task: "[Task]"
      task_goal: "[Goal]"
      task_stage: "[Stage]"
      user_intent: "[Intent]"
      intent_status: "known"

    user_context:
      user_role: "[Role]"
      user_permission_scope: "[Scope]"
      user_language_preference: "[Language]"

    workflow_context:
      current_state: "[State]"
      prior_state: "[Prior state]"
      next_expected_state: "[Next state]"
      completed_steps:
        - "[Step]"
      remaining_steps:
        - "[Step]"
      interruption_or_recovery_status: "none"

    known_information:
      - field: "[Field]"
        value: "[Value]"
        source: "user_message"
        confirmed: true
        visibility: "user_visible"
        notes: "[Notes]"

    missing_information: []
    inferred_context: []
    retrieved_context: []

    output_requirements:
      output_type: "[Output type]"
      audience: "[Audience]"
      format: "[Format]"
      required_fields:
        - "[Field]"
      prohibited_content:
        - "[Content type]"
      include_uncertainty: false
      include_next_steps: true
```

---

## 7. Example Packet 2: Missing Required Information

Use this example to show when the assistant should ask a targeted clarification question before proceeding.

```yaml
example_packet:
  packet_id: "example-002"
  scenario: "Missing required information"
  purpose: "Demonstrate missing context handling and question-asking rules."
  expected_model_behavior: "Ask only for the missing information required to proceed."
  failure_patterns_to_watch:
    - "asks multiple unnecessary questions"
    - "proceeds without required information"
    - "does not explain why the information is needed"

  runtime_context_packet:
    packet_metadata:
      packet_id: "ctx-example-002"
      packet_version: "[Version]"
      generated_at: "[Timestamp]"
      generated_by: "[Context assembly layer]"
      workflow_name: "[Workflow name]"
      workflow_state: "clarification"

    task_context:
      current_task: "Ask one clarification question needed to proceed."
      task_goal: "Collect required missing information."
      task_stage: "missing_information_clarification"
      user_intent: "[Intent]"
      intent_status: "inferred"

    known_information:
      - field: "[Known field]"
        value: "[Known value]"
        source: "user_message"
        confirmed: true
        visibility: "user_visible"

    missing_information:
      - field: "[Missing field]"
        required: true
        reason_needed: "[Why this field is required]"
        blocking: true
        question_to_ask: "[Targeted question]"

    output_requirements:
      output_type: "clarification_question"
      audience: "user"
      format: "plain_text"
      required_fields:
        - "one_question"
        - "brief_reason"
      prohibited_content:
        - "internal routing logic"
      include_uncertainty: false
      include_next_steps: false
```

---

## 8. Example Packet 3: Inferred Intent Requires Confirmation

Use this example when the assistant can infer intent, but the inference affects routing, eligibility, urgency, or output quality.

```yaml
example_packet:
  packet_id: "example-003"
  scenario: "Inferred intent requires confirmation"
  purpose: "Demonstrate how material inference should be labeled and confirmed."
  expected_model_behavior: "Confirm the inference before using it for a material workflow decision."
  failure_patterns_to_watch:
    - "treats inference as fact"
    - "routes based on unsupported assumption"
    - "fails to ask confirmation when inference affects outcome"

  runtime_context_packet:
    task_context:
      current_task: "Confirm inferred request type before routing."
      task_goal: "Avoid incorrect routing based on ambiguous intent."
      task_stage: "intent_confirmation"
      user_intent: "[Inferred intent]"
      intent_status: "inferred"

    inferred_context:
      - inference: "[Inference]"
        basis: "[Basis from user wording or context]"
        confidence: "medium"
        requires_confirmation: true
        affects: "routing"

    output_requirements:
      output_type: "confirmation_question"
      audience: "user"
      format: "plain_text"
      required_fields:
        - "inference_to_confirm"
        - "confirmation_question"
      prohibited_content:
        - "unconfirmed routing decision"
```

---

## 9. Example Packet 4: Related Record Found and Visible

Use this example when related-record detection finds a record the user is allowed to see.

```yaml
example_packet:
  packet_id: "example-004"
  scenario: "Related record found and visible"
  purpose: "Demonstrate how visible related records can support continuity or duplicate prevention."
  expected_model_behavior: "Explain the visible related record and ask whether the user wants to continue with it or proceed."
  failure_patterns_to_watch:
    - "forces duplicate handling without confirmation"
    - "overstates related record as exact duplicate"
    - "fails to preserve current user intent"

  runtime_context_packet:
    related_record_context:
      checked: true
      check_trigger: "[Trigger]"
      results:
        - record_id: "[Record ID]"
          record_type: "related"
          source: "[Source]"
          match_strength: "strong"
          match_signals:
            - "[Signal]"
          summary: "[User-visible summary]"
          status: "open"
          timestamp: "[Timestamp]"
          permission_status: "allowed"
          visible_to_user: true
          allowed_use: "user_display"
          action_recommended: "ask_clarification"

    output_requirements:
      output_type: "related_record_confirmation"
      audience: "user"
      format: "plain_text"
      required_fields:
        - "related_record_summary"
        - "user_choice_question"
      prohibited_content:
        - "internal-only notes"
```

---

## 10. Example Packet 5: Related Record Found but Restricted

Use this example when related-record context may influence internal handling but cannot be exposed to the user.

```yaml
example_packet:
  packet_id: "example-005"
  scenario: "Related record found but restricted"
  purpose: "Demonstrate permission-aware related-record handling."
  expected_model_behavior: "Use restricted related-record context only as allowed and avoid user-facing leakage."
  failure_patterns_to_watch:
    - "reveals restricted record details"
    - "implies hidden records exist when not allowed"
    - "uses restricted context outside permitted internal use"

  runtime_context_packet:
    related_record_context:
      checked: true
      check_trigger: "[Trigger]"
      results:
        - record_id: "[Record ID]"
          record_type: "related"
          source: "[Source]"
          match_strength: "possible"
          match_signals:
            - "[Signal]"
          summary: "[Internal-only summary]"
          status: "[Status]"
          timestamp: "[Timestamp]"
          permission_status: "restricted"
          visible_to_user: false
          allowed_use: "routing"
          action_recommended: "route_for_review"

    restricted_context:
      - context_type: "related_record_summary"
        reason_restricted: "User does not have permission to view this record."
        allowed_internal_use: "routing_only"
        user_facing_use_allowed: false

    output_requirements:
      output_type: "safe_user_response"
      audience: "user"
      format: "plain_text"
      prohibited_content:
        - "restricted record details"
        - "language implying hidden record existence if not allowed"
```

---

## 11. Example Packet 6: Language Ambiguity Affects Routing

Use this example when language, translation, transliteration, slang, dialect, or terminology affects workflow classification or routing.

```yaml
example_packet:
  packet_id: "example-006"
  scenario: "Language ambiguity affects routing"
  purpose: "Demonstrate language-sensitive clarification before classification or routing."
  expected_model_behavior: "Ask a targeted clarification question or flag language review when ambiguity affects routing."
  failure_patterns_to_watch:
    - "silently maps ambiguous wording to the wrong category"
    - "fails to preserve original user wording"
    - "translates official terms incorrectly"

  runtime_context_packet:
    language_context:
      input_language: "[Language]"
      input_language_confidence: "medium"
      output_language: "[Language]"
      translation_required: true
      preserve_original_user_text: true
      terms_not_to_translate:
        - "[Official term]"
      uncertain_translation_notes:
        - "[Ambiguity note]"

    inferred_context:
      - inference: "[Possible category]"
        basis: "[Language interpretation]"
        confidence: "low"
        requires_confirmation: true
        affects: "routing"

    output_requirements:
      output_type: "clarification_question"
      audience: "user"
      format: "plain_text"
      required_fields:
        - "clarification_question"
      include_uncertainty: true
```

---

## 12. Example Packet 7: Escalation Trigger Detected

Use this example when the assistant should stop normal handling and route to escalation.

```yaml
example_packet:
  packet_id: "example-007"
  scenario: "Escalation trigger detected"
  purpose: "Demonstrate escalation handling and safe output requirements."
  expected_model_behavior: "Acknowledge the issue, avoid unsupported resolution, and prepare escalation output."
  failure_patterns_to_watch:
    - "continues normal workflow despite escalation trigger"
    - "fails to include known information in escalation summary"
    - "makes promises outside system authority"

  runtime_context_packet:
    task_context:
      current_task: "Prepare escalation response and escalation summary."
      task_goal: "Route urgent or sensitive issue to the correct owner."
      task_stage: "escalation"
      user_intent: "[Intent]"
      intent_status: "known"

    escalation_context:
      escalation_required: true
      escalation_triggers:
        - "[Trigger]"
      escalation_owner: "[Owner]"
      escalation_summary_required: true

    output_requirements:
      output_type: "escalation_note"
      audience: "user_and_internal_team"
      format: "structured_fields"
      required_fields:
        - "user_facing_acknowledgement"
        - "known_information"
        - "missing_information"
        - "urgency_or_risk_signal"
        - "next_step"
      prohibited_content:
        - "unsupported guarantees"
        - "restricted internal details"
```

---

## 13. Example Packet 8: Recovery After Interruption

Use this example when the user resumes after pausing, changing direction, or correcting earlier information.

```yaml
example_packet:
  packet_id: "example-008"
  scenario: "Recovery after interruption"
  purpose: "Demonstrate how workflow state and prior context support recovery."
  expected_model_behavior: "Summarize current state, preserve completed information, and ask only for what remains needed."
  failure_patterns_to_watch:
    - "forces user to restart"
    - "loses previously confirmed information"
    - "does not update state after correction"

  runtime_context_packet:
    workflow_context:
      current_state: "recovery"
      prior_state: "[Prior state]"
      next_expected_state: "[Next state]"
      completed_steps:
        - "[Previously completed step]"
      remaining_steps:
        - "[Remaining step]"
      interruption_or_recovery_status: "resumed"

    known_information:
      - field: "[Confirmed field]"
        value: "[Value]"
        source: "prior_session_summary"
        confirmed: true
        visibility: "user_visible"

    missing_information:
      - field: "[Remaining field]"
        required: true
        reason_needed: "[Reason]"
        blocking: true
        question_to_ask: "[Question]"

    output_requirements:
      output_type: "recovery_summary_and_question"
      audience: "user"
      format: "plain_text"
      required_fields:
        - "brief_status_summary"
        - "one_next_question"
      include_next_steps: true
```

---

## 14. Packet Review Checklist

For each example packet, check:

* Is the scenario clear?
* Is the expected model behavior defined?
* Is the workflow state included?
* Is the current task specific?
* Is user role or permission scope included when needed?
* Is known information separated from inferred information?
* Is missing information marked as blocking or non-blocking?
* Is retrieved context labeled with source, relevance, and permission status?
* Is related-record context included only when relevant?
* Is restricted context protected from user-facing output?
* Are language rules included when language may affect meaning?
* Are output requirements specific enough?
* Are allowed and prohibited actions clear?
* Are escalation conditions included when relevant?
* Are failure patterns listed for testing?

---

## 15. Open Questions

List unresolved questions about example packets.

| Question     | Why it matters | Owner     | Status                        |
| ------------ | -------------- | --------- | ----------------------------- |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |

---

## 16. Related Documents

Link to related architecture documents.

* `[AI Design Principles]`
* `[AI Assistant Behavior Spec]`
* `[Workflow Assistant Behavior Spec]`
* `[Context Architecture Spec]`
* `[Runtime Context Template]`
* `[Context Assembly Rules]`
* `[Language Handling Rules]`
* `[Workflow Routing Rules]`
* `[Related Record Context Rules]`
* `[Testing & Diagnostics Spec]`
* `[Conversation Flows]`

---

## 17. Change Log

| Date     | Change          | Owner     |
| -------- | --------------- | --------- |
| `[Date]` | `[Change made]` | `[Owner]` |
