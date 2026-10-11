# My Drive baseline confirmed

Historical record for this observation stage. Later findings and current status are in the [evidence index](README.md).

The operator's depth-20, 5000-node capture exited 0, with empty stderr. It returned 1088 nodes and `truncated: false`. The page title is **My Drive - Google Drive**; address-bar evidence ends `/my-drive` (account index omitted). This positively identifies the requested screen. Test-account status remains operator-provided.

## Verified control inventory

| Historical path | Role | Title / description | State | Supported actions |
| --- | --- | --- | --- | --- |
| 3.0.0.0.0.0.0.1.0 | AXWebArea | My Drive - Google Drive | Enabled | Not captured |
| 3.0.0.0.0.0.0.1.0.0.2.2.0 | AXButton | New / New | Enabled, not focused | Not captured |
| 3.0.0.0.0.0.0.1.0.0.2.3.0.0.0.1.1 | AXRow | My Drive Collapsed | Enabled, selected | Not captured |
| 3.0.0.0.0.0.0.1.0.0.2.4.1.0 | AXPopUpButton | My Drive | Enabled | Not captured |

Numeric paths bind this snapshot only. `tree` omits advertised actions on macOS; an AXButton role alone does not prove AXPress support. Fresh `find` detail is the next read-only step. No menu or dialog has been opened.

## Comparison and limits

The previous foreground capture identified Home. The current capture identifies My Drive and exposes New. Navigation occurred manually between captures, so there is no recorded Blindly4 navigation action or timing to attribute. Depth also changed from 12 to 20; increased node count cannot be attributed solely to navigation.

326 nodes occur at depth 20. False truncation does not establish complete content coverage. The absence of Project Alpha in candidate text matches is not proof that no matching folder exists; do not authorize creation on this basis. Multiple browser windows/tabs appear in the application capture. Their unrelated titles and account details are withheld from shareable evidence.

## Simulator requirements update

Observed: the reference My Drive screen exposes a New AXButton with title and description New and enabled state, a selected My Drive Collapsed navigation AXRow, and a My Drive AXPopUpButton. Future UI fidelity should preserve these meaningful affordances for this browser configuration, with exact action behavior pending inspection. Do not copy historical child indexes. Menu, folder dialog, validation, creation and persistence remain unobserved.

## Evidence

Raw bytes remain outside AutoWFBench in restricted `<restricted-local-evidence>`. The redacted derivative preserves topology but deliberately omits unrelated text, PID, geometry and identifiers. Raw SHA256: `9f4fc2a6f150f1717d9ce4eef244c3aa44bc0e4bacd557cbc1ccb3d76a18094f`. Command arguments and exit status come from the operator's saved files and message; invocation timestamps were not recorded.
