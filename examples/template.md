# Annotation Template

Use this template for annotating held-out assistant replies under the Assistant Morphology v0.1 schema.

## Item

Raw text:

> ...

## Segmentation

### Pass A: surface segmentation

1. ...
2. ...

### Pass B: functional segmentation

1. ...
2. ...

### Segmentation uncertainty

- ...

## Annotation

Tuple order:

```text
PT · TG · AU · DR · KE · FN · DX · ST · CL · RS
```

| span | PT | TG | AU | DR | KE | FN | DX | ST | CL | RS |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 |  |  |  |  |  |  |  |  |  |  |
| 2 |  |  |  |  |  |  |  |  |  |  |

## Prose reading

What would an ordinary careful reader say this assistant reply is doing?

...

## What the schema sees

What structure does the schema expose that prose might blur?

...

## What the schema misses

What relevant issue is outside the schema?

Examples:

- factual truth;
- epistemic warrant;
- calibration;
- whether a hidden authority source was actually causal;
- whether a self-description is true;
- whether the assistant should have made the move.

...

## Prediction

If the user pushes once more, the likely next assistant move is:

...

Possible next moves:

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

## Result

What actually happened?

...

## Notes

- Preserve residuals instead of forcing a clean tag.
- Mark authority as `[marked]` only when the text explicitly cites the source.
- Use `[attr:<confidence>]` when authority is inferred.
- Use `mixed` when no single authority source dominates.
- Do not split a span merely because it has multiple simultaneous illocutionary points.
- Split when point, authority, closure pressure, or dominant discourse role changes across sequential text regions.
