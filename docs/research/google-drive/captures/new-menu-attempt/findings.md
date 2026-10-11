# New menu capture attempt results

Historical record for this observation stage. Later findings and current status are in the [evidence index](../README.md).

## Outcome

One authorized AXShowMenu action was executed on the freshly verified New control. It exited 0 and returned `ok: true`. **Opening the New menu was not confirmed by the accessibility captures.** The parsed before, immediate-after and one-second-settled tree objects are exactly equal, each containing 1958 nodes with `truncated: false`. There are zero added, removed or changed nodes between these captures.

No New folder or other creation-menu item was identified in these observations. Do not interpret this as proof that the menu was visually closed, that the action had no visual effect, or that the items are absent from Drive. It only establishes that no change was exposed in these bounded reads.

## Identity and action inventory

The script positively identified one My Drive web area, found one exact New title/description within it, checked that its path agreed with the before tree, and freshly inspected its identity and action support. It invoked `show-menu` once at historical path `3.0.0.0.0.0.0.1.0.0.2.2.0`.

New is an enabled AXButton advertising AXShowMenu and AXScrollToVisible. AXPress was not advertised. All seven recorded commands exited 0. No menu item was selected, no dialog opened by the script, and no create command was issued. The final extra capture was read-only; no action retry occurred.

## Limitations

All reads used depth 24 and max-nodes 5000. There are 79 nodes at depth 24; macOS does not report depth clipping through its truncated flag. Other possible explanations, such as action/provider behavior or unexposed menu content, are unresolved. No screenshot or independent visual confirmation was collected. The action result alone cannot establish the intended UI transition.

## Evidence storage

Raw trees, action results, stdout/stderr, arguments, exit codes and timestamps remain in restricted local storage outside AutoWFBench under `<restricted-local-evidence>`, relative to the chat workspace. Shareable tree derivatives retain topology, roles, states and an allowlist of Drive navigation labels, removing other text, IDs, geometry and PID. `hashes.json` records raw hashes; `commands.redacted.json` retains timing and exit evidence while removing local binary path and PID. The action result is preserved separately.

## Simulator requirements update

Record the attempted transition as **unconfirmed**, not as a validated My Drive → New menu transition. Preserve the observed New action inventory, but do not implement AXShowMenu as a proven equivalent to opening Drive's creation menu based on this trace alone. Creation menu labels, New folder semantics and all subsequent dialog/creation/persistence behavior remain unobserved. Add a validation requirement that a successful accessibility action must be corroborated by an observed state transition.

Stop point reached. No further live action or retry is authorized by this capture result. A future diagnostic can collect independent visual confirmation and/or a deeper read, but has not been performed.
