# Read-only capture attempt

Historical record for this observation stage. Later findings and current status are in the [evidence index](README.md).

No Drive tree was captured and no UI action was performed.

- Blindly4 `apps`: exit 0, raw JSON `{"apps":[]}`; no Chrome PID available in this execution context.
- `/usr/bin/pgrep -x 'Google Chrome'`: exit 3; `sysmon request failed with error: sysmond service not found`; `pgrep: Cannot get process list`.
- Non-prompting `AXIsProcessTrusted()` check in the execution context: `accessibility_trusted=false`.

Stopped before any accessibility-tree read because Chrome could not be identified and the execution context is untrusted. This does not contradict the successful manual Terminal setup; permissions and desktop visibility differ by execution context.

Raw stdout, stderr and command metadata are retained outside the AutoWFBench repository in the restricted local directory:
`<restricted-local-evidence>` relative to the chat workspace. No account-bearing evidence is included here. No observed controls, transitions or simulator requirements can be inferred from this failed attempt.

## Manual Terminal attempt

The user's Terminal `apps` command identified Chrome PID <CHROME_PID>. The subsequent `tree --pid <CHROME_PID> --depth 8 --max-nodes 1500` exited 77 with code `accessibility_permission_denied` and message `Accessibility access is not enabled`. Stderr was empty. The saved JSON is an error object, not an accessibility tree. Raw files remain in the restricted `<restricted-local-evidence>` directory outside AutoWFBench. No UI observations are available. Permission repair is required in the Terminal execution context before a fresh capture.
