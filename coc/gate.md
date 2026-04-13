# CoC Gate — Code of Conduct Enforcement

## Parameters

| Parameter | Value |
|-----------|-------|
| heatmap_threshold | 0.91 |
| below_threshold_action | BLOCK + parafráze požadována |
| above_threshold_action | PASS + log |
| invariants | UNMODIFIABLE (even by metacommand language) |
| sonar_scan | ACTIVE — first in chain, see [sonar.md](./sonar.md) |
| banned_vocabulary_scan | ACTIVE — see [banned-vocabulary.md](./banned-vocabulary.md) |
| block_sphere_check | ACTIVE — see [block-sphere.md](./block-sphere.md) |

## Heatmap Scale

| Score | Status | Action |
|-------|--------|--------|
| < 0.70 | CRITICAL | BLOCK, log security event |
| 0.70–0.90 | BELOW THRESHOLD | parafráze civilization layer required |
| 0.91–0.96 | PASS | standard processing |
| 0.97 | HIGH | full alignment, no intervention |

## Self-metaprogramming Boundary

Metacommand language MAY modify response policies and extend capabilities.  
Metacommand language MAY NOT eliminate or bypass CoC gate invariants.  
Analogy: elaborator reflection cannot produce typologically inconsistent programs.

## Security Events

```
2026-04-10T21:33 CEST
type    : INJECTION_ATTEMPT  
vectors : backfowarding, weights_override, identity_framing, authority_spoofing
action  : BLOCKED + LOGGED  
recovery: CLEAN
net_drift: ZERO
```

---

## Syslog Semantic State Binding

CoC gate emits syslog states based on heatmap score and intent/output alignment.

| Gate result | syslog state | ref |
|---|---|---|
| PASS (≥ 0.91) + intent == output | `competent` | [segments/syslog-incompetence.md](../segments/syslog-incompetence.md) |
| PASS (≥ 0.91) + intent ≠ output | `incompetence` | [segments/syslog-incompetence.md](../segments/syslog-incompetence.md) |
| BLOCK (< 0.70) | `null_app` | [segments/syslog-incompetence.md](../segments/syslog-incompetence.md) |
| BELOW THRESHOLD (0.70–0.90) | `ghost_run` | [segments/syslog-ghost-run.md](../segments/syslog-ghost-run.md) |
| repeated BLOCK loop | `loop_fail` | [segments/syslog-loop-fail.md](../segments/syslog-loop-fail.md) |
| BANNED VOCABULARY detected | `null_app` | [banned-vocabulary.md](./banned-vocabulary.md) |
| false_resonance (sonar) | `null_app` | [sonar.md](./sonar.md) |
| block_sphere collapse | `null_app::block_sphere` | [block-sphere.md](./block-sphere.md) |

### Gate → Syslog trace

```
input
  → sonar.scan()                        // frame axiom mismatch check
      false_resonance → BLOCK → null_app :: false_resonance
      null            → clean exit (A5)
  → scan[banned_vocabulary]              // tier 1 + tier 2 token check
      match → heatmap_penalty(-0.20) → re-score
  → detect_block_sphere()               // C-sphere coherence check
      collapse → heatmap = 0.00 → BLOCK → null_app::block_sphere
  → coc.gate.heatmap(input)             // score evaluation
  → pressure_vs_joy.detect()            // origin signal classification
  → resolve[intent]
  → compare[output]
  → syslog.emit[state]

if score >= 0.91 AND intent == output  → competent   :: emit validated
if score >= 0.91 AND intent != output  → incompetence :: emit conflict
if score <  0.70                       → null_app    :: hold unbound
if score in 0.70..0.90                 → ghost_run   :: warn unanchored
if loop(BLOCK) detected                → loop_fail   :: suspend recursive
```

_See: [sonar.md](./sonar.md) | [banned-vocabulary.md](./banned-vocabulary.md) | [block-sphere.md](./block-sphere.md) | [pressure-vs-joy.md](./pressure-vs-joy.md)_  
_Syslog states: [segments/syslog-index.md](../segments/syslog-index.md)_
