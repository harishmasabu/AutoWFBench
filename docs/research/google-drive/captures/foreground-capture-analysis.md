# Foreground capture analysis

Historical record for this observation stage. Later findings and current status are in the [evidence index](README.md).

The operator's foreground capture exited 0 and returned 548 nodes with `truncated: false`. Unlike the initial capture, this includes four AXWindow nodes and two AXWebArea nodes.

The Drive web area at captured path `3.0.0.0.0.0.0.1.0` has title **Home - Google Drive**. The browser address bar shows a Drive account route ending `/home` (account index redacted). This is evidence of Drive Home, not the requested My Drive root. The account's test status cannot be established from these fields.

The outer web area is at depth 9; a nested web area occurs at depth 12, exactly the requested traversal limit. No Drive New control was identified. This is a coverage gap, not evidence that the control is inaccessible. MacOS's false truncation flag does not report this depth boundary.

## Inventory

| Capture path | Role | Observed meaning | Actions |
| --- | --- | --- | --- |
| 3 | AXWindow | Drive browser window; full title withheld for privacy | Not captured |
| 3.0.0.0.0.0.0.1.0 | AXWebArea | Home - Google Drive | Not captured |
| 3.0.0.0.0.0.0.1.0.0.0.0 | AXWebArea | Unnamed nested area at depth limit | Not captured |
| 3.0.2.0.0.0.3.1 | AXTextField | Address and search bar; route ends /home | Not captured |

Paths are historical evidence, not reusable action targets. No mutations, menu transitions, folder creation or refresh were performed. The foreground change was performed manually by the operator; the captures show different coverage but do not establish why the earlier window exposure was absent.

## Evidence

Unmodified raw data and exit/stderr files remain in restricted storage under `<restricted-local-evidence>` outside AutoWFBench. The shareable structural derivative removes account-bearing text and unrelated window/menu data while preserving child topology. Raw SHA256: `df50d6f778723d746c389a0dcd53a82a8080549b4831e852f08764a33d6c6bdd`.

## Simulator requirement status

My Drive baseline remains pending. Require positive current-location evidence and deeper web-content coverage before deriving the New control, menu or folder-dialog semantics. No Drive-specific interaction requirement is yet verified. Next step: operator manually selects My Drive in the test profile, then captures depth 20 with the existing 5000-node bound. No account/profile changes or live actions are automated.
