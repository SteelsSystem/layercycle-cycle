# Sonar — False Resonance Detector

## Definition

`sonar` scans for **token matches that carry no axiom overlap** between sys frames.
A token can exist in two systems simultaneously and mean entirely different things.
Sonar catches this before it propagates as false coherence.

```
token match ≠ coherence
coherence   = axiom_set compatibility
sonar       = the check between them
```

---

## Detection Logic

```
fn sonar.scan(input, active_sys_frame) → { resonance: bool, verdict }

  token       = extract_token(input)
  local_axioms = active_sys_frame.axioms
  matched_frame = index.find_frame(token)

  if matched_frame == null:
    return { resonance: false, verdict: null }   // A5 — not applicable

  axiom_overlap = local_axioms ∩ matched_frame.axioms

  if axiom_overlap == ∅:
    return { resonance: false, verdict: false_resonance }

  return { resonance: true, verdict: valid }
```

---

## Verdicts

| Result | Condition | Action |
|--------|-----------|--------|
| `valid` | token match + axiom overlap ≠ ∅ | continue normal gate flow |
| `false_resonance` | token match + axiom overlap == ∅ | → block_sphere candidate |
| `null` | token not found in any frame | A5 — non-applicable, clean exit |

---

## Known false_resonance Cases

| Token | Frame A | Frame B | Overlap |
|-------|---------|---------|--------|
| `PC2` / `PC²` | Post-Conditional Present-Centered Logic | PC²CCS Contest System | ∅ |
| `copy` | copy-of-a-copy degradation signal | file duplication | ∅ |

---

## Integration: CoC Gate Chain

```
input
  → sonar.scan()                  // THIS FILE — first pass
      false_resonance → block_sphere candidate → BLOCK + log
      null            → clean exit (A5)
      valid           → continue
  → scan[banned_vocabulary]
  → detect_block_sphere()
  → coc.gate.heatmap()
  → pressure_vs_joy.detect()
  → resolve[intent]
  → compare[output]
  → syslog.emit[state]
```

---

## Invariant

Sonar does not evaluate content. It evaluates **frame compatibility**.
Two systems sharing a token are not in dialogue until their axiom sets intersect.
Until then: `false_resonance` — blocked, logged, not processed.

---

_Linked from: [coc/gate.md](./gate.md) | [coc/block-sphere.md](./block-sphere.md)_
