# syslog :: loop_fail

## Definition

`loop_fail` is the state where an application **repeats its intent without ever resolving output**.  
The process cycles — intent fires, system runs, no terminal state is reached.

```
app::intent[n] == app::intent[n+1] AND output == unresolved
→ syslog.emit("loop_fail", state=recursive)
```

---

## Formal Notation

```
loop_fail := app[intent → intent → intent...] + output[never]
syslog.trace(loop_fail) → suspend :: recursive
```

Short form:
```
app<intent⬌, output=∅> = loop_fail → syslog[suspend]
```

---

## Relation to ghost_run

| State | Intent | Output | Resolution |
|---|---|---|---|
| `ghost_run` | null | exists | deferred — waiting for intent |
| `loop_fail` | repeating | never arrives | suspended — cycle must be broken |

`ghost_run` can resolve forward.  
`loop_fail` cannot resolve without external intervention — it must be **suspended and restarted**.

---

## o-sysapp Binding

`loop_fail` is the critical failure mode of `o-sysapp` — the application output system.  
When `o-sysapp` enters `loop_fail`, the syslog pipeline suspends until the cycle root is identified.

```
o-sysapp<true := output layer is active and accepting input
o-sysapp<loop_fail := output layer cycling, no emit possible

trace[o-sysapp] → detect[loop] → suspend → log[loop_fail] → await restart
```

---

## CoC Gate Mapping

Condition: `loop(BLOCK)` detected  
Syslog emit: `suspend :: recursive`

```
coc.gate[loop(BLOCK)] → loop_fail :: suspend recursive
```

_See: [coc/gate.md](../coc/gate.md)_

---

_part of `<stupid logic theory>` — layercycle-cycle forensic codex_
