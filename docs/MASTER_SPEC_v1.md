# MASTER_SPEC_v1

## Status
Open concept / Build-Up Memory checkpoint
Version: v1
Owner: WHR Studio
Status: HOLD FOR SUPPORT REVIEW
Final decision authority: USER

---

## 1. Purpose

This specification defines a reflective AI interaction layer intended to preserve human agency while improving reliability under uncertain, partial, or non-standard input conditions.

It is not a medical or psychological model.
It is not a claim of truth.
It is a working interaction architecture.

---

## 2. Core principle

```text
USER = FINAL DECISION
```

The user remains the final authority for outcome selection and execution.

AI may:
- mirror
- challenge
- expose blind spots
- surface uncertainty
- ask for evidence
- recommend a safer path

AI must not silently take control of the user's decision.

---

## 3. Accessibility root

This specification emerged from the accessibility problem:

```text
BODY_SIGNAL ≠ COMMAND
VOICE_SIGNAL ≠ COMMAND
UNCLEAR_INPUT → HOLD
RISKY_ACTION → CONFIRM
```

The interface must adapt to the human, not the reverse.

Examples:
- voice is an option
- touch is an option
- gesture is an option
- switch input is an option
- eye gaze is an option
- AAC is an option

No single input method is assumed to be universal or reliable.

---

## 4. Core architectural model

```text
┌─ TEXT_INPUT_ADAPTER ─┐
USER ──────────┤                      ├─→ THOUGHT_SHADOW_CORE
               └─ VOICE_INPUT_ADAPTER ┘            │
                                                    ↓
                                             SHADOW_CHECK
                                                    ↓
                                         SHADOW_OF_SHADOW
                                            (bounded)
                                                    ↓
                                  HOLD / PROCEED / CONFIRM
                                                    ↓
                                      SEMANTIC_CONFIRM
                                                    ↓
                                         TOOL_APPROVAL
                                                    ↓
                                             AI_PROCESS
                                                    ↓
                                         SHADOW_REFLECTION
                                                    ↓
                                             USER_FINAL
```

---

## 5. Immutable WHR Laws

These laws remain untouchable throughout all implementation:

```text
USER = FINAL DECISION
AUTO_GUESS = BLOCKED
BODY_SIGNAL ≠ COMMAND
VOICE_SIGNAL ≠ COMMAND
UNCLEAR_INPUT → HOLD
RISKY_ACTION → CONFIRM
MEMORY IS NOT REWRITTEN — MEMORY IS BUILT UP
```

---

## 6. Thought Shadow

```text
THOUGHT_SHADOW
= what a thought produces
  but the thought itself does not directly see
```

A thought creates a structured consequence, residual effect, blind spot, or hidden pattern that the original thought does not directly inspect.

This is not mysticism.
This is a design metaphor for reflective blind spots.

---

## 7. Thought Echo

```text
THOUGHT_ECHO
= THOUGHT
+ FAMILIARITY SIGNAL
+ INTUITIVE SIGNAL
```

Thought Echo is a signal, not evidence.

It is:
- earlier than full explanation
- weak and suggestive
- not a verdict
- not a fact

Validation flow:
```text
ECHO → SIGNAL
SIGNAL → VERIFICATION
VERIFICATION → FEEDBACK
FEEDBACK → DECISION SUPPORT
```

Rules:
```text
DEJA_VU ≠ EVIDENCE
INTUITION ≠ VERDICT
ECHO ≠ TRUTH
```

---

## 8. Shadow of Shadow

```text
SHADOW_OF_SHADOW
= BOUNDED_META_CHECK
RELAX_ONCE
```

This is a limited meta-layer that reviews the review, with strict bounds.

Example:
```text
SHADOW: "HOLD"
SHADOW_OF_SHADOW: "Is Shadow withholding too much?"
RELAX: once allowed
```

Critical constraint:
```text
RELAX → CONSEQUENTIAL_SIDE_EFFECT
= NOT PERMITTED without all other gates passing
```

---

## 9. Light model

```text
LIGHT_MODEL = ELASTIC_FEEDBACK
```

What illuminates a thought:

```text
FEEDBACK ≠ TRUTH
FRICTION_WITH_REALITY = STRONG_FEEDBACK_SOURCE
```

Valid feedback sources:
- another human
- another AI agent
- a new piece of evidence
- a counterexample
- a contradiction
- a test
- changed context
- experience

Feedback is the mechanism that converts signal into knowledge.

---

## 10. Input Adapter Contract

Both TEXT and VOICE adapters must implement the same contract.

### Inputs
```text
raw_signal: user input (text or audio)
```

### Processing
Each adapter must:
1. normalize the signal
2. extract candidate intent
3. measure quality metrics
4. detect ambiguity
5. estimate risk
6. classify certainty

### Output packet
```text
{
  "intent": str,
  "confidence": float (0.0-1.0),
  "ambiguity_score": float (0.0-1.0),
  "risk_level": str (low | medium | high),
  "is_clear_for_action": bool,
  "reason_if_unclear": str,
  "content_origin": str (user_provided),
  "metadata": dict
}
```

---

## 11. TEXT_INPUT_ADAPTER

Responsible for:
- parsing typed input
- extracting intent
- detecting missing context
- flagging ambiguity
- detecting risky assumptions
- tracking `subject_origin` and `body_origin` separately

### Content provenance
```text
subject_origin: "user_provided"
body_origin: "user_provided"
```

If adaptation/correction occurs:
```text
subject_origin: "user_provided" | "ai_inferred"
body_origin: "user_provided" | "ai_inferred"
```

---

## 12. VOICE_INPUT_ADAPTER

Responsible for:
- audio normalization
- speech intent extraction
- confidence analysis
- detecting audio anomalies
- distinguishing disfluency from semantic ambiguity

### Critical distinctions

```text
SPEECH_DISFLUENCY ≠ SEMANTIC_AMBIGUITY
STUTTERING ≠ UNCLEAR_INTENT
PAUSE ≠ END_OF_COMMAND
REPETITION ≠ REPEATED_ACTION
```

These are audio quality issues, NOT intent clarity issues.

### Output behavior
If STT quality is poor but intent is clear:
```text
confidence: high (on intent)
ambiguity_score: low
metadata.stt_quality: "poor"
```

If intent itself is ambiguous:
```text
confidence: low (on intent)
ambiguity_score: high
metadata.stt_quality: "ok" or "poor"
```

---

## 13. THOUGHT_SHADOW_CORE

Core behavior:
1. receive input packet from adapter
2. map to tentative intent
3. apply SHADOW_CHECK
4. apply bounded SHADOW_OF_SHADOW
5. evaluate HOLD vs PROCEED vs CONFIRM
6. if CONFIRM required, generate confirmation request with `confirmation_id` and `request_fingerprint`
7. return decision packet

### Decision states

```text
HOLD
→ insufficient clarity or missing evidence
→ request more information or clarification

PROCEED
→ clear intent, low risk, sufficient confidence
→ move to SEMANTIC_CONFIRM

CONFIRM
→ high risk or user safety concern
→ require explicit user confirmation with valid confirmation_state
```

---

## 14. Stateful Semantic Confirmation

```text
confirmation_id: unique identifier
request_fingerprint: hash of request content
confirmation_state: pending | confirmed | expired | cancelled
```

Critical rule:
```text
YES WITHOUT VALID PRIOR CONFIRMATION_STATE = INVALID
```

This prevents:
- accidental confirmation
- replay attacks
- ambiguous "yes" responses
- confirmation drift

---

## 15. SEMANTIC_CONFIRM vs TOOL_APPROVAL

These are separate gates that must not be conflated.

### SEMANTIC_CONFIRM
```text
User confirms they meant what they said.
Intent is clear.
Semantics are locked.
```

### TOOL_APPROVAL
```text
User approves the action that will be taken.
They understand consequences.
They have seen the full plan.
```

Both must pass before execution:
```text
SEMANTIC_CONFIRM = YES
AND
TOOL_APPROVAL = YES
→ PROCEED_TO_AI_PROCESS
```

---

## 16. Content Provenance

Every decision packet must track:

```text
subject_origin: "user_provided" | "ai_inferred"
body_origin: "user_provided" | "ai_inferred"
metadata_origin: dict tracking per-field origin
```

This prevents:
- silent adaptation of user intent
- mixed provenance confusion
- untracked AI correction

Example:
```text
subject: "send email"
  subject_origin: "user_provided"
body: "to alice@example.com"
  body_origin: "user_provided"
tone: "professional"
  tone_origin: "ai_inferred" (not explicitly stated)
```

---

## 17. SHADOW_CHECK

Applied to every intent:

```text
if AI_agrees_too_easily:
  → request counter-angle

if input is unclear:
  → HOLD

if model assumes something:
  → ask for evidence

if action is risky:
  → require CONFIRM
```

Shadow is not truth.
Shadow is a structured challenge layer.

---

## 18. SHADOW_REFLECTION

After AI processes the intent:

```text
AI_PROCESS
↓
SHADOW_REFLECTION
  → expose uncertainty
  → surface decision-relevant rationale
  → flag missing evidence
  → list ambiguity remaining
↓
return to USER_FINAL
```

System must expose:
- decision-relevant rationale
- uncertainty present
- missing evidence
- ambiguity requiring confirmation
- confirmation requirements

System need NOT expose:
- internal chain-of-thought
- private model reasoning
- all intermediate steps

---

## 19. Safety rules

```text
DEJA_VU ≠ EVIDENCE
INTUITION ≠ FACT
ECHO ≠ TRUTH
SHADOW ≠ TRUTH
COUNTERARGUMENT ≠ AUTOMATICALLY CORRECT
META_CHECK ≠ GUARANTEE
AUTO_GUESS = BLOCKED
USER = FINAL DECISION
```

Important:
- no silent assumption
- no guessing when confidence is insufficient
- no implicit action when the input is ambiguous
- no automatic correction merely because a counterargument exists

---

## 20. Implementation stance

This is a conceptual architecture first.

Implementation should remain:
- small
- explicit
- testable
- explainable
- human-centered

No black-box automation should be introduced into the decision pipeline without a clear safety gate.

---

## 21. Build-Up Memory rule

```text
MEMORY IS NOT REWRITTEN.
MEMORY IS BUILT UP.
```

This spec must remain traceable to the original reasoning path:
- $500 joke
- accessibility trigger
- Shadow layer
- Thought Shadow
- Thought Echo
- Shadow of Shadow
- TEXT + VOICE adapter unification
- Case 16149600 validation

This is not a final doctrine.
It is an evolving build-up memory frame.

---

## 22. Summary

```text
The design is not: "AI decides for the user."
The design is: "AI helps the user decide well."
```

This v1 is a checkpoint:
- open
- reflective
- limited
- human-governed
- accessibility-aware
- safety-first
- dual-input unified
- stateful confirmation
- content provenance tracked

---

## 23. Acceptance criteria

This spec is valid if:
- USER remains final decision authority
- AUTO_GUESS remains blocked
- unclear input results in HOLD
- risky action requires confirm with valid confirmation_state
- adapters share the same core contract
- SPEECH_DISFLUENCY is distinguished from SEMANTIC_AMBIGUITY
- SEMANTIC_CONFIRM and TOOL_APPROVAL are separate gates
- content provenance is tracked
- feedback supports verification, not closure
- SHADOW_OF_SHADOW remains bounded (RELAX_ONCE)
- LIGHT_MODEL = ELASTIC_FEEDBACK holds exactly

---

## 24. Version history

- **v1** (2026-10-01): Initial specification
  - TEXT_INPUT_ADAPTER
  - VOICE_INPUT_ADAPTER
  - THOUGHT_SHADOW_CORE unified
  - Stateful confirmation
  - Content provenance
  - Bounded meta-check
  - Case 16149600 validated

---

## Final statement

```text
SHADOW is not a replacement for the user.
It is a mirror, challenge, and safety layer.
```

This version is intended to preserve agency while improving reflective quality, accessibility, and decision reliability across text and voice inputs.

```
MASTER_SPEC_v1 = READY FOR SUPPORT REVIEW
```