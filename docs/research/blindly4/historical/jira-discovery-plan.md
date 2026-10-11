# Jira UI discovery execution plan

**Historical, superseded plan; no live Jira discovery was performed.** Google Drive is the current first simApp target.

Prepared 2026-10-11. Ready for a separately authorized discovery session. No commands in this plan have been executed against Jira. This is a runbook with command templates, not an unattended script: live paths and accessible labels must be discovered at each step.

Use with the [source investigation](../investigation.md). The initial target is Jira Cloud in a dedicated desktop browser profile on macOS. Windows is a second backend validation pass. If the intended deployment is Jira Data Center, record that change and investigate it separately.

## Required setup and authorization

| Requirement | Owner and completion evidence |
| --- | --- |
| Permission to inspect the named Jira test tenant and retain accessibility observations | Tenant owner approves tenant URL, project/space key, data retention location and participants |
| Permission to create test issues, add comments, edit fields and transition test issues | Explicit scope for the later session; current request authorizes planning only |
| Dedicated test identity and synthetic data | Owner signs in manually and completes MFA before recording; no credentials in command files or trees |
| Configured project/space and workflow | Record project type, issue/work types, fields, transition names, permission scheme and relevant automation |
| Browser profile and desktop | One Jira tab, fixed locale/zoom/viewport, supported browser/version, no other user or automation touching the desktop |
| macOS Accessibility access | Grant the actual terminal/agent host in Privacy & Security → Accessibility; verify a read of the test browser |
| Windows alternative | Windows 10/11 unlocked interactive desktop, same integrity level, .NET SDK 10 for build or a trusted self-contained build |
| Artifact storage | Restricted local directory for raw trees, redacted copies for sharing, owner-defined retention and deletion policy |

A dedicated disposable Jira test site/project is sufficient; do not assume access to a paid Atlassian sandbox feature. Select a currently supported browser from [Atlassian's browser support page](https://support.atlassian.com/atlassian-account/docs/supported-browsers-for-atlassian-cloud-products/), then record the exact build. Supported browser status does not guarantee complete AX exposure.

The tenant owner should grant the test actor only the permissions needed for the chosen operations: browse, create, edit, comment, assign if tested, and transition. Issue security and workflow conditions also affect availability. Record the current names shown by the tenant; current Atlassian docs use space/work-item terminology in some places. See [space access permissions](https://support.atlassian.com/jira-cloud-administration/docs/permissions-for-company-managed-projects/). A second restricted actor is useful for permission-denial observations. If the plan/tier cannot provide the required permission variant, mark it untested. No Jira API token or OAuth scope is needed for this UI-only investigation.

Use a synthetic marker such as `AUTOWF-DISCOVERY-R01-CASE01`, with a new run suffix per repeat. Configure notifications, integrations and automation so test writes do not reach real customers. Do not delete existing issues or change production permission schemes to create test conditions.

## Stage 1 Build and establish provenance

These are future setup commands. Run from the existing Blindly4 checkout after setup is authorized. They build locally but do not install a global binary.

```sh
cd /path/to/discovery-workspace/work/repositories/blindly4
git rev-parse HEAD
# Expected inspected baseline: 1369de4cdb2893b16da23c16e5a03e2331aea916
swift --version
swift build -c release
./.build/release/blindly4 --self-test
./.build/release/blindly4 schema
./.build/release/blindly4 request-permission
```

Confirm `trusted: true`; requesting permission alone does not establish it. The source has no external Swift package dependency declarations. If the toolchain is missing, record the exact error and arrange installation separately. Do not silently change the pinned source revision.

Windows equivalent, from the repository root in PowerShell:

```powershell
.\windows\build.ps1
.\.build\windows\blindly4.exe --self-test
.\.build\windows\blindly4.exe schema
```

The build defaults to host architecture; use `-Runtime win-x64` or `-Runtime win-arm64` if explicitly targeting one. The optional `--integration-test` opens a local WinForms fixture and uses focus/input; run only in the reserved interactive session before Jira discovery. It is not permission-free desktop observation.

Record OS/build, browser/build, Blindly4 commit and binary hash, command schema, locale, viewport/zoom, timestamp/timezone, actor alias, tenant alias, project configuration and planned test cases in `manifest.md`. Use Sapiens4 commit `71e93cb3de00183c263ec440919b313cf92ed8af` if testing that integration. Set `SAPIENS_BLINDLY_BINARY` to the verified binary before starting its host if using the separate checkout.

## Stage 2 Establish identity and capture raw evidence

The following shell helpers are a copyable macOS runbook fragment. They store each invocation's three result channels, and return the original exit status. Set a unique directory for each session. Never use shell tracing while inspecting account-bearing data.

```sh
BLINDLY='/path/to/discovery-workspace/work/repositories/blindly4/.build/release/blindly4'
DISCOVERY_RUN='/path/to/discovery-workspace/work/jira-discovery/R01'
umask 077
mkdir -p "$DISCOVERY_RUN"
capture() {
  local label="$1"
  shift
  if [ -e "$DISCOVERY_RUN/$label.exit" ]; then
    printf 'Capture label already exists; use a new label\n' >&2
    return 64
  fi
  printf '%s\n' "$@" > "$DISCOVERY_RUN/$label.argv.txt"
  date -u '+%Y-%m-%dT%H:%M:%SZ' > "$DISCOVERY_RUN/$label.started.txt"
  "$BLINDLY" "$@" > "$DISCOVERY_RUN/$label.stdout" 2> "$DISCOVERY_RUN/$label.stderr"
  local result_code=$?
  printf '%s\n' "$result_code" > "$DISCOVERY_RUN/$label.exit"
  date -u '+%Y-%m-%dT%H:%M:%SZ' > "$DISCOVERY_RUN/$label.ended.txt"
  return "$result_code"
}
capture 000-schema schema
capture 001-apps apps
```

Read `001-apps.stdout`, identify the dedicated browser, and set `JIRA_PID` to its observed PID (not an example number). Raw macOS `apps` lists name, pid and bundleId; Windows app metadata differs. The browser PID is application identity only. Record the active tab/window title, tenant, project/space, issue key or create-dialog state separately; if AX does not expose URL, document how the operator verified it. Never infer tenant identity from a generic browser title.

```sh
: "${JIRA_PID:?Set JIRA_PID from the observed apps result}"
capture 002-before tree --pid "$JIRA_PID" --depth 8 --max-nodes 1500
capture 003-outline show --pid "$JIRA_PID" --depth 8
capture 004-focus focused --pid "$JIRA_PID"
```

Run each command separately and inspect it before advancing. A missing focus does not invalidate an otherwise useful tree; record it as a separate error. Parse tree stdout as JSON only after checking exit 0. Preserve original bytes. Derive paths by the ordered child arrays without filtering/reindexing, or use fresh `find` results.

If target coverage is missing, try one larger read (depth 12, max-nodes 5000), then a targeted `find`. Neither is a native subtree read. If still missing, record a coverage gap and stop that action. Scrolling is a separate mutation with its own before/after evidence. Do not claim full-screen completeness from `truncated: false`.

## Stage 3 Discover and perform one action at a time

The operator fills variables from actual observations. Exact roles, labels and numeric paths below are deliberately not invented.

```sh
: "${TARGET_LABEL:?Set the label observed on the intended Jira control}"
capture 010-find find --pid "$JIRA_PID" --title "$TARGET_LABEL" --depth 12 --limit 25
# If title is empty, use the observed description with --description instead.
: "${TARGET_PATH:?Set the unique fresh path after reviewing find output}"
capture 011-inspect inspect --pid "$JIRA_PID" --path "$TARGET_PATH"
capture 012-actions actions --pid "$JIRA_PID" --path "$TARGET_PATH"
```

Inspect role, enabled state, label, enclosing screen and available actions. If more than one match exists, resolve it by screen/context; do not take the first match automatically. A path from a previous action is not acceptable. If the page changes during this sequence, rediscover.

After the later session has authorization for the specific effect, a supported button/navigation action can use:

```sh
capture 013-action press --pid "$JIRA_PID" --path "$TARGET_PATH"
capture 014-after tree --pid "$JIRA_PID" --depth 8 --max-nodes 1500
```

Do not execute those lines as a blind batch. Inspect the action result; if it fails, capture the resulting screen before deciding recovery. Use `--expect-description` only when the actual description has been observed; it is a substring guard, not a complete identity check.

For a writable draft, discover and inspect the editable child, not its container. Example guarded text entry and submission, after fresh discovery of both controls:

```sh
: "${DRAFT_PATH:?Set the live editable control path}"
: "${DRAFT_TEXT:?Set approved synthetic text}"
capture 020-paste paste --pid "$JIRA_PID" --target-path "$DRAFT_PATH" --text "$DRAFT_TEXT"
# Review result, then rediscover the draft and Submit paths before continuing.
: "${SUBMIT_PATH:?Set the freshly inspected Submit control path}"
capture 021-submit press --pid "$JIRA_PID" --path "$SUBMIT_PATH" \
  --require-value-path "$DRAFT_PATH" --require-value "$DRAFT_TEXT"
capture 022-after tree --pid "$JIRA_PID" --depth 8 --max-nodes 1500
```

A one-field draft guard does not verify every field in a create form. Inspect the full form, project, type, summary and destination before Submit. A rich editor may fail the guarded paste precondition; record that limitation. One bounded rediscovery of the real editable child is allowed for a verified pre-input wrong-control rejection. Do not bypass exact-draft or focus failures with unguarded Return. If a key is the only verified submission method, use `key --pid PID --key return --target-path PATH --require-value TEXT` after focusing and verifying that same control.

For asynchronous UI, capture immediate state and then fresh reads at approximately 1, 2 and 5 seconds, stopping early when the intended semantic outcome is visible. Store every observation and its timestamp. These are proposed research bounds, not Jira timing guarantees. Stop at the bound and mark unsettled; do not resubmit. A success toast, empty editor or exit 0 alone does not prove the issue/comment persisted: reopen the test issue and inspect the stored result in a separately recorded navigation step.

## Stage 4 Jira scenario matrix

All rows are hypotheses to test, not claims of observed screen labels. Test one browser/backend first; repeat the core path twice with new case markers, then validate Windows separately if cross-platform fidelity is required.

| Case | Starting state and operation | Evidence and simulator requirement to derive |
| --- | --- | --- |
| J01 Identify and search | Signed-in test project; search unique marker that does not exist | Search field semantics, result/empty state, loading and keyboard focus |
| J02 Open create form | No existing issue for marker; open Create | Dialog hierarchy, default values, project/type selectors, required fields and dismissal |
| J03 Required-field error | Blank required summary; attempt create once | Actual validation trigger, error location/text/role, focus change; prove no new issue |
| J04 Create valid issue | Fill synthetic summary/description and submit once | Editable roles, guard feasibility, success state, generated key; reopen to verify persistence |
| J05 Deduplication decision | Search the exact marker again | Existing issue discovery; choose it rather than create a duplicate; this is workflow logic, not assumed Jira uniqueness enforcement |
| J06 Add follow-up | Open that issue; enter and submit one synthetic comment | Editor/button semantics, comment identity/content and persistence after reopening |
| J07 Transition | Observe available status actions, choose one allowed transition | Menu/control roles, optional transition form, resulting status and activity evidence |
| J08 Restricted actor | Owner-prepared second actor with limited rights | Hidden/disabled control or actual denial; capture what occurs without assuming a particular error presentation |
| J09 Cancel and revisit | Draft form/comment then cancel; navigate back | Draft retention/loss, focus restoration and unchanged persisted state |
| J10 Coverage stress | Owner-seeded long description/comment list; bounded scroll | Virtualization, truncation, inaccessible editor or delayed content; record constraints |

Attachments, assignment and reopen transitions can extend the plan after the core is observed. Network failures/rate limits must not be manufactured against a shared service; capture naturally encountered failures, or mark their simulator behavior as an unvalidated design proposal.

## Observation record and evidence directory

Keep raw files under each session and derive one transition record per action. A proposed record shape follows; it is our research schema, not Blindly4's output schema.

```json
{
  "record_version": 1,
  "case_id": "J04",
  "step_id": "R01-J04-03",
  "provenance": {
    "blindly_commit": "1369de4cdb2893b16da23c16e5a03e2331aea916",
    "backend": "macOS AX",
    "browser_version": "record-at-run-time",
    "actor_alias": "test-writer",
    "tenant_alias": "test-site"
  },
  "screen": {
    "application_pid": null,
    "window_title": null,
    "verified_url_or_route": null,
    "project_key": null,
    "issue_key": null,
    "screen_kind": "create-dialog"
  },
  "before": {"raw_file": null, "depth": 8, "max_nodes": 1500, "coverage_notes": []},
  "target": {"observed_path": null, "role": null, "label": null, "actions_file": null},
  "action": {"argv_file": null, "risk": "external-commit", "authorized_effect": null},
  "result": {"stdout_file": null, "stderr_file": null, "exit_code": null},
  "after": {"raw_files": [], "settled": false, "verification_file": null},
  "observed_transition": null,
  "errors_and_limits": [],
  "simulation_requirements": [],
  "classification": "planned"
}
```

Replace planned with observed, blocked or untested as appropriate. Every available-control inventory should include path-at-capture, semantic role/name, value type, enabled/selected/focus state, advertised actions, editability evidence, enclosing screen, capture reference and coverage limits. Unknown and absent are distinct from false. Do not manufacture a permanent selector from the numeric path.

For each simulator requirement assign an ID, source step IDs, preconditions, action, visible response, domain effect, failure variants and acceptance test. Example **hypothesis pending J03**: `JIRA-VAL-001` requires the observed required-summary validation with no persisted issue. Do not promote it to an observed requirement until J03 captures the actual behavior.

Preserve raw captures in restricted storage; share redacted derivatives with consistent aliases. Hash retained files, record any transformations, and keep PIDs/timestamps for provenance while excluding them from semantic equality comparisons. Do not convert JSON to XML unless a future consumer specifically needs it; any conversion must be labelled derived and keep the raw JSON.

## Sapiens4 comparison pass

After direct CLI discovery, use one managed Sapiens turn with exclusive computer ownership to repeat a small authorized navigation/editing case. Its supported wrapper syntax is:

```sh
python /path/to/sapiens4/sapiens/computer/commands.py blindly tree --pid "$JIRA_PID" --depth 8 --max-nodes 1500
python /path/to/sapiens4/sapiens/computer/commands.py read --pid "$JIRA_PID" --depth 12
# Use returned snapshot token and next_offset only for reading the same snapshot.
python /path/to/sapiens4/sapiens/computer/commands.py read --snapshot "$SNAPSHOT_TOKEN" --offset "$NEXT_OFFSET"
```

Run from the managed agent workspace so `host-control.json` is found. Do not start a competing raw capture process while another turn owns the computer. Schedule raw captures and wrapper captures serially within the same controlled session; document elapsed time and any resulting state difference. Compare omitted fields, path retention, text clipping and pagination. Cached reader pages are observations of the earlier snapshot, not fresh targets for mutation.

## Completion and handoff

The discovery operator delivers the manifest, raw captures, control inventory, per-action records, semantic transition table, limitations log and simulator requirement list. Every selected case is either evidenced or explicitly blocked/untested. Repeated trials should produce the same semantic business outcomes; record UI variations instead of hiding them.

The architecture handoff maps requirements to AutoWFBench's domain state/operations, a new accessible browser UI, scenario fixtures/fault variants, authoritative evaluation checks, and a proposed Sapiens solution adapter. A later fidelity test must use Blindly4 against the simulator in an interactive desktop and verify domain outcomes from runtime evidence. Screenshots or static tree replays alone do not satisfy this test.

Stop conditions: uncertain tenant/issue identity, stale or ambiguous target, failed draft guard, nonzero mutation result, desktop ownership conflict, unexpected external destination, or unsettled outcome. Preserve evidence and report the exact blocker. Do not retry ambiguous commits. Current completion is the source report and this executable plan; live captures and simulator implementation remain intentionally unexecuted.
