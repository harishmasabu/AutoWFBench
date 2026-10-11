# New control action inspection

Historical record for this observation stage. Later findings and current status are in the [evidence index](README.md).

Read-only `find --title New --role AXButton --depth 20 --limit 25` exited 0 with empty stderr. Five matches were returned: one exact New title/description and four New Tab results. Substring search must not be treated as an exact match.

The exact New match at historical path `3.0.0.0.0.0.0.1.0.0.2.2.0` is enabled and advertises **AXScrollToVisible** and **AXShowMenu**. It does **not advertise AXPress**. This does not establish that an attempted AXPress would fail; no such attempt was made. Its path agrees with the prior My Drive web-area subtree.

Candidate next action is Blindly4 `show-menu`, after user authorization and fresh discovery/identity checks. AXShowMenu availability alone does not prove it will open the intended creation menu; capture the result before making that claim. No menu-opening transition, folder dialog, create operation or validation behavior has been observed.

Simulator requirement: the New control should reproduce observed action exposure for this backend, including AXShowMenu; do not assume an ordinary AXPress-only button is equivalent. Verify the next transition before fixing implementation behavior.

Raw JSON SHA256: `be02c26d5f3ab2838f77a7510c9fe4849dec7f09bf8114086d37203335cb602c`. Raw command outputs remain in restricted `<restricted-local-evidence>`. The shareable derivative contains only the exact New match with PID and geometry removed. Timestamps were not recorded at invocation time.
