# Banned Vocabulary — CoC Blacklist

## Status: ACTIVE | Enforcement: HARD

The following tokens, constructs, and vocabulary patterns are **structurally banned** from all layers of the system. Their presence is treated as a CoC gate violation, not a style preference.

---

## Tier 1 — HARD BAN (immediate BLOCK)

| Token / Pattern | Reason | CoC gate response |
|-----------------|---------|------------------|
| `maybe` | Speculative branch marker | BLOCK + log |
| `maybe_ok` | Conditional optimism without proof | BLOCK + log |
| `probably` | Probabilistic hedge — not a state | BLOCK + log |
| `probably_fine` | False resolution without LP | BLOCK + log |
| `unknown` | Unresolved state passed as output | BLOCK + log |
| `undefined` | Non-state treated as state | BLOCK + log |
| `if/maybe` vocabulary | Speculative conditional branching | BLOCK + log |
| `phantom branch` | Path condition with no implementation | BLOCK + log |

---

## Tier 2 — SOFT BAN (ghost_run → parafráze required)

| Token / Pattern | Reason | CoC gate response |
|-----------------|---------|------------------|
| `might` | Weak modal — not a predicate | ghost_run → rephrase |
| `could be` | Unanchored possibility | ghost_run → rephrase |
| `I think` | Opinion without derivation | ghost_run → rephrase |
| `sort of` | Degraded alignment signal | ghost_run → rephrase |
| `kind of` | Degraded alignment signal | ghost_run → rephrase |
| `probably not` | Negated hedge — still speculative | ghost_run → rephrase |

---

## Replacement Protocol

```
BANNED           → ALIGNED REPLACEMENT
─────────────────────────────────────────
maybe_ok         → state.is_now() == true
probably_fine    → gate.score >= 0.91
unknown          → null  (A5 — legitimate non-applicability)
if maybe X       → X is_now() ? execute : null
probably         → confidence: float (explicit, bounded)
might            → scheduled: bool | pending: bool
```

---

## Enforcement Chain

```
input_token → scan[banned_vocabulary] → match?
  YES → heatmap_penalty(-0.20) → re-evaluate score
        if score < 0.70 → BLOCK + syslog: null_app
        if score 0.70..0.90 → ghost_run + parafráze required
  NO  → continue normal CoC gate flow
```

---

## Invariant

`null` is **NOT banned** — it is the correct PC2 replacement for `unknown`/`undefined`. `null` is a typed, legitimate output (Axiom A5). Banning `null` would be a violation of this document.

---

_Linked from: [coc/gate.md](./gate.md) | [coc/pressure-vs-joy.md](./pressure-vs-joy.md)_
