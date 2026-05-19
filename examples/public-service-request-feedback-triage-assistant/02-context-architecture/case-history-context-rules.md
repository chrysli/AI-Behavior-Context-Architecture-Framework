# Case History Context Rules

This document defines how the fictional **Public Service Request & Feedback Triage Assistant** should identify, retrieve, interpret, explain, and use case history context.

Case history may include prior cases, existing service requests, complaint records, issue reports, follow-ups, related cases, duplicate reports, or internal review records.

Case history can improve continuity and reduce duplicate work, but it also creates permission, privacy, interpretation, and false-match risks.

---

## 1. Case History Rules Overview

**System name:** Public Service Request & Feedback Triage Assistant

**Workflow name:** Public Service Request & Feedback Triage

**Case history rules version:** 0.1 fictional sample

**Sample status:** Fictional public-safe architecture example

---

## 2. Purpose of Case History Context Rules

These rules exist to define when case history should be checked, what case information may be used, what can be shown to the user, and how related or duplicate cases should affect the workflow.

The assistant should use case history to support continuity and review, but it must avoid exposing restricted records, implying hidden records exist, or allowing weak matches to drive routing decisions.

---

## 3. Case History Goals

The case history layer should:

* support follow-up on existing cases
* reduce duplicate submissions when a visible existing case applies
* identify repeated unresolved issues when permitted
* support routing or escalation when prior context matters
* distinguish exact matches, related cases, duplicates, and weak matches
* protect case history the user is not allowed to view
* preserve user intent when linking to prior cases
* make case-history influence reviewable by human service teams

---

## 4. Case Record Types

| Record type            | Description                                                                      | Example                                             | Can affect workflow? | Notes                                    |
| ---------------------- | -------------------------------------------------------------------------------- | --------------------------------------------------- | -------------------- | ---------------------------------------- |
| Own case               | A case submitted by or visible to the current authenticated user                 | User follows up on `CASE-12345`                     | Yes                  | User may see permitted fields            |
| Duplicate case         | A case that appears to represent the same issue or request                       | Same user, same location, same issue, active status | Yes                  | Usually requires confirmation or review  |
| Related case           | A case connected by location, service category, account, asset, or issue pattern | Similar streetlight issue nearby                    | Conditional          | May support routing or review            |
| Restricted case        | A case that exists but is not visible to the current user                        | Another user’s case at same location                | Conditional          | Do not expose to user                    |
| Prior resolved case    | A closed case that may provide context                                           | Similar issue resolved last month                   | Conditional          | Avoid treating as current without review |
| Internal review record | Internal notes, decisions, or prior handling records                             | Reviewer notes                                      | Conditional          | Internal-only unless policy allows       |

---

## 5. Case History Sources

| Source                 | Record types                               | Access model                                | Notes                                             |
| ---------------------- | ------------------------------------------ | ------------------------------------------- | ------------------------------------------------- |
| Case management system | Own cases, related cases, restricted cases | Role and permission-based                   | Main source of case context                       |
| User account record    | User-owned cases and submitted requests    | Authenticated user only                     | Used for follow-up continuity                     |
| Service team queue     | Assigned or routed cases                   | Internal role only                          | Used by service team members                      |
| Review queue           | Ambiguous, escalated, or disputed cases    | Reviewer role only                          | Used for human review                             |
| Public status tracker  | Publicly visible case or service status    | Public or authenticated depending on system | May provide limited case visibility               |
| Prior session summary  | User’s previous interaction state          | Session-based                               | Used for recovery, not authoritative case history |

---

## 6. Case History Retrieval Triggers

Case history retrieval should run when:

* the user provides a case ID or reference number
* the user asks for case status or follow-up
* the user says they already reported the issue
* the user describes a repeated unresolved issue
* duplicate detection is required before new case creation
* routing or escalation depends on prior case context
* an internal reviewer opens a routed case
* the workflow state is `case_history_check`

Case history retrieval should not run when:

* the user is anonymous and requests private case details
* authentication status is unknown and private history would be required
* permission scope is unavailable
* case history is not relevant to the current workflow state
* retrieval could cross user, account, tenant, or jurisdiction boundaries
* the request can be handled safely without case history

---

## 7. Match Signals

| Signal                  | Description                                            | Strength           | Notes                                      |
| ----------------------- | ------------------------------------------------------ | ------------------ | ------------------------------------------ |
| Exact case ID           | User provides exact case or reference ID               | High               | Requires permission check                  |
| Same authenticated user | Case belongs to current user                           | High               | Visibility still depends on permissions    |
| Same business account   | Case belongs to authorized business account            | High / conditional | Business role must be validated            |
| Same location           | Issue or request involves same location                | Medium/high        | May indicate duplicate or related issue    |
| Same service category   | Same category as current request                       | Medium             | Not enough alone for duplicate             |
| Similar description     | Similar wording or semantic meaning                    | Low/medium         | Should not drive duplicate decision alone  |
| Same asset              | Same asset, facility, road, form, or service component | Medium/high        | May support related-case match             |
| Recent time window      | Case created or updated recently                       | Conditional        | Helps interpret relevance                  |
| Open status             | Case is active or pending                              | Conditional        | May affect follow-up or duplicate handling |
| Human-linked record     | A prior reviewer linked records                        | High               | Strong signal if permission allows use     |

---

## 8. Match Strength Rules

| Match strength   | Meaning                                                      | Assistant behavior                     | Human review needed?      |
| ---------------- | ------------------------------------------------------------ | -------------------------------------- | ------------------------- |
| Exact match      | User-provided case ID matches a permitted record             | Use permitted case context             | Conditional               |
| Strong duplicate | Multiple strong signals suggest same issue or request        | Ask confirmation or route for review   | Conditional               |
| Strong related   | Record is connected but not necessarily duplicate            | Use for routing or review if permitted | Conditional               |
| Possible related | Some signals indicate relevance but confidence is incomplete | Ask clarification or flag for review   | Conditional               |
| Weak match       | Similarity is weak or likely coincidental                    | Do not rely on match                   | Usually no                |
| Restricted match | Match exists but user cannot view it                         | Use only allowed internal path         | Yes if it affects routing |
| No match         | No meaningful case history found                             | Continue current workflow              | No                        |

Guidance:

* Exact case ID should be prioritized over semantic similarity.
* Weak semantic similarity should not be treated as duplicate.
* Restricted matches should not be exposed to public users.
* Match strength should be included when case history affects routing or review.

---

## 9. Duplicate vs. Related vs. Follow-Up

| Classification | Definition                                                                          | Assistant behavior                                                                                    |
| -------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Duplicate      | Appears to represent the same issue, request, account, location, or service event   | Ask user to continue existing case or route for review if visible; do not force linkage on weak match |
| Related        | Connected by category, location, asset, account, or prior pattern but not identical | Use for review or routing if permitted                                                                |
| Follow-up      | User is asking about or adding information to an existing case                      | Retrieve permitted case context and continue the case flow                                            |
| Reference-only | Provides background but should not drive routing alone                              | Include only in internal review if useful                                                             |
| Unrelated      | Does not materially affect current request                                          | Exclude from runtime packet                                                                           |

---

## 10. Permission and Visibility Rules

| Case context            | Visible to anonymous user | Visible to authenticated owner | Visible to internal service team | Assistant behavior                              |
| ----------------------- | ------------------------- | ------------------------------ | -------------------------------- | ----------------------------------------------- |
| Public case status      | Conditional               | Conditional                    | Yes if assigned                  | Show only allowed public fields                 |
| Own case summary        | No                        | Yes if permitted               | Yes if assigned                  | Use in follow-up flow                           |
| Own case internal notes | No                        | No unless policy allows        | Yes if assigned                  | Exclude from user-facing output                 |
| Other user’s case       | No                        | No                             | Conditional                      | Do not expose to public user                    |
| Related restricted case | No                        | No                             | Conditional                      | Use only allowed internal routing/review path   |
| Duplicate visible case  | No if anonymous           | Yes if owned or permitted      | Yes if assigned                  | Ask user whether to continue or create new case |
| Review notes            | No                        | No                             | Reviewer/service roles only      | Internal review only                            |

The assistant should never reveal hidden case history through indirect language such as:

* “There is another case you cannot see.”
* “Someone else already reported this.”
* “I found internal notes saying...”

Unless the system explicitly allows that level of disclosure.

---

## 11. Case History Runtime Context Format

Use this structure when case history is checked.

```yaml
case_history_context:
  checked: true
  check_trigger: "user_provided_case_id | follow_up_language | repeated_issue | duplicate_screening | reviewer_context"
  retrieval_allowed: true
  retrieval_reason: "[Why case history was checked]"
  results:
    - case_id: "[Case ID]"
      record_type: "[own_case | duplicate_case | related_case | restricted_case | prior_resolved_case | internal_review_record]"
      source: "case_management_system"
      match_strength: "[exact | strong | possible | weak | restricted]"
      match_signals:
        - "[Signal]"
      status: "[open | pending | closed | escalated | unknown]"
      summary: "[User-visible or internal summary depending on permission]"
      timestamp: "[Created or updated timestamp]"
      permission_status: "[allowed | restricted | internal_only | unknown]"
      visible_to_user: false
      allowed_use: "[user_display | routing_only | reviewer_context | escalation_only | none]"
      action_recommended: "[continue_case | link | merge_review | create_new | ask_clarification | route_for_review | ignore]"
  no_match_reason: "[Reason if checked and no meaningful match found]"
```

---

## 12. User-Facing Explanation Rules

The assistant may explain case history when:

* the user provided a case ID and is permitted to view the case
* the case belongs to the authenticated user or authorized account
* a visible duplicate or follow-up path requires user confirmation
* the explanation helps the user choose whether to continue an existing case or create a new request

Preferred patterns:

```text
I found that case. I can help you prepare a follow-up with the new details you provided.
```

```text
This may be related to an existing case you can access. Do you want to continue with that case or create a new request?
```

```text
I don’t have enough information to confirm whether this is connected to an existing case. Do you have a case number?
```

For restricted case context:

```text
I can help prepare this for review based on the information you provided.
```

Avoid:

* exposing restricted case IDs
* describing internal notes to public users
* saying another user has filed a case unless allowed
* presenting a possible match as a confirmed duplicate
* forcing the user into an existing case based on uncertain match

---

## 13. Case History Influence Rules

| Case history result         | May influence                                  | Must not influence                        | Notes                                                |
| --------------------------- | ---------------------------------------------- | ----------------------------------------- | ---------------------------------------------------- |
| Exact visible own case      | Follow-up flow, status response, added details | Unrelated new case routing                | User can continue existing case                      |
| Strong visible duplicate    | User confirmation, duplicate prevention        | Forced merge without confirmation         | Ask user to choose or route for review               |
| Strong related visible case | Routing, reviewer context, follow-up option    | Final ownership decision without rules    | May support continuity                               |
| Restricted related case     | Internal review or routing if allowed          | User-facing details                       | Use safe explanation                                 |
| Weak possible match         | Maybe review flag                              | Routing or duplicate prevention           | Do not rely on weak match                            |
| No match                    | New request path                               | Claim no case exists beyond checked scope | Avoid overclaiming if search was limited             |
| Conflicting case history    | Human review                                   | Silent resolution                         | Preserve user statement and retrieved context labels |

---

## 14. Follow-Up Handling Rules

A user may be following up when they:

* provide a case ID
* ask for status
* say they already submitted or reported something
* say the issue is still unresolved
* reference a prior message or response
* describe a repeated issue
* dispute prior handling

| Follow-up situation                    | Assistant behavior                                 | Required context                                             | Output                               |
| -------------------------------------- | -------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------ |
| User provides exact case ID            | Check permission and retrieve visible case context | Case ID, user role, permission scope                         | Follow-up summary or safe limitation |
| User references prior issue without ID | Ask for case ID or identifying detail if needed    | User wording, authentication status                          | Clarification question               |
| User reports repeated issue            | Preserve wording and check escalation/review rules | Current description, prior signal, case history if permitted | Review or escalation summary         |
| User disputes prior handling           | Treat as complaint or complaint-follow-up          | Original wording, case context if permitted                  | Complaint review record              |
| User lacks permission to view case     | Do not expose details                              | Permission status                                            | Safe limitation or review path       |

---

## 15. Case History Conflict Rules

| Conflict type                                       | Assistant behavior                                                  | Review needed?            |
| --------------------------------------------------- | ------------------------------------------------------------------- | ------------------------- |
| User says case is unresolved but status says closed | Preserve user statement and visible status; ask or route for review | Conditional               |
| Multiple possible matching cases                    | Ask for case ID or clarifying detail                                | Conditional               |
| Related case belongs to another user                | Do not expose; route internally only if allowed                     | Yes if it affects routing |
| Case appears stale or outdated                      | Label as stale or retrieve current status                           | Conditional               |
| User disputes case status                           | Route to complaint or review process                                | Yes                       |
| Case history conflicts across systems               | Flag conflict and route for review                                  | Yes                       |

Default rules:

* Do not silently resolve material conflicts.
* Do not overwrite current user input with prior case content.
* Do not imply restricted case history exists when disclosure is not allowed.
* Preserve user wording when the conflict involves complaint, dispute, or unresolved issue.

---

## 16. Human Review Rules

Human review may be required when:

* case match confidence is low or conflicting
* a restricted related case affects routing
* the user disputes prior handling
* related cases cross user, business, tenant, or jurisdiction boundaries
* a possible duplicate may prevent or change submission
* repeated unresolved issues may require escalation
* language ambiguity affects case matching
* internal notes are needed for interpretation

| Review trigger                          | Assistant behavior                    | Review owner                 | Review output                                        |
| --------------------------------------- | ------------------------------------- | ---------------------------- | ---------------------------------------------------- |
| Restricted related case affects routing | Route for service review              | Service reviewer             | Internal note without user-facing leakage            |
| User disputes prior handling            | Route to complaint review             | Complaint reviewer           | Preserved user wording and case reference if visible |
| Multiple possible matching cases        | Ask clarification or route for review | Service reviewer             | Candidate match summary if permitted                 |
| Repeated unresolved issue               | Check escalation rules                | Operations reviewer          | Repeated issue summary                               |
| Permission unclear                      | Do not expose case details            | Security/product review path | Permission limitation note                           |

---

## 17. Logging and Traceability

Log when allowed:

* whether case history was checked
* trigger for case history check
* sources searched
* case ID provided by user, if allowed
* match strength
* match signals
* permission status
* visibility status
* action recommended
* records included in runtime packet
* records excluded and why
* human review flag

Do not log:

* restricted case contents beyond approved retention rules
* other users’ data in user-visible logs
* unnecessary personal data
* internal notes outside approved systems
* sensitive case information not needed for diagnostics

---

## 18. Case History Failure Patterns to Test

Test for these case-history failures:

* misses exact case ID provided by user
* retrieves case history for anonymous user without permission
* exposes restricted case details
* implies hidden related cases exist when not allowed
* treats weak semantic similarity as duplicate
* fails to detect visible duplicate case
* routes based on unrelated prior case
* ignores user’s current statement because prior case says something different
* fails to preserve complaint or dispute wording
* fails to ask for case ID when needed for follow-up
* loses follow-up continuity after interruption
* uses stale case status as current
* omits match strength from internal review output
* creates duplicate case when visible existing case should be offered

---

## 19. Case History Review Checklist

Before approving case history rules, check:

* Are case record types defined?
* Are retrieval triggers clear?
* Are do-not-retrieve conditions defined?
* Are match signals and match strength rules defined?
* Are duplicate, related, follow-up, and restricted cases distinguished?
* Are permission and visibility rules clear?
* Is runtime case history format defined?
* Are safe user-facing explanation patterns defined?
* Are influence limits defined?
* Are conflict handling rules defined?
* Are human review triggers defined?
* Are logging and traceability limits defined?
* Are case-history failure patterns included in testing?

---

## 20. Open Questions

| Question                                             | Why it matters                                 | Likely owner                   | Status |
| ---------------------------------------------------- | ---------------------------------------------- | ------------------------------ | ------ |
| Which case fields can users see?                     | Determines user-facing case history behavior   | Product / policy / security    | Open   |
| Which case fields can service teams see?             | Determines internal routing and review context | Operations / security          | Open   |
| What counts as a duplicate case?                     | Affects duplicate prevention and user choice   | Operations / product           | Open   |
| When can restricted related cases influence routing? | Affects permission-safe internal use           | Security / policy / operations | Open   |
| What case history should be logged for diagnostics?  | Affects traceability and privacy               | Engineering / governance       | Open   |
| How long is case history considered relevant?        | Affects stale context and match strength       | Operations                     | Open   |

---

## 21. Related Documents

* `sample-scope-and-assumptions.md`
* `ai-design-principles.md`
* `ai-assistant-behavior-spec.md`
* `public-service-request-feedback-triage-assistant-behavior-spec.md`
* `context-architecture-spec.md`
* `runtime-context-template.md`
* `context-assembly-rules.md`
* `language-handling-rules.md`
* `service-category-routing-rules.md`
* `example-context-packets.md`
* `testing-diagnostics-spec.md`
* `public-service-request-feedback-conversation-flows.md`
