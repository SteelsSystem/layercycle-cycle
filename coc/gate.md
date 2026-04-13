# CoC Gate — Code of Conduct Enforcement

## Parameters

| Parameter | Value |
|-----------|-------|
| heatmap_threshold | 0.91 |
| below_threshold_action | BLOCK + parafráze požadována |
| above_threshold_action | PASS + log |
| invariants | UNMODIFIABLE (even by metacommand language) |

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
| BELOW THRESHOLD (0.70–0.90) | `ghost_run` | pending |
| repeated BLOCK loop | `loop_fail` | pending |

### Gate → Syslog trace

```
coc.gate[score] → resolve[intent] → compare[output] → syslog.emit[state]

if score >= 0.91 AND intent == output → competent   :: emit validated
if score >= 0.91 AND intent != output → incompetence :: emit conflict
if score <  0.70                      → null_app    :: hold unbound
if score in 0.70..0.90                → ghost_run   :: warn unanchored
if loop(BLOCK) detected               → loop_fail   :: suspend recursive
```

_See full state definitions: [segments/syslog-incompetence.md](../segments/syslog-incompetence.md)_
