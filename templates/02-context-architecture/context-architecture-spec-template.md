# Context Architecture Spec Template

Use this document to define the context architecture for an AI-enabled workflow or system.

This spec should describe what context exists, where it comes from, when it is available, how it is labeled, how it is prioritized, and how the model should interpret it.

This document is the high-level context map. More specific rules should be documented in related files such as runtime context templates, context assembly rules, language handling rules, workflow routing rules, and related-record context rules.

---

## 1. Context Architecture Overview

**System name:** `[Insert system/product/service name]`

**AI assistant name:** `[Insert assistant name]`

**Workflow name:** `[Insert workflow name or scope]`

**Primary users:** `[Insert user groups or roles]`

**Document owner:** `[Insert owner/team]`

**Last updated:** `[Insert date]`

---

## 2. Purpose of Context Architecture

Describe why context architecture is needed for this system.

```text
This context architecture exists to...
```

Prompts:

* What behavior depends on context?
* What decisions require workflow state?
* What information must be retrieved dynamically?
* What information must be protected or hidden?
* What context must be labeled so the model can interpret it correctly?
* What failures are likely if context is missing, messy, outdated, or misclassified?

---

## 3. Context Architecture Goals

Define what the context architecture should make possible.

Goals may include:

* provide the model with enough context to behave correctly
* reduce unnecessary user questions
* preserve workflow continuity
* distinguish known, inferred, retrieved, and restricted information
* prevent permission leakage
* support multilingual interactions
* enable structured outputs
* support diagnostics and failure analysis
* make context decisions reviewable by humans

System-specific goals:

* `[Goal]`
* `[Goal]`
* `[Goal]`

---

## 4. Context Types

Define the main context types used by the system.

| Context type           | Description                                                                     | Source     | Example     | Notes     |
| ---------------------- | ------------------------------------------------------------------------------- | ---------- | ----------- | --------- |
| Known context          | Information explicitly provided by the user or system                           | `[Source]` | `[Example]` | `[Notes]` |
| Inferred context       | Information derived from interaction but not directly stated                    | `[Source]` | `[Example]` | `[Notes]` |
| Retrieved context      | Information pulled from approved sources                                        | `[Source]` | `[Example]` | `[Notes]` |
| Workflow-state context | Information about where the user is in the workflow                             | `[Source]` | `[Example]` | `[Notes]` |
| Role context           | Information about the user’s role or permission level                           | `[Source]` | `[Example]` | `[Notes]` |
| Tenant context         | Information about account, organization, or data boundary                       | `[Source]` | `[Example]` | `[Notes]` |
| Language context       | Information about language, translation, terminology, and multilingual handling | `[Source]` | `[Example]` | `[Notes]` |
| Output context         | Information about required output format, schema, or downstream use             | `[Source]` | `[Example]` | `[Notes]` |
| Diagnostic context     | Information used to test or evaluate behavior                                   | `[Source]` | `[Example]` | `[Notes]` |

Add system-specific context types:

| Context type     | Description     | Source     | Example     | Notes     |
| ---------------- | --------------- | ---------- | ----------- | --------- |
| `[Context type]` | `[Description]` | `[Source]` | `[Example]` | `[Notes]` |
| `[Context type]` | `[Description]` | `[Source]` | `[Example]` | `[Notes]` |

---

## 5. Context Sources

List all sources that may provide context to the AI system.

| Source     | Context provided | Access method                                                      | Owner     | Reliability                       | Notes     |
| ---------- | ---------------- | ------------------------------------------------------------------ | --------- | --------------------------------- | --------- |
| `[Source]` | `[Context]`      | `[API / database / retrieval / user input / system state / other]` | `[Owner]` | `[High / Medium / Low / Unknown]` | `[Notes]` |
| `[Source]` | `[Context]`      | `[API / database / retrieval / user input / system state / other]` | `[Owner]` | `[High / Medium / Low / Unknown]` | `[Notes]` |
| `[Source]` | `[Context]`      | `[API / database / retrieval / user input / system state / other]` | `[Owner]` | `[High / Medium / Low / Unknown]` | `[Notes]` |

Possible sources:

* user message
* user profile or account record
* workflow state manager
* case management system
* document repository
* knowledge base
* policy database
* service catalog
* CRM or support system
* prior session summary
* uploaded files
* language detection service
* analytics or logs
* human reviewer input

---

## 6. Context Availability

Define when each context type is available.

| Context     | Always available? | Conditional availability | Missing-context behavior                           |
| ----------- | ----------------- | ------------------------ | -------------------------------------------------- |
| `[Context]` | `[Yes / No]`      | `[Condition]`            | `[What the assistant/system should do if missing]` |
| `[Context]` | `[Yes / No]`      | `[Condition]`            | `[What the assistant/system should do if missing]` |
| `[Context]` | `[Yes / No]`      | `[Condition]`            | `[What the assistant/system should do if missing]` |

Guidance:

* Do not assume all context is always available.
* Define fallback behavior when required context is missing.
* Identify which missing context should block the workflow.
* Identify which missing context can be handled with a labeled assumption.

---

## 7. Context Status Labels

Define labels the system should use so the model can interpret context correctly.

Suggested labels:

| Label         | Meaning                                              | Model behavior                                                   |
| ------------- | ---------------------------------------------------- | ---------------------------------------------------------------- |
| `known`       | Explicitly provided or confirmed information         | Treat as available fact unless contradicted                      |
| `inferred`    | Derived but not directly confirmed                   | Use carefully; confirm if material                               |
| `retrieved`   | Pulled from an approved source                       | Use according to source, relevance, timestamp, and permissions   |
| `restricted`  | Not visible or not usable in user-facing output      | Do not expose directly or indirectly                             |
| `missing`     | Required or useful information not available         | Ask, infer safely, proceed partially, or escalate based on rules |
| `uncertain`   | Information with low confidence or possible conflict | Surface uncertainty when it affects the workflow                 |
| `conflicting` | Context conflicts with another source                | Do not resolve silently; follow conflict rules                   |
| `stale`       | Information may be outdated                          | Use caution; retrieve updated source or ask for confirmation     |

System-specific labels:

| Label     | Meaning     | Model behavior |
| --------- | ----------- | -------------- |
| `[Label]` | `[Meaning]` | `[Behavior]`   |
| `[Label]` | `[Meaning]` | `[Behavior]`   |

---

## 8. Context Priority Rules

Define how the system should prioritize context when multiple sources are available.

Priority order:

1. `[Highest priority context/source]`
2. `[Next priority context/source]`
3. `[Next priority context/source]`
4. `[Lowest priority context/source]`

Suggested priority considerations:

* confirmed user input may override inferred context
* current workflow state may override prior session assumptions
* authoritative system records may override outdated user memory
* permission rules override convenience
* source freshness may matter for policies, statuses, or availability
* retrieved context should not override confirmed workflow rules unless explicitly allowed

Conflict example:

```text
If user-provided information conflicts with retrieved system information, the assistant should [insert behavior].
```

---

## 9. Context Boundary Rules

Define what context must not be passed to the model, must not be used for reasoning, or must not appear in user-facing output.

| Context or data type  | Boundary                                                           | Reason     | Allowed use     | Prohibited use     |
| --------------------- | ------------------------------------------------------------------ | ---------- | --------------- | ------------------ |
| `[Context/data type]` | `[Do not retrieve / do not expose / internal only / role-limited]` | `[Reason]` | `[Allowed use]` | `[Prohibited use]` |
| `[Context/data type]` | `[Do not retrieve / do not expose / internal only / role-limited]` | `[Reason]` | `[Allowed use]` | `[Prohibited use]` |

Boundary categories:

* role-based visibility
* tenant or account boundaries
* personally identifiable information
* confidential business information
* security-sensitive information
* legal, financial, medical, or compliance-sensitive information
* internal-only workflow logic
* human reviewer notes
* hidden scoring or risk signals

---

## 10. Retrieval Context Rules

Define what retrieval is allowed to access and how retrieved context should be used.

Retrieval is allowed for:

* `[Allowed source/use case]`
* `[Allowed source/use case]`
* `[Allowed source/use case]`

Retrieval is not allowed for:

* `[Restricted source/use case]`
* `[Restricted source/use case]`
* `[Restricted source/use case]`

Retrieved context should include metadata such as:

* source
* timestamp
* owner
* relevance score or reason
* permission status
* visibility status
* confidence or reliability signal
* retrieval query or trigger

Retrieved context should be excluded or down-weighted when:

* source is outdated
* relevance is weak
* permissions are unclear
* context conflicts with higher-priority rules
* source cannot be interpreted safely
* user-facing explanation would reveal restricted information

---

## 11. Workflow-State Context Rules

Define how workflow state affects context selection and assistant behavior.

| Workflow state | Context needed     | Context excluded     | Assistant behavior |
| -------------- | ------------------ | -------------------- | ------------------ |
| `[State]`      | `[Needed context]` | `[Excluded context]` | `[Behavior]`       |
| `[State]`      | `[Needed context]` | `[Excluded context]` | `[Behavior]`       |
| `[State]`      | `[Needed context]` | `[Excluded context]` | `[Behavior]`       |

Examples of workflow states:

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

## 12. Role Context Rules

Define how user role affects context access, assistant behavior, and output.

| Role     | Context available     | Context restricted     | Output differences  | Notes     |
| -------- | --------------------- | ---------------------- | ------------------- | --------- |
| `[Role]` | `[Available context]` | `[Restricted context]` | `[Output behavior]` | `[Notes]` |
| `[Role]` | `[Available context]` | `[Restricted context]` | `[Output behavior]` | `[Notes]` |
| `[Role]` | `[Available context]` | `[Restricted context]` | `[Output behavior]` | `[Notes]` |

Guidance:

* Role context should be explicit in runtime packets.
* User-facing explanations should reflect what the role is allowed to know.
* The assistant should not reveal restricted information through indirect summaries, comparisons, or explanations.

---

## 13. Language Context Rules

Define how language affects context use.

Language context may include:

* input language
* output language
* user language preference
* official service language
* translated content
* preserved original wording
* terms that should not be translated
* uncertain translations

| Language situation                            | Context rule | Assistant behavior |
| --------------------------------------------- | ------------ | ------------------ |
| User writes in a supported language           | `[Rule]`     | `[Behavior]`       |
| User switches languages mid-flow              | `[Rule]`     | `[Behavior]`       |
| Retrieved context is in a different language  | `[Rule]`     | `[Behavior]`       |
| Official term has no direct translation       | `[Rule]`     | `[Behavior]`       |
| Translation may affect routing or eligibility | `[Rule]`     | `[Behavior]`       |

Reference related document:

* `[Language Handling Rules]`

---

## 14. Output Context Rules

Define how context should shape outputs.

Outputs may require different context depending on audience.

| Output type | Audience                          | Required context     | Restricted context   | Format     |
| ----------- | --------------------------------- | -------------------- | -------------------- | ---------- |
| `[Output]`  | `[User / internal team / system]` | `[Context required]` | `[Context excluded]` | `[Format]` |
| `[Output]`  | `[User / internal team / system]` | `[Context required]` | `[Context excluded]` | `[Format]` |
| `[Output]`  | `[User / internal team / system]` | `[Context required]` | `[Context excluded]` | `[Format]` |

Guidance:

* User-facing outputs should include only visible/allowed information.
* Internal outputs may include additional context if explicitly permitted.
* Structured outputs should preserve context source labels where useful.
* Outputs should separate known facts, inference, missing information, and retrieved context when the workflow needs reviewability.

---

## 15. Context Assembly Overview

Summarize how context should be assembled before the model is asked to respond.

High-level assembly sequence:

1. Identify workflow state.
2. Identify user role and permission scope.
3. Load always-required system context.
4. Add current user input.
5. Add known session context.
6. Add retrieved context if triggered and permitted.
7. Add inferred context with labels.
8. Apply language rules.
9. Apply output format requirements.
10. Exclude restricted or irrelevant context.
11. Label context sources and status.
12. Create runtime context packet.

Reference related documents:

* `[Runtime Context Template]`
* `[Context Assembly Rules]`

---

## 16. Context Failure Patterns

List context-related failure patterns the system should test for.

* model receives too little context
* model receives too much irrelevant context
* retrieved context is used without permission checks
* inferred context is treated as fact
* stale context is treated as current
* conflicting context is resolved silently
* user-facing output reveals restricted information
* workflow state is missing or incorrect
* role context is missing or incorrect
* language rules are not applied
* structured output loses source labels
* model asks for information already available in context

System-specific failure patterns:

* `[Failure pattern]`
* `[Failure pattern]`
* `[Failure pattern]`

---

## 17. Diagnostic Questions

Use these questions when reviewing or testing context architecture.

* What context does the model need to behave correctly in this workflow state?
* Is the context explicitly labeled by source and status?
* Does the model know what is known, inferred, retrieved, missing, restricted, or uncertain?
* Are permission boundaries applied before context reaches the model?
* Are language rules applied before output is generated?
* Can the system explain why context was included or excluded?
* Can failures be traced to retrieval, assembly, workflow state, permissions, or model behavior?
* Does the context packet support the expected output format?

---

## 18. Open Questions

List unresolved context architecture decisions.

| Question     | Why it matters | Owner     | Status                        |
| ------------ | -------------- | --------- | ----------------------------- |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |

---

## 19. Related Documents

Link to related architecture documents.

* `[AI Design Principles]`
* `[AI Assistant Behavior Spec]`
* `[Workflow Assistant Behavior Spec]`
* `[Runtime Context Template]`
* `[Context Assembly Rules]`
* `[Language Handling Rules]`
* `[Workflow Routing Rules]`
* `[Related Record Context Rules]`
* `[Testing & Diagnostics Spec]`
* `[Conversation Flows]`

---

## 20. Change Log

| Date     | Change          | Owner     |
| -------- | --------------- | --------- |
| `[Date]` | `[Change made]` | `[Owner]` |
