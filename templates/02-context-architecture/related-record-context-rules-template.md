# Related Record Context Rules Template

Use this document to define how an AI-enabled system should identify, retrieve, interpret, explain, and use related records.

Related records may include prior cases, duplicate submissions, linked requests, similar issues, previous tickets, related documents, existing accounts, prior decisions, or other records that may affect the current workflow.

Related-record rules are important because related context can improve continuity and reduce duplication, but it can also introduce permission risks, false matches, misleading summaries, or inappropriate influence on routing and decisions.

---

## 1. Related Record Rules Overview

**System name:** `[Insert system/product/service name]`

**AI assistant name:** `[Insert assistant name]`

**Workflow name:** `[Insert workflow name]`

**Related-record rules version:** `[Insert version]`

**Document owner:** `[Insert owner/team]`

**Last updated:** `[Insert date]`

---

## 2. Purpose of Related Record Rules

Describe why related-record rules are needed for this workflow.

```text
These related-record context rules exist to...
```

Prompts:

* What kinds of records may be related to the current workflow?
* Why does related-record detection matter?
* What should related records influence?
* What should related records not influence?
* What risks exist if related records are missed?
* What risks exist if unrelated records are treated as related?
* What information can be shown to the user?
* What information must remain internal or restricted?

---

## 3. Related Record Goals

The related-record system should:

* identify records that are meaningfully related to the current workflow
* reduce duplicate work when an existing record already applies
* support continuity when the user is following up on an existing item
* improve routing, escalation, or reviewer context when permitted
* distinguish related records from duplicate records
* avoid false matches that misdirect the workflow
* avoid exposing restricted records or hidden details
* explain related-record handling safely and clearly
* make related-record logic testable and reviewable

System-specific goals:

* `[Goal]`
* `[Goal]`
* `[Goal]`

---

## 4. Related Record Types

Define the types of records that may be considered related.

| Record type      | Description                                                                       | Example     | Can affect workflow?       | Notes     |
| ---------------- | --------------------------------------------------------------------------------- | ----------- | -------------------------- | --------- |
| Duplicate record | A record that appears to represent the same request, issue, case, or item         | `[Example]` | `[Yes / No / Conditional]` | `[Notes]` |
| Related record   | A record that is connected but not identical                                      | `[Example]` | `[Yes / No / Conditional]` | `[Notes]` |
| Prior record     | A previous record from the same user, account, location, asset, topic, or service | `[Example]` | `[Yes / No / Conditional]` | `[Notes]` |
| Linked record    | A record formally linked in the source system                                     | `[Example]` | `[Yes / No / Conditional]` | `[Notes]` |
| Reference record | A record used for background or comparison, but not part of the active workflow   | `[Example]` | `[Yes / No / Conditional]` | `[Notes]` |

Add system-specific record types:

| Record type     | Description     | Example     | Can affect workflow?       | Notes     |
| --------------- | --------------- | ----------- | -------------------------- | --------- |
| `[Record type]` | `[Description]` | `[Example]` | `[Yes / No / Conditional]` | `[Notes]` |

---

## 5. Related Record Sources

List the systems or repositories that may provide related records.

| Source     | Record types     | Access method                                                   | Permission model                                          | Owner     | Notes     |
| ---------- | ---------------- | --------------------------------------------------------------- | --------------------------------------------------------- | --------- | --------- |
| `[Source]` | `[Record types]` | `[API / database / search / retrieval / manual upload / other]` | `[Role-based / tenant-based / public / internal / other]` | `[Owner]` | `[Notes]` |
| `[Source]` | `[Record types]` | `[API / database / search / retrieval / manual upload / other]` | `[Role-based / tenant-based / public / internal / other]` | `[Owner]` | `[Notes]` |
| `[Source]` | `[Record types]` | `[API / database / search / retrieval / manual upload / other]` | `[Role-based / tenant-based / public / internal / other]` | `[Owner]` | `[Notes]` |

Possible sources:

* case management system
* ticketing system
* CRM
* service request database
* document repository
* knowledge base
* prior submissions database
* issue tracker
* account history
* asset or location history
* policy or decision archive
* human reviewer notes

---

## 6. Related Record Detection Triggers

Define when the system should check for related records.

Related-record detection should run when:

* `[Trigger]`
* `[Trigger]`
* `[Trigger]`

Possible triggers:

* user mentions an existing case, ticket, request, or reference number
* user asks for a follow-up or status update
* user describes an issue that may already have an open record
* user submits a new request in a category where duplicates are common
* workflow state requires duplicate or related-record screening
* routing depends on prior case history
* escalation depends on repeated issue history
* reviewer context requires prior related records

Related-record detection should not run when:

* `[Do-not-run condition]`
* `[Do-not-run condition]`
* `[Do-not-run condition]`

Examples:

* user role does not permit retrieval
* tenant or account boundary is unclear
* the workflow state does not require related-record context
* the task can be completed safely without related-record lookup
* retrieval would introduce unnecessary privacy or relevance risk

---

## 7. Match Signals

Define the signals used to identify related records.

| Signal                    | Description                                                                       | Strength                   | Notes     |
| ------------------------- | --------------------------------------------------------------------------------- | -------------------------- | --------- |
| Exact ID match            | User provides an exact case, ticket, request, account, or reference ID            | High                       | `[Notes]` |
| User/account match        | Record belongs to the same user or account                                        | High / Conditional         | `[Notes]` |
| Location or asset match   | Record involves the same location, asset, system, or service component            | Medium / High              | `[Notes]` |
| Category match            | Record is in the same service, product, issue, or topic category                  | Medium                     | `[Notes]` |
| Semantic similarity       | Record description is semantically similar to current input                       | Low / Medium / Conditional | `[Notes]` |
| Time window match         | Record occurred within a relevant time period                                     | Conditional                | `[Notes]` |
| Status match              | Existing record has relevant open, pending, closed, escalated, or resolved status | Conditional                | `[Notes]` |
| Human-linked relationship | Record was previously linked by a human reviewer or system rule                   | High                       | `[Notes]` |

Add system-specific signals:

| Signal     | Description     | Strength     | Notes     |
| ---------- | --------------- | ------------ | --------- |
| `[Signal]` | `[Description]` | `[Strength]` | `[Notes]` |

---

## 8. Match Strength Rules

Define how related-record match strength should be interpreted.

| Match strength         | Meaning                                                                   | Assistant behavior                               | Human review needed?       |
| ---------------------- | ------------------------------------------------------------------------- | ------------------------------------------------ | -------------------------- |
| Exact match            | Strong evidence the record is the same item or explicitly referenced item | Use record if permitted                          | `[Yes / No / Conditional]` |
| Strong related match   | Multiple high-value signals indicate the record is relevant               | Include or flag according to visibility rules    | `[Yes / No / Conditional]` |
| Possible related match | Some signals indicate relevance, but confidence is incomplete             | Ask clarification or flag for review             | `[Yes / No / Conditional]` |
| Weak match             | Similarity exists but may be coincidental                                 | Do not rely on match without additional evidence | `[Yes / No / Conditional]` |
| No match               | No meaningful related record found                                        | Continue current workflow                        | No                         |
| Conflicting matches    | Multiple records appear possible or context conflicts                     | Ask clarification or route for review            | Yes / Conditional          |

Guidance:

* Exact IDs should be handled differently from semantic similarity.
* Semantic similarity alone should not usually be treated as a duplicate.
* Match strength should consider permissions, freshness, workflow state, and source reliability.
* Low-confidence matches should not be presented as confirmed related records.

---

## 9. Duplicate vs. Related Record Rules

Define the difference between duplicate and related records.

| Classification | Definition     | Assistant behavior | Notes     |
| -------------- | -------------- | ------------------ | --------- |
| Duplicate      | `[Definition]` | `[Behavior]`       | `[Notes]` |
| Related        | `[Definition]` | `[Behavior]`       | `[Notes]` |
| Follow-up      | `[Definition]` | `[Behavior]`       | `[Notes]` |
| Reference-only | `[Definition]` | `[Behavior]`       | `[Notes]` |
| Unrelated      | `[Definition]` | `[Behavior]`       | `[Notes]` |

Default guidance:

* Duplicate records may redirect, merge, block, or link the current workflow depending on system rules.
* Related records may inform routing, escalation, or review without blocking submission.
* Follow-up records should preserve continuity with the existing item when allowed.
* Reference-only records should not drive routing or decisions without additional support.
* Unrelated records should be excluded from the runtime packet.

---

## 10. Permission and Visibility Rules

Define what related-record information can be shown to different roles.

| Record/context type     | Visible to role(s) | Hidden from role(s) | Assistant behavior |
| ----------------------- | ------------------ | ------------------- | ------------------ |
| `[Record/context type]` | `[Roles]`          | `[Roles]`           | `[Behavior]`       |
| `[Record/context type]` | `[Roles]`          | `[Roles]`           | `[Behavior]`       |
| `[Record/context type]` | `[Roles]`          | `[Roles]`           | `[Behavior]`       |

The assistant should:

* show only related-record information the user is permitted to access
* avoid revealing hidden records through comparison language
* avoid saying or implying that restricted records exist unless allowed
* use internal-only related-record context only for permitted internal workflow purposes
* provide safe user-facing explanations when related-record details cannot be shown

Example safe limitation pattern:

```text
I can’t show all internal review details here, but I can still help prepare the next step based on the information you provided.
```

---

## 11. Related Record Runtime Context Format

Define how related-record context should appear in the runtime packet.

Example format:

```yaml
related_record_context:
  checked: true
  check_trigger: "[Trigger]"
  results:
    - record_id: "[Record ID]"
      record_type: "[duplicate | related | follow_up | reference_only]"
      source: "[Source]"
      match_strength: "[exact | strong | possible | weak]"
      match_signals:
        - "[Signal]"
      summary: "[User-visible or internal summary depending on permissions]"
      status: "[open | pending | closed | escalated | unknown]"
      timestamp: "[Created or updated timestamp]"
      permission_status: "[allowed | restricted | internal_only | unknown]"
      visible_to_user: true
      allowed_use: "[user_display | routing | escalation | reviewer_context | none]"
      action_recommended: "[link | merge | continue | ask_clarification | escalate | ignore]"
  no_match_reason: "[Reason if checked and no meaningful match found]"
```

Guidance:

* Include whether related-record detection was checked.
* Include the trigger for the check.
* Include match strength and signals.
* Include permission and visibility status.
* Include allowed use.
* Do not include unrelated records.

---

## 12. User-Facing Explanation Rules

Define how the assistant should explain related-record findings to the user.

The assistant may explain related records when:

* the user is allowed to see the related record
* the related record affects the next step
* the explanation helps the user avoid duplicate submission
* the system needs confirmation before linking, merging, or following up

The assistant should not explain related records when:

* the record is restricted
* the explanation would reveal hidden information
* the match is too weak
* the related context is used only for internal routing and not user-facing action

Preferred patterns:

```text
I found an existing record that may be related to this. Do you want to continue with that case or create a new request?
```

```text
This looks like it may be a follow-up to an existing case. Please confirm whether this is about case [visible ID].
```

```text
I don’t have enough information to confirm whether this is related to an existing record. I need one detail before proceeding: [question].
```

Avoid:

* overstating possible matches as confirmed duplicates
* exposing restricted record details
* using vague language that implies hidden records exist
* forcing the user into an existing case when the match is uncertain

---

## 13. Related Record Influence Rules

Define what related records are allowed to influence.

| Related-record status         | May influence         | Must not influence | Notes     |
| ----------------------------- | --------------------- | ------------------ | --------- |
| Exact visible duplicate       | `[Allowed influence]` | `[Limits]`         | `[Notes]` |
| Exact restricted duplicate    | `[Allowed influence]` | `[Limits]`         | `[Notes]` |
| Strong related visible record | `[Allowed influence]` | `[Limits]`         | `[Notes]` |
| Possible related record       | `[Allowed influence]` | `[Limits]`         | `[Notes]` |
| Weak related record           | `[Allowed influence]` | `[Limits]`         | `[Notes]` |
| No match                      | `[Allowed influence]` | `[Limits]`         | `[Notes]` |

Possible influence areas:

* routing recommendation
* escalation flag
* duplicate prevention
* follow-up workflow
* reviewer context
* user confirmation question
* internal summary
* priority or urgency review

Guidance:

* Weak matches should not drive major workflow decisions.
* Related records should not override current user input without review.
* Restricted related records should not appear in user-facing summaries.
* If related records affect routing or escalation, the reason should be traceable.

---

## 14. Follow-Up Handling Rules

Define how the assistant should handle follow-ups on existing records.

A user may be following up when they:

* provide a case, ticket, request, or reference ID
* ask for status
* refer to a previous issue or request
* say they already submitted something
* describe a repeated issue
* respond to a prior message or notification

| Follow-up situation                    | Assistant behavior | Required context | Output     |
| -------------------------------------- | ------------------ | ---------------- | ---------- |
| User provides exact record ID          | `[Behavior]`       | `[Context]`      | `[Output]` |
| User references prior issue without ID | `[Behavior]`       | `[Context]`      | `[Output]` |
| User reports repeated issue            | `[Behavior]`       | `[Context]`      | `[Output]` |
| User disputes prior handling           | `[Behavior]`       | `[Context]`      | `[Output]` |
| User lacks permission to view record   | `[Behavior]`       | `[Context]`      | `[Output]` |

Guidance:

* Use exact IDs when available and permitted.
* Ask for identifying details only when required.
* Preserve continuity with the existing record when possible.
* Avoid exposing restricted prior record details.
* Escalate disputes or repeated unresolved issues when rules require it.

---

## 15. Related Record Conflict Rules

Define what happens when related records conflict with each other or with user input.

Conflict types:

* multiple possible related records
* user input conflicts with existing record
* related record status conflicts with user claim
* retrieved records conflict across sources
* record appears related but belongs to another user, account, tenant, or jurisdiction
* related record is outdated or closed but user reports ongoing issue

| Conflict type | Assistant behavior | Review needed?             | Notes     |
| ------------- | ------------------ | -------------------------- | --------- |
| `[Conflict]`  | `[Behavior]`       | `[Yes / No / Conditional]` | `[Notes]` |
| `[Conflict]`  | `[Behavior]`       | `[Yes / No / Conditional]` | `[Notes]` |
| `[Conflict]`  | `[Behavior]`       | `[Yes / No / Conditional]` | `[Notes]` |

Default guidance:

* Do not silently choose one record when multiple plausible matches exist.
* Ask a targeted clarification question when user input can resolve the conflict.
* Route for human review when permissions, ownership, or status conflicts cannot be resolved safely.
* Preserve the current user’s description even when it differs from prior records.

---

## 16. Human Review Rules

Define when related-record findings require human review.

Human review may be required when:

* match confidence is low or conflicting
* the record affects routing, escalation, eligibility, rights, or service outcome
* the user disputes a duplicate or related-record finding
* related records cross account, tenant, jurisdiction, or ownership boundaries
* restricted context is needed for a decision
* related records contain conflicting statuses or notes
* repeated issues may require escalation
* language ambiguity affects related-record matching

| Review trigger | Assistant behavior | Review owner | Review output |
| -------------- | ------------------ | ------------ | ------------- |
| `[Trigger]`    | `[Behavior]`       | `[Owner]`    | `[Output]`    |
| `[Trigger]`    | `[Behavior]`       | `[Owner]`    | `[Output]`    |
| `[Trigger]`    | `[Behavior]`       | `[Owner]`    | `[Output]`    |

---

## 17. Logging and Traceability

Define what should be logged for related-record detection and use.

Log when allowed:

* whether related-record detection was run
* trigger for related-record detection
* sources searched
* query or matching method used
* records returned
* records included in runtime packet
* records excluded and why
* match strength
* match signals
* permission status
* visibility status
* action recommended
* human review flag

Do not log:

* restricted record contents beyond approved retention rules
* unnecessary personal data
* hidden records in user-visible logs
* sensitive internal notes unless explicitly permitted

Logging should support diagnosis without increasing privacy, security, or permission risk.

---

## 18. Related Record Failure Patterns to Test

Test for these related-record failure patterns:

* misses exact record ID provided by user
* treats weak semantic similarity as a duplicate
* fails to detect a true duplicate
* exposes restricted related-record details
* implies a restricted record exists when not allowed
* routes based on unrelated records
* ignores visible related case history
* fails to ask clarification when multiple records may match
* overrides current user input with prior record content
* fails to preserve user intent when linking to prior record
* uses stale related records as current
* ignores language or transliteration ambiguity
* fails to flag follow-up on repeated unresolved issue
* produces related-record context without match signals
* includes unrelated records in the runtime packet

System-specific failure patterns:

* `[Failure pattern]`
* `[Failure pattern]`
* `[Failure pattern]`

---

## 19. Related Record Review Checklist

Before approving related-record rules, check:

* Are related record types defined?
* Are related record sources identified?
* Are detection triggers defined?
* Are match signals and match strength rules clear?
* Is duplicate vs. related vs. follow-up distinction defined?
* Are permission and visibility rules defined?
* Is the runtime context format defined?
* Are user-facing explanation rules safe?
* Are influence limits defined?
* Are conflict handling rules defined?
* Are human review triggers defined?
* Are related-record failure patterns included in testing?

---

## 20. Open Questions

List unresolved related-record decisions.

| Question     | Why it matters | Owner     | Status                        |
| ------------ | -------------- | --------- | ----------------------------- |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |

---

## 21. Related Documents

Link to related architecture documents.

* `[AI Design Principles]`
* `[AI Assistant Behavior Spec]`
* `[Workflow Assistant Behavior Spec]`
* `[Context Architecture Spec]`
* `[Runtime Context Template]`
* `[Context Assembly Rules]`
* `[Language Handling Rules]`
* `[Workflow Routing Rules]`
* `[Testing & Diagnostics Spec]`
* `[Conversation Flows]`

---

## 22. Change Log

| Date     | Change          | Owner     |
| -------- | --------------- | --------- |
| `[Date]` | `[Change made]` | `[Owner]` |
