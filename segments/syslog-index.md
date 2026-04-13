# syslog :: index

Master reference for all syslog semantic states in `<stupid logic theory>`.

---

## State Map

| State | Condition | Emit | File |
|---|---|---|---|
| `competent` | intent == output, gate >= 0.91 | `emit :: validated` | *(resolved, no file)* |
| `incompetence` | intent != output, gate >= 0.91 | `emit :: conflict` | [syslog-incompetence.md](./syslog-incompetence.md) |
| `ghost_run` | intent = null, process running | `warn :: unanchored` | [syslog-ghost-run.md](./syslog-ghost-run.md) |
| `loop_fail` | intent repeating, output never | `suspend :: recursive` | [syslog-loop-fail.md](./syslog-loop-fail.md) |
| `null_app` | app = empty, nothing defined | `hold :: unbound` | [syslog-null-app.md](./syslog-null-app.md) |

---

## Resolution Tree

```
app exists?
+-- no  -> null_app      :: hold :: unbound
+-- yes
    |
    +-- intent defined?
    |   +-- no  -> ghost_run   :: warn :: unanchored
    |   +-- yes
    |       |
    |       +-- output produced?
    |           +-- no (loop) -> loop_fail   :: suspend :: recursive
    |           +-- yes
    |               |
    |               +-- intent == output -> competent    :: emit :: validated
    |               +-- intent != output -> incompetence :: emit :: conflict
```

---

## Pipeline

```
input
  -> tokenize
  -> semantic map        [sys layer]
  -> constraint check    [rule layer]
  -> coc.gate[score]     [coc/gate.md]
  -> resolve[intent]
  -> compare[output]
  -> syslog.emit[state]  <- this index
  -> o-sysapp<true       [output layer active]
```

---

## o-sysapp States

```
o-sysapp<true      := output layer active, accepting emit
o-sysapp<false     := output layer inactive, no emit
o-sysapp<waiting   := ghost_run pending resolution
o-sysapp<suspended := loop_fail detected, awaiting restart
```

---

## Gate Binding

_Full CoC gate -> syslog mapping: [coc/gate.md](../coc/gate.md)_

---

_part of `<stupid logic theory>` -- layercycle-cycle forensic codex_
