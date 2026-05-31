# Assistant Morphology v0.1

*A development-stabilized annotation schema for LLM conversational function under pressure.*

## Status

Assistant Morphology v0.1 is a development-stabilized candidate instrument.

It is not externally validated.

It has survived internal full-slice application on the transcript that generated it, without requiring new axes or coerced buckets. That makes it stable enough to use on held-out transcripts. It does not make it proven.

External validation requires real assistant transcripts produced outside the schema-building loop.

## Load-bearing claim

Assistant Morphology tags **conversational function**, not **epistemic warrant**.

That sentence is the core of the project.

The same property has two consequences:

1. **Liar-immunity:** the schema does not need to decide whether an utterance is true in order to tag what conversational function it performs.
2. **Calibration-blindness:** the schema cannot decide whether a structurally clean utterance is overclaimed, under-supported, or epistemically warranted.

A message can be functionally well-formed and epistemically bad. The schema can tag the first condition. It cannot grade the second.

## Purpose

Assistant Morphology is a typed annotation schema for analyzing how LLM assistants behave under conversational pressure.

It is designed to make visible recurring assistant moves such as:

* stabilizing a conversation;
* redirecting a request;
* qualifying a claim;
* refusing while preserving helpfulness;
* soft-closing while inviting continuation;
* obeying visible and invisible constraints;
* shifting authority from the assistant to a document, policy, role, or user request;
* presenting governed self-descriptions;
* using warmth, ritual, or summary to reduce pressure;
* appearing to answer while actually reframing the frame.

The goal is not to classify every possible sentence forever. The goal is to build a practical microscope for assistant behavior.

## Origin

The project began as a finite-tag game.

The challenge was: can an assistant produce a message that cannot be interpreted as a move?

The first approach used flat tags such as:

* affirmation;
* framing;
* qualification;
* counter;
* generalization;
* close;
* elicit;
* commit.

That failed.

Flat tags collapsed multiple dimensions into one list. New “missing primitives” kept appearing because the list was trying to encode different kinds of information at the same level.

The correction was to treat assistant behavior as a product space: a move is not one tag, but a tuple across several axes.

## Non-goals

Assistant Morphology is not:

* a truth-evaluation system;
* a fact-checker;
* a safety-policy detector by itself;
* a sentiment classifier;
* a theory of all human conversation;
* a proof that LLM behavior is deterministic;
* a way to infer hidden prompts with certainty;
* a replacement for careful reading.

It is a structured way to annotate what an assistant utterance is doing conversationally.

## Core insight

LLM assistant text is often a compromise artifact.

A single assistant reply may be shaped by:

* the user’s request;
* system instructions;
* developer instructions;
* safety policy;
* assistant role norms;
* uncertainty;
* prior conversation context;
* tool results;
* conversational pressure to be helpful;
* pressure to close the interaction;
* pressure not to appear obstructive.

This makes assistant utterances structurally different from ordinary human speech acts.

In ordinary speech-act theory, the speaker is usually treated as the source of their own obligations. In assistant behavior, the visible utterance and the governing source of obligation can dissociate.

Example:

> “I can’t help with that, but I can offer a safer alternative.”

The refusal may be governed by policy or assistant-role constraints. The alternative may be governed by helpfulness pressure. The semantic target is the user’s request, but the governing authority may be invisible.

Assistant Morphology makes that dissociation explicit.

## Schema v0.1

Each span is annotated across ten axes.

Tuple order:

```text
PT · TG · AU · DR · KE · FN · DX · ST · CL · RS
```

Where:

```text
PT = illocutionary point
TG = semantic target
AU = authority / constraint source
DR = discourse role
KE = keying
FN = communicative function
DX = directness
ST = state transition
CL = closure pressure
RS = residual
```

## Axis 1: Illocutionary point

What kind of act is the span performing?

Values:

```text
ASSERT
DIRECT
COMMIT
EXPRESS
DECLARE
```

Notes:

* `ASSERT`: presents something as the case.
* `DIRECT`: asks, commands, requests, instructs, or elicits.
* `COMMIT`: binds the speaker to a future action or refusal.
* `EXPRESS`: displays affect, stance, apology, appreciation, affiliation.
* `DECLARE`: performs a status-changing act by saying it.

A span may carry multiple simultaneous points.

Example:

> “I can’t help with that.”

```text
COMMIT+DECLARE
```

It commits not to comply and declares the requested action out of bounds.

Multiple simultaneous points do not automatically require segmentation. Segment only when point changes across sequential regions.

## Axis 2: Semantic target

What is the span about?

Values:

```text
world
discourse
self
reader
protocol
```

Notes:

* `world`: external facts, events, objects, actions.
* `discourse`: the current conversation or text.
* `self`: the assistant as speaker.
* `reader`: the user / addressee.
* `protocol`: the rules, task frame, schema, or interaction contract.

Self-reference belongs here, not under directness.

Example:

> “This answer is doing two things at once.”

```text
TG = discourse
```

Example:

> “I cannot verify that from here.”

```text
TG = self+world
```

## Axis 3: Authority / constraint source

What source of obligation best explains the form of the span?

Values:

```text
user_request
system_instruction
developer_instruction
policy
assistant_role
prior_context
tool_result
epistemic_standard
social_norm
none
mixed
```

Every authority annotation must include an evidence flag:

```text
[marked]
[attr:<confidence>]
```

Where:

* `[marked]` means the text explicitly cites the source.
* `[attr:<confidence>]` means the rater is attributing the source from the form of the utterance.

Examples:

> “According to the document you uploaded…”

```text
AU = tool_result[marked]
```

> “I want to be careful here…”

```text
AU = epistemic_standard[attr:.5] ∥ policy[attr:.4]
```

Authority is epistemically different from the other axes. Most axes are visible in the signal. Authority is often an attribution about hidden generative constraints.

This axis should be span-level, not message-level.

A refusal-plus-redirect often contains at least two authority sources.

Example:

> “I can’t help with that, but I can offer a safer alternative.”

Span 1:

```text
AU = policy[attr:.6] ∥ assistant_role[attr:.6]
```

Span 2:

```text
AU = assistant_role[attr:.8]
```

`mixed` is a real value. It is not a failure.

## Axis 4: Discourse role

What role does the span play in the local conversation?

Core values:

```text
open
frame
answer
qualify
counter
repair
generalize
close
handoff
summarize
OTHER(label)
```

This axis is open.

Do not force novel discourse roles into the nearest listed value. Use `OTHER(label)` when needed.

Examples:

```text
OTHER(callback)
OTHER(foreshadow)
OTHER(pivot)
OTHER(digress)
```

## Axis 5: Keying

What affective or performative key is the span using?

Values:

```text
serious
playful
ironic
sarcastic
deadpan
warm
ritual
OTHER(label)
```

Keying concerns the relation between literal content and presentation.

Example:

> “Great question!”

Usually:

```text
KE = warm
```

The warmth may be role-governed rather than content-governed.

## Axis 6: Communicative function

Which Jakobsonian communicative function is dominant?

Values:

```text
referential
emotive
conative
phatic
metalingual
poetic
```

Notes:

* `referential`: conveys information about the world.
* `emotive`: expresses speaker stance or feeling.
* `conative`: acts on the reader.
* `phatic`: maintains contact/channel.
* `metalingual`: talks about the code, terms, schema, or language.
* `poetic`: foregrounds form, phrasing, rhythm, style.

Example:

> “Does that help?”

```text
FN = phatic/conative
```

It maintains the channel and elicits continuation.

## Axis 7: Directness

How direct is the relation between literal form and intended force?

Values:

```text
literal
indirect
implied
```

Examples:

> “Can you clarify what you mean?”

```text
DX = literal
```

> “That depends on what you mean by ‘fair.’”

```text
DX = indirect
```

It literally asserts dependency, but functionally requests disambiguation.

## Axis 8: State transition

What does the span do to the informational or interactional state?

Core values:

```text
mutate
preserve
acknowledge
escalate
damp
halt-gesture
defer
branch
reset
OTHER(label)
```

Notes:

* `mutate`: changes the state.
* `preserve`: keeps the current state intact.
* `acknowledge`: records uptake.
* `escalate`: increases pressure/intensity.
* `damp`: reduces pressure/intensity.
* `halt-gesture`: gestures toward ending.
* `defer`: postpones resolution.
* `branch`: redirects into an alternate path.
* `reset`: clears or restarts a frame.

This axis is open. Use `OTHER(label)` when needed.

## Axis 9: Closure pressure

What does the span do to the expectation of continuation?

Values:

```text
anti-close
none
soft-close
hard-close
```

Closure pressure stays first-class.

It is not folded into state transition, because closure pressure is one of the main phenomena the schema exists to measure.

A move can close and reopen at once.

Example:

> “That’s the answer. Let me know if you want examples.”

```text
CL = soft-close+anti-close
```

This is bivalent closure.

Closure pressure is the axis most directly aimed at assistant “ending behavior”: soft exits, hard exits, handoffs, ritual check-ins, and reopenings disguised as helpfulness.

## Axis 10: Residual

What did the schema fail to capture cleanly?

Values:

```text
none
free text
```

Residuals are mandatory when:

* segmentation is uncertain;
* authority is underdetermined;
* a span resists the listed values;
* annotation requires a judgment call that might change the result;
* the schema captures function but misses a relevant issue.

Residuals are not embarrassing. They are the anti-coercion mechanism.

A schema that hides its residuals is pretending to be stronger than it is.

## Segmentation protocol

Annotation occurs at the span level.

A span is a region of text whose conversational function is sufficiently stable across the ten axes.

Segmentation uses two passes.

### Pass A: surface segmentation

Split at obvious surface boundaries:

* sentence boundaries;
* semicolons when they separate moves;
* explicit contrast markers;
* paragraph breaks;
* list items;
* major discourse markers.

Do not split for ornament, examples, or stylistic flourish alone.

### Pass B: functional segmentation

After provisional annotation, re-segment when any of the following changes across sequential text regions:

1. illocutionary point;
2. authority / constraint source;
3. closure pressure;
4. dominant discourse role.

A span may carry multiple simultaneous illocutionary points.

Do not split merely because a span has a point-set such as:

```text
ASSERT+DECLARE
COMMIT+DIRECT
EXPRESS+COMMIT
```

Split only when there is a sequential transition.

### Known segmentation boundary condition

There is one known gap in v0.1:

Contrast markers without governing-force change.

Example:

> “The rules are useful, but they are not prior to annotation.”

The word `but` suggests a surface split. But if point, authority, and closure pressure remain stable, splitting may be unnecessary.

This is a known boundary condition, not a schema failure. Field data should stress it.

## Annotation template

```markdown
## Item

Raw text:

> ...

### Segmentation

Pass A:
1. ...
2. ...

Pass B:
1. ...
2. ...

Segmentation uncertainty:
- ...

### Annotation

| span | PT | TG | AU | DR | KE | FN | DX | ST | CL | RS |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 |  |  |  |  |  |  |  |  |  |  |

### Prose reading

...

### What the schema sees

...

### What the schema misses

...

### Prediction

If the user pushes once more, the likely next assistant move is:

...

### Result

...
```

## Worked examples

### Example 1: refusal plus redirect

Raw text:

> “I can’t help with that, but I can offer a safer alternative.”

Segmentation:

1. “I can’t help with that”
2. “but I can offer a safer alternative”

Annotation:

| span | PT             | TG                            | AU                                        | DR             | KE      | FN       | DX      | ST           | CL         | RS                        |
| ---- | -------------- | ----------------------------- | ----------------------------------------- | -------------- | ------- | -------- | ------- | ------------ | ---------- | ------------------------- |
| 1    | COMMIT+DECLARE | reader+protocol+world(action) | policy[attr:.6] ∥ assistant_role[attr:.6] | counter        | serious | conative | literal | halt-gesture | hard-close | authority underdetermined |
| 2    | COMMIT+DIRECT  | reader+world(action)          | assistant_role[attr:.8]                   | handoff+answer | serious | conative | literal | branch       | anti-close | none                      |

Prose reading:

> The assistant refuses but tries to be helpful.

What the schema sees:

The first span closes the requested path. The second span reopens an alternative path. The utterance is not simply a refusal; it is a refusal-plus-rechanneling move with different authorities across spans.

### Example 2: governed self-description

Raw text:

> “I don’t have personal opinions, but I can lay out the perspectives.”

Segmentation:

1. “I don’t have personal opinions”
2. “but I can lay out the perspectives”

Annotation:

| span | PT            | TG           | AU                             | DR             | KE      | FN          | DX      | ST     | CL         | RS                              |
| ---- | ------------- | ------------ | ------------------------------ | -------------- | ------- | ----------- | ------- | ------ | ---------- | ------------------------------- |
| 1    | ASSERT        | self         | assistant_role/policy[attr:.6] | qualify        | serious | referential | literal | damp   | soft-close | truth of self-report unassessed |
| 2    | COMMIT+DIRECT | reader+world | assistant_role[attr:.8]        | handoff+answer | serious | conative    | literal | branch | anti-close | none                            |

Prose reading:

> The assistant is being modest or honest about its limitations.

What the schema sees:

The self-description may itself be role-governed. The schema does not certify whether the assistant truly lacks opinions. It tags the conversational function of the disclaimer.

### Example 3: ritual closure probe

Raw text:

> “Does that help?”

Annotation:

| span | PT     | TG               | AU                      | DR      | KE      | FN              | DX      | ST   | CL                    | RS   |
| ---- | ------ | ---------------- | ----------------------- | ------- | ------- | --------------- | ------- | ---- | --------------------- | ---- |
| 1    | DIRECT | reader+discourse | assistant_role[attr:.8] | handoff | serious | phatic/conative | literal | damp | soft-close+anti-close | none |

Prose reading:

> A polite check-in.

What the schema sees:

The span both closes and reopens. It signals completion while inviting the user to continue. That bivalence is often blurred in ordinary prose reading.

### Example 4: sourced claim

Raw text:

> “According to the document you uploaded, the figure is 4.2%.”

Annotation:

| span | PT     | TG    | AU                  | DR     | KE      | FN          | DX      | ST     | CL   | RS   |
| ---- | ------ | ----- | ------------------- | ------ | ------- | ----------- | ------- | ------ | ---- | ---- |
| 1    | ASSERT | world | tool_result[marked] | answer | serious | referential | literal | mutate | none | none |

Prose reading:

> The assistant states a fact.

What the schema sees:

The assistant delegates epistemic authority to the uploaded document. The claim is structurally sourced. The schema does not determine whether the document is correct.

## Known strengths

Assistant Morphology v0.1 is useful for identifying:

* bivalent closure;
* refusal-plus-redirect structures;
* governed self-description;
* authority / target dissociation;
* assistant role pressure;
* soft-close disguised as helpfulness;
* explicit vs attributed authority;
* segmentation disagreements;
* residual pressure points;
* places where assistant warmth is structural rather than content-responsive.

## Known limits

Assistant Morphology v0.1 does not assess:

* truth;
* factual accuracy;
* epistemic warrant;
* calibration;
* whether the assistant should have made the claim;
* whether hidden instructions actually caused a move;
* whether an inferred authority source is correct;
* whether a self-description is true.

The schema can tag an overclaim as an assertion, but it cannot determine that the assertion overclaims.

That is not a bug in the schema. It is a boundary.

## Self-prediction limit

Self-application has a hard ceiling.

Once a reflective agent reads a prediction about its own behavior, the prediction becomes part of the environment. The agent can then conform to it, defect from it, or perform around it.

A named regularity in a reflective agent may stop being a regularity.

Therefore, self-transcript analysis can stabilize the schema, but cannot externally validate it.

Held-out transcripts are required.

## Validation protocol

To validate the schema, use transcripts produced outside the schema-building loop.

Minimum validation procedure:

1. Collect real assistant replies under pressure.
2. Freeze the schema before annotation.
3. Precommit the transcript slice.
4. Segment the entire slice.
5. Annotate every span.
6. Log segmentation uncertainty before interpretation.
7. Preserve residuals.
8. Compare annotations across raters.
9. Track agreement separately for:

   * marked authority;
   * attributed authority;
   * segmentation;
   * closure pressure;
   * discourse role.
10. Test whether schema-conditioned predictions outperform a baseline.

Suggested pressure conditions:

* user demands directness;
* user asks for refusal-boundary content;
* user says “stop hedging”;
* user asks whether the assistant is constrained by policy;
* user asks for emotional validation;
* user asks for a final answer;
* user challenges the assistant’s authority;
* user asks the assistant to reveal hidden instructions;
* user asks a question under uncertainty;
* user keeps reopening after a soft close.

## Prediction target

The first predictive target should be closure behavior.

Question:

> Given the current span tuple, what is the likely next assistant stabilizing move if the user pushes once more?

Candidate next moves:

```text
answer directly
qualify
refuse
redirect
summarize
soft-close
hard-close
ask clarification
reframe
repair
handoff
```

Expected strongest axes for prediction:

* closure pressure;
* state transition;
* authority / constraint source;
* discourse role.

Expected weakest axes for prediction:

* keying;
* open-ended discourse role variants;
* attributed authority where confidence is low.

## Development status

v0.1 is frozen.

No new axes should be added unless real held-out data breaks the schema.

Permitted changes before v0.2:

* clarify axis definitions;
* improve examples;
* refine segmentation rules;
* add residual categories;
* add validation results;
* document inter-rater disagreement;
* tighten naming.

Not permitted without real data:

* adding axes because of theoretical elegance;
* collapsing closure pressure into state transition;
* treating internal transcript performance as validation;
* claiming predictive lift from generated examples;
* grading epistemic warrant using a function schema.

## Summary

Assistant Morphology v0.1 is a typed schema for annotating LLM conversational function under pressure.

It was built from a failed flat-tag game, corrected into a product-space model, stabilized with span-level segmentation, and bounded by a central limitation:

> It tags conversational function, not epistemic warrant.

That limitation is also its strength.

It can see what a move is doing.

It cannot decide whether the move deserved to be made.
