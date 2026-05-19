# Public Service Request & Feedback Triage Assistant

This folder contains a fictional sample architecture for a workflow-aware AI assistant that supports public service request and feedback intake.

The sample demonstrates how the AI Behavior & Context Architecture Framework can be applied to a practical reasoning-system workflow.

It is intentionally fictional, simplified, and public-safe. It does not represent a real government entity, public service platform, or production implementation.

---

## Sample purpose

The purpose of this sample is to show how behavior specifications, context architecture, runtime context, routing rules, diagnostics, and SD/UX workflow artifacts can work together.

The sample assistant supports users who may want to submit or follow up on:

* a question
* a complaint
* a service request
* an issue report
* a feedback item
* an existing case or prior request

The assistant’s role is not only to respond conversationally. It must help classify the request, identify missing information, preserve user intent, check permitted context, prepare a structured case record, and route the request to the appropriate next step.

---

## Why this sample was chosen

A public service request and feedback workflow is small enough to understand quickly, but complex enough to demonstrate why AI systems need context architecture.

This workflow may require:

* workflow-state awareness
* request type classification
* service category routing
* missing information detection
* role-aware behavior
* permission-aware case history
* multilingual handling
* structured outputs
* escalation logic
* human review readiness
* recovery after interruption

Those conditions make it a useful sample for showing the system around the model.

---

## Folder structure

```text
public-service-request-feedback-triage-assistant/
├── README.md
├── 00-system-overview/
│   ├── ai-design-principles.md
│   └── sample-scope-and-assumptions.md
├── 01-behavioral-specifications/
│   ├── ai-assistant-behavior-spec.md
│   └── public-service-request-feedback-triage-assistant-behavior-spec.md
├── 02-context-architecture/
│   ├── context-architecture-spec.md
│   ├── runtime-context-template.md
│   ├── context-assembly-rules.md
│   ├── language-handling-rules.md
│   ├── service-category-routing-rules.md
│   ├── case-history-context-rules.md
│   └── example-context-packets.md
├── 03-testing-diagnostics/
│   └── testing-diagnostics-spec.md
└── 04-sd-ux-workflow/
    └── public-service-request-feedback-conversation-flows.md
```

---

## How to read this sample

Suggested reading order:

1. `README.md`
2. `00-system-overview/sample-scope-and-assumptions.md`
3. `00-system-overview/ai-design-principles.md`
4. `01-behavioral-specifications/ai-assistant-behavior-spec.md`
5. `01-behavioral-specifications/public-service-request-feedback-triage-assistant-behavior-spec.md`
6. `02-context-architecture/context-architecture-spec.md`
7. `02-context-architecture/runtime-context-template.md`
8. `02-context-architecture/context-assembly-rules.md`
9. `02-context-architecture/language-handling-rules.md`
10. `02-context-architecture/service-category-routing-rules.md`
11. `02-context-architecture/case-history-context-rules.md`
12. `02-context-architecture/example-context-packets.md`
13. `03-testing-diagnostics/testing-diagnostics-spec.md`
14. `04-sd-ux-workflow/public-service-request-feedback-conversation-flows.md`

The documents are designed to be read together. Each file covers a different layer of the same fictional AI system.

---

## Sample workflow summary

A user enters the public service request and feedback flow.

The assistant helps determine whether the user is trying to:

* ask a question
* file a complaint
* report an issue
* submit feedback
* request a service
* follow up on an existing case

The assistant then:

1. identifies the likely request type
2. checks what information is already available
3. asks only for missing information needed to proceed
4. applies language handling rules when needed
5. checks permitted case history or related records when appropriate
6. prepares a structured case record
7. recommends routing or escalation
8. supports user review before submission or handoff

---

## Key architecture questions this sample demonstrates

This sample is designed to help teams inspect questions such as:

* What does the model need to know at this moment to behave correctly?
* Which context should always be included?
* Which context depends on workflow state?
* Which context depends on user role or permission scope?
* What can the assistant infer safely?
* What must be confirmed by the user?
* When should case history be retrieved?
* What related-case information can be shown to the user?
* How should multilingual input be handled?
* What information should be preserved for human review?
* What makes a routing decision reviewable?
* How should failures be diagnosed by layer?

---

## Important assumptions

This sample assumes a fictional public service environment with:

* a public-facing service portal
* a service catalog
* a case management system
* basic user roles
* multilingual user input
* internal service teams
* human review and escalation paths
* permission rules for case history

The sample does not define a real policy environment, legal framework, security model, service taxonomy, or production data architecture.

Those would need to be defined by the implementing organization.

---

## What this sample is not

This sample is not:

* a production-ready implementation
* a legal or compliance recommendation
* a complete government service design
* a complete security or privacy model
* a complete multilingual service policy
* a complete case management design
* a substitute for stakeholder review

It is a documentation pattern for making AI behavior and context architecture more explicit and reviewable.

---

## License

This sample is part of the AI Behavior & Context Architecture Framework repository and is licensed under CC BY 4.0 unless otherwise noted.

Suggested attribution:

> “AI Behavior & Context Architecture Framework” by Chrys Li / 7modes, licensed under CC BY 4.0.
