# Google Drive discovery plan for macOS

2026-10-11. Original execution plan, now accompanied by partial live observations in the [evidence index](captures/README.md). Remaining dialog/creation steps require their own authorization; no simulator is implemented. This replaces Jira as the first discovery proof of concept. The single task is **Create a folder named Project Alpha in My Drive**.

This plan follows the existing Blindly4 investigation and Jira runbook. Its commands were checked against official Blindly4 commit `1369de4cdb2893b16da23c16e5a03e2331aea916`. All control labels, roles, hierarchy and paths must come from future observations. Screen names below describe investigation targets, not claimed AX output.

Confirmed direction: simApp-first, even with available APIs; optional simMCP comes later. This discovery specifies the primary UI. Docs and Sheets as Drive files remain future scope.

## Environment and permissions

Use macOS 13+ and a Swift 6 toolchain or an already verified Blindly4 binary. In a future authorized setup session, run from the Blindly4 checkout:

```sh
swift build -c release
./.build/release/blindly4 --self-test
./.build/release/blindly4 schema
./.build/release/blindly4 request-permission
```

These commands have not been run for this task. Do not install a missing toolchain automatically. The invoking terminal/agent host needs macOS Accessibility permission. A permission prompt is not proof of access: require `trusted: true` and a successful test tree. AX-only capture needs no screenshot command or Screen Recording permission; screenshots would be a separate, optional facility.

The account owner must authorize the later session to inspect a specified test account and create exactly one folder. Use a dedicated browser profile containing one signed-in test Drive tab, with the owner completing login/MFA before capture. No API token or OAuth integration is needed for this UI-only plan. Record browser/version, OS/build, locale, zoom, viewport, account alias, Blindly4 commit/binary hash and run identifier. Reserve the whole desktop: no concurrent automation or human input.

Arrange an empty or small synthetic My Drive root with no existing `Project Alpha` child before the session. If that precondition is not met, report it and use a separately prepared account; do not delete or rename existing content. Do not infer absence from a bounded tree in a large account. The exact target name must remain `Project Alpha`, without a run suffix. Repeated trials need independently reset fixtures or separately authorized cleanup, outside the single-create run.

## Screens and transition evidence

| State | Capture | Transition to investigate |
| --- | --- | --- |
| D0 My Drive | Account/location identity, breadcrumb/navigation, initial root list, creation entry control | Open the creation menu |
| D1 New menu | Full visible menu context, items, focus and advertised actions | Select the observed folder-creation item |
| D2 Folder dialog empty/default | Dialog context, editable name control, default value, commit/cancel controls | Enter the requested name |
| D3 Folder dialog filled | Exact draft value and fresh commit-control details | Commit once using the draft guard |
| D4 Folder list after creation | Dialog dismissal, visible new entry, folder-type evidence, current parent, any feedback | Refresh current My Drive page |
| D5 Folder list after refresh | Freshly loaded location and persisted entry, coverage, errors | Verify without another create |

Capture an immediate after-tree for every action, including text entry and refresh. If unsettled, collect fresh observations approximately 1, 2 and 5 seconds later, stopping as soon as the semantic outcome appears. These are research bounds, not claims about Drive timing. Record actual timestamps. Stop on uncertain outcomes; do not repeat Submit.

For each screen inventory observed control name/description, role/subrole, path at capture, value type, enabled/selected/focused state, attributes, actions and surrounding context. Missing data means unknown. Record errors and transient states if encountered; do not invent a validation message or provoke account/network failures.

## Evidence capture commands

Run the following fragments only in the future authorized session, one invocation at a time. Variables are populated from observations; no numeric path or control label is supplied in advance. All commands use real CLI syntax. The recorder is a shell runbook fragment, not repository implementation.

```sh
BLINDLY='/absolute/path/to/blindly4/.build/release/blindly4'
DRIVE_RUN='/absolute/path/to/restricted/evidence/drive-R01'
umask 077
mkdir -p "$DRIVE_RUN"
capture() {
  local label="$1"
  shift
  # A separate directory prevents an accidental overwrite of an earlier step.
  mkdir "$DRIVE_RUN/$label" || return 64
  printf '%s\n' "$@" > "$DRIVE_RUN/$label/argv.txt"
  date -u '+%Y-%m-%dT%H:%M:%SZ' > "$DRIVE_RUN/$label/started.txt"
  "$BLINDLY" "$@" > "$DRIVE_RUN/$label/stdout" 2> "$DRIVE_RUN/$label/stderr"
  local result_code=$?
  printf '%s\n' "$result_code" > "$DRIVE_RUN/$label/exit.txt"
  date -u '+%Y-%m-%dT%H:%M:%SZ' > "$DRIVE_RUN/$label/ended.txt"
  return "$result_code"
}
capture 000-schema schema
capture 001-apps apps
```

Choose the dedicated browser PID from `apps`; verify its test account/tab and current My Drive root with the operator and accessible context. PID alone does not distinguish tabs or accounts. Keep authentication screens out of captures.

```sh
: "${DRIVE_PID:?Set the observed browser PID}"
capture 002-my-drive tree --pid "$DRIVE_PID" --depth 8 --max-nodes 1500
capture 003-outline show --pid "$DRIVE_PID" --depth 8
capture 004-focus focused --pid "$DRIVE_PID"
```

Raw `tree` stdout is JSON on success; keep original bytes, exit status and stderr. `show` is text. If target coverage is missing, use one deeper read (`--depth 12 --max-nodes 5000`) and targeted `find`; mark blocked if still inadequate. No native subtree-read flag exists.

### Discover and inspect each intended control

Use the same sequence with a new capture prefix for each control. Set `CONTROL_TEXT` from the live screen/tree, never from an assumed English label. Choose the filter matching the observed property.

```sh
: "${STEP:?Set a unique step prefix}"
: "${CONTROL_TEXT:?Set observed title text}"
capture "$STEP-find" find --pid "$DRIVE_PID" --title "$CONTROL_TEXT" --depth 12 --limit 25
# Alternative, when observed text is a description rather than a title:
# capture "$STEP-find-description" find --pid "$DRIVE_PID" --description "$CONTROL_TEXT" --depth 12 --limit 25
: "${CONTROL_PATH:?Set the unique current match path}"
capture "$STEP-inspect" inspect --pid "$DRIVE_PID" --path "$CONTROL_PATH"
capture "$STEP-actions" actions --pid "$DRIVE_PID" --path "$CONTROL_PATH"
```

Verify match uniqueness, screen/account/location, role, enabled state and action support. If ambiguity remains, stop. A fresh `find` can be restricted with `--role "$OBSERVED_ROLE"`, using the actual role. Do not reuse paths after navigation. On macOS, `inspect` does not echo path; retain the command arguments as the binding.

### Open New menu and folder dialog

For each transition D0→D1 and D1→D2, rediscover the respective control using the sequence above, then, only if it advertises AXPress:

```sh
capture "$STEP-press" press --pid "$DRIVE_PID" --path "$CONTROL_PATH"
capture "$STEP-after" tree --pid "$DRIVE_PID" --depth 8 --max-nodes 1500
```

If a nonempty AXDescription was observed, the guarded variant is:

```sh
capture "$STEP-press-guarded" press --pid "$DRIVE_PID" --path "$CONTROL_PATH" --expect-description "$OBSERVED_DESCRIPTION"
```

Use one variant, not both. `--expect-description` is a substring check, not an exact identity/account guard. If the live control only supports AXShowMenu, `show-menu --pid "$DRIVE_PID" --path "$CONTROL_PATH"` is available, but use it only when inspection establishes that it opens the intended menu. Do not assume the New control uses that action. Caller verification is still required. `press` is classified external-commit even for navigation; successful input is not proof of the intended transition.

### Fill and commit the folder name

Discover the actual editable child and inspect it; do not select an outer group merely because it contains text. With `NAME_PATH` set to that fresh path:

```sh
: "${NAME_PATH:?Set observed editable name-control path}"
capture 030-name-paste paste --pid "$DRIVE_PID" --target-path "$NAME_PATH" --text 'Project Alpha'
capture 031-filled-dialog tree --pid "$DRIVE_PID" --depth 8 --max-nodes 1500
```

Require successful exact-draft verification. Guarded paste clears AXValue, checks it is empty, clicks live bounds and verifies the new text. A rejection can occur after clearing or pasting; preserve the resulting state. Stop if unsupported rather than bypassing the guard with raw typing.

Immediately rediscover and inspect both name and commit controls in the same dialog. Confirm My Drive is still the destination. Then:

```sh
: "${CREATE_PATH:?Set freshly observed commit-control path}"
capture 040-create press --pid "$DRIVE_PID" --path "$CREATE_PATH" \
  --require-value-path "$NAME_PATH" --require-value 'Project Alpha'
capture 041-created-list tree --pid "$DRIVE_PID" --depth 8 --max-nodes 1500
```

Add `--expect-description "$CREATE_DESCRIPTION"` only if that description was observed. The exact draft guard verifies text, not destination, folder type or persistence. macOS guarded press returns local action status without a `verified` field. Never interpret its absence as failure or repeat submission for that reason.

If no actionable commit button is exposed, record a blocker. A later approved alternative, only after observing that Return submits this dialog, is `focus --pid PID --path PATH` followed by `key --pid PID --key return --target-path PATH --require-value 'Project Alpha'`. Do not attempt this fallback speculatively or after an ambiguous submission.

## Refresh and persistence verification

First inspect the created entry and current root context. `find --value 'Project Alpha'` may locate the name field, while `find --title 'Project Alpha'` may locate a list entry; choose based on actual properties. Substring matches are candidate discovery only: inspect the full name and distinguish folder entries from feedback text, shortcuts or search matches.

Verify the intended tab is active and no modal/draft is open. Prefer a freshly inspected browser reload control with AXPress. Alternatively, after confirming the chosen browser uses Command-R to reload:

```sh
capture 050-refresh-before tree --pid "$DRIVE_PID" --depth 8 --max-nodes 1500
capture 051-refresh key --pid "$DRIVE_PID" --key command+r
capture 052-refresh-after tree --pid "$DRIVE_PID" --depth 8 --max-nodes 1500
```

`command+r` is supported by Blindly4's key parser. It has a PID input guard but no account/tab guard; this invocation intentionally has no draft guard because it is a refresh, not a submission. Keep reload evidence distinct from create evidence. Observe loading/reloaded content where exposed; if refresh completion cannot be established, report persistence as inconclusive.

After settling, rediscover the entry from the new tree, inspect its name/type and root context, and count matching entries only when list coverage is sufficient. Optionally open the observed folder and inspect parent/breadcrumb context, then return, recording both actions. UI evidence supports persistence across refresh; it cannot prove server-wide uniqueness or internal object identity when those are not exposed. The private simulator oracle supplies that stronger check.

## Limits, privacy and handoff

MacOS tree nodes contain pid and available summary attributes with optional children; they omit actions and paths. `find` adds paths; `inspect`/`actions` supplement capabilities. AX reads may omit unavailable attributes. macOS `truncated: false` does not rule out depth clipping, and `find` has no truncation indicator. Virtualized content, duplicate labels, delayed dialogs, inaccessible controls and browser chrome can all limit coverage. Record such limitations rather than fabricating a tree.

Blindly4 is stateless and does not lock the desktop. Preserve exclusive access externally or through a managed Sapiens computer owner. Numeric paths are volatile; trees are traversals over time, not atomic snapshots. Stop after nonzero mutations, stale identity, draft failure or uncertain commit. Read-only observation may continue to diagnose a failed mutation, but never retry the create blindly.

Raw AX captures can reveal account names, document titles, URLs and unrelated browser content. Use a synthetic account/profile; close unrelated windows. Store raw artifacts outside git in access-restricted storage, retain hashes and a manifest, and establish retention with the owner. Share redacted derivatives with consistent aliases and transformation notes. Do not capture passwords, login sessions, clipboard contents or credentials. Guarded paste temporarily uses the clipboard, another reason to reserve the desktop.

Each transition record must link identity, before JSON, discovered control details/actions, exact argv, exit/stdout/stderr, after JSON, settling observations, observed result, errors and derived simulator requirement IDs. Label records planned/observed/blocked. Deliver D0–D5 captures plus the inventory and requirements before implementation begins.

## Verified command sources

All links pin the inspected official commit:

- [Tree and search commands](https://github.com/aleski-green/blindly4/blob/1369de4cdb2893b16da23c16e5a03e2331aea916/Sources/blindly4/CLI/Commands/TreeCommands.swift)
- [Inspect, actions and guarded press](https://github.com/aleski-green/blindly4/blob/1369de4cdb2893b16da23c16e5a03e2331aea916/Sources/blindly4/CLI/Commands/ElementCommands.swift)
- [Guarded paste and key](https://github.com/aleski-green/blindly4/blob/1369de4cdb2893b16da23c16e5a03e2331aea916/Sources/blindly4/CLI/Commands/InputCommands.swift)
- [Letter/modifier parsing including Command-R](https://github.com/aleski-green/blindly4/blob/1369de4cdb2893b16da23c16e5a03e2331aea916/Sources/blindly4/Input/SyntheticInput.swift)
- [AX serialization and text matching](https://github.com/aleski-green/blindly4/blob/1369de4cdb2893b16da23c16e5a03e2331aea916/Sources/blindly4/Accessibility/AXElement.swift)
