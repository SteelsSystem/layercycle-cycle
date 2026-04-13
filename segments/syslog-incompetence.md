# syslog :: incompetence

## Definition

`incompetence` is a semantic application state — not a runtime error, not a syntax failure.  
It occurs when an application's declared intent diverges from its actual output.

```
app::intent ≠ app::output
→ syslog.emit("incompetence", state=conflict)
```

---

## State Classification

| syslog state   | condition                        | output                   |
|----------------|----------------------------------|--------------------------|
| `competent`    | `intent == output`               | `emit :: validated`      |
| `incompetence` | `intent ≠ output`                | `emit :: conflict`       |
| `null_app`     | application undefined            | `hold :: unbound`        |
| `ghost_run`    | running without declared intent  | `warn :: unanchored`     |
| `loop_fail`    | intent repeats, no result        | `suspend :: recursive`   |

---

## Trace Pipeline

```
trace[app] → resolve[intent] → compare[output] → emit[syslog]

if intent == output → competent
if intent ≠ output  → incompetence :: semantic_drift
if intent == null   → null_app :: unbound
```

---

## Formal Notation

```
incompetence := app[intent] → output[conflict]
syslog.trace(incompetence) → emit :: semantic_drift
```

Short form:
```
app<intent≠output> = incompetence → syslog[conflict]
```

---

## Relation to Other States

- `false` — evaluation rejected, no output produced  
- `null` — state unresolvable, legitimately undefined  
- `incompetence` — output produced, but **wrong meaning** relative to intent  

`incompetence` is the only state where the system appears to function while failing semantically.  
This makes it the most dangerous state in `<stupid logic theory>` — invisible to syntax checkers, visible only to semantic trace.

---

## Layer Placement

```
sys > app > syslog
           ↑
    incompetence lives here
    — after application runs
    — before output is accepted as valid
```

---

_part of `<stupid logic theory>` — layercycle-cycle forensic codex_
