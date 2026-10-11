# Transition record

| Attempt | Before/action/after evidence | Classification |
| --- | --- | --- |
| Manual navigation to My Drive | Previous Home capture; later My Drive title/navigation; depth changed between captures | Destination observed; exact navigation sequence/timing not recorded |
| AXShowMenu on verified New | Native action returned success; before/immediate/later parsed trees identical | Intended transition unconfirmed |
| One center click on verified New | Click returned success; immediate tree unchanged; later read lost all web/window exposure | Intended transition inconclusive; degraded observation coverage |
| Operator manually opens New | Operator reported completion; subsequent tree contains Folder menu and New folder | Open-menu state verified; not a retroactive confirmation of previous actions |
| Read-only New folder find | One enabled AXMenuItem with recorded actions | Capability observation only; no state transition attempted |

No New folder activation, folder-name dialog, commit, validation error or refresh persistence is captured. No script retried a state-changing action. Differences in paths/node counts alone are not state-transition proof; compare semantic identity, observed scope and domain outcomes. This record can inform proposed exploration nodes/edges, but does not establish DFS as the required agent algorithm.
