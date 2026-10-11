# Manually opened New menu confirmed

Historical record for this observation stage. Later findings and current status are in the [evidence index](../README.md).

The raw capture exited 0 and contains 969 nodes, with truncated false and no nodes at depth 32. A focused, enabled AXMenu titled and described **Folder menu** is exposed beneath the Drive web-content hierarchy at historical path `3.0.0.0.0.0.0.1.0.0.2.2.2.0`. The enabled New folder item is exposed as an AXMenuItem. This confirms accessibility exposure of the menu in this snapshot.

The operator reported completing a manual menu opening before capture. Whether the menu remained visually visible throughout capture was not separately reported. The positive AX evidence establishes menu exposure at read time without establishing timing or outcomes of earlier AXShowMenu/click experiments.

## Menu inventory

| Observed AXMenuItem title | Enabled |
| --- | --- |
| New folder ^C then F | True |
| File upload ^C then U | True |
| Folder upload ^C then I | True |
| Google Docs | True |
| Google Sheets | True |
| Google Slides | True |
| Google Vids | True |
| Google Forms | True |
| More | True |

Titles containing `^C then ...` are literal observed accessible labels, not keyboard instructions validated by this investigation. Supported actions for the items remain unknown because this is tree output, not find/inspect detail. No item was selected during the capture.

## Findings and comparison

The menu is a separate AXMenu inside the existing Drive web area, not another top-level application or window. The menu appears at depth 16 and its direct items at depth 17, within the earlier depth-24 bound. This weakens the hypothesis that depth alone concealed this same menu hierarchy during earlier attempts; it does not prove what those attempts displayed. Earlier attempts lacked this observed menu. This later manually opened snapshot is not a controlled immediate before/after pair, so overall node-count differences cannot all be attributed to opening the menu.

The New folder label includes shortcut text; exact full-title matching against only `New folder` would miss it. Discovery should use substring search plus role, menu context, and unique-match verification. Historical paths must not be reused for actions.

## Simulator requirements update

Verified open-state requirements: expose a focused enabled AXMenu with accessible name Folder menu and enabled AXMenuItem for New folder, including the observed accessible-label suffix for this configuration. The remaining observed items are reference inventory; implementing their unrelated workflows is outside the folder-create scope. Preserve appropriate focus and semantic hierarchy. The method for reliably opening the menu through Blindly4 remains unresolved. Item actions, folder dialog, validation, creation and persistence are unobserved.

## Evidence

Raw capture remains in restricted storage outside AutoWFBench at `<restricted-local-evidence>`. Raw SHA256: `e88e5d359858f63a7997cec0ba6fc2dcb1bc5f12fd4ece7b896a096e25a2cd5e`. The shareable tree is an explicitly redacted derivative. Exit/stderr remain in the original directory; invocation timestamps were not recorded.

Stop point: menu captured, no menu selection or folder creation. Next read-only step is fresh `find` of New folder with AXMenuItem role to obtain actions, before any separately approved dialog-opening action.
