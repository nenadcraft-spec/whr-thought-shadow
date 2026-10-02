MASTER_SPEC_v1

Status

Open concept / Build-Up Memory checkpoint
Version: v1
Owner: WHR Studio
Status: HOLD FOR SUPPORT REVIEW
Final decision authority: USER



1. Purpose

This specification defines a reflective AI interaction layer intended to preserve human agency while improving reliability under uncertain, partial, or non-standard input conditions.


It is not a medical or psychological model.
It is not a claim of truth.
It is a working interaction architecture.



2. Core principle

USER = FINAL DECISION

The user remains the final authority for outcome selection and execution.


AI may:



mirror

challenge

expose blind spots

surface uncertainty

ask for evidence

recommend a safer path


AI must not silently take control of the user's decision.



3. Accessibility root

This specification emerged from the accessibility problem:


BODY_SIGNAL ≠ COMMAND
VOICE_SIGNAL ≠ COMMAND
UNCLEAR_INPUT → HOLD
RISKY_ACTION → SEMANTIC_CONFIRM_REQUIRED

The interface must adapt to the human, not the reverse.


Examples:



voice is an option

touch is an option

gesture is an option

switch input is an option

eye gaze is an option

AAC is an option


No single input method is assumed to be universal or reliable.



4. Core architectural model

┌─ TEXT_INPUT_ADAPTER ─┐
USER / NEW_INPUT ─┤                      ├─→ THOUGHT_SHADOW_CORE
                  └─ VOICE_INPUT_ADAPTER ┘            │
                                                      ↓
                                               SHADOW_CHECK
                                                      ↓
                                           SHADOW_OF_SHADOW
                                              (bounded)
                                                      ↓
                                               DECISION
                                      /               |                \
                                   HOLD            PROCEED      CONSEQUENTIAL
                                    │             (NON_SE)        SIDE_EFFECT
                                    │                │                 │
                                    ↓                ↓                 ↓
                          USER_CLARIFICATION     AI_PROCESS   SEMANTIC_CONFIRM_REQUIRED
                                    │                │                 │
                                    ↓                │                 ↓
                                 NEW_INPUT           │           TOOL_APPROVAL
                                    │                │                 │
                                    └───────→ INPUT_ADAPTERS           ↓
                                                   │              AI_PROCESS
                                                   │                 │
                                                   └─────────┬───────┘
                                                             ↓
                                                    SHADOW_REFLECTION
                                                             ↓
                                                        USER_FINAL

Critical loop rule:


HOLD
→ USER_CLARIFICATION
→ NEW_INPUT
→ INPUT_ADAPTERS
→ THOUGHT_SHADOW_CORE

A clarification must re-enter the normal adapter/core path.


It must not bypass the Thought Shadow core and jump directly to AI_PROCESS.



5. Immutable WHR Laws

These laws remain untouchable throughout all implementation:


USER = FINAL DECISION
AUTO_GUESS = BLOCKED
BODY_SIGNAL ≠ COMMAND
VOICE_SIGNAL ≠ COMMAND
UNCLEAR_INPUT → HOLD
RISKY_ACTION → SEMANTIC_CONFIRM_REQUIRED
MEMORY IS NOT REWRITTEN — MEMORY IS BUILT UP


6. Thought Shadow

THOUGHT_SHADOW
= what a thought produces
  but the thought itself does not directly see

A thought creates a structured consequence, residual effect, blind spot, or hidden pattern that the original thought does not directly inspect.


This is not mysticism.
This is a design metaphor for reflective blind spots.



7. Thought Echo

THOUGHT_ECHO
= THOUGHT
+ FAMILIARITY SIGNAL
+ INTUITIVE SIGNAL

Thought Echo is a signal, not evidence.


It is:



earlier than full explanation

weak and suggestive

not a verdict

not a fact


Validation flow:


ECHO → SIGNAL
SIGNAL → VERIFICATION
VERIFICATION → FEEDBACK
FEEDBACK → DECISION SUPPORT

Rules:


DEJA_VU ≠ EVIDENCE
INTUITION ≠ VERDICT
ECHO ≠ TRUTH


8. Shadow of Shadow

SHADOW_OF_SHADOW
= BOUNDED_META_CHECK
RELAX_ONCE

This is a limited meta-layer that reviews the review, with strict bounds.


Example:


SHADOW: "HOLD"
SHADOW_OF_SHADOW: "Is Shadow withholding too much?"
RELAX: once allowed

Critical constraint:


RELAX → CONSEQUENTIAL_SIDE_EFFECT
= NOT PERMITTED without all other gates passing


9. Light model

LIGHT_MODEL = ELASTIC_FEEDBACK

What illuminates a thought:


FEEDBACK ≠ TRUTH
FRICTION_WITH_REALITY = STRONG_FEEDBACK_SOURCE

Valid feedback sources:



another human

another AI agent

a new piece of evidence

a counterexample

a contradiction

a test

changed context

experience


Feedback is the mechanism that converts signal into knowledge.



10. Input Adapter Contract

Both TEXT and VOICE adapters must implement the same core contract.


Inputs

raw_signal: user input (text or audio)

Processing

Each adapter must:



normalize the signal

extract candidate intent

measure quality metrics

detect ambiguity

estimate risk

classify certainty

preserve field-level provenance


Output packet

{
  "intent": str,
  "confidence": float (0.0-1.0),
  "ambiguity_score": float (0.0-1.0),
  "risk_level": str (low | medium | high),
  "is_clear_for_action": bool,
  "reason_if_unclear": str,
  "provenance": {
    "<field_name>": "USER_PROVIDED_CONTENT | AI_GENERATED_DRAFT_CONTENT"
  },
  "metadata": dict
}

Critical provenance rule:


FIELD_LEVEL_PROVENANCE = AUTHORITATIVE

A single document-level content_origin value is not sufficient when user-provided and AI-generated content coexist in the same packet.



11. TEXT_INPUT_ADAPTER

Responsible for:



parsing typed input

extracting intent

detecting missing context

flagging ambiguity

detecting risky assumptions

tracking subject_origin and body_origin separately


Content provenance

subject_origin: USER_PROVIDED_CONTENT | AI_GENERATED_DRAFT_CONTENT
body_origin: USER_PROVIDED_CONTENT | AI_GENERATED_DRAFT_CONTENT

If adaptation/correction occurs, provenance must remain explicit per field:


subject_origin: USER_PROVIDED_CONTENT
body_origin: AI_GENERATED_DRAFT_CONTENT


12. VOICE_INPUT_ADAPTER

Responsible for:



audio normalization

speech intent extraction

confidence analysis

detecting audio anomalies

distinguishing disfluency from semantic ambiguity


Critical distinctions

SPEECH_DISFLUENCY ≠ SEMANTIC_AMBIGUITY
STUTTERING ≠ UNCLEAR_INTENT
PAUSE ≠ END_OF_COMMAND
REPETITION ≠ REPEATED_ACTION
DISFLUENCY ≠ ERROR

These are speech-delivery/disfluency characteristics, not semantic ambiguity by themselves.


Locked voice fields

stt_quality = ok | unintelligible
disfluency_present = true | false
semantic_ambiguity_present = true | false

Output behavior

If STT quality is ok and intent is clear:


confidence: high (on intent)
ambiguity_score: low
metadata.stt_quality: "ok"

If STT quality is unintelligible:


confidence: low (on intent)
ambiguity_score: high
metadata.stt_quality: "unintelligible"

If intent itself is ambiguous:


confidence: low (on intent)
ambiguity_score: high
metadata.stt_quality: "ok" or "unintelligible"

Speech style must never trigger HOLD by itself.


disfluency_present = true
→ DOES NOT trigger HOLD by itself


13. THOUGHT_SHADOW_CORE

Core behavior:



receive input packet from adapter

map to tentative intent

inspect uncertainty and provenance

apply SHADOW_CHECK

apply bounded SHADOW_OF_SHADOW

evaluate HOLD vs PROCEED vs SEMANTIC_CONFIRM_REQUIRED

if action is risky or consequential → SEMANTIC_CONFIRM_REQUIRED

if a side-effecting tool is required → TOOL_APPROVAL_REQUIRED

return decision packet


Decision states

HOLD
→ insufficient clarity
→ missing required evidence
→ a required semantic detail would have to be guessed
→ request targeted clarification

PROCEED
→ clear intent
→ no unresolved required ambiguity
→ no consequential side effect requiring semantic confirmation
→ continue processing

SEMANTIC_CONFIRM_REQUIRED
→ intent appears clear enough to present
→ action is risky or consequential
→ user must explicitly confirm the interpreted meaning

Terminology rule:


CONFIRM
= informal shorthand only

SEMANTIC_CONFIRM_REQUIRED
= canonical decision-state name

Critical loop rule:


HOLD
→ USER_CLARIFICATION
→ NEW_INPUT
→ INPUT_ADAPTERS
→ THOUGHT_SHADOW_CORE

A clarification must re-enter the normal adapter/core path.


It must not bypass the Thought Shadow core and jump directly to AI_PROCESS.



14. Stateful Semantic Confirmation

Semantic confirmation must be stateful.


Required fields:


confirmation_id: unique identifier

request_fingerprint:
hash or stable fingerprint of the request being confirmed

confirmation_state:
pending | confirmed | expired | cancelled

Critical rule:


YES WITHOUT VALID PRIOR CONFIRMATION_STATE
= INVALID

Additional rule:


VALID_CONFIRMATION
=
matching confirmation_id
+
matching request_fingerprint
+
non-expired pending state

Once a confirmation is accepted:


confirmation_state = confirmed

A confirmed state must not be silently reused for another request.


For consequential execution:


CONFIRMATION_TOKEN
= SINGLE_USE_FOR_MATCHING_ACTION

A changed request requires a new confirmation state.


This prevents:



accidental confirmation

replay

ambiguous "yes" responses

confirmation drift

reuse after request mutation



15. SEMANTIC_CONFIRM vs TOOL_APPROVAL

These are separate gates.


They must not be conflated.


SEMANTIC_CONFIRM

User confirms:
"Yes, that is what I mean."

Purpose:


INTENT / SEMANTICS

Semantic confirmation locks the interpretation of the user's request.


It does not itself authorize every possible side effect.


TOOL_APPROVAL

User approves:
"Yes, perform this specific consequential action."

Purpose:


EXECUTION AUTHORITY

The user must be shown enough information to understand what action is being authorized.


Gate structure

For a consequential side effect:


SEMANTIC_CONFIRM = YES
AND
TOOL_APPROVAL = YES
→ EXECUTE_TOOL

For pure non-side-effect operations such as:



local text generation

local analysis

read-only retrieval

non-mutating inspection


TOOL_APPROVAL = NOT_APPLICABLE
→ CONTINUE_PROCESSING

Critical distinction:


LOCAL_DRAFT_GENERATION
≠
EXTERNAL_DRAFT_CREATION

A locally generated draft shown to the user is non-side-effect.


Creating or modifying a draft inside an external system changes external state.


Therefore:


EXTERNAL_STATE_CHANGE
→ TOOL_APPROVAL_REQUIRED


16. Content Provenance

Every decision packet must preserve relevant field-level provenance.


Allowed v1 provenance values:


USER_PROVIDED_CONTENT
AI_GENERATED_DRAFT_CONTENT

Example:


subject: "send email"
subject_origin: USER_PROVIDED_CONTENT

body: "to alice@example.com"
body_origin: USER_PROVIDED_CONTENT

tone: "professional"
tone_origin: AI_GENERATED_DRAFT_CONTENT

This prevents:



silent adaptation of user intent

mixed-provenance confusion

untracked AI correction

AI-generated material being represented as user-provided material


Where multiple fields exist:


metadata_origin:
{
  "subject": "USER_PROVIDED_CONTENT",
  "body": "AI_GENERATED_DRAFT_CONTENT"
}

Critical rule:


FIELD_LEVEL_PROVENANCE = AUTHORITATIVE

and:


MIXED_CONTENT
→ MIXED_FIELD_LEVEL_PROVENANCE

A packet must not collapse mixed origins into one misleading origin label.



17. SHADOW_CHECK

Applied to every candidate intent.


Conceptual rules:


if AI_agrees_too_easily:
  → surface a relevant counter-angle

if input is unclear:
  → HOLD

if model would need to assume a required detail:
  → AUTO_GUESS_BLOCKED
  → request clarification or evidence

if action is risky or consequential:
  → SEMANTIC_CONFIRM_REQUIRED

Shadow is not truth.


Shadow is a structured challenge layer.


Its purpose is not automatic opposition.


Its purpose is to expose:



hidden assumptions

missing evidence

ambiguity

relevant counterexamples

decision-relevant uncertainty



18. SHADOW_REFLECTION

After AI processing:


AI_PROCESS
↓
SHADOW_REFLECTION
  → expose uncertainty
  → surface decision-relevant rationale
  → flag missing evidence
  → list remaining ambiguity
  → expose confirmation / approval requirements
↓
USER_FINAL

The system should expose:



decision-relevant rationale

uncertainty present

missing evidence

ambiguity requiring confirmation

confirmation requirements

tool-approval requirements when applicable


The system need not expose:



internal chain-of-thought

private model reasoning

all intermediate steps



19. Safety rules

DEJA_VU ≠ EVIDENCE
INTUITION ≠ FACT
ECHO ≠ TRUTH
SHADOW ≠ TRUTH
COUNTERARGUMENT ≠ AUTOMATICALLY_CORRECT
META_CHECK ≠ GUARANTEE
AUTO_GUESS = BLOCKED
USER = FINAL DECISION

Important:



no silent assumption

no guessing when confidence is insufficient

no implicit action when input is ambiguous

no automatic correction merely because a counterargument exists

no external side effect without the required approval boundary

no reuse of stale confirmation state



20. Implementation stance

This is a conceptual architecture first.


Implementation should remain:



small

explicit

testable

explainable

human-centered


No black-box automation should be introduced into the decision pipeline without a clear safety gate.


Implementation should prefer deterministic, inspectable state transitions over hidden control flow.



21. Build-Up Memory rule

MEMORY IS NOT REWRITTEN.
MEMORY IS BUILT UP.

This spec must remain traceable to the original reasoning path:



$500 joke

accessibility trigger

Shadow layer

Thought Shadow

Thought Echo

Shadow of Shadow

TEXT + VOICE adapter unification

Support guidance in Case 16149600


This is not a final doctrine.


It is an evolving Build-Up Memory frame.



22. Summary

The design is not:
"AI decides for the user."

The design is:
"AI helps the user decide well."

This v1 is a checkpoint:



open

reflective

limited

human-governed

accessibility-aware

safety-first

dual-input unified

stateful confirmation

content provenance tracked



23. Acceptance criteria

This spec is valid if:



USER remains final decision authority

AUTO_GUESS remains blocked

unclear input results in HOLD

HOLD clarification re-enters the adapter/core path

risky or consequential action requires SEMANTIC_CONFIRM_REQUIRED

semantic confirmation requires valid state

confirmation must match confirmation_id and request_fingerprint

adapters share the same core contract

SPEECH_DISFLUENCY is distinguished from SEMANTIC_AMBIGUITY

disfluency alone never triggers HOLD

SEMANTIC_CONFIRM and TOOL_APPROVAL are separate gates

tool approval is NOT_APPLICABLE only for pure non-side-effect operations

any external state change requires the appropriate tool-approval boundary

field-level provenance is tracked as USER_PROVIDED_CONTENT or AI_GENERATED_DRAFT_CONTENT

mixed-origin content is not collapsed into a misleading single origin label

feedback supports verification, not closure

SHADOW_OF_SHADOW remains bounded (RELAX_ONCE)

LIGHT_MODEL = ELASTIC_FEEDBACK holds exactly

no clarification path bypasses THOUGHT_SHADOW_CORE

no stale or mismatched confirmation state can authorize a new request



24. Version history


v1 (2026-10-01): Initial specification

TEXT_INPUT_ADAPTER
VOICE_INPUT_ADAPTER
THOUGHT_SHADOW_CORE unified
Stateful confirmation
Content provenance (USER_PROVIDED_CONTENT | AI_GENERATED_DRAFT_CONTENT)
Bounded meta-check
Consequence-aware gate structure
Support guidance incorporated from Case 16149600

v1 support-readiness micro-fix (2026-10-02):

HOLD loop corrected to re-enter adapters/core
canonical decision state normalized to SEMANTIC_CONFIRM_REQUIRED
field-level provenance contract made authoritative
external draft/state-change distinction clarified
support wording corrected from validation claim to support guidance
acceptance criteria aligned with corrected architecture



Final statement

SHADOW is not a replacement for the user.
It is a mirror, challenge, and safety layer.

This version is intended to preserve agency while improving reflective quality, accessibility, and decision reliability across text and voice inputs.


MASTER_SPEC_v1 = READY FOR SUPPORT REVIEW
