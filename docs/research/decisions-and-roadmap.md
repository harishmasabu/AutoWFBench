# Decisions and implementation roadmap

## Confirmed direction

Alek's clarification, relayed by the project owner, establishes **simApp-first**. Every selected application defaults to a functional simulated UI, even if its real API exists. Implement the full UI behavior needed for the selected workflow; this does not require cloning every unrelated feature of the product. simMCP can be added later where the application supports suitable MCP capabilities, as an optimization over the same domain state. If MCP is unavailable or limited, the full simulated UI stays primary.

Google Drive is the first simApp. Docs and Sheets must be represented as files in its future scope. The original API/CLI/MCP research remains evidence for business semantics, permissions, data models and optional future integration fidelity. It no longer selects an API-first execution strategy. The repository's pre-existing generic HTTP simApp operations are a backend baseline, not the newly required accessible application UI.

## Proposed architecture details

Retain authoritative run-local domain state, UI over domain operations, private outcome predicates and isolated candidate execution. Proposed durable state/resume, logical time, event/fault scheduling and broader application models are specified in [Task 3](task3/proposed-simulation-architecture.md), not implemented here. UI fidelity, business-state correctness and optional simMCP performance should be reported separately.

DFS is Alek's exploration analogy, not a confirmed technical requirement. A useful proposed record is an observed state node plus authorized action edge and verified/inconclusive outcome. Stable semantic identity must not be confused with transient AX child paths. Ask whether actual DFS search, conceptual exploration or a coverage metric is intended before choosing an algorithm; no autonomous backtracking or retry policy follows from this comment.

## Delivery gates

1. **Research review, current PR.** Review original ten-persona/24-workflow research, provider evidence, architecture gaps and live UI findings. No simulator implementation or reusable tooling changes are included.
2. **Complete Drive observation.** Resolve safe New folder activation; capture dialog, draft and commit semantics under explicit authorization. Create one Project Alpha folder only with approval, then refresh and verify. Preserve ambiguous outcomes without retrying writes.
3. **Implement first simApp.** Add per-run Drive root/folder state, accessible workflow UI and private predicates for exact name, root identity, one creation, no unrelated changes and refresh persistence. Verify using Blindly4 in a dedicated isolated desktop.
4. **Integrate Sapiens execution.** Implement run/poll/cancel adapter, desktop ownership, evidence capture, isolation and independent finalized-state scoring. Preserve existing benchmark behavior.
5. **Extend Drive to Docs and Sheets.** Add file kinds sharing identity/name/parent/ownership plus separate content models. Capture creation/listing/opening and agree editor depth before implementation. Menu exposure alone is not proof of document/spreadsheet editing semantics.
6. **Expand workflows and infrastructure.** Select further SMB workflows and build simApps. Add required durable resume, logical time, events and fault cases; optionally add simMCP when supported, without dropping primary UI behavior.

## Current implementation boundary

Source investigation and research documents are complete enough for review. Live discovery is partial. Local operator-run capture helpers exist but remain outside this documentation-only PR. No Google Drive simulator, Docs/Sheets model, Sapiens benchmark bridge or generalized benchmark clock/scheduler was implemented. Pre-existing source features are documented as baseline, not credited as changes in this PR.

Private predicates are design specifications for the privileged evaluator. Later deployment must keep evaluator files/state/secrets outside the agent's filesystem and reachable endpoints; public research text is not itself an isolation mechanism.
