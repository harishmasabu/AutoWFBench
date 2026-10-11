# New menu diagnostic assessment

Historical record for this observation stage. Later findings and current status are in the [evidence index](../README.md).

## Evidence established

The before, immediate-after and settled trees are identical as parsed JSON. All have 1958 nodes. Root children include four AXWindow nodes and an AXMenuBar. All captured AXMenu and AXMenuItem nodes belong to the macOS menu-bar branch, not the Drive web area. The depth-24 boundary contains 77 AXStaticText and two AXGroup nodes. There is no observed separate Drive creation menu or popover hierarchy.

Blindly4's `show-menu` handler checks that AXShowMenu is advertised, calls AXUIElementPerformAction with that action, and prints success if the native call returns success. It performs no expanded-state or descendant-change verification. The source describes this command as opening an accessibility context menu. Advertised AXShowMenu does not prove equivalence to an ordinary button click or opening Drive's creation menu.

## Ranked hypotheses

Working hypothesis, not a proven cause: Chrome accepted AXShowMenu without producing the intended Drive creation-menu transition. It may be a context-menu action rather than the button's primary activation. This best fits the absence of any captured change, but the available evidence cannot distinguish a no-op from a visible but unexposed result.

Alternative: a menu appeared outside the exposed traversal, below the depth bound, or only transiently before the first post-action read. A longer delay than the one-second settled check is also possible. No screenshot or contemporaneous visual observation establishes these alternatives.

The read starts at the Chrome application root, not just the Drive web area, so it already includes sibling windows and the application menu bar. A same-process popup exposed through traversed AXChildren should be included if within bounds. However, macOS traversal adds AXWindows only when AXChildren contains no AXWindow; additional windows exposed only through AXWindows could be missed when other windows already exist in AXChildren. Unexposed related elements, another process, or deeper descendants are not ruled out. Missing attribute values can also be omitted silently. Blindly4 inspect lists attribute names but is not a generic reader of every AX relationship.

## Read-only experiment prepared

`<restricted-local-evidence>` is a caller-side diagnostic script outside the repository. It captures tree depth 32/max-nodes 10000, freshly finds and inspects exact New when unique, searches AXMenu, AXMenuItem, AXPopover and AXDialog roles across the Chrome application, searches folder titles/descriptions, and captures a depth-32 outline. Each command retains raw stdout, stderr, argv, timing and exit status in a new restricted directory. It never activates, shows a menu, presses, clicks, types or requests permissions. Read commands can still encounter incomplete AX exposure or timeout; the script stops on error. The commands are source-verified, but this diagnostic has not yet been run in the authorized Terminal.

The broader depth/role searches can locate a hierarchy absent from the previous bounded tree. A menu may also use generic groups/buttons rather than AXMenu, so label searches and the outline supplement exact role matching. Absence in all searches remains a coverage-limited finding, not proof of visual absence. A current read does not retroactively establish the state immediately after the earlier action.

## Independent visual confirmation

Smallest method: the operator reports whether a creation menu is currently visible and its visible labels, without selecting anything. For an image, macOS's built-in screenshot shortcut Shift-Command-4 can capture only the Drive menu region. Keep account avatar/email, unrelated tabs and file names out of the image; store the original in restricted local evidence. Record time and that the screenshot is manual and later than the previous AXShowMenu attempt. No screenshot utility installation is needed. Do not grant any new system permission automatically; cancel and report if prompted. Screenshot input may itself affect transient UI, so visual evidence must be correlated carefully with capture timing.

No computer-use or screenshot tool was invoked during this analysis.

## Interaction decision and smallest next step

First run the prepared read-only diagnostic and obtain visual confirmation. This requires no new Drive mutation authorization and uses the already authorized Terminal context. Risks: unrelated account/file text in broad application reads, changed UI since the prior capture, and AX latency; raw data remains restricted and redacted before sharing.

AXShowMenu was a reasonable capability-directed probe, but is not validated as the correct interaction for Drive New. AXPress was not advertised and should not be attempted speculatively. If diagnostics show no menu, a future separately approved single manual click on the freshly verified New button, followed immediately by read-only captures, can test its primary behavior. That click would change UI state and could act on the wrong target if the window moves; require explicit authorization, positive location/target confirmation, one attempt only, and no menu selection. No click or retry is currently authorized or performed. If a menu is already visually open, do not click again; capture its existing state instead.

## Simulator consequence

Keep New's observed action inventory separate from its unconfirmed behavior. No New folder role, menu structure, validation, or creation behavior is established. Do not implement speculative menu semantics as reference-derived findings. Preserve all earlier raw evidence unchanged.
