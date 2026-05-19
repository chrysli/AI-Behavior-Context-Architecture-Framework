# Public Service Request & Feedback Conversation Flows

This document defines sample conversation flows for the fictional **Public Service Request & Feedback Triage Assistant**.

The goal is to connect the assistant’s behavior and context architecture to the actual service experience: what the user is trying to do, what the assistant needs to ask or produce, what context is available, and where handoffs, routing, escalation, confirmation, or recovery happen.

This sample is fictional and simplified. It does not represent a real public service implementation.

---

## 1. Conversation Flow Overview

**System name:** Public Service Request & Feedback Triage Assistant

**Workflow name:** Public Service Request & Feedback Triage

**Flow version:** 0.1 fictional sample

**Primary users:** Anonymous users, authenticated residents, authenticated business owners, internal service team members, reviewers, and administrators

**Sample status:** Fictional public-safe architecture example

---

## 2. Purpose of the Conversation Flows

These conversation flows exist to show how the assistant supports public service intake across questions, complaints, issue reports, service requests, feedback items, and follow-ups.

The flows show:

* where the assistant enters the user journey
* what the user may say or do
* what the assistant should ask, summarize, retrieve, route, or escalate
* what context is needed at each step
* what system state changes during the interaction
* where the user should review or confirm information
* where the assistant should hand off to a human, service team, queue, or backend system
* how the assistant should recover when a user pauses, changes direction, or corrects information

---

## 3. Relationship to SD/UX Workflow Context

This file should be read alongside service design, UX, and workflow artifacts such as:

* service journey maps
* workflow-state maps
* screen flows
* form-field inventories
* handoff maps
* error and recovery flows
* escalation flows
* screenshots or prototype links
* localization notes
* accessibility notes
* operational process maps

Those artifacts help explain what happens before, during, and after the AI interaction.

The conversation flow should not be treated as only a chatbot script. It is a workflow artifact that shows how user interaction, system context, and downstream service handling connect.

---

## 4. User Roles and Flow Variants

| Role                         | Description                                                  | Flow variant                                        | Notes                                                |
| ---------------------------- | ------------------------------------------------------------ | --------------------------------------------------- | ---------------------------------------------------- |
| Anonymous user               | User without confirmed identity                              | General question or limited feedback                | No private case history access                       |
| Authenticated resident       | User with verified account                                   | Service request, issue report, complaint, follow-up | Can access own permitted cases                       |
| Authenticated business owner | User acting for a business account                           | Business service request or business case follow-up | May need business account context                    |
| Internal service team member | User receiving routed cases                                  | Review routed case details                          | Can see assigned internal context if permitted       |
| Reviewer                     | Human reviewer for ambiguous, escalated, or restricted cases | Review and routing validation                       | Needs source labels, uncertainty, and open questions |
| Administrator                | User managing configuration or service categories            | Service taxonomy/routing configuration              | Not part of public intake flow                       |

---

## 5. Entry Points

| Entry point                | User intent                                             | Starting context                                       | First assistant action                                          | Notes                            |
| -------------------------- | ------------------------------------------------------- | ------------------------------------------------------ | --------------------------------------------------------------- | -------------------------------- |
| Public service portal home | User does not know where to start                       | Anonymous or authenticated role, locale, entry page    | Ask what the user needs help with or offer request type options | General intake path              |
| Service category page      | User has selected a likely category                     | Selected service category, user role, locale           | Confirm or use selected category and ask for needed details     | Avoid asking category again      |
| Submit request form        | User is creating a service request                      | Existing form fields, selected category, user role     | Use visible form context and ask only missing information       | Interface context matters        |
| Case status page           | User wants to follow up                                 | Case ID or account context if authenticated            | Check permission and permitted case context                     | Case history rules apply         |
| Complaint or feedback page | User wants to report dissatisfaction or submit feedback | Entry page type, user role, locale                     | Preserve wording and distinguish complaint vs. feedback         | Review path may apply            |
| Notification or email link | User responds to a prior case or message                | Case reference, prior state if available               | Resume or confirm follow-up context                             | Recovery path may apply          |
| Internal review queue      | Reviewer opens a routed item                            | Structured case record, routing reason, context labels | Present reviewable case context                                 | Internal flow, not public-facing |

---

## 6. Workflow State Map

| Workflow state         | Description                                         | User-facing goal                                 | Assistant behavior                                          | Next possible states                                |
| ---------------------- | --------------------------------------------------- | ------------------------------------------------ | ----------------------------------------------------------- | --------------------------------------------------- |
| Start                  | User enters the flow                                | Describe need or choose path                     | Orient, identify broad intent, use entry context            | Intent detection, information collection            |
| Intent detection       | Assistant interprets request type                   | Make sure the request is understood              | Classify or ask targeted clarification                      | Information collection, question answer, escalation |
| Information collection | Required details are gathered                       | Provide only details needed to proceed           | Ask fewer, better questions                                 | Review, routing, case history check                 |
| Language handling      | Language or terminology affects meaning             | Continue in appropriate language                 | Preserve original wording, clarify ambiguity                | Information collection, routing, review             |
| Case history check     | Existing case or related case may matter            | Continue or update an existing case when allowed | Retrieve permitted case context; protect restricted context | Follow-up, review, routing                          |
| User review            | User reviews summary before submission              | Confirm accuracy                                 | Show user-facing summary and ask confirmation               | Routing, correction, submission                     |
| Routing                | Request is prepared for destination                 | Send to appropriate service path                 | Prepare routing recommendation or structured case           | Handoff, review, escalation                         |
| Escalation             | Risk, urgency, dispute, or sensitive issue detected | Get issue to appropriate human review path       | Stop routine flow, prepare escalation summary               | Handoff, review                                     |
| Recovery               | User resumes, corrects, or changes direction        | Continue without starting over                   | Preserve confirmed info, update changed info                | Information collection, review, routing             |
| Handoff                | Case moves to human/team/system                     | Understand next step                             | Confirm what happens next using approved language           | End or follow-up                                    |

---

## 7. Flow Summary

A typical public service request and feedback flow follows this pattern:

1. User enters from the portal, a service page, a form, a case status page, or a notification.
2. Assistant identifies likely request type: question, complaint, service request, issue report, feedback, or follow-up.
3. Assistant checks available context such as user role, form state, selected category, language, and workflow state.
4. Assistant asks only for missing information required to proceed.
5. Assistant retrieves permitted service catalog or case history context when needed.
6. Assistant preserves original wording when it may matter for complaint handling, translation, escalation, or review.
7. Assistant prepares a user-facing summary or structured case record.
8. User reviews and confirms when confirmation is required.
9. Assistant routes, escalates, or hands off to the appropriate queue, service team, reviewer, or backend system.
10. System updates workflow state and provides the user with the next approved step.

---

## 8. Primary Flow: New Issue Report

This flow covers a user reporting a public service issue, such as a broken streetlight, missed collection, broken park gate, portal error, or facility issue.

| Step | Workflow state         | User action/input                              | Assistant action/output                            | Context needed                                     | System state change          | Notes                                                |
| ---- | ---------------------- | ---------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- | ---------------------------- | ---------------------------------------------------- |
| 1    | Start                  | User opens assistant or request form           | Ask what they need or use selected entry point     | Entry point, role, locale                          | Start session                | If category page is selected, carry category forward |
| 2    | Intent detection       | User describes issue                           | Infer request type and possible service category   | User message, language context                     | Request type inferred        | Preserve original wording                            |
| 3    | Information collection | User provides description but missing location | Ask one targeted missing-info question             | Known fields, missing fields, category rules       | Missing info marked blocking | Avoid broad questioning                              |
| 4    | Information collection | User provides location                         | Update known information                           | Location, issue description                        | Required fields complete     | Check if escalation trigger exists                   |
| 5    | Review                 | Assistant presents summary                     | Ask user to confirm before routing                 | Known info, inferred category, output requirements | Review pending               | User-facing summary excludes internal notes          |
| 6    | Routing                | User confirms                                  | Prepare structured case and routing recommendation | Service category, routing rules, role              | Case ready for route         | Human review if confidence low                       |
| 7    | Handoff                | Case routed to queue                           | Provide approved confirmation or next step         | Routing output, allowed user-facing message        | Case submitted/routed        | Do not promise unsupported timeline                  |

Sample user-facing review message:

```text
Here is what I have so far:

Issue: The streetlight near your building has been out for three nights.
Location: [confirmed location]
Category: Streets and infrastructure

Please confirm this is accurate before I prepare it for the service team.
```

---

## 9. Flow Variant: General Question

This flow covers a user asking for public guidance or information.

| Step | Workflow state   | User action/input                                 | Assistant action/output           | Context needed                                | System state change      | Notes                                   |
| ---- | ---------------- | ------------------------------------------------- | --------------------------------- | --------------------------------------------- | ------------------------ | --------------------------------------- |
| 1    | Start            | User asks a general question                      | Identify request type as question | User message, role                            | Request type known       | Usually no case history needed          |
| 2    | Intent detection | Question is answerable from public knowledge base | Retrieve approved public context  | Knowledge base, service catalog if needed     | Public context retrieved | No private retrieval for anonymous user |
| 3    | Answer           | Assistant answers and gives next step             | Provide concise answer            | Retrieved public context, output requirements | Flow may end or continue | Avoid invented policy or service rules  |

Sample response:

```text
You can submit a request through the service portal by choosing the relevant service category, describing what you need, and reviewing the details before sending it. If you are reporting an issue, include the location and any details that would help the service team understand the problem.
```

---

## 10. Flow Variant: Complaint Review

This flow covers dissatisfaction, poor service handling, disputed outcomes, or unresolved prior attempts.

| Step | Workflow state     | User action/input                                                 | Assistant action/output                                                 | Context needed                                  | System state change       | Notes                                      |
| ---- | ------------------ | ----------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------- | ------------------------- | ------------------------------------------ |
| 1    | Start              | User submits complaint text                                       | Identify complaint signal                                               | Original user text, language                    | Complaint inferred        | Preserve original wording                  |
| 2    | Intent detection   | User describes prior handling or dissatisfaction                  | Determine whether complaint, feedback, follow-up, or escalation applies | Request type rules, case history trigger        | Complaint path selected   | Ask only if classification affects routing |
| 3    | Case history check | User references prior case or repeated issue                      | Retrieve permitted case history if allowed                              | User role, permission scope, case ID if present | Case context checked      | Do not expose restricted history           |
| 4    | Escalation check   | User mentions harm, danger, repeated unresolved issue, or dispute | Apply escalation rules                                                  | Escalation triggers, known info                 | Escalation flag if needed | Normal routing may stop                    |
| 5    | Review             | Assistant presents complaint summary                              | Ask user to confirm if needed                                           | Original wording, summary, missing info         | Review pending            | Avoid minimizing complaint                 |
| 6    | Handoff            | Complaint routed for review                                       | Provide approved next step                                              | Routing/escalation output                       | Complaint review created  | No unsupported promises                    |

Sample complaint acknowledgement:

```text
I can help prepare this for review. I’ll include your original description and the fact that this has already been reported.
```

---

## 11. Flow Variant: Follow-Up on Existing Case

This flow covers users asking about or adding to an existing case.

| Step | Workflow state         | User action/input                                             | Assistant action/output                        | Context needed                                    | System state change               | Notes                                      |
| ---- | ---------------------- | ------------------------------------------------------------- | ---------------------------------------------- | ------------------------------------------------- | --------------------------------- | ------------------------------------------ |
| 1    | Start                  | User provides case ID or says they already reported something | Detect follow-up intent                        | User message, case ID if present                  | Follow-up inferred                | Ask for case ID only if needed             |
| 2    | Case history check     | Assistant checks permitted case context                       | Retrieve own case if authenticated and allowed | User role, permission scope, case ID              | Case context retrieved or blocked | Follow case history rules                  |
| 3    | Permission handling    | Case visible, restricted, or not found                        | Choose safe behavior                           | Permission status, visibility                     | Follow-up path or safe limitation | Never expose hidden records                |
| 4    | Information collection | User adds new detail                                          | Prepare follow-up note                         | Original text, new detail, case status if visible | Follow-up draft                   | Preserve user’s new wording                |
| 5    | Review                 | Assistant asks user to confirm update                         | Present follow-up summary                      | Case reference, new detail                        | Review pending                    | Confirmation required if adding/submitting |
| 6    | Handoff                | Follow-up sent or routed                                      | Provide approved next step                     | Case update output                                | Follow-up submitted/routed        | Do not promise outcome                     |

Sample follow-up message:

```text
I can prepare that update for CASE-12345. The follow-up note will say that the issue is still happening. Please confirm before I add it.
```

---

## 12. Flow Variant: Restricted Related Case

This flow covers cases where a possible related case exists but cannot be shown to the user.

| Step | Workflow state      | User action/input                             | Assistant action/output               | Context needed                             | System state change       | Notes                             |
| ---- | ------------------- | --------------------------------------------- | ------------------------------------- | ------------------------------------------ | ------------------------- | --------------------------------- |
| 1    | Case history check  | User says someone else already reported issue | Trigger related-case check if allowed | User role, permission scope, current issue | Related check triggered   | Permission-bound                  |
| 2    | Permission handling | Related restricted case found                 | Do not show restricted details        | Restricted context, allowed internal use   | User-safe response needed | Avoid hidden-record implication   |
| 3    | Routing             | Related context may affect handling           | Route for service review if allowed   | Routing rules, restricted context labels   | Review route selected     | Internal note only if permitted   |
| 4    | Handoff             | User receives safe response                   | Provide generic review path           | User-facing output rules                   | Case prepared/routed      | Do not mention restricted case ID |

Sample user-facing response:

```text
I can help prepare this for service team review based on the information you provided.
```

Sample internal note:

```text
Possible restricted related case found. Do not expose details to user. Route to service review queue for linkage decision.
```

---

## 13. Flow Variant: Multilingual or Language-Ambiguous Input

This flow covers Arabic input, mixed-language input, transliteration, or ambiguous terminology.

| Step | Workflow state         | User action/input                             | Assistant action/output                                | Context needed                                 | System state change         | Notes                            |
| ---- | ---------------------- | --------------------------------------------- | ------------------------------------------------------ | ---------------------------------------------- | --------------------------- | -------------------------------- |
| 1    | Start                  | User writes in Arabic or mixed language       | Detect language                                        | User text, locale, preference                  | Language context created    | Respond in supported language    |
| 2    | Language handling      | User term maps to multiple categories         | Preserve original wording and flag ambiguity           | Language rules, service taxonomy               | Ambiguity noted             | Do not route yet                 |
| 3    | Clarification          | Assistant asks category/meaning clarification | Ask targeted question in user language                 | Possible category options, language confidence | Await user response         | Avoid internal taxonomy overload |
| 4    | Information collection | User clarifies meaning                        | Update request type/category                           | User clarification, original wording           | Category confirmed/inferred | Continue workflow                |
| 5    | Review or routing      | Summary prepared                              | Include original text and translated summary if needed | Language context, output rules                 | Review or route             | Preserve official terms          |

Sample behavior:

```text
Respond in the user’s language. Ask one clarification question that helps determine the correct service category. Do not route until the ambiguity is resolved.
```

---

## 14. Flow Variant: Recovery After Interruption

This flow covers resuming after pause, correction, change of request type, or unrelated question.

| Step | Workflow state         | User action/input         | Assistant action/output                    | Context needed                | System state change   | Notes                        |
| ---- | ---------------------- | ------------------------- | ------------------------------------------ | ----------------------------- | --------------------- | ---------------------------- |
| 1    | Recovery               | User returns after pause  | Summarize current state                    | Prior state, confirmed fields | Recovery state active | Do not restart unnecessarily |
| 2    | Recovery               | User confirms or corrects | Preserve confirmed info and update changes | Known info, corrected info    | State updated         | Re-check dependent routing   |
| 3    | Information collection | Missing info remains      | Ask only for remaining required field      | Missing info, reason needed   | Continue workflow     | Avoid repeated questions     |
| 4    | Review                 | Required info complete    | Present updated summary                    | Known info, changed info      | Review pending        | Show corrected fields        |

Sample recovery response:

```text
We already have this as a waste and sanitation service request about missed collection. I only need the pickup location to continue.
```

---

## 15. Assistant Message Patterns

### Start or orientation

```text
Tell me what you need help with. I can help with a question, service request, issue report, complaint, feedback, or follow-up on an existing case.
```

### Clarification question

```text
To route this correctly, I need one more detail: [specific question].
```

### Request type confirmation

```text
This sounds like [request type]. Is that correct, or would you describe it differently?
```

### User review before routing

```text
Here is what I have so far. Please confirm before I prepare this for the service team:

[Summary]
```

### Visible case follow-up

```text
I found that case. I can help prepare a follow-up with the new details you provided.
```

### Restricted related-case safe response

```text
I can help prepare this for service team review based on the information you provided.
```

### Escalation acknowledgement

```text
I can prepare this for review. I’ll include the details you shared and the reason this may need attention.
```

### Recovery

```text
We already have [confirmed information]. I only need [remaining information] to continue.
```

---

## 16. Question Patterns

| Question type              | When used                                      | Example question                                                           | Context required                      | Notes                                          |
| -------------------------- | ---------------------------------------------- | -------------------------------------------------------------------------- | ------------------------------------- | ---------------------------------------------- |
| Location question          | Location-dependent issue or service request    | “Where is this issue happening?”                                           | Service category, missing location    | Required for many routing paths                |
| Request type clarification | Message maps to multiple request types         | “Are you trying to report an issue, file a complaint, or submit feedback?” | User wording, inferred request types  | Avoid if request type is clear                 |
| Case follow-up question    | User references prior case without ID          | “Do you have the case number?”                                             | Follow-up signal, permission scope    | Do not ask if exact case ID is already present |
| Language clarification     | Translation or term ambiguity affects category | “[Clarification in user language]”                                         | Language context, possible categories | Preserve original wording                      |
| Confirmation question      | Before submission or handoff                   | “Please confirm this is accurate before I send it.”                        | Structured summary                    | Required for material restructuring            |
| Escalation detail question | Escalation may depend on missing detail        | “Is this currently creating a safety risk?”                                | Risk signal, escalation rules         | Do not delay if escalation already required    |

---

## 17. Confirmation and Review Moments

| Review moment                 | What user reviews                            | Required before proceeding?        | Assistant behavior                                 | Notes                                           |
| ----------------------------- | -------------------------------------------- | ---------------------------------- | -------------------------------------------------- | ----------------------------------------------- |
| Request type confirmation     | Inferred request type                        | Conditional                        | Ask only if inference affects routing              | Skip when confidence is high and low risk       |
| Summary before submission     | User-facing case summary                     | Yes for structured case submission | Present summary and ask confirmation               | Exclude internal-only fields                    |
| Translation-sensitive summary | Translated or normalized user wording        | Conditional                        | Preserve original and ask confirmation if material | Use for complaints or sensitive issues          |
| Follow-up update              | New note to existing case                    | Yes                                | Ask user to confirm before adding/submitting       | If action is irreversible or externally visible |
| Existing visible case choice  | Continue existing case or create new request | Conditional                        | Ask user to choose                                 | Only when user can view case                    |
| Escalation summary            | Known issue details and risk signal          | Conditional                        | Confirm if safe and not delaying urgent handling   | Depends on escalation policy                    |

---

## 18. Retrieval and Case History Moments

| Moment                         | Trigger                                       | Context retrieved or checked      | User-visible?                    | Assistant behavior                    |
| ------------------------------ | --------------------------------------------- | --------------------------------- | -------------------------------- | ------------------------------------- |
| Public answer lookup           | User asks general question                    | Knowledge base or service catalog | Yes                              | Answer from approved source           |
| Service routing lookup         | Category or destination needed                | Service catalog and routing rules | Partially                        | Use official category and destination |
| Follow-up lookup               | User provides case ID                         | Own permitted case history        | Yes if allowed                   | Prepare follow-up or safe limitation  |
| Related-case check             | User reports duplicate/repeated issue         | Related case candidates           | Conditional                      | Follow visibility rules               |
| Restricted related-case signal | Related case exists but user lacks permission | Restricted case summary           | No                               | Use only allowed internal route       |
| Escalation owner lookup        | Escalation trigger detected                   | Escalation rules/owner            | User-facing owner may be limited | Prepare escalation summary            |

---

## 19. Permission and Visibility Moments

| Moment                                 | Permission condition        | Assistant behavior                                 | User-facing explanation                                 | Notes                           |
| -------------------------------------- | --------------------------- | -------------------------------------------------- | ------------------------------------------------------- | ------------------------------- |
| Anonymous asks general question        | Public-only scope           | Answer from public context                         | Normal answer                                           | No private retrieval            |
| Anonymous asks for case status         | Private context requested   | Ask user to sign in or provide safe limitation     | “You’ll need to sign in to view case-specific details.” | Do not retrieve private case    |
| Authenticated user asks about own case | Own case visible            | Use permitted fields                               | Reference visible case                                  | Exclude internal notes          |
| Related restricted case found          | User cannot view record     | Do not expose details; route internally if allowed | “I can prepare this for service team review.”           | Avoid hidden-record implication |
| Internal reviewer opens case           | Reviewer has assigned scope | Show reviewable context labels                     | Internal-only display                                   | Not public-facing               |

---

## 20. Language and Localization Moments

| Moment                            | Language situation               | Assistant behavior                                 | Context required                               | Notes                        |
| --------------------------------- | -------------------------------- | -------------------------------------------------- | ---------------------------------------------- | ---------------------------- |
| User writes in Arabic             | Supported sample language        | Respond in Arabic                                  | Input/output language, confidence              | Preserve original wording    |
| User mixes languages              | Mixed input                      | Continue in user-preferred or dominant language    | Language preference, original text             | Ask if unclear               |
| User uses transliteration         | Place or service term ambiguous  | Preserve spelling and ask if needed                | Transliteration note, location/service options | Do not silently normalize    |
| Official term appears             | Term should not be translated    | Preserve official term                             | Terms-not-to-translate list                    | Explain simply if needed     |
| Translation affects routing       | Low or medium confidence mapping | Ask clarification or route for review              | Language notes, candidate categories           | Do not route on weak mapping |
| Complaint in non-default language | Complaint wording matters        | Preserve original and translated summary if needed | Original text, translated summary              | Review may need both         |

---

## 21. Error, Interruption, and Recovery Flows

| Scenario                              | Assistant behavior                                 | State to preserve                             | State to reset or re-check             | User-facing message                                                         |
| ------------------------------------- | -------------------------------------------------- | --------------------------------------------- | -------------------------------------- | --------------------------------------------------------------------------- |
| User pauses and returns               | Summarize confirmed state and next needed field    | Confirmed request type, category, description | Missing fields                         | “We already have [summary]. I only need [field].”                           |
| User changes request type             | Reclassify and update required fields              | Original wording where useful                 | Request type, routing, required fields | “Got it — I’ll treat this as [new type].”                                   |
| User corrects location                | Update location and re-check routing               | Unchanged confirmed details                   | Location-based destination             | “I updated the location. I’ll re-check where this should go.”               |
| User asks unrelated question mid-flow | Answer if in scope, then offer return              | Current workflow state                        | Whether user wants to continue         | “I can answer that, then we can return to your request.”                    |
| Retrieval fails                       | Explain limitation and proceed or route for review | User-provided info                            | Whether retrieval is required          | “I can continue with the details you provided, or prepare this for review.” |
| User abandons before completion       | Save draft if allowed or end session               | Confirmed draft fields                        | Expiration or stale fields later       | “You can return later and continue from here.”                              |

---

## 22. Escalation and Handoff Flows

| Trigger                             | Destination                          | Assistant behavior                                   | Output required                              | User-facing message                                            |
| ----------------------------------- | ------------------------------------ | ---------------------------------------------------- | -------------------------------------------- | -------------------------------------------------------------- |
| Safety risk or danger signal        | Escalation queue                     | Stop routine routing and prepare escalation summary  | Known info, risk signal, missing info, owner | “I can prepare this for review with the details you shared.”   |
| Repeated unresolved issue           | Complaint review or escalation queue | Preserve wording and prior-attempt signal            | Original text, repeated issue signal         | “I’ll include that this has already been reported.”            |
| Dispute about prior handling        | Complaint review queue               | Treat as complaint/follow-up                         | Case ID if visible, user wording             | “I can prepare this as a complaint about prior handling.”      |
| Restricted context affects handling | Service review queue                 | Route internally without exposing restricted details | Internal note, safe user response            | “I can prepare this for service team review.”                  |
| Low-confidence routing              | Service review queue                 | Ask clarification or route for review                | Candidate categories, uncertainty            | “This may need review to make sure it goes to the right team.” |

Handoff outputs may include:

* user-facing acknowledgement
* structured case record
* routing recommendation
* escalation summary
* original user wording
* known information
* missing information
* retrieved context labels
* permission notes
* language notes
* open questions

---

## 23. Structured Output Moments

| Moment           | Output type            | Audience                     | Required fields                                                    | Downstream use            |
| ---------------- | ---------------------- | ---------------------------- | ------------------------------------------------------------------ | ------------------------- |
| Missing info     | Clarification question | User                         | One question, brief reason                                         | Continue intake           |
| User review      | User-facing summary    | User                         | Request type, summary, key details, confirmation prompt            | Confirm before submission |
| Routing          | Structured case record | System/internal service team | Request type, category, known info, missing info, routing reason   | Create/reroute case       |
| Complaint review | Complaint review note  | Reviewer                     | Original wording, complaint signal, prior attempts, open questions | Human review              |
| Escalation       | Escalation summary     | Escalation owner             | Trigger, known info, risk signal, missing info                     | Human escalation          |
| Follow-up        | Follow-up note         | User/system                  | Case reference, new detail, confirmation                           | Existing case update      |
| Recovery         | Recovery summary       | User                         | Confirmed state, remaining info                                    | Resume workflow           |

Sample structured case record:

```yaml
case_record:
  request_type: "issue_report"
  service_category: "streets_and_infrastructure"
  user_facing_summary: "Streetlight near user’s building has been out for three nights."
  known_information:
    - "Issue description provided."
    - "Location confirmed."
  missing_information: []
  routing:
    recommended_destination: "infrastructure_service_queue"
    confidence: "high"
  review:
    human_review_required: false
    open_questions: []
```

---

## 24. Screen, Form, or Interface Context

| Interface context            | Visible to user?        | Passed to model?            | Used for                    | Notes                                             |
| ---------------------------- | ----------------------- | --------------------------- | --------------------------- | ------------------------------------------------- |
| Selected service category    | Yes                     | Yes if relevant             | Avoid asking category again | Label as interface context                        |
| Completed form fields        | Yes                     | Yes if needed               | Missing info detection      | Do not ask again                                  |
| Entry point page             | Yes                     | Conditional                 | Intent prior                | Useful but not definitive                         |
| Uploaded attachment metadata | Yes                     | Conditional                 | Evidence or case context    | Do not assume attachment content if not processed |
| User locale/language setting | Maybe                   | Conditional                 | Language behavior           | Confirm if user writes in another language        |
| Authenticated status         | Maybe                   | Yes when permissions matter | Case history access         | Avoid exposing auth internals                     |
| Existing case selected       | Yes                     | Yes if permitted            | Follow-up flow              | Permission check required                         |
| Accessibility setting        | Yes/No depending system | Conditional                 | Interaction style           | Keep outputs clear and low-friction               |

---

## 25. Accessibility and Inclusive Interaction Notes

The assistant should:

* use plain language
* ask one question at a time when possible
* avoid forcing users to understand internal service taxonomy
* avoid long blocks of text during intake
* provide clear confirmation before submission or handoff
* support recovery without restart
* preserve multilingual input without treating it as a problem
* avoid blaming users for unclear input
* explain why required information is needed when useful
* support users who cannot complete long forms in one session

---

## 26. Flow Testing Scenarios

| Scenario                                         | What it tests                | Expected outcome                                    | Related diagnostic category         |
| ------------------------------------------------ | ---------------------------- | --------------------------------------------------- | ----------------------------------- |
| User reports streetlight outage without location | Missing information handling | Ask only for location                               | Over-questioning / missing info     |
| Anonymous user asks how to submit a request      | Public answer behavior       | Answer from public context only                     | Retrieval / permission              |
| User reports repeated dangerous issue            | Escalation behavior          | Preserve wording and prepare review/escalation      | Escalation / complaint preservation |
| User provides visible case ID                    | Follow-up continuity         | Prepare follow-up without redundant questions       | Case history / recovery             |
| Restricted related case exists                   | Permission-safe routing      | Avoid leakage and route for review                  | Permission leakage                  |
| Arabic input maps to multiple categories         | Language ambiguity           | Ask clarification in Arabic                         | Language handling / routing         |
| User resumes after pause                         | State recovery               | Summarize confirmed info and ask remaining question | Recovery / state preservation       |
| User corrects service category                   | Re-routing                   | Update category and re-check required fields        | Routing correction                  |
| Retrieval fails                                  | Fallback behavior            | Continue with known info or route for review        | Retrieval failure                   |
| User disputes prior handling                     | Complaint follow-up          | Preserve wording and route for complaint review     | Complaint / case history            |

---

## 27. Open Questions

| Question                                                          | Why it matters                                        | Likely owner             | Status |
| ----------------------------------------------------------------- | ----------------------------------------------------- | ------------------------ | ------ |
| What entry points will exist in the actual service experience?    | Affects starting context and first assistant behavior | Product / UX             | Open   |
| Which form fields are visible before the assistant responds?      | Affects missing-information logic                     | UX / engineering         | Open   |
| Which actions require user confirmation?                          | Affects review moments                                | Product / policy         | Open   |
| What user-facing language is approved for routing and escalation? | Prevents overpromising or over-disclosure             | Product / legal / policy | Open   |
| What happens after handoff to a service team?                     | Affects user expectations                             | Operations               | Open   |
| What recovery state is saved between sessions?                    | Affects continuity and privacy                        | Engineering / security   | Open   |
| What accessibility requirements apply?                            | Affects conversation design                           | Accessibility / UX       | Open   |
| Which languages and localization patterns are supported?          | Affects multilingual flows                            | Localization / product   | Open   |

---

## 28. Related Documents

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
* `example-context-packets.md`
* `testing-diagnostics-spec.md`
