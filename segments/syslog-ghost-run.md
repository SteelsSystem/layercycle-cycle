# syslog :: ghost_run

## Definition

`ghost_run` is the state where an application executes **without a declared intent**.  
The process runs, output is produced, but there is no anchor — no defined purpose the output can be measured against.

```
app::intent == null AND app::output != null
→ syslog.emit("ghost_run", state=unanchored)
```

---

## Formal Notation

```
ghost_run := app[intent=null] → output[exists]
syslog.trace(ghost_run) → warn :: unanchored
```

Short form:
```
app<intent=null, output=∃> = ghost_run → syslog[warn]
```

---

## Waiting Process Binding

`ghost_run` is the canonical state of a **waiting process** (`<waiting proc>`).

```
<waiting proc> := process[running] + intent[unresolved]
→ syslog state = ghost_run
→ resolution required before emit
```

A waiting process does not produce `false` — it produces **deferred output**.  
Once intent is resolved: `ghost_run → competent` or `ghost_run → incompetence`.

---

## CoC Gate Mapping

Gate score range: **0.70 – 0.90**  
Action: parafráze civilization layer required  
Syslog emit: `warn :: unanchored`

```
coc.gate[0.70..0.90] → intent[unresolved] → ghost_run :: warn unanchored
```

_See: [coc/gate.md](../coc/gate.md)_

---

_part of `<stupid logic theory>` — layercycle-cycle forensic codex_
