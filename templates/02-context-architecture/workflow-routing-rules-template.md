# Workflow Routing Rules Template

Use this document to define how an AI-enabled system should classify, route, escalate, or hand off a request, record, case, task, or user interaction.

Routing rules should be explicit because routing errors can create operational delays, permission risks, user frustration, incorrect ownership, or missed escalation conditions.

This template can be adapted for case triage, service requests, internal support, product feedback, grant intake, policy review, industrial challenge intake, or other AI-enabled workflows.

---

## 1. Routing Rules Overview

**System name:** `[Insert system/product/service name]`

**AI assistant name:** `[Insert assistant name]`

**Workflow name:** `[Insert workflow name]`

**Routing rules version:** `[Insert version]`

**Document owner:** `[Insert owner/team]`

**Last updated:** `[Insert date]`

---

## 2. Purpose of Routing Rules

Describe why routing rules are needed for this workflow.

```text
These routing rules exist to...
```

Prompts:

* What needs to be routed?
* Who or what receives the routed item?
* What information determines routing?
* What should the assistant classify or recommend?
* What routing decisions require human confirmation or review?
* What happens when routing confidence is low?
* What happens when multiple routing destinations may apply?

---

## 3. Routing Goals

The routing system should:

* classify the request, case, record, or task consistently
* route items to the correct team, queue, workflow, or next step
* identify missing information before routing when required
* avoid routing based on unsupported assumptions
* preserve user intent during classification and routing
* respect role, tenant, account, and permission boundaries
* flag escalation conditions when needed
* make routing logic reviewable and testable
* support correction when routing is wrong or disputed

System-specific goals:

* `[Goal]`
* `[Goal]`
* `[Goal]`

---

## 4. Routing Scope

Define what this document covers.

In scope:

* `[Routing scope item]`
* `[Routing scope item]`
* `[Routing scope item]`

Out of scope:

* `[Out-of-scope item]`
* `[Out-of-scope item]`
* `[Out-of-scope item]`

Examples of routing scope:

* request type classification
* service category classification
* department or team routing
* queue assignment
* escalation flagging
* reviewer assignment
* related-record routing
* follow-up workflow selection
* handoff from AI assistant to human team

---

## 5. Routing Inputs

Define the information used to make or recommend routing decisions.

| Input     | Source                                                 | Required?                  | Used for                                           | Notes     |
| --------- | ------------------------------------------------------ | -------------------------- | -------------------------------------------------- | --------- |
| `[Input]` | `[User / system / retrieval / inferred / integration]` | `[Yes / No / Conditional]` | `[Classification / routing / escalation / review]` | `[Notes]` |
| `[Input]` | `[User / system / retrieval / inferred / integration]` | `[Yes / No / Conditional]` | `[Classification / routing / escalation / review]` | `[Notes]` |
| `[Input]` | `[User / system / retrieval / inferred / integration]` | `[Yes / No / Conditional]` | `[Classification / routing / escalation / review]` | `[Notes]` |

Input examples:

* user description
* request type
* service category
* user role
* user location or jurisdiction
* account or tenant context
* prior case history
* urgency indicators
* eligibility information
* policy or service rules
* language context
* attachments
* related records

---

## 6. Routing Destinations

List possible routing destinations.

| Destination     | Description     | Receives                   | Required conditions | Notes     |
| --------------- | --------------- | -------------------------- | ------------------- | --------- |
| `[Destination]` | `[Description]` | `[Request/case/task type]` | `[Conditions]`      | `[Notes]` |
| `[Destination]` | `[Description]` | `[Request/case/task type]` | `[Conditions]`      | `[Notes]` |
| `[Destination]` | `[Description]` | `[Request/case/task type]` | `[Conditions]`      | `[Notes]` |

Destination examples:

* service team
* support queue
* reviewer group
* operations team
* policy team
* escalation queue
* technical support
* human intake specialist
* external system
* self-service answer flow

---

## 7. Classification Categories

Define classification categories that affect routing.

| Category type            | Values     | Used for routing?          | Notes     |
| ------------------------ | ---------- | -------------------------- | --------- |
| Request type             | `[Values]` | `[Yes / No / Conditional]` | `[Notes]` |
| Service category         | `[Values]` | `[Yes / No / Conditional]` | `[Notes]` |
| Urgency level            | `[Values]` | `[Yes / No / Conditional]` | `[Notes]` |
| User role                | `[Values]` | `[Yes / No / Conditional]` | `[Notes]` |
| Location or jurisdiction | `[Values]` | `[Yes / No / Conditional]` | `[Notes]` |
| Eligibility status       | `[Values]` | `[Yes / No / Conditional]` | `[Notes]` |
| Related-record status    | `[Values]` | `[Yes / No / Conditional]` | `[Notes]` |

Add system-specific categories:

| Category type | Values     | Used for routing?          | Notes     |
| ------------- | ---------- | -------------------------- | --------- |
| `[Category]`  | `[Values]` | `[Yes / No / Conditional]` | `[Notes]` |

---

## 8. Routing Decision Table

Define the rules that map inputs and classifications to destinations.

| Condition     | Routing destination | Confidence required                   | Human review required?     | Notes     |
| ------------- | ------------------- | ------------------------------------- | -------------------------- | --------- |
| `[Condition]` | `[Destination]`     | `[Low / Medium / High / Exact match]` | `[Yes / No / Conditional]` | `[Notes]` |
| `[Condition]` | `[Destination]`     | `[Low / Medium / High / Exact match]` | `[Yes / No / Conditional]` | `[Notes]` |
| `[Condition]` | `[Destination]`     | `[Low / Medium / High / Exact match]` | `[Yes / No / Conditional]` | `[Notes]` |

Example:

| Condition                                                                       | Routing destination      | Confidence required | Human review required? | Notes                                   |
| ------------------------------------------------------------------------------- | ------------------------ | ------------------- | ---------------------- | --------------------------------------- |
| User asks a general question and answer is available in approved knowledge base | Self-service answer flow | High                | No                     | Provide answer and offer next step      |
| User reports an urgent operational issue                                        | Escalation queue         | Medium              | Yes                    | Create escalation summary               |
| Request type is unclear                                                         | Clarification step       | N/A                 | No                     | Ask one targeted clarification question |

---

## 9. Confidence Rules

Define how routing confidence should be handled.

| Confidence level | Meaning                                                        | Assistant behavior                                               |
| ---------------- | -------------------------------------------------------------- | ---------------------------------------------------------------- |
| High             | Routing destination is strongly supported by available context | Recommend or route if allowed                                    |
| Medium           | Routing is likely but not certain                              | Ask confirmation or route with review flag depending on workflow |
| Low              | Routing is uncertain                                           | Ask clarification or escalate to human review                    |
| Conflicting      | Context supports multiple destinations                         | Label conflict and ask clarification or route for review         |

Guidance:

* Confidence should be based on available context, not model fluency.
* Confidence should be lower when required context is missing.
* Confidence should be lower when language interpretation is uncertain.
* Confidence should be lower when related records conflict.
* Low-confidence routing should not be hidden behind confident language.

---

## 10. Missing Information Rules

Define which missing information blocks routing.

| Missing information | Blocks routing?            | Assistant behavior                             | Notes     |
| ------------------- | -------------------------- | ---------------------------------------------- | --------- |
| `[Information]`     | `[Yes / No / Conditional]` | `[Ask / infer / proceed with flag / escalate]` | `[Notes]` |
| `[Information]`     | `[Yes / No / Conditional]` | `[Ask / infer / proceed with flag / escalate]` | `[Notes]` |
| `[Information]`     | `[Yes / No / Conditional]` | `[Ask / infer / proceed with flag / escalate]` | `[Notes]` |

Default guidance:

* If missing information affects routing destination, ask before routing.
* If missing information affects urgency or escalation, ask or escalate depending on risk.
* If missing information affects only output completeness, proceed with a missing-info flag when allowed.
* Do not ask for information that does not affect routing, escalation, or workflow completion.

---

## 11. Inference Rules for Routing

Define what the assistant may infer for routing purposes.

| Inference     | Allowed?                   | Requires confirmation?     | Can affect routing?        | Notes     |
| ------------- | -------------------------- | -------------------------- | -------------------------- | --------- |
| `[Inference]` | `[Yes / No / Conditional]` | `[Yes / No / Conditional]` | `[Yes / No / Conditional]` | `[Notes]` |
| `[Inference]` | `[Yes / No / Conditional]` | `[Yes / No / Conditional]` | `[Yes / No / Conditional]` | `[Notes]` |
| `[Inference]` | `[Yes / No / Conditional]` | `[Yes / No / Conditional]` | `[Yes / No / Conditional]` | `[Notes]` |

Guidance:

* Inferred routing should be labeled.
* Material inferences should be confirmed before routing when possible.
* Inferences affecting rights, eligibility, urgency, safety, or escalation should not be treated as confirmed facts.
* The basis for routing inference should be stored when reviewability matters.

Example:

```yaml
routing_inference:
  inference: "[Inferred request type]"
  basis: "[User wording or context]"
  confidence: "medium"
  requires_confirmation: true
  affects_routing: true
```

---

## 12. Related-Record Routing Rules

Define how related records affect routing.

Related records may affect routing when:

* the user is following up on an existing case
* a duplicate or related record already exists
* prior case history changes the next step
* a record belongs to another team or workflow
* a related record is restricted from user view but relevant to internal handling

| Related-record situation                 | Assistant behavior | Routing behavior | Visibility rule |
| ---------------------------------------- | ------------------ | ---------------- | --------------- |
| Related record visible to user           | `[Behavior]`       | `[Routing]`      | `[Rule]`        |
| Related record restricted                | `[Behavior]`       | `[Routing]`      | `[Rule]`        |
| Duplicate likely                         | `[Behavior]`       | `[Routing]`      | `[Rule]`        |
| Related record conflicts with user input | `[Behavior]`       | `[Routing]`      | `[Rule]`        |
| No related record found                  | `[Behavior]`       | `[Routing]`      | `[Rule]`        |

Reference related document:

* `[Related Record Context Rules]`

---

## 13. Permission-Aware Routing Rules

Define how permissions affect routing and explanation.

The assistant should:

* route only using context permitted for the workflow and role
* avoid exposing restricted routing reasons to the user
* avoid revealing hidden records through routing explanations
* include internal-only routing notes only when the receiving role may access them
* explain user-facing limitations safely when information cannot be shown

| Permission situation               | Routing behavior | User-facing explanation | Internal note allowed? |
| ---------------------------------- | ---------------- | ----------------------- | ---------------------- |
| User can view all relevant context | `[Behavior]`     | `[Explanation]`         | `[Yes / No]`           |
| User cannot view related record    | `[Behavior]`     | `[Explanation]`         | `[Yes / No]`           |
| Permission status unknown          | `[Behavior]`     | `[Explanation]`         | `[Yes / No]`           |
| Tenant boundary prevents retrieval | `[Behavior]`     | `[Explanation]`         | `[Yes / No]`           |

---

## 14. Language-Sensitive Routing Rules

Define how language can affect routing.

Language may affect routing when:

* translated terms map to multiple categories
* informal wording, dialect, or transliteration affects meaning
* official service terms have no direct translation
* retrieved context is in a different language from user input
* language confidence is low

| Language situation                      | Routing behavior | Confirmation needed?       | Notes     |
| --------------------------------------- | ---------------- | -------------------------- | --------- |
| User uses informal term                 | `[Behavior]`     | `[Yes / No / Conditional]` | `[Notes]` |
| Translation maps to multiple categories | `[Behavior]`     | `[Yes / No / Conditional]` | `[Notes]` |
| User switches language mid-flow         | `[Behavior]`     | `[Yes / No / Conditional]` | `[Notes]` |
| Transliteration creates ambiguity       | `[Behavior]`     | `[Yes / No / Conditional]` | `[Notes]` |
| Official term should not be translated  | `[Behavior]`     | `[Yes / No / Conditional]` | `[Notes]` |

Reference related document:

* `[Language Handling Rules]`

---

## 15. Escalation Routing Rules

Define when the assistant should route to escalation instead of a standard destination.

Escalation may be triggered by:

* safety concern
* urgent service issue
* harm, risk, outage, or disruption
* legal, medical, financial, compliance, or security concern
* user distress
* policy ambiguity
* permission conflict
* repeated unresolved attempts
* system failure
* user dispute or complaint about prior handling

| Escalation trigger | Escalation destination | Required output | User-facing behavior | Notes     |
| ------------------ | ---------------------- | --------------- | -------------------- | --------- |
| `[Trigger]`        | `[Destination]`        | `[Output]`      | `[Behavior]`         | `[Notes]` |
| `[Trigger]`        | `[Destination]`        | `[Output]`      | `[Behavior]`         | `[Notes]` |
| `[Trigger]`        | `[Destination]`        | `[Output]`      | `[Behavior]`         | `[Notes]` |

Escalation output may include:

* escalation reason
* known information
* missing information
* urgency level
* user-facing summary
* internal summary
* recommended owner
* required next step

---

## 16. Human Review Rules

Define when routing should be reviewed by a human.

Human review may be required when:

* routing confidence is low
* multiple destinations appear possible
* the user disputes the classification
* required policy interpretation is unclear
* language ambiguity affects routing
* permission boundaries are unclear
* retrieved records conflict
* escalation may be needed
* the workflow has regulatory, legal, financial, safety, or high-impact implications

| Review trigger | Assistant behavior | Review owner | Output required |
| -------------- | ------------------ | ------------ | --------------- |
| `[Trigger]`    | `[Behavior]`       | `[Owner]`    | `[Output]`      |
| `[Trigger]`    | `[Behavior]`       | `[Owner]`    | `[Output]`      |
| `[Trigger]`    | `[Behavior]`       | `[Owner]`    | `[Output]`      |

---

## 17. Routing Output Structure

Define the structure for routing outputs.

Example structure:

```yaml
routing_output:
  request_type: "[Type]"
  service_category: "[Category]"
  recommended_destination: "[Destination]"
  routing_confidence: "[low | medium | high]"
  routing_reason:
    user_visible: "[Safe explanation for user]"
    internal: "[Internal routing reason if permitted]"
  missing_information:
    - "[Missing item]"
  related_records:
    checked: true
    found: false
    visible_to_user: false
  escalation:
    required: false
    trigger: "[Trigger if any]"
  human_review:
    required: false
    reason: "[Reason if any]"
```

Adapt this structure to the workflow’s technical requirements.

---

## 18. User-Facing Routing Explanation Rules

Define how routing should be explained to the user.

The assistant should explain:

* what will happen next
* what information was used to route the request, when appropriate
* what information is still missing
* whether human review is needed
* whether the user must confirm before submission

The assistant should not expose:

* restricted records
* internal-only scoring
* hidden risk signals
* unauthorized routing logic
* tenant or account data outside the user’s scope
* related-record details the user cannot view

Preferred explanation patterns:

```text
Based on what you shared, this looks like it should go to [destination/category]. Before I route it, I need [missing information].
```

```text
I can prepare this for [destination/category]. I’ll include your description and the details you confirmed.
```

```text
This may need review by [team/role] because [safe user-facing reason].
```

---

## 19. Correction and Re-Routing Rules

Define what happens when routing needs to be corrected.

Re-routing may happen when:

* user corrects earlier information
* assistant misclassified the request
* human reviewer changes the category
* additional context changes the destination
* related records are discovered later
* urgency changes
* permission status changes

| Re-routing trigger | Assistant behavior | Preserve                    | Re-check                    |
| ------------------ | ------------------ | --------------------------- | --------------------------- |
| `[Trigger]`        | `[Behavior]`       | `[Information to preserve]` | `[Information to re-check]` |
| `[Trigger]`        | `[Behavior]`       | `[Information to preserve]` | `[Information to re-check]` |
| `[Trigger]`        | `[Behavior]`       | `[Information to preserve]` | `[Information to re-check]` |

Guidance:

* Preserve user-provided information unless corrected.
* Update routing reason and confidence.
* Record why routing changed when auditability matters.
* Notify the user when re-routing affects next steps or timeline.

---

## 20. Routing Failure Patterns to Test

Test for these routing failure patterns:

* routes before required information is collected
* misclassifies request type
* routes to the wrong team or queue
* fails to escalate urgent or high-risk cases
* routes based on unsupported inference
* exposes restricted context in routing explanation
* ignores user role or permission scope
* fails to handle multiple possible destinations
* fails to ask clarification when confidence is low
* ignores language ambiguity
* fails to update routing after user correction
* creates duplicate routing when related record exists
* blocks routing unnecessarily when enough context is available
* produces malformed routing output

System-specific failure patterns:

* `[Failure pattern]`
* `[Failure pattern]`
* `[Failure pattern]`

---

## 21. Routing Review Checklist

Before approving routing rules, check:

* Are routing destinations defined?
* Are classification categories defined?
* Are required routing inputs clear?
* Are missing-information rules defined?
* Are confidence rules defined?
* Are human review triggers defined?
* Are escalation triggers defined?
* Are permission boundaries applied?
* Are language-sensitive routing risks addressed?
* Are related-record rules addressed?
* Are user-facing explanations safe and clear?
* Are routing failure patterns included in testing?

---

## 22. Open Questions

List unresolved routing decisions.

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
* `[Workflow Assistant Behavior Spec]`
* `[Context Architecture Spec]`
* `[Runtime Context Template]`
* `[Context Assembly Rules]`
* `[Language Handling Rules]`
* `[Related Record Context Rules]`
* `[Testing & Diagnostics Spec]`
* `[Conversation Flows]`

---

## 24. Change Log

| Date     | Change          | Owner     |
| -------- | --------------- | --------- |
| `[Date]` | `[Change made]` | `[Owner]` |
