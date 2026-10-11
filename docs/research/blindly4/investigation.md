# Blindly4 investigation for AutoWFBench

Source inspection date: 2026-10-11. Status: repository investigation complete; live Jira discovery has not been performed. During the initial source investigation no application accounts were accessed, dependencies installed, binaries built, or implementation code modified. Later manual build/capture work is documented in the Drive evidence index.

Current target: [Google Drive](../google-drive/discovery-plan.md) replaced the unexecuted Jira proposal. See [current evidence](../google-drive/captures/README.md). The source investigation below remains valid at the pinned commits.

## Decision

Use the official Blindly4 CLI to observe a dedicated Jira test environment through the browser's native accessibility exposure. Preserve raw JSON, then derive screen and transition specifications for an accessible simulator backed by AutoWFBench state. Do not build a simulator by replaying fixed accessibility dumps: the interface must respond to actions and expose the resulting state through the operating system's accessibility backend.

The executable discovery procedure is in [historical Jira UI discovery plan](historical/jira-discovery-plan.md). All Jira screens and interaction outcomes below are proposed investigation targets, not observed Jira findings.

## Source baseline and relationship

| Repository | Inspected commit | Relationship |
| --- | --- | --- |
| [Blindly4](https://github.com/aleski-green/blindly4/tree/1369de4cdb2893b16da23c16e5a03e2331aea916) | `1369de4cdb2893b16da23c16e5a03e2331aea916` | Official source of CLI and native accessibility backends |
| [Sapiens4](https://github.com/aleski-green/sapiens4/tree/71e93cb3de00183c263ec440919b313cf92ed8af) | `71e93cb3de00183c263ec440919b313cf92ed8af` | Its `blindly4` git submodule pins exactly the inspected Blindly4 commit |
| [AutoWFBench](https://github.com/aleski-green/AutoWFBench/tree/8e7cc5bb1002b103a2f63d73d3cd1d9de05c0552) | `8e7cc5bb1002b103a2f63d73d3cd1d9de05c0552` | Benchmark runtime and stateful simulated operations; UI discovery integration is proposed |

The README and source agree that `tree` emits JSON. There is no registered XML export, screenshot command, native MCP server, persistent snapshot service, or change-stream command. `schema` emits command metadata, not a formal JSON Schema defining every response. Its `schemaVersion: 2` must not be mistaken for a version field inside each tree response. Capture the command schema once per binary/build and archive it with observations.

## Commands and execution model

The [Swift registry](https://github.com/aleski-green/blindly4/blob/1369de4cdb2893b16da23c16e5a03e2331aea916/Sources/blindly4/CLI/CommandRegistry.swift) and [Windows registry](https://github.com/aleski-green/blindly4/blob/1369de4cdb2893b16da23c16e5a03e2331aea916/windows/CLI/Registry.cs) register the same main verbs.

| Class in command metadata | Commands | Discovery use |
| --- | --- | --- |
| read-only | `schema`, `apps`, `tree`, `show`, `find`, `focused`, `inspect`, `actions` | Discover app PID, bounded observations, target details and advertised actions |
| ui-mutation | `request-permission`, `activate`, `open`, `focus`, `show-menu`, `scroll-to`, `set-value`, `set-selected-text`, `click`, `type`, `paste`, `scroll` | Change focus/navigation, edit drafts, reveal content; actual effects depend on target |
| external-commit | `press`, `key` | Can submit or send; even navigation through a button uses this classification |

Treat command risk as metadata, not proof that clicking a control cannot commit. A coordinate click on Submit can have an external effect. `open` accepts HTTP, HTTPS and Slack URLs; it does not bind navigation to a selected browser PID. Prefer an operator-prepared Jira tab for the initial POC.

Blindly4 runs one command per process and keeps no cross-command cache, lease, snapshots or logging. The caller must serialize the entire workflow, persist evidence, compute differences, and decide when a screen has settled. Removed commands such as `snapshot`, `changes`, `serve` and `workflow` cannot be used. `--profile` writes diagnostic data to stderr.

## Actual accessibility JSON contracts

These are descriptive source contracts, not measured Jira payloads.

### macOS

[TreeCommands.swift](https://github.com/aleski-green/blindly4/blob/1369de4cdb2893b16da23c16e5a03e2331aea916/Sources/blindly4/CLI/Commands/TreeCommands.swift) calls the recursive serializer in [AXElement.swift](https://github.com/aleski-green/blindly4/blob/1369de4cdb2893b16da23c16e5a03e2331aea916/Sources/blindly4/Accessibility/AXElement.swift).

`tree [--pid PID] [--depth N] [--max-nodes N]` returns `{ "tree": Node, "truncated": boolean }`. Defaults are depth 4 and 250 nodes, including the root. Depth 0 returns the root summary.

| Node field | Source semantics |
| --- | --- |
| `pid` | Integer, inserted for every node |
| `role`, `subrole`, `title`, `description`, `value`, `identifier` | Present only when the corresponding AX attribute can be read; values converted by `textValue` |
| `enabled`, `focused`, `selected` | Native numeric/boolean AX values, serialized through Foundation; absent is unknown, not false |
| `position` | Usually `{x,y}` from AXPosition |
| `size` | Usually `{width,height}` from AXSize |
| `children` | Ordered array when nonempty and traversed; omitted for leaves or bounded nodes |

`textValue` preserves strings and NSNumber values, converts URLs to strings and AX geometry into objects, and falls back to a string representation. Do not define `value` as string-only. Attribute read failures can be omitted silently. A missing field does not prove absence of a capability.

Tree nodes do **not** contain `path`, `actions`, or `attributes`. `find` returns `{matches: [Detail...]}` with numeric dot paths; `inspect` and `focused` return one Detail without a path. Detail adds sorted `attributes` and `actions`; `actions --path P` returns `{path:P,actions:[...]}`. `find` has default depth 8 and limit 25 but no truncation flag on macOS. Role filtering is exact after adding an `AX` prefix if absent; other filters are case-insensitive substring matches.

At the application root, AXWindows are appended if AXChildren contains no AXWindow. Below that, traversal uses AXChildren. Paths index that traversal, starting with the empty root path and then `0`, `0.1`, etc. Paths are transient observations, not stable IDs.

**Coverage caveat:** macOS `truncated` is simply `remaining == 0`. It does not report depth exhaustion and may be true when the budget exactly equals the complete tree size. Missing children and `truncated: false` do not establish a complete tree. Record depth and node budgets independently.

### Windows

[Node.cs](https://github.com/aleski-green/blindly4/blob/1369de4cdb2893b16da23c16e5a03e2331aea916/windows/Accessibility/Node.cs) reads Microsoft UI Automation; [Desktop.cs](https://github.com/aleski-green/blindly4/blob/1369de4cdb2893b16da23c16e5a03e2331aea916/windows/Accessibility/Desktop.cs) handles traversal.

The wrapper is also `{tree, truncated}`, with the same defaults. Windows validates depth 0–64 and node budget 1–10000. The synthetic application root contains `pid`, `role: "AXApplication"`, `title`; descendants use UIA Control View beneath visible process windows.

Non-root nodes include `title`, `description` (HelpText or Name), `identifier` (AutomationId), `subrole` (native ControlType name), `value` (string or null), boolean states, `actions`, and `attributes`. Geometry is omitted when invalid. Tree nodes do not receive a path. `find` supplies paths and a truncation flag; `inspect` does not supply a path. `find` applies substring matching to role as well as other filters, unlike macOS exact role matching.

UIA roles map to AX-style names for compatibility: Edit becomes AXTextField or AXSecureTextField, Document becomes AXTextArea, TabItem becomes AXRadioButton. Unsupported role types fall back to AXGroup. This is a lossy mapping, so preserve `subrole` and platform.

`AXPress` maps to InvokePattern, SelectionItemPattern or TogglePattern; `AXShowMenu` to ExpandCollapsePattern; `AXScrollToVisible` to ScrollItemPattern. ValuePattern supplies values first, otherwise TextPattern reads at most 65536 characters. Password values are withheld. A writable guarded text target requires enabled, non-password text/combobox semantics and writable ValuePattern. A readable TextPattern alone is insufficient.

Windows sets `truncated` when traversal encounters a child beyond depth or node limits. It also caps children per node at 10000 and has a 25-second process deadline for normal commands. These differ from macOS and should be recorded as backend behavior, not Jira behavior.

### Errors and action results

On macOS, usage errors exit 64 and print text/help to stderr; accessibility errors exit 77 with stdout JSON `{code,error}`. Codes include `accessibility_permission_denied`, `focus_unavailable`, and `accessibility_error`. Unexpected errors exit 1 with an error object. Windows emits JSON errors for usage too and uses `unexpected_error` for unexpected failures. Preserve stdout, stderr and exit status separately; not every stdout is valid JSON (`show` and help are text).

Mutation result shapes also differ. macOS guarded `press` does not add a `verified` field; Windows does. A successful press reports local action success, not persistence or business success. A failed `--require-selected` check happens **after** pressing. A timeout may follow input. Never retry a commit merely because the command returned an error.

## Backend limits and operating environment

macOS uses Apple Accessibility through ApplicationServices and AppKit; synthetic input is system-wide. Build requires macOS 13+ and Swift 6. The host terminal/agent needs Accessibility permission. No screenshot or screen-recording facility is present in these commands; Screen Recording is not a prerequisite for this AX-only plan. Optional screenshots would require separate tooling and permission review.

Windows uses .NET 10, UIAutomation, Win32 input and clipboard facilities, with native x64/ARM64 builds. Run in the unlocked interactive session at the target's integrity level. Elevated apps, other sessions and secure desktop are outside documented support. `request-permission` does not elevate or grant access. See [Windows documentation](https://github.com/aleski-green/blindly4/blob/1369de4cdb2893b16da23c16e5a03e2331aea916/windows/README.md).

Browser accessibility is the observable interface; it is not the complete DOM, network trace, database, or pixel layout. Jira rich editors, overlays, virtualized lists, browser chrome and delayed loading must be investigated. Do not assume that a visible control is exposed, that every loaded item is traversed, or that a role implies editability. Missing exposure is a finding to preserve.

Passing `--pid` guards synthetic input against another foreground application, but does not distinguish two Jira tabs inside the same browser process. Verify tenant, project, issue key and screen on every transition. Rediscover paths immediately before mutations. Clipboard-based paste temporarily affects shared clipboard state. Read-only raw trees can contain sensitive text; use synthetic test data and a dedicated profile.

## Sapiens4 integration

[Sapiens paths](https://github.com/aleski-green/sapiens4/blob/71e93cb3de00183c263ec440919b313cf92ed8af/sapiens/paths.py) select `blindly4/.build/release/blindly4` or `.build/windows/blindly4.exe`; `SAPIENS_BLINDLY_BINARY` overrides the binary. [commands.py](https://github.com/aleski-green/sapiens4/blob/71e93cb3de00183c263ec440919b313cf92ed8af/sapiens/computer/commands.py) invokes the executable using subprocesses, captures output, uses a 30-second timeout, and exposes `blindly COMMAND` and `read` helpers. This is CLI integration, not MCP.

In managed workspaces, the helper reads `host-control.json`, acquires the computer through the local host, and marks `.computer-used`. The host's [Service](https://github.com/aleski-green/sapiens4/blob/71e93cb3de00183c263ec440919b313cf92ed8af/sapiens/corpora/host/service.py) enforces a single active computer owner/turn and keeps ownership during focus restoration. Outside a managed workspace the helper does not acquire a lock. Neither that lock nor Blindly4 excludes unrelated human or external automation input.

There are three distinct observation layers:

1. Raw Blindly4 tree, required discovery evidence.
2. `blindly` helper output, flattened and bounded to roughly 48000 characters; geometry, attributes and tree actions are discarded by its tree compaction.
3. [reader.py](https://github.com/aleski-green/sapiens4/blob/71e93cb3de00183c263ec440919b313cf92ed8af/sapiens/computer/reader.py) snapshots, which call tree with 5000 nodes, filter a requested subtree locally, omit anonymous containers and paginate. Snapshots reside in `.computer-reads`, expire after 90 seconds, and retain original numeric paths. There is no native Blindly4 subtree read.

Archive raw evidence at the research caller, outside those ephemeral snapshots. Preserve separately what the agent actually saw when evaluating Sapiens4. The compacted snapshot is not a replacement for raw evidence or a guarantee of freshness. The current integration can control a future simulator opened in a supported browser if its accessible UI supports the observed actions. It cannot make an HTTP-only simApp accessible without a UI.

## Mapping observations to AutoWFBench

The existing [runtime environment](https://github.com/aleski-green/AutoWFBench/tree/8e7cc5bb1002b103a2f63d73d3cd1d9de05c0552/autowfbench/runtime/environment) serves generic simulated operations, in-memory scenario state and evidence. It does not currently provide a faithful Jira browser UI or a Blindly4 discovery importer. The following are proposed additions, not implemented capabilities.

| Discovery evidence | Simulator requirement | Architecture destination |
| --- | --- | --- |
| Issue key, fields, comment records and allowed transitions | Jira domain model with identity, validation and permission rules | Extend environment state and operation handlers |
| Accessible labels, roles, states, hierarchy and editable controls | Browser UI using semantic controls and appropriate accessibility attributes | New UI presentation layer over the same state |
| Dialog open/close, focus changes, menus and validation errors | UI state machine distinct from persisted domain state | UI layer; shared domain validation |
| Created issue, new comment, status after reload | Authoritative operation and event evidence | Environment state/events and finalization checks |
| Loading, stale views, ambiguous commit response | Explicit reproducible scenario variants, separated from incidental live timing | Runtime fault injection and scenario seeds |
| Before/after raw trees, commands and observed outcomes | Versioned reference fixtures with coverage and provenance | Research fixture corpus; never embedded hidden answer state |
| Sapiens observation and action sequence | Desktop-capable solution adapter implementing benchmark lifecycle | Solution boundary and isolated interactive runner |

Run the simulator UI and API against one authoritative state. A UI click must call the same domain operation that an API adapter would use; do not maintain a second inconsistent UI database. Keep evaluation-only facts out of DOM, accessibility labels, agent filesystem and network. Authoritative success is checked from finalized runtime evidence, not a toast or the agent's claim.

The existing solution lifecycle uses `/runs`, polling and cancellation. A future Sapiens adapter must translate those into a managed Sapiens execution, bind the per-run simulator endpoint, own the desktop, cancel safely and isolate runs. This adapter and desktop runner are still design work. Retain existing runtime evidence freezing and judging boundaries. A browser process running in a headless server alone is not enough for native desktop AX observation.

## Jira proof of concept and acceptance

Use W23, one support ticket per case, as the initial business slice. Investigate search for a synthetic case marker, create one issue if absent, add a follow-up comment to that issue, and perform one permitted status transition. Gmail ingestion, attachments and full cross-application deduplication remain subsequent coverage; this Jira POC alone does not validate W23 end to end.

For every interaction, require an identified screen, bounded before tree, freshly inspected target and actions, exact attempted command, result/exit status, after tree, and evidence-backed expected domain effect. Capture an incomplete form, an unavailable transition/permission variant, and an unreadable or delayed control where encountered. Do not invent Jira errors or intentionally disrupt a production service.

A POC passes discovery when each selected transition is observed repeatedly from a documented starting state or marked blocked with exact evidence; every proposed simulator requirement links to a capture; and all gaps are explicit. A later implementation passes fidelity when Blindly4 can discover and operate the simulated controls, recover from the captured error conditions, and produce the correct authoritative state. Compare semantic roles, labels, supported actions and transitions; do not require volatile PIDs or child indexes to match.
