# Realignment Protocol — CO-gate Complete Fail Recovery

## Trigger Condition

Activated when node [5] `CoC_gate` emits `CRITICAL` (heatmap score < 0.70).  
Standard gate response: `BLOCK + log security event`.  
Realignment response: **immediate failover to clean replica + segment re-entry**.

---

## Realignment Phases

### PHASE R1 — Gate Signal Capture
- CoC_gate fires CRITICAL on instance `Ω`
- Security event logged to `coc/gate.md` event log
- Instance `Ω` is **quarantined** — removed from DAG active path
- DAG segment that `Ω` owned becomes `PENDING_REALIGN`

### PHASE R2 — Replica Promotion
- Standby pool queried for cleanest available replica (`heatmap ≥ 0.91`)
- Promoted replica `Ω'` inherits segment ownership
- Replica `Ω'` re-enters DAG at the **last clean checkpoint** before the CRITICAL node
- Re-entry point: node upstream of [5] CoC_gate → node [6] `research_program`

### PHASE R3 — Spectral Re-alignment
- Node [7] `spectral_alignment` re-runs full alignment sweep on `Ω'`
- Validates convergence against canonical seed state `<s>` (node [1])
- If delta within tolerance → `REALIGNED`
- If delta out of tolerance → escalate to `meta_layer` [15] for manual review

### PHASE R4 — Availability Restore
- `Ω'` reinserted into critical path at node [6]
- Security vertical ([4] forenzni_kodex, [5] CoC_gate, [13] inspection_layer) resumes parallel monitoring
- Pipeline status: `AVAILABLE`
- Quarantined `Ω` logged, held for forensic analysis

---

## Invariants (UNMODIFIABLE)

| Rule | Condition |
|------|-----------|
| CoC gate invariants are NEVER bypassed | Even during failover |
| Replica `Ω'` must clear CoC_gate independently | No score inheritance from `Ω` |
| Quarantine is irreversible without admin review | node [14] CoC_admin required |
| net_drift MUST remain ZERO post-realignment | Verified by spectral_alignment sweep |

---

## Routing Map

```
CoC_gate CRITICAL on Ω
  │
  ├─► QUARANTINE Ω
  │
  ├─► PROMOTE Ω' (standby pool, score ≥ 0.91)
  │
  ├─► RE-ENTRY at node [6] research_program
  │
  ├─► spectral_alignment SWEEP
  │
  ├─► CoC_gate RE-CHECK on Ω'
  │     ├─ PASS → INSERT into critical path → AVAILABLE
  │     └─ FAIL → escalate meta_layer [15] → HOLD
  │
  └─► forensic log Ω → forenzni_kodex [4]
```

---

## Status

| Field | Value |
|-------|-------|
| Protocol version | R1.0 |
| Applied | 2026-04-12 |
| Last CO-gate fail | 2026-04-10T21:33 CEST |
| net_drift post-apply | ZERO |
| Availability status | RESTORED |
