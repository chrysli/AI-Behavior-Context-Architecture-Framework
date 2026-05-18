# AI Behavior & Context Architecture Framework

A practical framework for designing the system around an AI model: behavior specifications, runtime context, workflow state, retrieval rules, diagnostics, and review layers.

This repository contains reusable documentation templates and a fictional sample architecture for a workflow-aware AI system.

The goal is to help teams move from prompt-only implementation toward explicit, testable AI behavior and context architecture.

---

## Why this framework exists

AI behavior is shaped by more than the prompt.

A model’s response depends on the operating conditions around it, including:

* what context it receives
* what context it can retrieve
* what workflow state it can see
* what user role it is responding to
* what data boundaries apply
* what language rules are active
* what output structure is expected
* what uncertainty it is allowed to surface
* what it should do when information is missing
* how the interaction recovers after interruption

When those conditions matter, the design problem becomes architectural.

The purpose of this framework is to make those operating conditions explicit enough to design, test, review, and improve.

---

## What this repository includes

This repository has two main parts:

1. **Templates**
   Empty-ish reusable documents that teams can adapt to their own AI-enabled workflows.

2. **Sample architectures**
   Filled fictional examples showing how the same framework can be applied to different AI-enabled workflows.

The first sample architecture is a **Public Service Request & Feedback Triage Assistant**. Future examples can use the same folder structure with different workflow contexts.

The sample is intentionally fictional and simplified. It is designed to demonstrate architecture patterns, not to represent a real government entity, public service platform, or production-ready implementation.

---

## Repository structure

```text
ai-behavior-context-architecture-framework/
├── README.md
├── assets/
│   └── ai_behavior_and_context_framework_diagram.png
├── templates/
│   ├── 00-system-overview/
│   │   └── ai-design-principles-template.md
│   ├── 01-behavioral-specifications/
│   │   ├── ai-assistant-behavior-spec-template.md
│   │   └── workflow-assistant-behavior-spec-template.md
│   ├── 02-context-architecture/
│   │   ├── context-architecture-spec-template.md
│   │   ├── runtime-context-template.md
│   │   ├── context-assembly-rules-template.md
│   │   ├── language-handling-rules-template.md
│   │   ├── workflow-routing-rules-template.md
│   │   ├── related-record-context-rules-template.md
│   │   └── example-context-packets-template.md
│   ├── 03-testing-diagnostics/
│   │   └── testing-diagnostics-spec-template.md
│   └── 04-sd-ux-workflow/
│       └── conversation-flows-template.md
└── examples/
    ├── public-service-request-feedback-triage-assistant/
    │   ├── 00-system-overview/
    │   │   ├── ai-design-principles.md
    │   │   └── sample-scope-and-assumptions.md
    │   ├── 01-behavioral-specifications/
    │   │   ├── ai-assistant-behavior-spec.md
    │   │   └── public-service-request-feedback-triage-assistant-behavior-spec.md
    │   ├── 02-context-architecture/
    │   │   ├── context-architecture-spec.md
    │   │   ├── runtime-context-template.md
    │   │   ├── context-assembly-rules.md
    │   │   ├── language-handling-rules.md
    │   │   ├── service-category-routing-rules.md
    │   │   ├── case-history-context-rules.md
    │   │   └── example-context-packets.md
    │   ├── 03-testing-diagnostics/
    │   │   └── testing-diagnostics-spec.md
    │   └── 04-sd-ux-workflow/
    │       └── public-service-request-feedback-conversation-flows.md
    ├── future-example-name/
    │   ├── 00-system-overview/
    │   ├── 01-behavioral-specifications/
    │   ├── 02-context-architecture/
    │   ├── 03-testing-diagnostics/
    │   └── 04-sd-ux-workflow/
    └── another-future-example-name/
        ├── 00-system-overview/
        ├── 01-behavioral-specifications/
        ├── 02-context-architecture/
        ├── 03-testing-diagnostics/
        └── 04-sd-ux-workflow/
```

---

## Framework layers

### 00-system-overview

Defines the purpose of the AI system and the principles that should guide its behavior.

This layer should answer:

* What is this AI system for?
* What should it improve?
* What should it protect?
* What principles should guide behavior across workflows?
* What assumptions need to be made visible before implementation?

### 01-behavioral-specifications

Defines how the assistant should behave in general and within specific workflows.

This layer should answer:

* What is the assistant responsible for?
* What is outside its role?
* When should it ask questions?
* When should it infer?
* When should it retrieve information?
* When should it escalate?
* How should it handle incomplete input?
* How should it preserve user intent?

### 02-context-architecture

Defines what context exists, where it comes from, when it is included, and how the model should interpret it.

This layer should answer:

* What context is always included?
* What context depends on workflow state?
* What context depends on user role?
* What context is retrieved dynamically?
* What context is inferred from the current interaction?
* What context is restricted from model access?
* How should context be labeled?
* How are conflicts handled?
* How are multilingual inputs handled?

### 03-testing-diagnostics

Defines how AI behavior will be tested, diagnosed, and improved.

This layer should answer:

* Did the system behave correctly given the context it received?
* Did the model use retrieved context appropriately?
* Did the assistant stay aligned to workflow state?
* Did the assistant ask unnecessary questions?
* Did it expose restricted information?
* Did it hallucinate workflow rules, record details, or escalation logic?
* Which layer failed: prompt, retrieval, packet assembly, workflow state, permissions, output schema, or model behavior?

### 04-sd-ux-workflow

Connects AI behavior to the user experience, service design, and workflow design.

This layer should answer:

* Where does the AI interaction begin?
* What are the conversation stages?
* What questions should the assistant ask?
* When does the user review or confirm information?
* What happens after interruption?
* What handoffs occur?
* What structured outputs are produced?
* How does the AI interaction support the service or product workflow?

---

## Sample architectures

The `examples/` folder is designed to hold multiple filled sample architectures.

Each example should use the same core folder structure:

```text
example-name/
├── 00-system-overview/
├── 01-behavioral-specifications/
├── 02-context-architecture/
├── 03-testing-diagnostics/
└── 04-sd-ux-workflow/
```

This makes it easier to compare how the framework changes across different workflow contexts while keeping the documentation pattern consistent.

### Current sample: Public Service Request & Feedback Triage Assistant

The first sample architecture demonstrates how the framework can be applied to a fictional workflow-aware AI assistant.

The assistant helps a resident, business owner, or internal service team describe and submit:

* a question
* a complaint
* a service request
* an issue report
* a feedback item
* a follow-up on an existing case

The system may need to:

* identify missing information
* classify the request by type and service category
* preserve the user’s original intent
* check related case history when permitted
* apply language handling rules
* prepare a structured case record
* route the case to the appropriate service team
* flag escalation conditions
* support review by a human service team

This sample is meant to be small enough to understand and complex enough to show why behavior and context architecture matter.

---

## How to use this repository

### For product and engineering teams

Use the templates to define the operating environment around your AI system before relying on model behavior.

Start with:

1. system purpose and design principles
2. assistant behavior specifications
3. runtime context needs
4. context assembly rules
5. workflow and UX flows
6. testing and diagnostic scenarios

### For UX, service design, and system design teams

Use the framework to connect AI behavior to the actual user journey, service process, workflow state, and handoff model.

The goal is not only to design an interface. The goal is to define the conditions that allow the assistant to behave correctly inside the workflow.

### For AI strategy, governance, and transformation teams

Use the framework to make AI behavior more reviewable.

The documents can help stakeholders inspect:

* what the AI is expected to do
* what context it uses
* what boundaries apply
* what risks need testing
* what decisions still need human review

---

## Suggested starting point

If you are new to the framework, start with the sample architecture first.

Read these files in order:

1. `examples/public-service-request-feedback-triage-assistant/00-system-overview/sample-scope-and-assumptions.md`
2. `examples/public-service-request-feedback-triage-assistant/00-system-overview/ai-design-principles.md`
3. `examples/public-service-request-feedback-triage-assistant/01-behavioral-specifications/public-service-request-feedback-triage-assistant-behavior-spec.md`
4. `examples/public-service-request-feedback-triage-assistant/02-context-architecture/runtime-context-template.md`
5. `examples/public-service-request-feedback-triage-assistant/03-testing-diagnostics/testing-diagnostics-spec.md`

Then use the templates to create your own version.

---

## Important notes

This framework is not a substitute for legal, security, accessibility, privacy, or compliance review.

For production systems, teams should validate assumptions with the appropriate product, engineering, data, policy, legal, security, service operations, and stakeholder groups.

The sample files are intentionally incomplete in the way real early-stage architecture documents are incomplete. Open questions and assumptions are part of the work.

Future examples can reuse the same structure with different workflow domains, such as internal knowledge support, policy review, grant intake, industrial challenge triage, or product feedback operations.

---

## Companion article

This repository accompanies the article:

**The System Around the Model: Building an AI Behavior & Context Architecture Framework**

The article explains the reasoning behind the framework. This repository provides the reusable structure and sample documentation.

---

## Status

Work in progress.

Initial structure and sample files are being developed.

---

## License

Documentation, templates, diagrams, and sample architecture files in this repository are licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0), unless otherwise noted.

You may share and adapt the material, including for commercial use, with attribution.

Suggested attribution:

> “AI Behavior & Context Architecture Framework” by Chrys Li / 7modes, licensed under CC BY 4.0.

If code is added to this repository later, it may be licensed separately under a software license such as MIT or Apache 2.0.