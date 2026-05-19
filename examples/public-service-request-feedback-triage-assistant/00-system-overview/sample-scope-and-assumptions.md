# Sample Scope and Assumptions

This document defines the scope, assumptions, limitations, and open questions for the fictional **Public Service Request & Feedback Triage Assistant** sample architecture.

The purpose of this file is to make clear what is intentionally defined, what is assumed for demonstration purposes, and what would need validation in a real implementation.

---

## 1. Sample Overview

**Sample name:** Public Service Request & Feedback Triage Assistant

**Sample type:** Fictional AI behavior and context architecture example

**Primary workflow:** Public service request, feedback, complaint, issue report, question, and follow-up intake

**Primary users:** Residents, business owners, internal service teams, reviewers, and administrators

**Purpose:** Demonstrate how the AI Behavior & Context Architecture Framework can be applied to a workflow-aware public service intake system.

---

## 2. Sample Purpose

This sample exists to show how a public-facing AI assistant could support service request and feedback intake by using explicit behavior rules, runtime context, workflow state, routing logic, language handling, case history rules, diagnostics, and SD/UX workflow artifacts.

The sample is designed to show the system around the model.

It demonstrates how an AI assistant might:

* identify the user’s request type
* distinguish between a question, complaint, service request, issue report, feedback item, or follow-up
* collect only missing information needed to proceed
* preserve the user’s original intent
* apply language handling rules
* retrieve permitted case history or service information
* prepare a structured case record
* recommend routing or escalation
* support user review before submission
* prepare information for human service teams

---

## 3. What This Sample Is

This sample is:

* a fictional architecture example
* a public-safe demonstration of the framework
* a reusable pattern for thinking about AI behavior and context architecture
* a simplified workflow model
* an example of how documentation layers can work together
* a starting point for teams adapting the framework to their own systems

---

## 4. What This Sample Is Not

This sample is not:

* a production-ready system design
* a real government service implementation
* a legal, compliance, privacy, or security recommendation
* a complete public-sector operating model
* a complete service taxonomy
* a complete case management model
* a substitute for stakeholder, policy, legal, accessibility, security, or operational review
* a claim that all public service systems should behave this way

Any real implementation would need local policy, legal, service, data, security, accessibility, and operational validation.

---

## 5. Workflow Scope

This sample covers a simplified intake and triage workflow.

In scope:

* user starts a request or feedback interaction
* assistant identifies likely request type
* assistant asks for missing required information
* assistant preserves user wording when needed
* assistant applies language handling rules
* assistant checks permitted case history or related records when appropriate
* assistant prepares a structured case record
* assistant recommends routing to a service team or review queue
* assistant flags escalation conditions
* user reviews important information before submission or handoff

Out of scope:

* full case resolution
* payment processing
* identity verification design
* authentication flow design
* legal adjudication
* formal eligibility determination
* emergency response operations
* full CRM or case management implementation
* production data governance model
* technical infrastructure design
* procurement or vendor architecture

---

## 6. Fictional System Assumptions

The sample assumes a fictional public service platform with the following capabilities.

### Public service portal

Users can access a digital portal to ask questions, submit feedback, report issues, request services, or follow up on existing cases.

### AI-assisted intake

The AI assistant supports the intake process by helping users describe what they need and preparing structured information for downstream handling.

### Service catalog

The system has a service catalog containing service categories, descriptions, responsible teams, and routing rules.

### Case management system

The system can create, update, retrieve, or reference service cases, depending on user permissions and workflow state.

### User roles

The system has at least these fictional roles:

* anonymous user
* authenticated resident
* authenticated business owner
* internal service team member
* reviewer
* administrator

### Multilingual interaction

The assistant may receive input in more than one language. The sample assumes multilingual handling is relevant, but does not define a complete localization strategy.

### Human review and escalation

Some cases may require review or escalation to a human service team.

---

## 7. User Role Assumptions

| Role                         | Assumption                                                                      | Notes                                           |
| ---------------------------- | ------------------------------------------------------------------------------- | ----------------------------------------------- |
| Anonymous user               | Can ask general questions and submit some non-sensitive feedback                | May not access case history                     |
| Authenticated resident       | Can submit requests and view their own cases                                    | Case history access is limited to own records   |
| Authenticated business owner | Can submit business-related service requests and view authorized business cases | May require account or license context          |
| Internal service team member | Can view routed cases assigned to their team                                    | May see internal notes depending on permissions |
| Reviewer                     | Can review structured case records and routing recommendations                  | May see diagnostic context when approved        |
| Administrator                | Can manage service categories, routing rules, and permissions                   | Full access is still bounded by policy          |

These roles are simplified for demonstration.

A real implementation would require a formal role and permission model.

---

## 8. Request Type Assumptions

The sample uses these request types:

| Request type    | Description                                                                           |
| --------------- | ------------------------------------------------------------------------------------- |
| Question        | User wants information or guidance                                                    |
| Complaint       | User reports dissatisfaction, harm, poor handling, or unacceptable service experience |
| Service request | User wants a service action performed                                                 |
| Issue report    | User reports something broken, unavailable, unsafe, delayed, or not working           |
| Feedback item   | User provides a suggestion, opinion, experience report, or improvement note           |
| Follow-up       | User asks about an existing case, prior request, or repeated issue                    |

These request types are fictional and simplified.

A real implementation would need a validated service taxonomy.

---

## 9. Service Category Assumptions

The sample may reference generic service categories such as:

* transport and mobility
* streets and infrastructure
* utilities
* licensing and permits
* business services
* public facilities
* waste and sanitation
* parks and public spaces
* digital services
* general inquiry

These categories are illustrative only.

The sample does not define a complete service catalog.

---

## 10. Case History Assumptions

The sample assumes case history may exist and may be relevant when:

* the user follows up on an existing case
* the user provides a case ID
* the user reports a repeated issue
* a related open case may already exist
* service teams need prior context for routing or review

Case history use is permission-bound.

The assistant should not expose case history unless the user is permitted to view it.

Restricted case history may sometimes be used for internal routing or review, but only if the context architecture explicitly allows it.

---

## 11. Language Assumptions

The sample assumes multilingual handling may be required.

Language handling may include:

* detecting input language
* responding in the user’s language when supported
* preserving original user wording
* translating summaries for internal review
* preserving official service names or IDs
* flagging uncertain translation when meaning affects routing or escalation

The sample does not define a complete language policy, supported-language list, or translation quality model.

---

## 12. Routing Assumptions

The sample assumes routing may depend on:

* request type
* service category
* user role
* location or jurisdiction
* missing information
* urgency
* related case history
* escalation triggers
* language ambiguity
* permission boundaries

The assistant may recommend routing, prepare routing context, or send the case to a review queue depending on workflow rules.

The sample does not assume the assistant has authority to make final operational decisions in all cases.

---

## 13. Escalation Assumptions

The sample assumes escalation may be required when:

* the user reports safety risk, harm, urgency, or service disruption
* the user reports repeated unresolved issues
* permission conflicts prevent normal handling
* language ambiguity affects risk, urgency, or eligibility
* the assistant cannot safely continue without human review
* the user disputes prior handling

Escalation behavior is simplified and does not represent emergency response design.

---

## 14. Data and Privacy Assumptions

The sample assumes that data access is restricted by role, account, tenant, and workflow state.

The assistant should:

* avoid unnecessary personal data collection
* avoid exposing restricted case history
* avoid revealing internal notes to unauthorized users
* preserve user-provided details only when needed for workflow, review, or compliance
* support traceability without over-logging sensitive content

The sample does not define a complete privacy, security, or retention model.

---

## 15. AI Behavior Assumptions

The assistant should:

* preserve user intent
* ask fewer, better questions
* use available context before asking the user to repeat information
* distinguish known information, retrieved context, inference, uncertainty, and missing information
* avoid presenting inference as fact
* avoid exposing restricted information
* adapt behavior to workflow state
* provide clear next steps
* support user review before submission or handoff

---

## 16. Known Limitations of This Sample

This sample intentionally leaves some areas incomplete.

Known limitations:

* no complete legal or policy model
* no complete accessibility model
* no complete authentication or identity flow
* no complete service taxonomy
* no production database schema
* no full multilingual policy
* no complete human operations model
* no real service-level agreements
* no actual integration architecture
* no real jurisdictional rules

These gaps are intentional. They demonstrate where real teams would need stakeholder input and implementation-specific decisions.

---

## 17. Open Questions for a Real Implementation

| Question                                                    | Why it matters                                   | Likely owner                        |
| ----------------------------------------------------------- | ------------------------------------------------ | ----------------------------------- |
| What request types are officially supported?                | Affects classification, routing, and outputs     | Product / service operations        |
| What service categories exist?                              | Affects routing and service-team ownership       | Service operations                  |
| Which roles can view case history?                          | Affects permissions and user-facing explanations | Security / policy / product         |
| Which languages are supported?                              | Affects language handling and user experience    | Product / localization / operations |
| What information is required for each request type?         | Affects missing-information questions            | Product / service operations        |
| Which cases require escalation?                             | Affects risk handling and routing                | Operations / policy                 |
| What structured output format does the case system require? | Affects runtime packet and output schema         | Engineering / operations            |
| What should be logged for diagnostics?                      | Affects testing, privacy, and governance         | Engineering / security / governance |
| What accessibility requirements apply?                      | Affects conversation design and UI               | UX / accessibility                  |
| What review process is required before launch?              | Affects release readiness                        | Product / governance                |

---

## 18. How to Adapt This Sample

Teams adapting this sample should:

1. Replace fictional roles with real roles.
2. Replace sample request types with validated workflow categories.
3. Replace generic service categories with the actual service taxonomy.
4. Define real permission and visibility rules.
5. Validate language handling with localization and operations teams.
6. Define real routing and escalation criteria.
7. Replace sample runtime packet fields with implementation-specific fields.
8. Add real test scenarios and failure cases.
9. Review all assumptions with legal, security, privacy, accessibility, and operational stakeholders.
10. Remove or rewrite any sample assumptions that do not apply.

---

## 19. Related Documents

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
* `public-service-request-feedback-conversation-flows.md`
