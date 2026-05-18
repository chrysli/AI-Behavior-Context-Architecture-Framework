# AI Design Principles Template

Use this document to define the operating principles that should guide AI behavior across the system.

These principles should be specific enough to influence design, implementation, testing, and review. They should not be abstract brand values or generic AI ethics statements.

---

## 1. System Name

**System name:** `[Insert AI system or workflow name]`

**Primary workflow:** `[Insert workflow this AI system supports]`

**Primary users:** `[Insert user groups or roles]`

**Document owner:** `[Insert owner/team]`

**Last updated:** `[Insert date]`

---

## 2. Purpose of the AI System

Describe why this AI system exists.

The purpose should explain what the AI is intended to improve, reduce, clarify, accelerate, support, or protect.

```text
This AI system exists to...
```

Example prompts:

* What user or workflow problem does the AI address?
* What should become easier, clearer, faster, or more reliable?
* What should the AI help humans avoid missing?
* What should remain under human control?

---

## 3. Behavioral Design Principles

Define the principles that should shape AI behavior across the workflow.

Each principle should include:

* principle statement
* why it matters
* expected behavior
* failure pattern to avoid
* how it can be tested

---

### Principle 1: `[Insert principle name]`

**Statement**

```text
[Write the principle as a clear behavioral expectation.]
```

**Why this matters**

```text
[Explain why this principle matters for the user, workflow, organization, or system.]
```

**Expected behavior**

The AI should:

* `[Expected behavior]`
* `[Expected behavior]`
* `[Expected behavior]`

**Failure patterns to avoid**

The AI should avoid:

* `[Failure pattern]`
* `[Failure pattern]`
* `[Failure pattern]`

**How this can be tested**

Test by checking whether the AI:

* `[Test condition]`
* `[Test condition]`
* `[Test condition]`

---

### Principle 2: `[Insert principle name]`

**Statement**

```text
[Write the principle as a clear behavioral expectation.]
```

**Why this matters**

```text
[Explain why this principle matters for the user, workflow, organization, or system.]
```

**Expected behavior**

The AI should:

* `[Expected behavior]`
* `[Expected behavior]`
* `[Expected behavior]`

**Failure patterns to avoid**

The AI should avoid:

* `[Failure pattern]`
* `[Failure pattern]`
* `[Failure pattern]`

**How this can be tested**

Test by checking whether the AI:

* `[Test condition]`
* `[Test condition]`
* `[Test condition]`

---

### Principle 3: `[Insert principle name]`

**Statement**

```text
[Write the principle as a clear behavioral expectation.]
```

**Why this matters**

```text
[Explain why this principle matters for the user, workflow, organization, or system.]
```

**Expected behavior**

The AI should:

* `[Expected behavior]`
* `[Expected behavior]`
* `[Expected behavior]`

**Failure patterns to avoid**

The AI should avoid:

* `[Failure pattern]`
* `[Failure pattern]`
* `[Failure pattern]`

**How this can be tested**

Test by checking whether the AI:

* `[Test condition]`
* `[Test condition]`
* `[Test condition]`

---

## 4. Suggested Principle Categories

Use these categories as prompts. Not every system needs every category.

### User effort

How should the AI reduce effort without removing important user control?

Possible principle:

```text
The AI should reduce avoidable user effort while keeping important decisions visible to the user.
```

### User intent

How should the AI preserve what the user meant when clarifying, restructuring, summarizing, or translating input?

Possible principle:

```text
The AI should preserve the user’s original intent before improving structure, wording, or format.
```

### Context use

How should the AI distinguish between known facts, retrieved information, inferred context, and uncertainty?

Possible principle:

```text
The AI should treat known facts, retrieved information, inferred context, and uncertainty as separate context types.
```

### Question asking

When should the AI ask questions, and when should it safely proceed with available context?

Possible principle:

```text
The AI should ask fewer, better questions and only request information that affects the workflow outcome.
```

### Workflow alignment

How should the AI stay aligned to the current workflow step?

Possible principle:

```text
The AI should adapt its behavior to the user’s current workflow state rather than responding as a generic assistant.
```

### Permissions and boundaries

How should the AI handle restricted information, role boundaries, and tenant boundaries?

Possible principle:

```text
The AI should protect restricted information even when disclosure would make the interaction feel more convenient.
```

### Uncertainty

How should the AI communicate uncertainty without becoming vague or unhelpful?

Possible principle:

```text
The AI should surface uncertainty when it affects the user’s decision, routing, eligibility, or next action.
```

### Recovery

How should the AI recover when the user changes direction, interrupts, corrects information, or resumes later?

Possible principle:

```text
The AI should support interruption and recovery without forcing the user to restart the workflow.
```

---

## 5. Principle Review Checklist

Before approving these principles, check whether each principle is:

* specific enough to guide behavior
* relevant to the actual workflow
* testable through scenarios or outputs
* connected to context architecture
* connected to UX or service workflow decisions
* clear enough for product, design, engineering, and review teams
* not just a generic value statement

---

## 6. Open Questions

List unresolved decisions or assumptions.

| Question     | Why it matters | Owner     | Status                        |
| ------------ | -------------- | --------- | ----------------------------- |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |
| `[Question]` | `[Impact]`     | `[Owner]` | `[Open / Pending / Resolved]` |

---

## 7. Change Log

| Date     | Change          | Owner     |
| -------- | --------------- | --------- |
| `[Date]` | `[Change made]` | `[Owner]` |
