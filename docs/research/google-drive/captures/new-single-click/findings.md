# Single-click experiment findings

Historical record for this observation stage. Later findings and current status are in the [evidence index](../README.md).

## Result

**New menu opening remains unconfirmed.** One click was sent to the freshly verified New button center at x=66.5, y=258.0. The command exited 0 and reported ok. No second click or other input was issued.

All eleven recorded commands exited 0. Both foreground checks identified Chrome. The script verified the My Drive web area, freshly discovered the exact New AXButton inside it, checked identity and stable bounds, and saved the before tree. This establishes the attempted target and input, not the resulting application behavior.

## Before and after comparison

| Observation | Nodes | Window/web exposure | Comparison |
| --- | --- | --- | --- |
| Before | 2152 | Four AXWindow and three AXWebArea nodes | Baseline |
| Immediate after | 2152 | Same | Parsed JSON exactly equal to before |
| Settled read after an additional one-second wait | 347 | Only application root and macOS menu-bar subtree; no AXWindow or AXWebArea | Application-content coverage lost |

All trees reported truncated false, and none reached depth 32. Thus depth-32 clipping does not explain missing menu content in this particular experiment. Provider omissions, timing, or loss of application-window exposure remain possible. The term settled refers only to the scheduled read; stability was not independently established.

The final read-only `find --title 'New folder' --depth 32 --limit 100` returned an empty matches array. This does not prove that New folder was visually absent: the preceding tree had lost all browser-window content. Path-index churn from that loss must not be mistaken for a Drive domain transition.

## Menu and control inventory

No Drive menu or New folder control was observed. The baseline New control remains verified, but the menu's labels, roles, supported actions and hierarchy are unknown. No menu item was selected and no folder was created by this script. No independent visual observation of this click outcome was supplied.

## Interpretation

The click did not yield confirming AX evidence. It is not justified to declare either that the menu opened or that the click failed. The immediate tree was unchanged and subsequent observation coverage degraded. Chrome was foreground before input; there is no recorded post-click foreground check, so later focus state cannot be inferred. Do not attribute the coverage loss to the operator or to Chrome without further evidence.

Unlike the earlier depth-24 run, increasing the depth now reached no boundary nodes. This weakens depth clipping as the explanation for this run, while leaving native accessibility exposure and action delivery questions unresolved. Command success alone remains insufficient.

## Evidence and simulator update

Raw output, errors, timestamps, arguments, foreground checks and click-center record are preserved outside AutoWFBench under `<restricted-local-evidence>`, relative to the chat workspace. Redacted trees preserve ordered topology and selected navigation labels; hashes identify raw source bytes. No existing evidence was overwritten.

Do not promote the menu-opening transition or New folder semantics to verified simulator requirements. Add an observation-quality check: a post-action result that loses the previously observed web area is inconclusive, even with exit 0. Evaluation must distinguish inaccessible observation from unchanged application state. Browser-window coverage loss is a diagnostic limitation, not an established Google Drive product behavior to reproduce.

The authorized experiment is complete and stopped. No action retry or further input is performed.
