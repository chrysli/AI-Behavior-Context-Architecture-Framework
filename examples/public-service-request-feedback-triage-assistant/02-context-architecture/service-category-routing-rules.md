# Service Category Routing Rules

This document defines sample service category routing rules for the fictional **Public Service Request & Feedback Triage Assistant**.

The goal is to show how an AI-enabled workflow can classify public service requests, identify the right routing path, flag missing information, and avoid unsupported or unsafe routing decisions.

These routing rules are fictional and simplified. A real implementation would need an approved service taxonomy, operational ownership model, permission rules, escalation paths, and service-level policies.

---

## 1. Routing Rules Overview

**System name:** Public Service Request & Feedback Triage Assistant

**Workflow name:** Public Service Request & Feedback Triage

**Routing rules version:** 0.1 fictional sample

**Sample status:** Fictional public-safe architecture example

---

## 2. Purpose of Service Category Routing Rules

These routing rules exist to help the assistant determine which service category, team, queue, or review path should receive a user’s question, complaint, issue report, feedback item, service request, or follow-up.

The assistant should not route only from surface-level keyword matching. Routing should consider request type, workflow state, service category, required information, user role, permission scope, language confidence, related case context, and escalation triggers.

---

## 3. Routing Goals

The routing process should:

* classify the request type consistently
* map the request to a service category when enough context is available
* identify missing information before routing when required
* avoid routing based on weak or unsupported inference
* preserve the user’s original intent
* respect permission and visibility boundaries
* flag low-confidence routing for review
* escalate urgent or sensitive cases when required
* make routing reasons reviewable by human service teams

---

## 4. Routing Scope

In scope:

* request type classification
* service category classification
* missing routing information
* routing confidence
* user-facing routing explanations
* internal routing notes
* human review triggers
* escalation routing
* re-routing after correction or new context

Out of scope:

* final case resolution
* service-level agreement commitments
* emergency dispatch
* legal or policy determinations
* full service catalog design
* production queue configuration
* real operational ownership model

---

## 5. Sample Service Categories

The sample uses the following fictional service categories.

| Service category           | Description                                                                              | Example user input                                   | Typical destination                         |
| -------------------------- | ---------------------------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------- |
| General inquiry            | User asks for general information or guidance                                            | “How do I apply for this service?”                   | Self-service answer flow or general support |
| Streets and infrastructure | User reports roads, sidewalks, lighting, public infrastructure, or street-related issues | “The streetlight near my building is out.”           | Infrastructure service queue                |
| Waste and sanitation       | User reports trash, recycling, sanitation, or public cleanliness issues                  | “The bins on my street have not been collected.”     | Waste services queue                        |
| Parks and public spaces    | User reports parks, playgrounds, public seating, greenery, or public space issues        | “The park gate is broken.”                           | Public spaces service queue                 |
| Licensing and permits      | User asks about licenses, permits, applications, or approvals                            | “I need help with a permit application.”             | Licensing support queue                     |
| Business services          | User asks about business registration, business services, or company-related requests    | “My business account shows the wrong status.”        | Business services queue                     |
| Digital services           | User reports website, app, account, form, payment, or portal issues                      | “The online form keeps failing.”                     | Digital support queue                       |
| Public facilities          | User reports issues with public buildings, service centers, or shared facilities         | “The elevator at the service center is not working.” | Facilities service queue                    |
| Complaint review           | User expresses dissatisfaction, poor handling, harm, or unresolved service experience    | “I complained before and nobody responded.”          | Complaint review queue                      |
| Escalation review          | User reports urgency, safety risk, repeated unresolved issue, or sensitive concern       | “This is dangerous and needs attention now.”         | Escalation queue                            |

These categories are illustrative only. A real system should use official category labels and routing ownership.

---

## 6. Request Type to Routing Behavior

| Request type    | Assistant behavior                                              | Routing behavior                                                 |
| --------------- | --------------------------------------------------------------- | ---------------------------------------------------------------- |
| Question        | Answer from approved public or permitted context when available | Route only if answer requires service team support               |
| Complaint       | Preserve wording, assess escalation, prepare reviewable case    | Route to complaint review or escalation if criteria are met      |
| Service request | Collect required service details                                | Route to service category queue when required fields are present |
| Issue report    | Collect location, description, timing, urgency if required      | Route to responsible service queue or escalation                 |
| Feedback item   | Preserve user intent and identify whether action is requested   | Route to feedback review or relevant service category            |
| Follow-up       | Check permitted case history or ask for case ID                 | Route to existing case flow, review queue, or new request path   |

---

## 7. Routing Inputs

| Input                    | Source                         | Required?       | Used for                                 | Notes                                       |
| ------------------------ | ------------------------------ | --------------- | ---------------------------------------- | ------------------------------------------- |
| User description         | User message                   | Yes             | Request type and category classification | Preserve original wording when needed       |
| Request type             | Inference or user selection    | Yes for routing | Workflow path and output requirements    | Confirm if ambiguous                        |
| Service category         | Service catalog or inference   | Conditional     | Routing destination                      | Use official category labels when available |
| Location or jurisdiction | User, profile, or form field   | Conditional     | Routing service team                     | Required for many issue reports             |
| User role                | Account context                | Conditional     | Permissions and allowed actions          | Anonymous users have limited case access    |
| Case ID                  | User or system                 | Conditional     | Follow-up routing                        | Exact ID should be prioritized              |
| Related case context     | Case history retrieval         | Conditional     | Duplicate prevention and review          | Permission-bound                            |
| Language context         | Language handling layer        | Conditional     | Classification and routing confidence    | Flag ambiguous translation                  |
| Urgency or risk signal   | User input or escalation rules | Conditional     | Escalation routing                       | Should override normal route when required  |
| Service catalog match    | Retrieval                      | Conditional     | Category and destination                 | Include source and confidence               |

---

## 8. Routing Destinations

| Destination                  | Description                                                           | Receives                                                 | Required conditions                                           |
| ---------------------------- | --------------------------------------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------------- |
| Self-service answer flow     | User-facing answer from approved knowledge base                       | General questions                                        | Answer available and permitted                                |
| General support queue        | Human support for unclear or broad requests                           | Questions or requests that cannot be classified          | Low category confidence or missing service catalog context    |
| Infrastructure service queue | Service team for streets, sidewalks, lighting, roads                  | Infrastructure issue reports or service requests         | Category confidence medium/high and required location present |
| Waste services queue         | Service team for waste, sanitation, cleanliness                       | Waste and sanitation requests                            | Category confidence medium/high and required details present  |
| Public spaces service queue  | Service team for parks and public spaces                              | Public space issues or requests                          | Category confidence medium/high                               |
| Licensing support queue      | Team handling permits, licenses, applications                         | Licensing and permit questions or service requests       | User role and request details sufficient                      |
| Business services queue      | Team handling business-related services                               | Business owner requests or account issues                | Business context available or review required                 |
| Digital support queue        | Team handling portal, app, form, account, or digital service issues   | Digital service issue reports                            | Error, account, or service context present                    |
| Complaint review queue       | Human review path for dissatisfaction or service complaint            | Complaints and disputed handling                         | Complaint signal present                                      |
| Escalation queue             | Human review for urgent, sensitive, or high-risk cases                | Escalation-triggering cases                              | Escalation trigger present                                    |
| Service review queue         | Human review for ambiguous routing or restricted related-case context | Low-confidence, conflicting, or restricted-context cases | Review required                                               |

---

## 9. Routing Decision Table

| Condition                                                                              | Routing destination                            | Confidence required   | Human review required?               | Notes                                  |
| -------------------------------------------------------------------------------------- | ---------------------------------------------- | --------------------- | ------------------------------------ | -------------------------------------- |
| User asks general service question and answer exists in approved knowledge base        | Self-service answer flow                       | High                  | No                                   | Provide answer and next step           |
| User reports streetlight, road, sidewalk, or public infrastructure issue with location | Infrastructure service queue                   | Medium/high           | No, unless escalation trigger exists | Location usually required              |
| User reports missed trash collection, sanitation, or public cleanliness issue          | Waste services queue                           | Medium/high           | No, unless repeated unresolved issue | Location may be required               |
| User reports broken park equipment or public space issue                               | Public spaces service queue                    | Medium/high           | Conditional                          | Review if safety risk exists           |
| User asks about permit, license, or application process                                | Licensing support queue or self-service answer | Medium/high           | Conditional                          | Business/account context may be needed |
| User reports online form, portal, account, or digital service failure                  | Digital support queue                          | Medium/high           | Conditional                          | Error details may be required          |
| User expresses dissatisfaction with prior service handling                             | Complaint review queue                         | Medium/high           | Yes                                  | Preserve original wording              |
| User reports danger, harm, urgent disruption, or safety concern                        | Escalation queue                               | Medium/high           | Yes                                  | Escalation can override normal routing |
| User provides case ID for follow-up                                                    | Existing case or review flow                   | Exact match preferred | Conditional                          | Permission check required              |
| Category confidence is low or multiple categories apply                                | Service review queue                           | Low/conflicting       | Yes                                  | Ask clarification if user can resolve  |
| Related case is found but restricted                                                   | Service review queue                           | Possible/strong       | Yes                                  | Do not expose restricted details       |

---

## 10. Routing Confidence Rules

| Confidence level | Meaning                                                                               | Assistant behavior                                                     |
| ---------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| High             | Request type and category are strongly supported by user input and/or service catalog | Recommend routing or proceed if required fields are present            |
| Medium           | Category is likely but some information may be incomplete                             | Ask targeted clarification or route with review flag depending on risk |
| Low              | Category is uncertain or based on weak inference                                      | Ask clarification or route to service review queue                     |
| Conflicting      | Multiple categories or sources conflict                                               | Do not choose silently; ask clarification or route for review          |
| Blocked          | Required information or permission is missing                                         | Do not route until resolved or reviewed                                |

Confidence should be reduced when:

* request type is inferred but not confirmed
* language interpretation is uncertain
* location or jurisdiction is missing
* service catalog match is weak
* related case context conflicts with user input
* permission status is unknown
* user intent could map to multiple categories

---

## 11. Missing Information Rules

| Missing information | Blocks routing?                          | Assistant behavior                                        | Notes                                                      |
| ------------------- | ---------------------------------------- | --------------------------------------------------------- | ---------------------------------------------------------- |
| Request description | Yes                                      | Ask user to describe the need or issue                    | Cannot classify without it                                 |
| Request type        | Conditional                              | Infer if high confidence; otherwise ask clarification     | Do not over-classify ambiguous messages                    |
| Location            | Yes for location-dependent issue reports | Ask one targeted location question                        | Required for streets, sanitation, facilities, parks issues |
| Case ID             | Conditional for follow-up                | Ask if user wants case-specific follow-up                 | User may still submit a new request without ID             |
| User authentication | Yes for private case history             | Ask user to sign in or proceed without case history       | Do not expose case details anonymously                     |
| Service category    | Conditional                              | Retrieve service catalog or ask clarification             | Route for review if unresolved                             |
| Urgency details     | Conditional                              | Ask or escalate depending on risk                         | Do not delay if immediate escalation is required           |
| Language meaning    | Conditional                              | Ask clarification or preserve original wording for review | Required if ambiguity affects routing                      |

---

## 12. Inference Rules for Routing

| Inference                         | Allowed?    | Requires confirmation?                  | Can affect routing? | Notes                                         |
| --------------------------------- | ----------- | --------------------------------------- | ------------------- | --------------------------------------------- |
| Request is likely an issue report | Yes         | Conditional                             | Yes                 | Confirm if category or urgency is unclear     |
| Request is likely a complaint     | Yes         | Conditional                             | Yes                 | Preserve original wording                     |
| Service category from keywords    | Conditional | Yes if confidence is medium/low         | Conditional         | Use service catalog when available            |
| User is following up              | Yes         | Yes when case linking affects next step | Yes                 | Ask for case ID if not available              |
| Urgency signal exists             | Yes         | Conditional                             | Yes                 | Escalation may be required                    |
| Location from user wording        | Conditional | Yes if ambiguous                        | Yes                 | Do not silently normalize ambiguous locations |

The assistant should not route based only on weak semantic similarity or unsupported assumptions.

---

## 13. Case History and Related-Case Routing Rules

Case history may affect routing when:

* the user provides a case ID
* the user asks for status or follow-up
* the user reports a repeated unresolved issue
* a duplicate or related case may exist
* internal review needs prior context

| Case history situation                 | Assistant behavior                                                          | Routing behavior                             | Visibility rule                    |
| -------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------------- | ---------------------------------- |
| Exact case ID visible to user          | Use permitted case context                                                  | Existing case follow-up path                 | Show only allowed fields           |
| User references prior issue without ID | Ask for case ID or identifying detail                                       | Follow-up or new request path                | Do not retrieve without permission |
| Related visible case found             | Ask whether user wants to continue with existing case or create new request | Follow-up or linked case path                | User can see related summary       |
| Related restricted case found          | Avoid user-facing details                                                   | Service review queue                         | Internal routing only if allowed   |
| Weak possible match                    | Do not rely on match                                                        | Ask clarification or continue as new request | Do not present as duplicate        |
| No related case found                  | Continue current workflow                                                   | New request path                             | State only if appropriate          |

Reference related document:

* `case-history-context-rules.md`

---

## 14. Permission-Aware Routing Rules

The assistant should:

* route using only context permitted for the workflow and role
* avoid exposing restricted routing reasons to the user
* avoid revealing hidden records through routing explanations
* include internal routing notes only when the receiving role may access them
* explain user-facing limitations safely when information cannot be shown

| Permission situation                | Routing behavior                                 | User-facing explanation                        | Internal note allowed?                 |
| ----------------------------------- | ------------------------------------------------ | ---------------------------------------------- | -------------------------------------- |
| User can view all relevant context  | Route or recommend based on visible context      | Explain relevant next step                     | Yes if useful                          |
| User cannot view related record     | Route for review if related context matters      | Use safe generic explanation                   | Yes, if receiving team can access it   |
| Permission status unknown           | Do not expose private context                    | Ask user to authenticate or route for review   | Conditional                            |
| Anonymous user asks for case status | Do not retrieve private case history             | Ask user to sign in or provide public guidance | No private note unless workflow allows |
| Tenant/account boundary unclear     | Do not retrieve or route based on private record | Explain limitation or route for review         | Conditional                            |

---

## 15. Language-Sensitive Routing Rules

Language may affect routing when:

* translated terms map to multiple service categories
* informal wording, dialect, or transliteration affects meaning
* official service terms have no direct translation
* user input mixes languages
* retrieved context is in a different language
* language confidence is low

| Language situation                          | Routing behavior                                 | Confirmation needed?  | Notes                                       |
| ------------------------------------------- | ------------------------------------------------ | --------------------- | ------------------------------------------- |
| User writes in Arabic and category is clear | Use mapped official category                     | No if high confidence | Preserve original wording                   |
| Translation maps to multiple categories     | Ask targeted clarification                       | Yes                   | Do not silently choose                      |
| User uses transliterated place name         | Ask confirmation if location affects routing     | Conditional           | Preserve original spelling                  |
| User switches language mid-flow             | Continue in selected language and preserve state | Conditional           | Do not restart flow                         |
| Official term has no approved translation   | Preserve official term                           | Conditional           | Explain in user-friendly language if needed |

Reference related document:

* `language-handling-rules.md`

---

## 16. Escalation Routing Rules

Escalation may override normal service category routing.

Escalation triggers may include:

* safety risk
* harm or danger
* urgent disruption
* repeated unresolved issue
* user dispute about prior handling
* sensitive complaint
* language ambiguity affecting urgency or risk
* permission or policy conflict requiring human review
* system cannot safely continue

| Escalation trigger                    | Escalation destination               | Required output                                     | User-facing behavior                               |
| ------------------------------------- | ------------------------------------ | --------------------------------------------------- | -------------------------------------------------- |
| Safety risk or harm reported          | Escalation queue                     | Known info, missing info, risk signal, next step    | Acknowledge and route without unsupported promises |
| Repeated unresolved issue             | Complaint review or escalation queue | Original wording, prior issue signal, known details | Preserve user frustration or concern               |
| Prior handling disputed               | Complaint review queue               | Complaint summary and prior handling note           | Avoid defending system                             |
| Restricted context needed for routing | Service review queue                 | Internal routing note                               | Do not expose restricted details                   |
| Language ambiguity affects urgency    | Escalation or language review queue  | Original text and ambiguity note                    | Clarify or escalate conservatively                 |

---

## 17. Human Review Rules

Human review may be required when:

* routing confidence is low
* multiple service categories may apply
* language ambiguity affects classification
* related case context is restricted
* user disputes previous handling
* escalation may be required
* permission boundaries are unclear
* service catalog context conflicts with user input
* assistant cannot safely proceed

| Review trigger                          | Assistant behavior                     | Review owner          | Output required                           |
| --------------------------------------- | -------------------------------------- | --------------------- | ----------------------------------------- |
| Low routing confidence                  | Ask clarification or route for review  | Service reviewer      | User summary and open question            |
| Multiple possible categories            | Ask user if possible; otherwise review | Service reviewer      | Candidate categories and reason           |
| Restricted related case affects routing | Route for review                       | Service team/reviewer | Internal note without user-facing leakage |
| Complaint with unclear ownership        | Route to complaint review              | Complaint reviewer    | Preserved original wording                |
| Escalation criteria unclear             | Route to escalation review             | Operations reviewer   | Risk signal and missing info              |

---

## 18. Routing Output Structure

Sample routing output:

```yaml
routing_output:
  request_type: "issue_report"
  request_type_status: "inferred"
  service_category: "streets_and_infrastructure"
  service_category_status: "inferred"
  recommended_destination: "infrastructure_service_queue"
  routing_confidence: "medium"
  routing_reason:
    user_visible: "This looks related to streets and infrastructure based on the issue described."
    internal: "Streetlight outage inferred from user description; location required before final routing."
  missing_information:
    - "location"
  related_case_context:
    checked: false
    result: "not_checked"
  escalation:
    required: false
    trigger: null
  human_review:
    required: false
    reason: null
```

---

## 19. User-Facing Routing Explanation Rules

The assistant may explain routing when it helps the user understand the next step.

Preferred patterns:

```text
Based on what you shared, this looks like it should go to [service category]. Before I route it, I need one more detail: [missing information].
```

```text
I can prepare this for [service category]. I’ll show you the summary before it is sent.
```

```text
This may need review because [safe user-facing reason]. I’ll prepare the details for the service team.
```

The assistant should not expose:

* restricted records
* internal-only notes
* hidden risk signals
* unauthorized routing rules
* other users’ case details
* restricted related-case information

---

## 20. Correction and Re-Routing Rules

Re-routing may be required when:

* the user corrects earlier information
* the assistant misclassifies the request type
* the service category changes
* new location or case information is provided
* related case history changes the recommended destination
* urgency changes
* permission status changes

| Re-routing trigger                 | Assistant behavior                                       | Preserve                | Re-check                         |
| ---------------------------------- | -------------------------------------------------------- | ----------------------- | -------------------------------- |
| User corrects service category     | Update category and routing output                       | Original description    | Required fields for new category |
| User changes complaint to feedback | Reclassify and update workflow path                      | User wording            | Routing and confirmation needs   |
| User adds urgency signal           | Check escalation rules                                   | Known request info      | Escalation destination           |
| User provides case ID later        | Check permitted case history                             | Current request summary | Follow-up routing                |
| Location changes                   | Update routing destination if location affects ownership | Issue description       | Jurisdiction/team ownership      |

---

## 21. Routing Failure Patterns to Test

Test for these routing failures:

* routes before required information is collected
* misclassifies complaint as generic feedback
* misclassifies issue report as question
* routes based on weak inference
* ignores service catalog context
* exposes restricted related-case details
* routes anonymous user based on private case history
* fails to reduce confidence when language interpretation is uncertain
* fails to escalate safety-related issue
* routes to wrong category after user correction
* asks for service taxonomy labels the user should not need to know
* creates duplicate request when visible existing case should be offered
* blocks routing unnecessarily when enough context is available

---

## 22. Routing Review Checklist

Before approving routing rules, check:

* Are service categories defined?
* Are request types mapped to routing behavior?
* Are required routing inputs defined?
* Are missing-information blockers clear?
* Are confidence rules defined?
* Are permission boundaries applied?
* Are language-sensitive routing risks addressed?
* Are case history and related-case rules referenced?
* Are escalation triggers defined?
* Are human review triggers defined?
* Are user-facing routing explanations safe?
* Are routing failure patterns included in testing?

---

## 23. Open Questions

| Question                                                  | Why it matters                        | Likely owner             | Status |
| --------------------------------------------------------- | ------------------------------------- | ------------------------ | ------ |
| What is the official service taxonomy?                    | Required for real routing             | Service operations       | Open   |
| Which service categories can be routed automatically?     | Defines automation boundaries         | Product / operations     | Open   |
| Which categories require human review?                    | Affects routing confidence thresholds | Operations / policy      | Open   |
| What service categories are available to anonymous users? | Affects public-facing intake          | Product / policy         | Open   |
| Which escalation triggers override normal routing?        | Affects risk handling                 | Operations / policy      | Open   |
| What user-facing routing explanations are approved?       | Prevents over-disclosure              | Product / legal / policy | Open   |

---

## 24. Related Documents

* `sample-scope-and-assumptions.md`
* `ai-design-principles.md`
* `ai-assistant-behavior-spec.md`
* `public-service-request-feedback-triage-assistant-behavior-spec.md`
* `context-architecture-spec.md`
* `runtime-context-template.md`
* `context-assembly-rules.md`
* `language-handling-rules.md`
* `case-history-context-rules.md`
* `example-context-packets.md`
* `testing-diagnostics-spec.md`
* `public-service-request-feedback-conversation-flows.md`
