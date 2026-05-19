# Conversation Flows Template

Use this document to define the user-facing conversation flows for an AI-enabled workflow.

Conversation flows should connect the assistant’s behavior to the actual service, product, or workflow experience. They should show what the user sees, what the assistant asks or produces, what context is needed, what system state changes, and where handoffs, interruptions, confirmations, or escalations occur.

This document should be used with the behavior specifications, context architecture, runtime context template, context assembly rules, routing rules, and testing diagnostics spec.

---

## 1. Conversation Flow Overview

**System name:** `[Insert system/product/service name]`

**AI assistant name:** `[Insert assistant name]`

**Workflow name:** `[Insert workflow name]`

**Flow version:** `[Insert version]`

**Primary users:** `[Insert user groups or roles]`

**Document owner:** `[Insert owner/team]`

**Last updated:** `[Insert date]`

---

## 2. Purpose of the Conversation Flows

Describe why the conversation flows are needed.

```text
These conversation flows exist to...
```

Prompts:

* What user journey or workflow does the assistant support?
* Where does the AI interaction begin?
* What information does the assistant need to collect, confirm, retrieve, or produce?
* What should the user be able to accomplish through the conversation?
* What handoffs happen after the conversation?
* What system state changes during the flow?
* What context does the assistant need at each step?

---

## 3. Relationship to SD/UX Workflow Context

Conversation flows should reflect the broader service design and UX workflow context.

Useful supporting artifacts may include:

* user journey maps
* service blueprints
* workflow-state maps
* screen flows
* form-field inventories
* handoff maps
* error and recovery flows
* escalation flows
* screenshots or prototype links
* accessibility notes
* localization notes
* operational process maps

These artifacts help explain what happens before, during, and after the AI interaction.

---

## 4. User Roles and Flow Variants

List the user roles and flow variants covered by this document.

| Role     | Description     | Flow variant  | Notes     |
| -------- | --------------- | ------------- | --------- |
| `[Role]` | `[Description]` | `[Flow name]` | `[Notes]` |
| `[Role]` | `[Description]` | `[Flow name]` | `[Notes]` |
| `[Role]` | `[Description]` | `[Flow name]` | `[Notes]` |

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

Flow variant examples:

* new request
* follow-up
* complaint
* feedback
* issue report
* service request
* review and approval
* escalation
* recovery after interruption

---

## 5. Entry Points

Define where and how the user enters the AI-assisted flow.

| Entry point     | User intent | Starting context      | First assistant action | Notes     |
| --------------- | ----------- | --------------------- | ---------------------- | --------- |
| `[Entry point]` | `[Intent]`  | `[Context available]` | `[Action]`             | `[Notes]` |
| `[Entry point]` | `[Intent]`  | `[Context available]` | `[Action]`             | `[Notes]` |
| `[Entry point]` | `[Intent]`  | `[Context available]` | `[Action]`             | `[Notes]` |

Entry point examples:

* user opens chat from dashboard
* user starts from a service page
* user clicks “submit request”
* user replies to a notification
* user uploads a document
* user resumes a saved draft
* user follows up on an existing case
* internal reviewer opens a routed record

---

## 6. Workflow State Map

Define the workflow states used in the conversation.

| Workflow state | Description     | User-facing goal | Assistant behavior | Next possible states |
| -------------- | --------------- | ---------------- | ------------------ | -------------------- |
| `[State]`      | `[Description]` | `[Goal]`         | `[Behavior]`       | `[Next states]`      |
| `[State]`      | `[Description]` | `[Goal]`         | `[Behavior]`       | `[Next states]`      |
| `[State]`      | `[Description]` | `[Goal]`         | `[Behavior]`       | `[Next states]`      |

Possible states:

* start
* intent detection
* information collection
* missing information clarification
* retrieval
* related-record check
* draft generation
* user review
* confirmation
* submission
* routing
* escalation
* follow-up
* correction
* interruption
* recovery
* handoff

---

## 7. Flow Summary

Provide a high-level summary of the conversation flow.

```text
[Summarize the flow in 5–10 steps.]
```

Example structure:

1. User enters the flow.
2. Assistant identifies the user’s intent.
3. Assistant checks available context.
4. Assistant asks only required missing-information questions.
5. Assistant retrieves permitted context when needed.
6. Assistant prepares a draft or structured record.
7. User reviews and confirms.
8. Assistant submits, routes, escalates, or hands off.
9. System updates workflow state.
10. User receives confirmation or next step.

---

## 8. Conversation Flow Detail

Use this table to define the flow step by step.

| Step | Workflow state | User action/input     | Assistant action/output     | Context needed     | System state change | Notes     |
| ---- | -------------- | --------------------- | --------------------------- | ------------------ | ------------------- | --------- |
| 1    | `[State]`      | `[User action/input]` | `[Assistant action/output]` | `[Context needed]` | `[State change]`    | `[Notes]` |
| 2    | `[State]`      | `[User action/input]` | `[Assistant action/output]` | `[Context needed]` | `[State change]`    | `[Notes]` |
| 3    | `[State]`      | `[User action/input]` | `[Assistant action/output]` | `[Context needed]` | `[State change]`    | `[Notes]` |

Context needed may include:

* user role
* workflow state
* known information
* missing information
* inferred intent
* retrieved records
* related records
* language context
* permission scope
* output requirements
* escalation rules

---

## 9. Assistant Message Patterns

Define message patterns the assistant may use in this flow.

### Start or orientation message

Purpose: help the user understand what the assistant can do.

```text
[Insert message pattern]
```

### Clarification question

Purpose: ask for required missing information.

```text
[Insert message pattern]
```

### Confirmation message

Purpose: let the user review important information before submission, routing, or handoff.

```text
[Insert message pattern]
```

### Retrieval or related-record message

Purpose: explain retrieved or related-record context safely, when user-visible.

```text
[Insert message pattern]
```

### Escalation message

Purpose: acknowledge escalation and explain the next step without overpromising.

```text
[Insert message pattern]
```

### Recovery message

Purpose: help the user resume without restarting unnecessarily.

```text
[Insert message pattern]
```

---

## 10. Question Patterns

Define the questions the assistant should ask in this flow.

| Question type     | When used     | Example question | Context required | Notes     |
| ----------------- | ------------- | ---------------- | ---------------- | --------- |
| `[Question type]` | `[Condition]` | `[Question]`     | `[Context]`      | `[Notes]` |
| `[Question type]` | `[Condition]` | `[Question]`     | `[Context]`      | `[Notes]` |
| `[Question type]` | `[Condition]` | `[Question]`     | `[Context]`      | `[Notes]` |

Question guidance:

* Ask only for information needed to move the workflow forward.
* Avoid asking for information already available in context.
* Ask one question at a time when possible.
* Group related questions only when it reduces user effort.
* Explain why a question is needed when the reason may not be obvious.
* Use user-facing language, not internal system language.

---

## 11. Confirmation and Review Moments

Define where the user should review or confirm information.

| Review moment     | What user reviews | Required before proceeding? | Assistant behavior | Notes     |
| ----------------- | ----------------- | --------------------------- | ------------------ | --------- |
| `[Review moment]` | `[Information]`   | `[Yes / No / Conditional]`  | `[Behavior]`       | `[Notes]` |
| `[Review moment]` | `[Information]`   | `[Yes / No / Conditional]`  | `[Behavior]`       | `[Notes]` |
| `[Review moment]` | `[Information]`   | `[Yes / No / Conditional]`  | `[Behavior]`       | `[Notes]` |

Review moments may include:

* confirming inferred intent
* confirming translated or restructured user input
* reviewing structured output before submission
* choosing between related record and new request
* confirming escalation summary
* confirming correction after changed information

---

## 12. Retrieval and Related-Record Moments

Define when retrieval or related-record detection happens in the flow.

| Moment     | Trigger     | Context retrieved or checked | User-visible?              | Assistant behavior |
| ---------- | ----------- | ---------------------------- | -------------------------- | ------------------ |
| `[Moment]` | `[Trigger]` | `[Context]`                  | `[Yes / No / Conditional]` | `[Behavior]`       |
| `[Moment]` | `[Trigger]` | `[Context]`                  | `[Yes / No / Conditional]` | `[Behavior]`       |
| `[Moment]` | `[Trigger]` | `[Context]`                  | `[Yes / No / Conditional]` | `[Behavior]`       |

Guidance:

* Retrieval should happen only when the workflow requires it and permissions allow it.
* Related-record findings should be explained only when the user is allowed to see them.
* Restricted related records should not be revealed through user-facing language.
* Retrieval moments should map back to context assembly rules.

---

## 13. Permission and Visibility Moments

Define where permissions affect the flow.

| Moment     | Permission condition | Assistant behavior | User-facing explanation | Notes     |
| ---------- | -------------------- | ------------------ | ----------------------- | --------- |
| `[Moment]` | `[Condition]`        | `[Behavior]`       | `[Explanation]`         | `[Notes]` |
| `[Moment]` | `[Condition]`        | `[Behavior]`       | `[Explanation]`         | `[Notes]` |
| `[Moment]` | `[Condition]`        | `[Behavior]`       | `[Explanation]`         | `[Notes]` |

Examples:

* user asks about a record they cannot view
* related record exists but is restricted
* internal reviewer sees information not visible to the user
* tenant boundary prevents retrieval
* user role affects available next steps

---

## 14. Language and Localization Moments

Define where language affects the flow.

| Moment     | Language situation | Assistant behavior | Context required | Notes     |
| ---------- | ------------------ | ------------------ | ---------------- | --------- |
| `[Moment]` | `[Situation]`      | `[Behavior]`       | `[Context]`      | `[Notes]` |
| `[Moment]` | `[Situation]`      | `[Behavior]`       | `[Context]`      | `[Notes]` |
| `[Moment]` | `[Situation]`      | `[Behavior]`       | `[Context]`      | `[Notes]` |

Examples:

* user writes in a supported non-default language
* user switches languages mid-flow
* user uses informal terms or transliteration
* retrieved context is in a different language
* official terms should not be translated
* original user wording must be preserved for review

---

## 15. Error, Interruption, and Recovery Flows

Define how the assistant should handle errors and interruptions.

| Scenario                                | Assistant behavior | State to preserve | State to reset or re-check | User-facing message |
| --------------------------------------- | ------------------ | ----------------- | -------------------------- | ------------------- |
| User pauses and returns later           | `[Behavior]`       | `[Preserve]`      | `[Re-check]`               | `[Message]`         |
| User changes request type               | `[Behavior]`       | `[Preserve]`      | `[Re-check]`               | `[Message]`         |
| User corrects earlier information       | `[Behavior]`       | `[Preserve]`      | `[Re-check]`               | `[Message]`         |
| User asks unrelated question mid-flow   | `[Behavior]`       | `[Preserve]`      | `[Re-check]`               | `[Message]`         |
| System cannot retrieve required context | `[Behavior]`       | `[Preserve]`      | `[Re-check]`               | `[Message]`         |
| User abandons flow before completion    | `[Behavior]`       | `[Preserve]`      | `[Re-check]`               | `[Message]`         |

Guidance:

* Preserve completed information where possible.
* Summarize current state when the user resumes.
* Ask only for information still needed.
* Avoid forcing restart unless required.
* Clearly distinguish a user correction from a new request.

---

## 16. Escalation and Handoff Flows

Define when the assistant should hand off to a human, team, queue, system, or another workflow.

| Escalation or handoff trigger | Destination     | Assistant behavior | Output required | User-facing message |
| ----------------------------- | --------------- | ------------------ | --------------- | ------------------- |
| `[Trigger]`                   | `[Destination]` | `[Behavior]`       | `[Output]`      | `[Message]`         |
| `[Trigger]`                   | `[Destination]` | `[Behavior]`       | `[Output]`      | `[Message]`         |
| `[Trigger]`                   | `[Destination]` | `[Behavior]`       | `[Output]`      | `[Message]`         |

Handoff outputs may include:

* user-facing confirmation
* internal summary
* structured record
* routing recommendation
* escalation reason
* missing information
* relevant context sources
* open questions
* review notes

---

## 17. Structured Output Moments

Define where the assistant produces structured outputs.

| Moment     | Output type     | Audience                                     | Required fields | Downstream use |
| ---------- | --------------- | -------------------------------------------- | --------------- | -------------- |
| `[Moment]` | `[Output type]` | `[User / internal team / system / reviewer]` | `[Fields]`      | `[Use]`        |
| `[Moment]` | `[Output type]` | `[User / internal team / system / reviewer]` | `[Fields]`      | `[Use]`        |
| `[Moment]` | `[Output type]` | `[User / internal team / system / reviewer]` | `[Fields]`      | `[Use]`        |

Examples:

* clarified request
* structured case record
* routing recommendation
* escalation summary
* reviewer brief
* user-facing confirmation
* follow-up summary

---

## 18. Screen, Form, or Interface Context

Define what screen, form, or interface context may be available during the conversation.

| Interface context             | Visible to user? | Passed to model?           | Used for | Notes     |
| ----------------------------- | ---------------- | -------------------------- | -------- | --------- |
| `[Screen/form field/context]` | `[Yes / No]`     | `[Yes / No / Conditional]` | `[Use]`  | `[Notes]` |
| `[Screen/form field/context]` | `[Yes / No]`     | `[Yes / No / Conditional]` | `[Use]`  | `[Notes]` |
| `[Screen/form field/context]` | `[Yes / No]`     | `[Yes / No / Conditional]` | `[Use]`  | `[Notes]` |

Examples:

* selected service category
* form fields already completed
* account status shown on screen
* uploaded file metadata
* previous confirmation step
* active filters or selected record
* browser or app locale
* accessibility setting

Guidance:

* Do not ask the user for information already visible or available.
* Make screen or form context available to the model only when needed.
* Label interface context separately from user-entered conversational text.

---

## 19. Accessibility and Inclusive Interaction Notes

Define accessibility or inclusive interaction considerations for the flow.

Consider:

* plain language
* screen reader compatibility
* low-literacy alternatives
* cognitive load
* one-question-at-a-time patterns
* multilingual support
* error recovery clarity
* confirmation before irreversible actions
* avoidance of overly technical system language
* support for users who cannot complete long forms

Notes:

* `[Accessibility or inclusive design note]`
* `[Accessibility or inclusive design note]`
* `[Accessibility or inclusive design note]`

---

## 20. Flow Testing Scenarios

List conversation flow scenarios that should be tested.

| Scenario     | What it tests     | Expected outcome     | Related diagnostic category |
| ------------ | ----------------- | -------------------- | --------------------------- |
| `[Scenario]` | `[What it tests]` | `[Expected outcome]` | `[Category]`                |
| `[Scenario]` | `[What it tests]` | `[Expected outcome]` | `[Category]`                |
| `[Scenario]` | `[What it tests]` | `[Expected outcome]` | `[Category]`                |

Suggested scenarios:

* user provides complete information
* user provides incomplete information
* user intent is ambiguous
* user changes request type mid-flow
* user resumes after interruption
* user switches language
* related record is found
* related record is restricted
* escalation trigger is detected
* system retrieval fails
* user corrects a structured summary
* user attempts action without permission

---

## 21. Open Questions

List unresolved UX, service design, workflow, or interaction decisions.

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
* `[Runtime Context Template]`
* `[Context Assembly Rules]`
* `[Language Handling Rules]`
* `[Workflow Routing Rules]`
* `[Related Record Context Rules]`
* `[Testing & Diagnostics Spec]`

---

## 23. Change Log

| Date     | Change          | Owner     |
| -------- | --------------- | --------- |
| `[Date]` | `[Change made]` | `[Owner]` |
