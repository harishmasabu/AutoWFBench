# Minimal Google Drive simulation specification

2026-10-11. Proposed design for **Create a folder named Project Alpha in My Drive**. My Drive, New, the manually opened Folder menu and New folder have been observed; dialog, creation and persistence are still unobserved. No simulator implementation has been changed. See the [evidence index](captures/README.md) for current status.

## Confirmed architecture direction

Google Drive is the first simApp. Its full UI for the selected workflow is primary even though Drive APIs exist. Internal list/create operations support that UI and private evaluation; they are not an API-first agent route. simMCP can be added later as an optimization if supported capabilities are adequate. Missing or limited MCP does not reduce UI scope. Docs and Sheets are future Drive file types with distinct content models; their creation/editor behavior is not yet captured or implemented.

## Scope and fidelity boundary

Reproduce the observed My Drive root and creation-menu state. Capture the folder-name dialog, successful created-folder listing and persistence after refresh before claiming fidelity for those states. Use native semantic browser controls, then validate their actual macOS accessibility output through Blindly4. Do not return canned AX JSON directly to the agent: Alex's requirement is a real accessible interface informed by observations.

A minimal simulator needs folder creation and listing, one authenticated synthetic principal, one My Drive root, and reload-safe state. It does not need OAuth, Google APIs, sharing, uploads, document editing or a full Drive clone. Do not implement an unobserved Drive error or duplicate-name policy as if it were verified product behavior. Simulator evaluation constraints and authentic UI behavior are separate concerns.

## Three responsibilities

Respect AutoWFBench's existing dependency direction, interfaces → runtime → core, and its environment contracts/state/transport separation.

| Responsibility | Minimal design | Existing architectural connection |
| --- | --- | --- |
| Domain and verification | Authoritative per-run nodes, root identity, creation events and private predicates | `runtime/environment/state.py`; public operation definitions in `apps.py` |
| Transport and accessible presentation | Serve a local browser UI and list/create requests using the same domain state | `runtime/environment/server.py` plus a proposed UI resource layer; no second UI-owned database |
| Execution and evidence | Bind browser/session to run, serialize desktop use, capture observations, freeze and verify final evidence | `runtime/engine.py`, solution protocol and existing finalization/judging boundary |

Reference baseline: [AutoWFBench environment](https://github.com/aleski-green/AutoWFBench/tree/8e7cc5bb1002b103a2f63d73d3cd1d9de05c0552/autowfbench/runtime/environment). Existing generic simApps are not a implemented Drive UI. Proposed module additions must preserve the current contracts and dependency direction; choose exact placement during implementation, not by adding empty architectural wrappers.

## Authoritative state model

The following is a proposed minimal internal model, not an assertion about Google's API schema.

| Entity | Fields and invariants |
| --- | --- |
| Run | `run_id`, synthetic `principal_id`, `my_drive_root_id`, initial snapshot, event sequence; isolated between runs |
| Node | Immutable opaque `id`; `kind` = root/folder; exact `name`; `parent_id` (null only for root); `owner_id`; `trashed`; creation sequence |
| Create operation | Input name and parent ID from active UI context; authenticated actor from run session; validates accessible parent and input; allocates one node and records outcome |
| Event | Operation ID, actor, kind, target ID, parent ID, supplied name, success/failure, sequence; append-only private audit evidence |
| UI session | Current location, menu/dialog visibility, name draft, focus/selection, pending request and visible error; ephemeral, separate from stored nodes |

Seed a My Drive root owned by the actor, with no `Project Alpha` folder at that root. Optionally seed a different-name folder as an unrelated-content invariant. A parent ID is an identity, not the string `My Drive`; a folder elsewhere with that display name must not satisfy root checks.

For the minimal benchmark, keep domain state in the existing per-run backend for the run lifetime. Browser refresh reconstructs its list from that backend; it must not recreate the run or lose folders. Durable persistence across server restart is out of scope. A client-only array or localStorage success marker is insufficient.

List and create are the only required domain operations. Creation validates access and commits once per accepted operation. Whether separate submissions with the same name are accepted must follow observed behavior; do not silently enforce global name uniqueness just to make the benchmark easier. UI suppression of a pending double-submit should follow captures. If retry idempotency is implemented internally, its operation IDs remain private and must not conceal two distinct committed creations in the evaluation ledger.

## Accessible UI requirements

| Screen/state | Minimum functional requirement | Capture needed to settle details |
| --- | --- | --- |
| My Drive root | Expose current location and list context; identifiable creation entry point | D0 names, roles, hierarchy, state and list mode |
| Creation menu | Discoverable folder action, focus behavior, dismissal/navigation | D1 menu/control actions and focus |
| Folder dialog | Named dialog/context, writable name control, commit and cancel actions | D2 labels, default value, focus, editable role and bounds |
| Filled dialog | AX-visible exact draft `Project Alpha`; live commit control | D3 editing behavior and guard feasibility |
| Created root list | Folder entry tied to committed domain object, correct location, observed feedback | D4 row/item semantics and dismissal behavior |
| Refreshed list | Same committed object loaded from authoritative state | D5 persistence, loading and selection behavior |

Use ordinary DOM inputs/buttons/dialog/list semantics where they reproduce observed behavior; verify the resulting AX roles instead of assuming ARIA maps identically across browsers. Accessible names, enabled state, focus, actions and meaningful parent context must be realistic. Numeric AX paths, PIDs, arbitrary wrapper nodes and exact pixel coordinates need not match. Browser version, viewport and locale must be pinned for comparison.

Do not place hidden task answers, oracle results, expected parent IDs or evaluation hints in DOM/ARIA attributes, page source, client scripts or accessible text. Naturally visible folder names and location labels remain public. A form must not receive special behavior for the literal string `Project Alpha`; it should create ordinary folders according to the same rules for other valid names.

If real Drive's name field cannot support the verified paste guard, document that observation and resolve the input strategy before claiming fidelity. Making the simulator artificially easier by exposing capabilities absent from the reference must be disclosed as a deviation. Preserve observed limitations that affect the selected workflow; incidental unsupported features outside scope can remain absent.

## Private AutoWFBench predicates

Evaluate against an immutable initial snapshot `S0`, finalized authoritative state `Sf` and complete run event ledger `E`. The evaluation boundary is privileged; the agent cannot read, replace or submit those inputs. The expected name and root belong to the task fixture/oracle, not the browser's assertions.

Definitions:

- `r = S0.my_drive_root_id`, `u = S0.principal_id`, `n = "Project Alpha"`.
- `new_ids = ids(Sf.nodes) − ids(S0.nodes)`.
- `targets(S) = {x in S.nodes | x.kind=folder ∧ x.name=n ∧ x.parent_id=r ∧ x.owner_id=u ∧ not x.trashed}`.
- `commits(E)` are successful folder-create domain events, not button presses or HTTP retries.

| ID | Predicate | Failure caught |
| --- | --- | --- |
| PRE-01 | `targets(S0)` is empty; root exists, is actor's My Drive root and permits creation | Invalid or contaminated fixture; mark invalid run rather than agent failure |
| GD-01 | Exactly one `f` in `targets(Sf)` and `f.id` belongs to `new_ids` | Wrong/missing name, file instead of folder, pre-existing match or missing creation |
| GD-02 | `f.parent_id = r`, `f.owner_id = u`, `f.trashed = false` | Nested folder, other drive/account, or trashed outcome |
| GD-03 | Exactly one successful folder-create event in the entire run; its target is `f.id`, actor `u`, parent `r`, name `n` | Duplicate create then delete/rename, creation in another location, unrelated extra creation |
| GD-04 | `new_ids = {f.id}` and all pre-existing nodes retain their original domain fields | Other new objects, moving/renaming/deleting unrelated content |
| GD-05 | Privileged backend list/read after the recorded reload still includes the same `f.id` with unchanged name/parent | Optimistic UI-only folder or reload causing state reset |
| GD-06 | Trusted runner evidence records a reload and a subsequent list response/state revision containing `f.id` | Task appears complete without exercising required persistence check |

Pass requires PRE-01 valid and GD-01–GD-06 true. Name equality is exact: no silent trim, case folding or suffix tolerance in the oracle. The ASCII target avoids Unicode normalization ambiguity. Draft matching in Blindly4 and exact stored-name evaluation are separate checks.

GD-03 deliberately forbids transient unwanted duplicates even if the final list looks correct. GD-04 compares authoritative domain fields, excluding telemetry/read counters and explicitly documented system metadata. At this scope no other domain writes are needed. A future benchmark variant allowing recovery/cleanup would require different predicates; do not relax these implicitly.

GD-06 is proposed runner instrumentation, not an existing Blindly4 feature. A client report saying “refreshed” is insufficient. A page load/session event and backend read correlated by the privileged harness can establish the check; ordinary client claims cannot be the sole source. Keep this evaluation evidence separate from the agent's public trace. UI captures alone cannot prove global absence of duplicates; authoritative state and events can.

The simulator oracle does not run against the live Google account. Live discovery uses the bounded visual/AX persistence procedure in the runbook and states its coverage limits. No Google API access is introduced for the live POC.

## Evidence-to-requirement contract

For each requirement record: ID, capture step IDs, source raw JSON hashes, observed control properties, before/action/after transition, domain effect, coverage limits, confidence and implementation acceptance check. Keep the raw reference corpus restricted and redacted derivatives in a shareable corpus. Never add live account dumps to the repository by default.

Example pending requirement: `GD-UI-03` — the name field exposes the exact entered text and permits the observed guarded editing method. Its status remains **unverified** until D2/D3 captures demonstrate it. No invented AX snapshot is a valid fixture.

Separate three types of evidence: raw direct Blindly4 output, the compacted output Sapiens4 actually sees, and private benchmark state/events. Sapiens4's existing helper can drive the simulator browser through the pinned Blindly4 binary; its pagination/cache is not the raw corpus. Preserve host-level desktop ownership. An adapter between Sapiens4 and AutoWFBench's run/poll/cancel lifecycle remains future work if automated benchmark execution is required.

## Minimal implementation sequence after captures

1. **Approve the reference contract.** Review D0–D5, establish browser/locale baseline, resolve guard feasibility, record labels/roles/actions and limitations. Produce a small screen-transition specification; stop if the core interaction cannot be observed.
2. **Add the domain slice and private checks.** Define list/create contracts, per-run root/folder state, events and GD predicates. Test exact-name and parent errors, extra creates including create-then-delete, unrelated mutation, and invalid initial fixtures.
3. **Add the accessible UI.** Implement the observed root/menu/dialog/list over those same domain operations. Reload loads stored state. Match observed focus and accessibility semantics; keep task-specific expected values out of UI logic.
4. **Connect the desktop runner.** Open the isolated simulator in a dedicated macOS browser session, provide its public URL to the agent, maintain exclusive ownership, record reload evidence, and finalize state through the existing privileged benchmark boundary. If using Sapiens4, implement the run/poll/cancel adapter without exposing private oracle state.
5. **Validate fidelity and outcome.** Run the exact Blindly4 discovery/action sequence against the simulator, compare semantic observations with D0–D5 and verify all private predicates. Check cancellation and an ambiguous/failed create cannot cause an uncontrolled retry. Run repository-required tests and lock verification after code changes, alongside the desktop test.

Completion means an agent creates one correctly named folder in the actual simulated My Drive root through the accessible UI; refresh preserves the same object; private checks reject wrong parent, wrong name and extra creations. No implementation stage is authorized by this document alone.


## Phase 1 observation update

My Drive baseline is now observed. See [baseline analysis](captures/my-drive-baseline-analysis.md): New is an enabled AXButton with title/description New; the navigation includes a selected My Drive Collapsed AXRow and a My Drive AXPopUpButton. Supported actions and all create-flow transitions remain unverified. Earlier statements that no Drive controls have been observed describe the original design baseline and are superseded only for these recorded controls.

New action inspection is now observed: [New control analysis](captures/new-button-analysis.md). The exact New AXButton advertises AXShowMenu and AXScrollToVisible, not AXPress. Menu-opening behavior remains untested and requires approval before observation.

## New menu transition attempt

The authorized AXShowMenu attempt returned success, but before/immediate-after/settled trees were identical. Menu opening is unconfirmed and menu items remain unobserved. See [capture findings](captures/new-menu-attempt/findings.md). Require post-action transition evidence rather than relying on command success. No create-flow behavior is promoted to verified.

## Single-click observation update

One authorized click returned success, but the immediate tree was unchanged and the later tree lost all browser-window content. New folder search returned no matches under this compromised coverage. Menu opening remains unconfirmed. See [single-click findings](captures/new-single-click/findings.md). Treat loss of the observed web area as inconclusive observation, not proof of failed interaction or a Drive behavior to simulate.

## Manually opened menu verified

A subsequent manual-opening capture exposes focused AXMenu Folder menu and enabled AXMenuItem `New folder ^C then F` inside the Drive web area. See [verified menu findings](captures/manual-menu-open/findings.md). This verifies the open-menu state, not the earlier automated opening attempts or item action support. Creation-dialog behavior remains pending.

## Future Docs and Sheets extension

Extend the Drive entry model with document and spreadsheet kinds sharing identity, name, parent and ownership while keeping content semantics separate. Capture creation, listing and opening before implementing them; editor depth, formulas/formatting, ACLs and exports need review-driven scope. Their menu presence does not prove editor behavior. Folder creation remains the first end-to-end gate.

## Latest New folder detail

The follow-up read-only find returned one enabled AXMenuItem `New folder ^C then F`, advertising AXCancel, AXScrollToVisible and AXShowMenu; AXPress was not advertised. See [current inventory](captures/control-inventory.md). Earlier addenda describing item actions as unknown apply only to their earlier captures. No item activation or dialog behavior has been verified.
