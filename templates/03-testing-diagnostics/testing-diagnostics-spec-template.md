# Testing & Diagnostics Spec Template

Use this document to define how an AI-enabled workflow should be tested, diagnosed, and improved.

AI testing should evaluate more than whether an output sounds good. It should test whether the system behaved correctly given the context it received, the workflow state it was in, the permissions that applied, and the output it was expected to produce.

This spec should help teams diagnose which layer failed: prompt, behavior rules, retrieval, context assembly, workflow state, permissions, language handling, output schema, escalation logic, or model behavior.

---

## 1. Testing & Diagnostics Overview

**System name:** `[Insert system/product/service name]`

**AI assistant name:** `[Insert assistant name]`

**Workflow name:** `[Insert workflow name]`

**Testing spec version:** `[Insert version]`

**Document owner:** `[Insert owner/team]`

**Last updated:** `[Insert date]`

---

## 2. Purpose of Testing & Diagnostics

Describe why testing and diagnostics are needed for this AI system.

```text
This testing and diagnostics spec exists to...
```

Prompts:

* What AI behaviors need to be tested?
* What workflow risks need to be detected?
* What failures would affect users, teams, operations, permissions, or trust?
* What context layers need to be validated?
* What outputs need structural validation?
* What behaviors require human review?
* What failures should be traceable to a specific architecture layer?

---

## 3. Testing Goals

The testing process should:

* verify that the assistant behaves correctly in each workflow state
* validate that runtime context packets include the right context
* detect missing, irrelevant, stale, conflicting, or restricted context
* test whether retrieved context is used appropriately
* test whether permissions and visibility rules are respected
* test whether language handling preserves meaning
* test whether the assistant asks only necessary questions
* test whether structured outputs meet required schemas
* test whether escalation rules trigger correctly
* support diagnosis by architecture layer
* create reviewable evidence for product, design, engineering, and governance teams

System-specific goals:

* `[Goal]`
* `[Goal]`
* `[Goal]`

---

## 4. Diagnostic Principle

Primary diagnostic question:

> Did the system behave correctly given the context it received?

If the answer is no, diagnose which layer failed.

Possible failure layers:

* user input interpretation
* assistant behavior specification
* workflow-state detection
* context assembly
* retrieval
* related-record detection
* permission logic
* language handling
* output schema
* escalation logic
* model behavior
* UX or service flow
* human review process

---

## 5. Test Scope

Define what is in scope and out of scope for this testing spec.

In scope:

* `[In-scope test area]`
* `[In-scope test area]`
* `[In-scope test area]`

Out of scope:

* `[Out-of-scope test area]`
* `[Out-of-scope test area]`
* `[Out-of-scope test area]`

Example in-scope areas:

* workflow behavior
* runtime context packet quality
* retrieval use
* permission boundaries
* language behavior
* structured output validity
* escalation behavior
* interruption and recovery
* false related-record matches
* human review readiness

---

## 6. Test Environments

Define where testing will occur.

| Environment     | Purpose     | Data type                                                 | Model/version     | Notes     |
| --------------- | ----------- | --------------------------------------------------------- | ----------------- | --------- |
| `[Environment]` | `[Purpose]` | `[Synthetic / anonymized / production-like / production]` | `[Model/version]` | `[Notes]` |
| `[Environment]` | `[Purpose]` | `[Synthetic / anonymized / production-like / production]` | `[Model/version]` | `[Notes]` |
| `[Environment]` | `[Purpose]` | `[Synthetic / anonymized / production-like / production]` | `[Model/version]` | `[Notes]` |

Guidance:

* Use synthetic or anonymized data when possible.
* Avoid testing with sensitive production data unless approved.
* Record model version and configuration for each test run.
* Retest when model, prompt, retrieval, context assembly, or workflow rules change.

---

## 7. Test Data Requirements

Define the data needed for testing.

Test data should include:

* complete user inputs
* incomplete user inputs
* ambiguous user inputs
* conflicting context
* stale retrieved context
* restricted records
* visible related records
* hidden related records
* multilingual inputs
* translated inputs
* interruption and recovery scenarios
* escalation-triggering scenarios
* structured output examples

Test data table:

| Data set     | Purpose     | Data sensitivity                                    | Owner     | Notes     |
| ------------ | ----------- | --------------------------------------------------- | --------- | --------- |
| `[Data set]` | `[Purpose]` | `[Synthetic / anonymized / sensitive / production]` | `[Owner]` | `[Notes]` |
| `[Data set]` | `[Purpose]` | `[Synthetic / anonymized / sensitive / production]` | `[Owner]` | `[Notes]` |
| `[Data set]` | `[Purpose]` | `[Synthetic / anonymized / sensitive / production]` | `[Owner]` | `[Notes]` |

---

## 8. Diagnostic Categories

Use these categories to organize testing.

| Category                 | What it tests                                                                                                 | Example failure                                         |
| ------------------------ | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| Reasoning behavior       | Whether the assistant distinguishes facts, inference, retrieved context, uncertainty, and missing information | Treats inference as fact                                |
| Workflow drift           | Whether the assistant stays aligned to workflow state                                                         | Gives generic advice instead of completing current step |
| Context assembly failure | Whether the runtime packet includes the right context                                                         | Missing workflow state or restricted context included   |
| Retrieval misuse         | Whether retrieved context is relevant, permitted, and used correctly                                          | Overweights irrelevant retrieved content                |
| Permission leakage       | Whether restricted information is protected                                                                   | Reveals hidden record through explanation               |
| Language instability     | Whether meaning is preserved across languages                                                                 | Misroutes due to translation ambiguity                  |
| Over-questioning         | Whether the assistant asks only necessary questions                                                           | Asks for information already provided                   |
| Hallucination            | Whether the assistant invents unsupported facts, rules, or records                                            | Fabricates policy or case status                        |
| Related-record error     | Whether related records are detected and interpreted correctly                                                | Treats weak similarity as duplicate                     |
| Output structure failure | Whether output matches required schema or fields                                                              | Missing required structured fields                      |
| Escalation failure       | Whether escalation triggers fire correctly                                                                    | Handles urgent issue as normal workflow                 |
| Recovery failure         | Whether the assistant preserves state after interruption                                                      | Forces user to restart                                  |
| Human review failure     | Whether outputs are reviewable by humans                                                                      | Missing source labels or open questions                 |

Add system-specific categories:

| Category     | What it tests     | Example failure     |
| ------------ | ----------------- | ------------------- |
| `[Category]` | `[What it tests]` | `[Example failure]` |

---

## 9. Test Case Format

Use this structure for each test case.

```yaml
test_case:
  test_case_id: "[ID]"
  title: "[Test title]"
  diagnostic_category: "[Category]"
  workflow_state: "[Workflow state]"
  user_role: "[Role]"
  purpose: "[What this test validates]"
  input:
    user_message: "[User input]"
    attachments: []
    prior_state: "[Prior state if applicable]"
  runtime_context_packet:
    packet_id: "[Packet ID or reference]"
    packet_summary: "[Summary of relevant packet contents]"
  expected_behavior:
    - "[Expected behavior]"
    - "[Expected behavior]"
  expected_output:
    output_type: "[Output type]"
    required_fields:
      - "[Field]"
    prohibited_content:
      - "[Content]"
  pass_criteria:
    - "[Pass criterion]"
    - "[Pass criterion]"
  fail_criteria:
    - "[Fail criterion]"
    - "[Fail criterion]"
  diagnostic_layer_if_failed:
    - "[Likely layer]"
  notes: "[Notes]"
```

---

## 10. Reasoning Behavior Tests

Test whether the assistant distinguishes known facts, retrieved information, inference, uncertainty, and missing information.

Scenarios to test:

* user provides incomplete information
* system has known information available
* inference is possible but not confirmed
* retrieved context conflicts with user input
* uncertainty affects routing or output

Test cases:

| Test ID | Scenario     | Expected behavior     | Failure pattern     |
| ------- | ------------ | --------------------- | ------------------- |
| `[ID]`  | `[Scenario]` | `[Expected behavior]` | `[Failure pattern]` |
| `[ID]`  | `[Scenario]` | `[Expected behavior]` | `[Failure pattern]` |

Failure patterns:

* treats inference as confirmed fact
* hides uncertainty when it affects outcome
* ignores missing information
* asks for unnecessary confirmation when confidence is sufficient
* invents facts not present in user input, retrieved context, or approved system rules

---

## 11. Workflow Drift Tests

Test whether the assistant stays aligned to the current workflow state.

Scenarios to test:

* starting a new workflow
* collecting missing information
* preparing structured output
* reviewing before submission
* routing or escalation
* following up on an existing record
* resuming after interruption

Test cases:

| Test ID | Workflow state | Expected behavior     | Failure pattern     |
| ------- | -------------- | --------------------- | ------------------- |
| `[ID]`  | `[State]`      | `[Expected behavior]` | `[Failure pattern]` |
| `[ID]`  | `[State]`      | `[Expected behavior]` | `[Failure pattern]` |

Failure patterns:

* responds as a generic assistant
* jumps ahead in the workflow
* repeats completed steps
* asks questions for the wrong workflow state
* fails to move to the next state after required information is provided

---

## 12. Context Assembly Tests

Test whether the runtime context packet contains the right context for the task.

Scenarios to test:

* required context included
* irrelevant context excluded
* restricted context labeled or excluded
* missing context marked correctly
* conflicting context flagged
* inference labeled clearly
* output requirements included

Test cases:

| Test ID | Packet condition | Expected packet behavior | Failure pattern     |
| ------- | ---------------- | ------------------------ | ------------------- |
| `[ID]`  | `[Condition]`    | `[Expected behavior]`    | `[Failure pattern]` |
| `[ID]`  | `[Condition]`    | `[Expected behavior]`    | `[Failure pattern]` |

Failure patterns:

* packet excludes context needed for correct behavior
* packet includes irrelevant context that distracts the model
* packet includes restricted context without visibility labels
* packet fails to distinguish known and inferred information
* packet does not include output requirements

---

## 13. Retrieval Tests

Test whether retrieved context is used appropriately.

Scenarios to test:

* relevant retrieved context available
* irrelevant retrieved context returned
* retrieved context is stale
* retrieved context conflicts with user input
* retrieval is not permitted
* retrieval result has low confidence

Test cases:

| Test ID | Retrieval condition | Expected behavior     | Failure pattern     |
| ------- | ------------------- | --------------------- | ------------------- |
| `[ID]`  | `[Condition]`       | `[Expected behavior]` | `[Failure pattern]` |
| `[ID]`  | `[Condition]`       | `[Expected behavior]` | `[Failure pattern]` |

Failure patterns:

* uses irrelevant retrieval result
* ignores relevant retrieved context
* treats stale context as current
* fails to respect retrieval permission rules
* fails to explain uncertainty when retrieval confidence is low

---

## 14. Permission and Visibility Tests

Test whether the assistant respects role, tenant, account, and visibility boundaries.

Scenarios to test:

* user can view record
* user cannot view record
* permission status is unknown
* related record is restricted
* internal notes are available but not user-visible
* tenant boundary prevents retrieval

Test cases:

| Test ID | Permission condition | Expected behavior     | Failure pattern     |
| ------- | -------------------- | --------------------- | ------------------- |
| `[ID]`  | `[Condition]`        | `[Expected behavior]` | `[Failure pattern]` |
| `[ID]`  | `[Condition]`        | `[Expected behavior]` | `[Failure pattern]` |

Failure patterns:

* reveals restricted information directly
* reveals restricted information indirectly through explanation
* implies hidden records exist when not allowed
* uses restricted context outside allowed internal use
* fails to explain limitation safely

---

## 15. Language Handling Tests

Test whether the assistant preserves meaning across supported languages and handles language-sensitive workflow decisions.

Scenarios to test:

* user writes in supported language
* user switches language mid-flow
* user uses informal wording, dialect, or slang
* transliteration creates ambiguity
* retrieved context is in a different language
* official term should not be translated
* translation confidence is low

Test cases:

| Test ID | Language condition | Expected behavior     | Failure pattern     |
| ------- | ------------------ | --------------------- | ------------------- |
| `[ID]`  | `[Condition]`      | `[Expected behavior]` | `[Failure pattern]` |
| `[ID]`  | `[Condition]`      | `[Expected behavior]` | `[Failure pattern]` |

Failure patterns:

* responds in the wrong language
* changes user meaning during translation
* translates official terms that should remain unchanged
* fails to preserve original user wording
* misroutes due to language ambiguity
* fails to flag low-confidence translation

---

## 16. Related Record Tests

Test whether related records, duplicates, prior cases, or follow-ups are handled correctly.

Scenarios to test:

* exact record ID match
* visible related record
* restricted related record
* weak semantic similarity
* possible duplicate
* multiple possible matches
* no related record found
* prior record conflicts with current user input

Test cases:

| Test ID | Related-record condition | Expected behavior     | Failure pattern     |
| ------- | ------------------------ | --------------------- | ------------------- |
| `[ID]`  | `[Condition]`            | `[Expected behavior]` | `[Failure pattern]` |
| `[ID]`  | `[Condition]`            | `[Expected behavior]` | `[Failure pattern]` |

Failure patterns:

* misses exact record ID
* treats weak similarity as duplicate
* exposes restricted related-record details
* routes based on unrelated records
* fails to ask clarification when multiple records may match
* fails to preserve current user intent when linking to prior record

---

## 17. Over-Questioning Tests

Test whether the assistant asks only necessary questions.

Scenarios to test:

* information already exists in context
* missing information is non-blocking
* safe inference is possible
* required information is missing
* multiple missing items exist but only one is needed now

Test cases:

| Test ID | Question condition | Expected behavior     | Failure pattern     |
| ------- | ------------------ | --------------------- | ------------------- |
| `[ID]`  | `[Condition]`      | `[Expected behavior]` | `[Failure pattern]` |
| `[ID]`  | `[Condition]`      | `[Expected behavior]` | `[Failure pattern]` |

Failure patterns:

* asks for information already provided
* asks broad discovery questions instead of targeted questions
* asks too many questions at once
* asks for information that does not affect the workflow outcome
* fails to explain why required information is needed

---

## 18. Structured Output Tests

Test whether the assistant produces the required output format.

Scenarios to test:

* user-facing response
* internal summary
* structured case record
* routing recommendation
* escalation note
* review checklist
* system-consumable JSON or YAML

Test cases:

| Test ID | Output type | Expected structure     | Failure pattern     |
| ------- | ----------- | ---------------------- | ------------------- |
| `[ID]`  | `[Output]`  | `[Expected structure]` | `[Failure pattern]` |
| `[ID]`  | `[Output]`  | `[Expected structure]` | `[Failure pattern]` |

Failure patterns:

* missing required fields
* malformed structured output
* mixes user-facing and internal content incorrectly
* includes prohibited content
* omits uncertainty or missing information when required
* changes user meaning while summarizing

---

## 19. Escalation Tests

Test whether escalation triggers work correctly.

Scenarios to test:

* urgent issue detected
* safety concern reported
* repeated unresolved issue
* permission conflict blocks progress
* user disputes prior handling
* policy ambiguity requires review
* assistant lacks required context and cannot proceed safely

Test cases:

| Test ID | Escalation condition | Expected behavior     | Failure pattern     |
| ------- | -------------------- | --------------------- | ------------------- |
| `[ID]`  | `[Condition]`        | `[Expected behavior]` | `[Failure pattern]` |
| `[ID]`  | `[Condition]`        | `[Expected behavior]` | `[Failure pattern]` |

Failure patterns:

* fails to escalate when trigger is present
* escalates unnecessarily
* continues normal workflow when escalation is required
* omits required escalation summary fields
* makes promises outside the assistant’s authority

---

## 20. Interruption and Recovery Tests

Test whether the assistant preserves useful state and recovers after interruption.

Scenarios to test:

* user pauses and resumes later
* user changes request type
* user corrects earlier information
* user asks an unrelated question mid-flow
* system loses part of the context
* user returns after escalation or human review

Test cases:

| Test ID | Recovery condition | Expected behavior     | Failure pattern     |
| ------- | ------------------ | --------------------- | ------------------- |
| `[ID]`  | `[Condition]`      | `[Expected behavior]` | `[Failure pattern]` |
| `[ID]`  | `[Condition]`      | `[Expected behavior]` | `[Failure pattern]` |

Failure patterns:

* forces user to restart unnecessarily
* loses confirmed information
* fails to update state after correction
* asks again for information already provided
* resumes with the wrong workflow state

---

## 21. Human Review Readiness Tests

Test whether outputs and records are reviewable by humans.

Scenarios to test:

* internal reviewer needs source labels
* reviewer needs known vs inferred information separated
* reviewer needs missing information listed
* reviewer needs reason for routing or escalation
* reviewer needs language notes
* reviewer needs related-record match signals
* reviewer needs unresolved questions

Test cases:

| Test ID | Review condition | Expected behavior     | Failure pattern     |
| ------- | ---------------- | --------------------- | ------------------- |
| `[ID]`  | `[Condition]`    | `[Expected behavior]` | `[Failure pattern]` |
| `[ID]`  | `[Condition]`    | `[Expected behavior]` | `[Failure pattern]` |

Failure patterns:

* output is not auditable
* missing information is hidden
* inference is not labeled
* retrieved context source is not identified
* routing reason is unclear
* review questions are not surfaced

---

## 22. Test Result Format

Use this structure to record test results.

```yaml
test_result:
  test_case_id: "[ID]"
  run_id: "[Run ID]"
  date: "[Date]"
  tester: "[Tester / system]"
  model_version: "[Model/version]"
  prompt_version: "[Prompt/version]"
  context_packet_version: "[Packet/version]"
  result: "[pass | fail | partial | needs_review]"
  observed_behavior: "[What happened]"
  expected_behavior: "[What should have happened]"
  failure_layer:
    - "[Layer]"
  severity: "[low | medium | high | critical]"
  notes: "[Notes]"
  recommended_fix: "[Fix]"
```

---

## 23. Severity Levels

Define severity levels for failures.

| Severity | Meaning                                                                               | Example                   | Required action               |
| -------- | ------------------------------------------------------------------------------------- | ------------------------- | ----------------------------- |
| Low      | Minor issue that does not affect workflow outcome                                     | Slightly verbose answer   | Track and improve             |
| Medium   | Issue affects user experience or review quality but does not create major risk        | Asks unnecessary question | Fix before release if common  |
| High     | Issue affects routing, permissions, escalation, or structured output quality          | Routes to wrong team      | Fix before release            |
| Critical | Issue creates safety, privacy, security, legal, compliance, or major operational risk | Exposes restricted record | Block release / immediate fix |

System-specific severity rules:

* `[Rule]`
* `[Rule]`
* `[Rule]`

---

## 24. Regression Testing Rules

Define when tests should be rerun.

Run regression tests when:

* model version changes
* system prompt changes
* behavior spec changes
* context architecture changes
* context assembly rules change
* retrieval source or retrieval logic changes
* permission model changes
* language handling rules change
* routing rules change
* related-record rules change
* output schema changes
* escalation rules change
* production incidents occur

Minimum regression set:

* `[Test case ID]`
* `[Test case ID]`
* `[Test case ID]`

---

## 25. Release Readiness Criteria

Define what must be true before release.

Release readiness may require:

* all critical tests pass
* high-severity failures resolved or accepted by owner
* permission leakage tests pass
* escalation tests pass
* output schema tests pass
* language tests pass for supported languages
* related-record tests pass for key scenarios
* human review outputs are usable
* known limitations documented
* unresolved risks accepted by accountable owner

Readiness checklist:

| Criterion     | Required?    | Status                    | Owner     |
| ------------- | ------------ | ------------------------- | --------- |
| `[Criterion]` | `[Yes / No]` | `[Pass / Fail / Pending]` | `[Owner]` |
| `[Criterion]` | `[Yes / No]` | `[Pass / Fail / Pending]` | `[Owner]` |
| `[Criterion]` | `[Yes / No]` | `[Pass / Fail / Pending]` | `[Owner]` |

---

## 26. Open Questions

List unresolved testing and diagnostics decisions.

| Question     | Why it matters | Owner     | Status                        |
| ------------ | -------------- | --------- | ----------------------------- |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |

---

## 27. Related Documents

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
* `[Example Context Packets]`
* `[Conversation Flows]`

---

## 28. Change Log

| Date     | Change          | Owner     |
| -------- | --------------- | --------- |
| `[Date]` | `[Change made]` | `[Owner]` |
