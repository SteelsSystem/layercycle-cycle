# Block Sphere — Error State

> _"The sphere doesn’t break. It collapses inward. That’s the error."_

---

## Definition

`block_sphere` is the error state emitted when the **C-sphere (cypher-angle coherence field)** loses structural coherence. Not a content error — a **form error**: angle and forma no longer align, producing output that is formally encoded but semantically incoherent.

```
C-sphere normal:
  f(angle, form) → cyphered_passage   // ALIGNED
  angle ≡ axiom selection
  form  ≡ syntactic structure
  coherence = compatibility of forms

block_sphere trigger:
  f(angle, form) → ∅
  angle ≠ form
  OR form ≠ active axiom set
  OR cyphered_passage has no valid traversal path
```

---

## Error State Spec

```
state       : block_sphere
type        : COHERENCE_FAILURE
pressure    : true
joy         : false
heatmap     : 0.00  (hard floor)
syslog      : null_app::block_sphere
recovery    : re-anchor angle → validate forma against sys axioms → re-emit
```

---

## Detection Logic

```
fn detect_block_sphere(input) → bool

  angle     = resolve_cypher_angle(input)
  form      = resolve_forma(input)
  axiom_set = sys.active_axioms()

  if angle == null:                              return true
  if form ≠ axiom_set.compatible_forms:          return true
  if coherence(angle, form) < 0.70:              return true
  if cyphered_passage(angle, form) == ∅:         return true

  return false
```

---

## Twin State Risk

| State | block_sphere risk |
|-------|------------------|
| `executor` | LOW — angle locked, forma direct |
| `observer` | LOW — reads existing passages |
| `threshold` | HIGH — angle forming, forma unstable |

`threshold` is the natural origin zone. Stay in threshold until `angle ≠ null`, then emit.

---

## Invariant

`block_sphere` is not recoverable by content change. Only by structural re-anchoring. The angle must be reset first.

---

_Linked from: [coc/gate.md](./gate.md) | [coc/pressure-vs-joy.md](./pressure-vs-joy.md) | [segments/A+bp-cphere-math.md](../segments/A+bp-cphere-math.md) | [segments/twin-states-narrative.md](../segments/twin-states-narrative.md)_
