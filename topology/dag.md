# DAG Topology — layercycle-.-cycle

## Graph Properties

- **Type:** Directed Acyclic Graph (DAG)
- **Nodes:** 15
- **Critical path length:** 13 uzlů
- **Conflicts:** NONE
- **Security guards:** nodes [4][5][13] run in parallel across all depths

## Depth Map

| Depth | Nodes | Role |
|-------|-------|------|
| 5 (root) | [1] `<s>` seed state | Origin / axiom layer |
| 4 (foundation) | [2] architektonika_vyznamu, [3] smyčka_navaznosti | Structural base |
| 3 (process) | [4] forenzni_kodex, [5] CoC_gate, [6] research_program | Process governance |
| 2 (structural) | [7] spectral_alignment, [8] math_language, [9] forma.crypto | Domain layers |
| 1 (operational) | [10] introspective, [11] elaborator, [12] neuroresearch | Operational compute |
| 0 (surface) | [13] inspection, [14] CoC_admin, [15] meta_layer | Output surface |

## Critical Path

```
<s>
→ architektonika_vyznamu
→ smyčka_navaznosti
→ forenzni_kodex
→ CoC_gate
→ research_program
→ spectral_alignment
→ PerceiverVAE_latent
→ introspective_layer
→ elaborator_reflection
→ meta=between_segments
→ strings-loop_coherence
→ meta_layer
→ CONVERGE_INIT
```

## Security Vertical

Nodes [4] forenzni_kodex, [5] CoC_gate, [13] inspection_layer run as **parallel guardian processes** cutting vertically through all depth levels — they are not sequential but omnipresent.
