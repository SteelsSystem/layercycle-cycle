# syslog :: null_app

## Definition

`null_app` is the state where an application is **structurally undefined** —  
no intent, no process, no output. The application slot exists but contains nothing.

```
app::intent == null AND app::process == null AND app::output == null
→ syslog.emit("null_app", state=unbound)
```

---

## Formal Notation

```
null_app := app[∅]
syslog.trace(null_app) → hold :: unbound
```

Short form:
```
app<∅> = null_app → syslog[hold]
```

---

## Distinction from Related States

| State | Intent | Process | Output | Resolution |
|---|---|---|---|---|
| `null_app` | null | null | null | nothing to resolve |
| `ghost_run` | null | running | exists | deferred — needs intent |
| `incompetence` | exists | running | wrong | fixable — semantic drift |
| `loop_fail` | repeating | cycling | never | suspended — needs restart |

`null_app` is the only state with **zero active components**.

---

## CoC Gate Mapping

Gate score: **< 0.70** → CRITICAL BLOCK  
Syslog emit: `hold :: unbound`

```
coc.gate[< 0.70] → null_app :: hold unbound
```

_See: [coc/gate.md](../coc/gate.md)_

---

_part of `<stupid logic theory>` — layercycle-cycle forensic codex_
