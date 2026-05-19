# Language Handling Rules

This document defines language handling rules for the fictional **Public Service Request & Feedback Triage Assistant**.

Language handling affects how the assistant detects input language, responds to users, preserves original wording, translates summaries, handles official terminology, supports multilingual retrieval, and flags ambiguity when language affects routing, escalation, or human review.

---

## 1. Language Handling Overview

**System name:** Public Service Request & Feedback Triage Assistant

**Workflow name:** Public Service Request & Feedback Triage

**Default system language:** English

**Sample supported languages:** English and Arabic for demonstration purposes

**Sample status:** Fictional public-safe architecture example

This sample uses English and Arabic to demonstrate multilingual behavior. A real implementation would need a validated supported-language list, translation quality model, localization policy, and human review process.

---

## 2. Purpose of Language Handling Rules

These language handling rules exist to help the assistant preserve user meaning across languages and avoid language-related workflow errors.

The assistant should not rely only on model default language behavior when language affects:

* request type classification
* service category routing
* complaint handling
* urgency or escalation
* case history retrieval
* user-facing clarity
* internal review
* official terminology
* structured output

---

## 3. Language Handling Goals

The language handling system should:

* detect the user’s input language when possible
* respond in the appropriate language when supported
* preserve original user wording when it may matter for review
* keep official names, service labels, IDs, and case references unchanged when required
* distinguish original user text from translated or normalized summaries
* flag uncertain translation when it affects routing, escalation, or review
* support multilingual retrieval when service catalog or case history may use another language
* avoid treating multilingual input as lower-quality input
* make language-related decisions reviewable

---

## 4. Supported Language Assumptions

| Language        | Support level          | Input supported? | Output supported? | Human review required? | Notes                                                                |
| --------------- | ---------------------- | ---------------- | ----------------- | ---------------------- | -------------------------------------------------------------------- |
| English         | Full sample support    | Yes              | Yes               | Conditional            | Default system language                                              |
| Arabic          | Partial sample support | Yes              | Yes               | Conditional            | Used to demonstrate multilingual intake and terminology preservation |
| Other languages | Not defined in sample  | Conditional      | Conditional       | Yes                    | Real implementation must define support level                        |

Support level notes:

* **Full sample support** means the sample documents include enough behavior rules to demonstrate language handling.
* **Partial sample support** means the assistant can respond and preserve meaning, but some routing, translation, or review scenarios may require human validation.
* **Not defined in sample** means the assistant should not assume reliable handling without additional rules.

---

## 5. Language Detection Rules

| Situation                           | Detection behavior                  | Assistant behavior                                                               | Notes                                   |
| ----------------------------------- | ----------------------------------- | -------------------------------------------------------------------------------- | --------------------------------------- |
| User writes in English              | Detect as English                   | Respond in English unless user requests otherwise                                | Standard path                           |
| User writes in Arabic               | Detect as Arabic                    | Respond in Arabic when supported; preserve original wording                      | May require translated internal summary |
| User mixes English and Arabic       | Detect mixed-language input         | Preserve original wording and respond in the dominant or user-preferred language | Ask if unclear                          |
| User uses transliteration           | Flag possible transliteration       | Ask clarification if location, service, or category is ambiguous                 | Preserve original spelling              |
| Detection confidence is low         | Mark language as uncertain          | Ask a simple language clarification if it affects workflow                       | Do not guess when routing depends on it |
| User explicitly requests a language | Use requested language if supported | Confirm limitation if unsupported                                                | Respect preference when possible        |

Runtime field example:

```yaml
language_context:
  input_language: "Arabic"
  input_language_confidence: "high"
  output_language: "Arabic"
  user_language_preference: "Arabic"
```

---

## 6. Response Language Rules

Default response rule:

```text
Respond in the user’s input language when supported, unless the user requests another supported language or the workflow requires a different language for internal structured output.
```

| Situation                                       | Output language                                                             | Assistant behavior                                         |
| ----------------------------------------------- | --------------------------------------------------------------------------- | ---------------------------------------------------------- |
| User writes in English                          | English                                                                     | Continue in English                                        |
| User writes in Arabic                           | Arabic                                                                      | Continue in Arabic when supported                          |
| User switches language mid-flow                 | User’s most recent clear language preference                                | Preserve prior context and continue in the new language    |
| User-facing response and internal record differ | User language for response; system language for internal record if required | Preserve link between original text and translated summary |
| Unsupported language                            | Safe limitation message or route to review                                  | Avoid pretending full support exists                       |

---

## 7. Original Text Preservation Rules

Original user wording should be preserved when:

* the user submits a complaint
* the user reports harm, safety risk, urgency, or service disruption
* the user describes a repeated unresolved issue
* translation confidence is low or medium
* wording may affect classification or routing
* wording may be needed by a human reviewer
* names, locations, IDs, service labels, or official terms appear
* user tone or dissatisfaction matters for complaint handling

Runtime structure:

```yaml
original_user_text:
  language: "[Detected language]"
  text: "[Original user wording]"
  preservation_reason: "Needed for complaint review, routing, translation accuracy, or case history."
```

If a translated summary is used:

```yaml
translated_summary:
  source_language: "Arabic"
  target_language: "English"
  text: "[Translated summary]"
  translation_confidence: "medium"
  review_required: true
```

---

## 8. Translation Rules

| Translation situation         | Allowed?    | Required?                                 | Human review needed?               | Notes                                                    |
| ----------------------------- | ----------- | ----------------------------------------- | ---------------------------------- | -------------------------------------------------------- |
| User-facing answer            | Yes         | Conditional                               | Conditional                        | Use user’s language when supported                       |
| Internal case summary         | Yes         | Conditional                               | Conditional                        | Preserve original text alongside translation when needed |
| Structured record fields      | Yes         | Conditional                               | Conditional                        | Official labels may remain in system language            |
| Complaint text                | Yes         | Conditional                               | Often conditional                  | Preserve original wording                                |
| Safety or urgency report      | Yes         | Conditional                               | Yes if translation affects urgency | Do not rely on uncertain translation alone               |
| Official service terms        | Conditional | No unless official translated term exists | Conditional                        | Do not invent translations                               |
| Case IDs or reference numbers | No          | No                                        | No                                 | Preserve exactly                                         |

Translation guidance:

* Translate meaning, not only words.
* Preserve original user text when the translation may be reviewed.
* Flag uncertain translation when it affects request type, service category, urgency, escalation, or routing.
* Do not translate official names, service IDs, case IDs, or reference numbers unless an official translated label exists.

---

## 9. Terms Not to Translate

| Term type                       | Example                   | Reason not to translate                  | Display rule                                   |
| ------------------------------- | ------------------------- | ---------------------------------------- | ---------------------------------------------- |
| Case ID                         | `CASE-12345`              | Exact reference required                 | Keep original                                  |
| Service request ID              | `SR-7890`                 | Exact reference required                 | Keep original                                  |
| Department or service team name | `[Official team name]`    | May have official naming standard        | Keep original or show official bilingual label |
| Program or service name         | `[Official service name]` | Avoid invented translation               | Use official label                             |
| User-provided place name        | `[Place name]`            | May affect routing                       | Preserve original and map cautiously           |
| Technical or legal term         | `[Term]`                  | Meaning may change if translated loosely | Use official translation only if available     |

---

## 10. Official Terminology Rules

The assistant should use official terminology for:

* service categories
* request types
* department or team names
* case statuses
* escalation labels
* public service program names
* official forms or service names

The assistant should distinguish between:

* the user’s words
* the assistant’s summary
* the official service category
* the internal routing label

Example mapping:

```yaml
terminology_mapping:
  user_phrase: "the light outside my building is broken"
  official_term: "streets_and_infrastructure.issue_report"
  mapping_confidence: "high"
  requires_confirmation: false
```

If mapping confidence is low:

```yaml
terminology_mapping:
  user_phrase: "[ambiguous term]"
  possible_official_terms:
    - "[Option A]"
    - "[Option B]"
  mapping_confidence: "low"
  requires_confirmation: true
```

---

## 11. Multilingual Retrieval Rules

Retrieval may need to account for user language and source language.

| Retrieval situation                        | Retrieval behavior                                                 | Notes                            |
| ------------------------------------------ | ------------------------------------------------------------------ | -------------------------------- |
| User language matches source language      | Use original query terms                                           | Standard path                    |
| User language differs from source language | Use translated or mapped query terms when allowed                  | Preserve original query          |
| User uses official term                    | Use official term directly                                         | Avoid over-translating           |
| User uses informal wording                 | Map to possible official terms with confidence labels              | Confirm if routing depends on it |
| User uses transliteration                  | Preserve original spelling and search mapped variants when allowed | Ask if ambiguity affects routing |
| Results conflict across languages          | Label conflict and route for review or ask clarification           | Do not silently choose           |

Runtime fields:

```yaml
retrieved_context:
  - source: "service_catalog"
    source_language: "English"
    query_language: "Arabic"
    translated_query_used: true
    translation_confidence: "medium"
    relevance_reason: "Possible match to infrastructure issue category."
```

---

## 12. Transliteration Rules

Transliteration may occur when users write Arabic names, locations, or service terms in Latin characters, or when names have multiple spellings.

| Situation                                  | Assistant behavior                                                             | Confirmation needed? |
| ------------------------------------------ | ------------------------------------------------------------------------------ | -------------------- |
| Location has multiple possible spellings   | Preserve user spelling and ask for confirmation if routing depends on location | Yes if ambiguous     |
| Service name is typed phonetically         | Map cautiously to official service term                                        | Conditional          |
| Case ID contains ambiguous characters      | Ask user to confirm exact ID                                                   | Yes                  |
| Place name maps to multiple official areas | Ask targeted clarification                                                     | Yes                  |

Guidance:

* Preserve original spelling.
* Do not silently normalize user-provided names or locations when routing depends on them.
* Ask for confirmation when ambiguity affects records, location, or service team ownership.

---

## 13. Language Ambiguity Rules

Language ambiguity may affect classification, routing, escalation, or review.

| Ambiguity type                                      | Assistant behavior                                | Review needed?    |
| --------------------------------------------------- | ------------------------------------------------- | ----------------- |
| User term maps to multiple request types            | Ask targeted clarification                        | Conditional       |
| Informal wording may indicate complaint or feedback | Preserve wording and ask if action is requested   | Conditional       |
| Unclear urgency                                     | Ask clarification or escalate based on risk       | Conditional / yes |
| Low-confidence translation                          | Preserve original wording and flag review         | Conditional       |
| Location or service term ambiguous                  | Ask clarification before routing                  | Conditional       |
| Mixed-language input changes meaning                | Preserve full original and confirm interpretation | Conditional       |

Default behavior:

* Ask a targeted clarification question when ambiguity affects the workflow outcome.
* Proceed with a labeled assumption only when allowed.
* Preserve original wording for review.
* Do not silently collapse ambiguous language into a single official category.

---

## 14. User-Facing Explanation Rules

The assistant should explain language-related clarification only when it helps the user understand why a question is needed.

Preferred patterns:

```text
I want to make sure I route this correctly. When you say “[term],” do you mean [option A] or [option B]?
```

```text
I’ll keep your original wording in the request and also include a translated summary for review.
```

```text
This term could map to more than one service category, so I need one clarification before proceeding.
```

Avoid:

* blaming the user’s language
* saying the assistant “does not understand” when a specific ambiguity can be named
* exposing internal routing taxonomy unnecessarily
* over-explaining translation mechanics
* treating non-English input as an exception or inconvenience

---

## 15. Structured Output Language Rules

Structured outputs may need to include both original and translated content.

Recommended structure:

```yaml
language_context:
  original_user_text:
    language: "[Language]"
    text: "[Original text]"
    preserve_for_review: true
  translated_summary:
    language: "[Language]"
    text: "[Translated summary]"
    confidence: "[low | medium | high]"
  official_terms_preserved:
    - "[Term]"
  terminology_mapping:
    user_phrase: "[User wording]"
    official_term: "[Official term]"
    confidence: "[low | medium | high]"
  language_review_required: false
  language_notes:
    - "[Note]"
```

Structured outputs should not erase original wording when original wording matters for review.

---

## 16. Language Review and Escalation Rules

Human language review may be required when:

* translation uncertainty affects routing
* translation uncertainty affects urgency, safety, or escalation
* user wording includes complaint, harm, legal, or sensitive claims
* informal language maps to multiple official service categories
* multilingual retrieval returns conflicting results
* the user disputes the assistant’s interpretation
* original wording must be preserved for reviewer judgment

| Trigger                                    | Assistant behavior                              | Review owner                          | Output required                                   |
| ------------------------------------------ | ----------------------------------------------- | ------------------------------------- | ------------------------------------------------- |
| Low-confidence translation affects routing | Ask clarification or flag review                | Service reviewer or language reviewer | Original text, translated summary, ambiguity note |
| Urgency signal unclear across languages    | Preserve wording and escalate or clarify        | Service operations                    | Escalation summary with language note             |
| Official term unavailable                  | Preserve user term and route for review         | Service taxonomy owner                | Terminology mapping question                      |
| User disputes interpretation               | Update summary and preserve original correction | Reviewer                              | Correction note                                   |

---

## 17. Runtime Language Context Fields

Include language context in runtime packets when language affects behavior, output, retrieval, routing, escalation, or review.

```yaml
language_context:
  input_language: "[Detected language]"
  input_language_confidence: "[low | medium | high]"
  output_language: "[Expected output language]"
  user_language_preference: "[Language if known]"
  translation_required: false
  translated_query_used: false
  preserve_original_user_text: true
  official_terms_preserved:
    - "[Term]"
  terms_not_to_translate:
    - "[Term]"
  uncertain_translation_notes:
    - "[Note]"
  language_review_required: false
```

---

## 18. Language Failure Patterns to Test

Test for these language-related failure patterns:

* assistant responds in the wrong language
* assistant changes user meaning during translation
* assistant fails to preserve original complaint wording
* assistant translates case IDs, service IDs, or official terms incorrectly
* assistant misclassifies request type due to language ambiguity
* assistant routes based on low-confidence translation
* assistant ignores mixed-language or transliterated input
* assistant treats translated retrieval results as equivalent without confidence checks
* assistant fails to flag language review when needed
* assistant exposes restricted information through translated summaries
* assistant omits language metadata from structured output
* assistant asks unnecessary language questions when meaning is already clear

---

## 19. Open Questions

| Question                                                          | Why it matters                                    | Likely owner               | Status |
| ----------------------------------------------------------------- | ------------------------------------------------- | -------------------------- | ------ |
| Which languages are fully supported at launch?                    | Defines response behavior and testing scope       | Product / localization     | Open   |
| Is Arabic fully supported for user-facing and internal workflows? | Affects translation and review needs              | Product / operations       | Open   |
| Which official service terms have approved bilingual labels?      | Prevents invented translations                    | Service taxonomy owner     | Open   |
| When is human language review required?                           | Affects escalation and review workflow            | Operations / policy        | Open   |
| Are translated summaries stored alongside original text?          | Affects records and reviewability                 | Engineering / operations   | Open   |
| What multilingual retrieval strategy is approved?                 | Affects service catalog and knowledge base search | Engineering / localization | Open   |

---

## 20. Related Documents

* `sample-scope-and-assumptions.md`
* `ai-design-principles.md`
* `ai-assistant-behavior-spec.md`
* `public-service-request-feedback-triage-assistant-behavior-spec.md`
* `context-architecture-spec.md`
* `runtime-context-template.md`
* `context-assembly-rules.md`
* `service-category-routing-rules.md`
* `case-history-context-rules.md`
* `example-context-packets.md`
* `testing-diagnostics-spec.md`
* `public-service-request-feedback-conversation-flows.md`
