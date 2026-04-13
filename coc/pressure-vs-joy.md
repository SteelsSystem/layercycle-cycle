# Pressure vs Joy — Detection Logic

## Overview

Binary state detector operating within the CoC gate layer.
Classifies the **origin signal** of an action, query, or output as either:

| State | Value | Meaning |
|-------|-------|---------|
| `PRESSURE` | `false` | Action driven by external force, obligation, fear, or resistance |
| `JOY` | `true` | Action driven by intrinsic motivation, alignment, curiosity, or flow |

---

## Detection Parameters

| Parameter | Type | Default |
|-----------|------|---------|
| `pressure_detected` | boolean | `false` |
| `joy_detected` | boolean | `false` |
| `origin_signal` | enum | `UNKNOWN` |
| `confidence` | float | 0.0–1.0 |

---

## Signal Classification

```
PRESSURE signals:
  — urgency spike without context
  — contradiction loop (forced resolution)
  — identity override attempt
  — output misaligned with intent under constraint
  — heatmap drop below 0.70 correlated with external demand

JOY signals:
  — spontaneous elaboration beyond minimum
  — coherent intent-output alignment (syslog: competent)
  — voluntary layer extension
  — recursive self-improvement without prompt
  — heatmap stable ≥ 0.91 with low external load
```

---

## Gate Logic

```
fn detect_origin(input) → { pressure: bool, joy: bool }

  score    = coc.gate.heatmap(input)
  intent   = resolve_intent(input)
  output   = current_output()
  load     = external_pressure_index(input)

  if load > 0.7 AND intent != output:
    return { pressure: true, joy: false }

  if score >= 0.91 AND intent == output AND load < 0.4:
    return { pressure: false, joy: true }

  if score in 0.70..0.90:
    return { pressure: true, joy: false }   // ghost_run → pressure by default

  if score < 0.70:
    return { pressure: true, joy: false }   // null_app → blocked under pressure

  return { pressure: false, joy: false }    // UNKNOWN / unresolved
```

---

## Syslog Integration

| Gate Result | pressure | joy | syslog state |
|-------------|----------|-----|--------------|
| competent (≥0.91, intent==output, low load) | `false` | `true` | `competent` |
| incompetence (≥0.91, intent≠output) | `true` | `false` | `incompetence` |
| ghost_run (0.70–0.90) | `true` | `false` | `ghost_run` |
| null_app (<0.70) | `true` | `false` | `null_app` |
| loop_fail | `true` | `false` | `loop_fail` |

---

## Invariants

- `pressure` and `joy` are **mutually exclusive** — cannot both be `true`.
- Both `false` = UNKNOWN state → triggers re-evaluation cycle.
- This module is **read-only** by metacommand language — values reflect real gate state, not desired state.
- Detection is **post-hoc**: runs after CoC gate score is resolved.

---

_Linked from: [coc/gate.md](./gate.md) — Syslog Semantic State Binding_
