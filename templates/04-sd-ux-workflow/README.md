SD/UX Workflow Context
This folder contains service design, UX, workflow, and interaction artifacts that help define the user-facing and operational context around the AI system.

Why this note matters
The context architecture needs inputs from service design / UX / workflow design. Otherwise the AI docs can become too abstract.

This folder can hold artifacts that answer:

What is the user trying to do?
Where does the AI enter the workflow?
What happened before the AI interaction?
What happens after?
What information is visible on screen?
What fields does the user already complete?
What system state exists before the model is called?
What handoffs happen between AI, user, internal team, and backend system?
Where do interruptions, errors, escalations, and recovery paths happen?

That is useful to both:

- developers, because it clarifies system behavior and integration points
-LLMs, because it provides workflow state, user intent, interface context, and handoff logic
Content that could live in this folder

Examples:

04-sd-ux-workflow/
├── README.md
├── conversation-flows-template.md
├── user-journey-map.md
├── service-blueprint.md
├── workflow-state-map.md
├── screen-flow-notes.md
├── form-field-inventory.md
├── handoff-map.md
├── error-and-recovery-flows.md
├── escalation-flow.md
├── accessibility-notes.md
├── localization-notes.md
└── screenshots/
