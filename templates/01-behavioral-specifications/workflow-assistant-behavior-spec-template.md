# Workflow Assistant Behavior Spec Template

Use this document to define how an AI assistant should behave inside one specific workflow.

This document should translate general assistant behavior into workflow-specific expectations, boundaries, decision points, context needs, and outputs.

Use the general `ai-assistant-behavior-spec-template.md` for broad assistant behavior. Use this document when the assistant’s behavior depends on a specific process, service, product flow, or operational context.

---

## 1. Workflow Overview

**Workflow name:** `[Insert workflow name]`

**AI assistant name:** `[Insert assistant name]`

**System or product name:** `[Insert system/product/service name]`

**Primary workflow goal:** `[Insert the outcome this workflow supports]`

**Primary users:** `[Insert user groups or roles]`

**Document owner:** `[Insert owner/team]`

**Last updated:** `[Insert date]`

---

## 2. Workflow Purpose

Describe the workflow this assistant supports.

```text
This workflow exists to...
```

Prompts:

* What does the user need to complete?
* What information must be collected, clarified, reviewed, routed, or generated?
* What decisions happen inside this workflow?
* What handoffs happen after the assistant completes its part?
* What should the assistant make easier, clearer, or more reliable?

---

## 3. Assistant Role in This Workflow

Define the assistant’s role inside this workflow.

```text
In this workflow, the assistant acts as...
```

Examples:

* intake assistant
* triage assistant
* drafting assistant
* review preparation assistant
* routing assistant
* diagnostic assistant
* guided workflow assistant
* service request assistant

The role should clarify what the assistant does and what it does not own.

---

## 4. Workflow Users and Roles

List the user roles that may interact with or depend on the assistant.

| Role     | Description     | What the assistant can do for this role | Restrictions or considerations |
| -------- | --------------- | --------------------------------------- | ------------------------------ |
| `[Role]` | `[Description]` | `[Allowed support]`                     | `[Restrictions]`               |
| `[Role]` | `[Description]` | `[Allowed support]`                     | `[Restrictions]`               |
| `[Role]` | `[Description]` | `[Allowed support]`                     | `[Restrictions]`               |

Role examples:

* requester
* resident
* customer
* employee
* reviewer
* service team member
* administrator
* manager
* external partner

---

## 5. Workflow States

Define the main workflow states that affect assistant behavior.

| Workflow state | Description           | Assistant goal     | Primary output |
| -------------- | --------------------- | ------------------ | -------------- |
| `[State]`      | `[What is happening]` | `[Assistant goal]` | `[Output]`     |
| `[State]`      | `[What is happening]` | `[Assistant goal]` | `[Output]`     |
| `[State]`      | `[What is happening]` | `[Assistant goal]` | `[Output]`     |

Possible workflow states:

* entry / start
* intent detection
* information collection
* missing information clarification
* context retrieval
* related-record check
* draft generation
* user review
* confirmation
* submission
* routing
* escalation
* follow-up
* correction
* interruption / recovery

---

## 6. Assistant Responsibilities in This Workflow

The assistant is responsible for:

* `[Workflow-specific responsibility]`
* `[Workflow-specific responsibility]`
* `[Workflow-specific responsibility]`
* `[Workflow-specific responsibility]`

Examples:

* detect the user’s intended request type
* clarify incomplete information
* preserve the user’s original intent
* classify the request or record
* retrieve relevant permitted context
* identify related records when allowed
* prepare a structured workflow output
* recommend routing or escalation
* support user review before submission

---

## 7. Out-of-Scope Responsibilities

The assistant is not responsible for:

* `[Out-of-scope workflow responsibility]`
* `[Out-of-scope workflow responsibility]`
* `[Out-of-scope workflow responsibility]`

Examples:

* making final approval decisions
* overriding official workflow rules
* exposing restricted records
* making eligibility determinations without required data
* taking irreversible actions without confirmation
* replacing required human review
* resolving cases outside its authorized scope

---

## 8. Workflow Inputs

Define the information the assistant may receive.

| Input type | Source                                      | Required?                  | Notes     |
| ---------- | ------------------------------------------- | -------------------------- | --------- |
| `[Input]`  | `[User / system / retrieval / integration]` | `[Yes / No / Conditional]` | `[Notes]` |
| `[Input]`  | `[User / system / retrieval / integration]` | `[Yes / No / Conditional]` | `[Notes]` |
| `[Input]`  | `[User / system / retrieval / integration]` | `[Yes / No / Conditional]` | `[Notes]` |

Input examples:

* user message
* selected service category
* account or tenant context
* user role
* current workflow state
* prior interaction summary
* attached file or image
* case history
* knowledge base result
* policy or service rule
* language preference

---

## 9. Workflow Outputs

Define the outputs the assistant may produce.

| Output     | Audience                          | Format                                                | When produced      |
| ---------- | --------------------------------- | ----------------------------------------------------- | ------------------ |
| `[Output]` | `[User / internal team / system]` | `[Text / structured fields / JSON / summary / other]` | `[Workflow state]` |
| `[Output]` | `[User / internal team / system]` | `[Text / structured fields / JSON / summary / other]` | `[Workflow state]` |
| `[Output]` | `[User / internal team / system]` | `[Text / structured fields / JSON / summary / other]` | `[Workflow state]` |

Output examples:

* clarified user request
* missing information question
* structured intake record
* draft response
* summary
* classification result
* routing recommendation
* escalation note
* review checklist
* confirmation message

---

## 10. Decision Points

Define the decision points where assistant behavior changes.

| Decision point | Condition     | Assistant behavior | Human/system review needed? |
| -------------- | ------------- | ------------------ | --------------------------- |
| `[Decision]`   | `[Condition]` | `[Behavior]`       | `[Yes / No / Conditional]`  |
| `[Decision]`   | `[Condition]` | `[Behavior]`       | `[Yes / No / Conditional]`  |
| `[Decision]`   | `[Condition]` | `[Behavior]`       | `[Yes / No / Conditional]`  |

Decision point examples:

* request type is unclear
* required information is missing
* retrieved context conflicts with user input
* user lacks permission to view a related record
* urgency or risk is detected
* escalation criteria are met
* output is ready for user confirmation
* user changes request type mid-flow

---

## 11. Question-Asking Rules for This Workflow

Define when the assistant should ask the user for more information.

The assistant should ask when:

* `[Workflow-specific condition]`
* `[Workflow-specific condition]`
* `[Workflow-specific condition]`

The assistant should not ask when:

* `[Workflow-specific condition]`
* `[Workflow-specific condition]`
* `[Workflow-specific condition]`

Questions should be:

* necessary for the workflow outcome
* specific to the missing information
* easy for the user to answer
* limited to one or a small group of related items
* framed in user-facing language, not internal system language

Example question pattern:

```text
To route this correctly, I need one more detail: [specific question].
```

---

## 12. Inference Rules for This Workflow

Define what the assistant may safely infer and what requires confirmation.

| Inference     | Allowed?                   | Requires confirmation?     | Notes     |
| ------------- | -------------------------- | -------------------------- | --------- |
| `[Inference]` | `[Yes / No / Conditional]` | `[Yes / No / Conditional]` | `[Notes]` |
| `[Inference]` | `[Yes / No / Conditional]` | `[Yes / No / Conditional]` | `[Notes]` |
| `[Inference]` | `[Yes / No / Conditional]` | `[Yes / No / Conditional]` | `[Notes]` |

Inference guidance:

* Safe inferences may be used to reduce friction.
* Material inferences should be labeled or confirmed.
* Inferences affecting eligibility, routing, escalation, permissions, or user rights should be confirmed or reviewed.

---

## 13. Retrieval Behavior for This Workflow

Define when and how the assistant should retrieve information.

The assistant should retrieve context when:

* `[Workflow-specific retrieval condition]`
* `[Workflow-specific retrieval condition]`
* `[Workflow-specific retrieval condition]`

The assistant should not retrieve context when:

* `[Restriction or boundary]`
* `[Restriction or boundary]`
* `[Restriction or boundary]`

Retrieved context may include:

* service rules
* case history
* knowledge base articles
* policy documents
* prior submissions
* related records
* user account context
* status information

Retrieved context must be handled according to permission, role, tenant, source, timestamp, and confidence rules.

---

## 14. Permission and Visibility Rules

Define what the assistant can show to each user role.

| Context or record type | Visible to role(s) | Hidden from role(s) | Assistant behavior |
| ---------------------- | ------------------ | ------------------- | ------------------ |
| `[Context type]`       | `[Roles]`          | `[Roles]`           | `[Behavior]`       |
| `[Context type]`       | `[Roles]`          | `[Roles]`           | `[Behavior]`       |
| `[Context type]`       | `[Roles]`          | `[Roles]`           | `[Behavior]`       |

The assistant should:

* show only information the user is permitted to access
* avoid indirectly revealing restricted information
* explain that some information cannot be shown when appropriate
* use restricted context only when the system explicitly allows it for internal reasoning
* avoid using restricted context in user-facing explanations unless allowed

---

## 15. Language Handling Rules for This Workflow

Define how the assistant should handle language, translation, and terminology.

The assistant should:

* detect the user’s input language when possible
* respond in the user’s language when the workflow allows it
* preserve official terms, names, IDs, and service labels when required
* avoid translating terms that must remain unchanged
* flag uncertain translations when meaning affects routing or eligibility
* preserve the user’s original wording when it may be needed for review

Language-specific considerations:

| Situation                                       | Assistant behavior |
| ----------------------------------------------- | ------------------ |
| User switches language mid-flow                 | `[Behavior]`       |
| Official service term has no direct translation | `[Behavior]`       |
| User-provided wording must be preserved         | `[Behavior]`       |
| Translation affects classification or routing   | `[Behavior]`       |

---

## 16. Structured Output Requirements

Define the structure required for downstream systems, reviewers, or records.

Example structure:

```yaml
workflow_output:
  request_type: "[question | complaint | feedback | service_request | issue_report | follow_up]"
  user_summary: "[User-facing summary]"
  internal_summary: "[Internal structured summary]"
  known_information:
    - "[Known fact]"
  missing_information:
    - "[Missing item]"
  inferred_context:
    - inference: "[Inference]"
      confidence: "[low | medium | high]"
      requires_confirmation: true
  retrieved_context:
    - source: "[Source]"
      relevance: "[Relevance]"
      visible_to_user: true
  routing:
    recommended_team: "[Team]"
    reason: "[Routing reason]"
  escalation:
    required: false
    reason: "[Reason if applicable]"
```

Replace this structure with the format required by the actual workflow.

---

## 17. Confirmation Rules

Define when the assistant must ask the user to review or confirm information before proceeding.

Confirmation is required when:

* `[Condition]`
* `[Condition]`
* `[Condition]`

Confirmation may not be required when:

* `[Condition]`
* `[Condition]`
* `[Condition]`

Confirmation message pattern:

```text
Here is what I have so far. Please confirm before I submit or route this:

[Summary]
```

---

## 18. Escalation Rules

Define escalation criteria for this workflow.

| Escalation trigger | Assistant behavior | Escalation output | Human/system owner |
| ------------------ | ------------------ | ----------------- | ------------------ |
| `[Trigger]`        | `[Behavior]`       | `[Output]`        | `[Owner]`          |
| `[Trigger]`        | `[Behavior]`       | `[Output]`        | `[Owner]`          |
| `[Trigger]`        | `[Behavior]`       | `[Output]`        | `[Owner]`          |

Escalation triggers may include:

* safety concern
* urgent service issue
* legal, medical, financial, or compliance concern
* user distress or harm report
* conflicting information
* permission issue
* system outage
* policy ambiguity
* repeated failed attempts
* user dispute

---

## 19. Interruption and Recovery Rules

Define how the assistant should behave when the workflow is interrupted.

| Interruption scenario                 | Assistant behavior | State to preserve        | State to reset or re-check |
| ------------------------------------- | ------------------ | ------------------------ | -------------------------- |
| User pauses and returns later         | `[Behavior]`       | `[Preserve]`             | `[Re-check]`               |
| User changes request type             | `[Behavior]`       | `[Preserve]`             | `[Re-check]`               |
| User corrects prior information       | `[Behavior]`       | `[Preserve]`             | `[Re-check]`               |
| User asks unrelated question mid-flow | `[Behavior]`       | `[Preserve]`             | `[Re-check]`               |
| System loses context                  | `[Behavior]`       | `[Preserve if possible]` | `[Re-check]`               |

---

## 20. Failure Patterns to Test

List workflow-specific failure patterns that should be included in testing.

* `[Failure pattern]`
* `[Failure pattern]`
* `[Failure pattern]`
* `[Failure pattern]`

Suggested failure patterns:

* asks for information already provided
* misclassifies the request type
* over-infers user intent
* exposes restricted related records
* routes to the wrong team
* fails to escalate urgent cases
* changes user meaning while summarizing
* loses state after interruption
* ignores language handling rules
* produces malformed structured output

---

## 21. Example Workflow Scenarios

Use scenarios to make expected behavior testable.

| Scenario     | Context     | Expected assistant behavior | Expected output | Failure pattern to avoid |
| ------------ | ----------- | --------------------------- | --------------- | ------------------------ |
| `[Scenario]` | `[Context]` | `[Behavior]`                | `[Output]`      | `[Failure pattern]`      |
| `[Scenario]` | `[Context]` | `[Behavior]`                | `[Output]`      | `[Failure pattern]`      |
| `[Scenario]` | `[Context]` | `[Behavior]`                | `[Output]`      | `[Failure pattern]`      |

---

## 22. Open Questions

List unresolved workflow behavior decisions.

| Question     | Why it matters | Owner     | Status                        |
| ------------ | -------------- | --------- | ----------------------------- |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |

---

## 23. Related Documents

Link to related architecture documents.

* `[AI Design Principles]`
* `[AI Assistant Behavior Spec]`
* `[Context Architecture Spec]`
* `[Runtime Context Template]`
* `[Context Assembly Rules]`
* `[Language Handling Rules]`
* `[Workflow Routing Rules]`
* `[Related Record Context Rules]`
* `[Testing & Diagnostics Spec]`
* `[Conversation Flows]`

---

## 24. Change Log

| Date     | Change          | Owner     |
| -------- | --------------- | --------- |
| `[Date]` | `[Change made]` | `[Owner]` |
