# Project overview for review

This documentation-only package connects completed research to the current Google Drive proof of concept.confirmed the simApp-first direction; detailed implementation designs remain proposals. No simulator implementation is included.

## Reading order

1. [Ten personas and 24 workflows](customer-workflows/customer-and-workflow-research.md).
2. [Application inventory](task3/application-inventory.md) and [capability matrix](task3/application-capability-matrix.md).
3. [Simulation requirements](task3/simulation-requirements.md) and [architecture investigation](task3/proposed-simulation-architecture.md).
4. [Blindly4 investigation](blindly4/investigation.md).
5. [Drive runbook](google-drive/discovery-plan.md), [design](google-drive/simulation-specification.md) and [evidence index](google-drive/captures/README.md).
6. [Confirmed direction and proposed roadmap](decisions-and-roadmap.md).

Evidence labels: **observed** means a recorded live capture; **source-verified** means inspected repository code or cited documentation; **operator-reported** means manual confirmation; **proposed** means unimplemented design. Provider verification labels are defined in the capability matrix.

## Confirmed simApp-first direction

simApp is always the default, including applications with APIs. Here simApp means a functional simulated application with its full UI for the selected workflow, not just the pre-existing generic HTTP simApp endpoints. simMCP may be added later as an optimization where MCP capabilities exist and are adequate. Missing or limited MCP leaves the simulated UI primary. Google Drive is first; Docs and Sheets are future Drive file types. API research remains valuable for domain behavior and constraints, not for selecting an API-first execution path.

## Original objective

Build AutoWFBench environments that evaluate whether Sapiens4 can understand natural-language requests and complete realistic SMB business processes. The motivating processes involve incomplete information, delayed replies, changing circumstances and multiple applications. A conventional workflow engine can also persist, wait and retry; the research does not establish an intrinsic AI advantage. The proposed benchmark should measure how well Sapiens handles those processes and exceptions, rather than merely rewarding an agent's completion claim.

## Research progression

| Stage | Completed evidence or artifact | Interpretation and limits |
| --- | --- | --- |
| Customer discovery | Ten SMB archetypes: consultant, professional/law practice, real estate, marketing agency, ecommerce, cleaning/service dispatch, recruiting, bookkeeping, freelance creative, IT/website services | Desk research using public customer/practitioner discussions and vendor material; not ten validated customer interviews or proof of willingness to pay |
| Workflow discovery | 24 workflow specifications with pain points, application examples, outcomes, edge cases and sources | Proposed benchmark inputs; distinguish direct problem evidence from proposed workflow extensions and illustrative stacks |
| Application capability analysis | Workflow/application inventory and API/CLI/MCP/UI matrix with verification labels and source links | Operation-level feasibility, eligibility and unknowns; no connectors implemented or exercised against all providers |
| Architecture investigation | Source-pinned AutoWFBench and Sapiens4 analysis, integration gaps, proposed simulation architecture | Existing repository behavior is source-inspected; proposed new architecture remains unimplemented |
| Benchmark requirements | State, time/events, faults, permissions, reset/resume, process isolation and private verification requirements | Design requirements, not implemented benchmark features in this project |
| Blindly4 investigation | Verified commands, JSON tree shapes, native backends and Sapiens CLI integration | JSON output is real; macOS and Windows schemas differ; command success is not proof of resulting UI state |
| Initial UI discovery | My Drive, New control, failed-to-confirm action experiments, manually opened Folder menu and New folder detail | Real captures establish selected UI states and limitations, not an end-to-end completed workflow |

The full 24-workflow catalog covers enquiry/quote follow-up, onboarding, appointment readiness, real-estate leads/viewings, content approvals/reporting, recruiting/interviews, attachment filing/missing documents, invoice/payment reconciliation, billing preparation/handover, recurring services/time records, order/shipping/returns/stock exceptions, email-to-ticket deduplication and domain/hosting renewals.

## Architectural findings and proposed decisions

The inspected AutoWFBench baseline already provides generic simApp contracts, per-case in-memory state, seeded variants/fault behavior, run lifecycle and authoritative evidence/finalization mechanisms. These are pre-existing repository capabilities, not additions from this research. They are not faithful branded SaaS UIs or a general persistent multi-day application/event platform.

Sapiens4 provides a persistent local host, agent workspaces and a Blindly4 subprocess interface with shared-computer ownership. The inspected source pins the same Blindly4 commit investigated here. Its compacted/paginated observations differ from raw CLI trees. A Sapiens-to-AutoWFBench run/poll/cancel adapter and isolated desktop execution arrangement remain gaps.

Proposed decisions for review:

1. Build simApps with complete simulated UI for each selected workflow over authoritative domain state, even where a real API exists. Add simMCP later only as an optimization when the application has suitable MCP capabilities. API documentation informs semantics; it does not replace the primary UI approach. Label domain, UI and optional MCP fidelity separately.
2. Keep application state across actions and browser refresh. Treat reset, resume and crash/restart durability as separate capabilities; the initial folder POC needs run-lifetime persistence, not the entire long-horizon platform.
3. Add explicit logical time, event ordering and deterministic fault scheduling for workflows that require delayed business events. Existing pulse-clock seams do not make Sapiens fully virtual-time controlled.
4. Isolate candidates from private fixtures, evaluator code, future event schedules and admin credentials. A separate process is not by itself a security sandbox.
5. Verify authoritative outcomes independently of agent prose and UI success messages. For the folder POC, check exact name, root-parent identity, one successful creation, no unrelated changes and persistence after reload.
6. Use real accessibility observations to specify a functional accessible UI; do not feed static tree dumps back to the agent as a substitute for an application.

## Why Google Drive is the first slice

The selected first end-to-end task is **Create a folder named Project Alpha in My Drive**. This is a small UI/domain primitive useful in broader document filing, onboarding and handover workflows. It is not itself completion of one of the entire multi-application workflows or a demonstration of persistent-agent superiority.

The sequence is My Drive → New menu → folder dialog → one created folder → refresh/readback. Jira was the earlier proposed discovery target but was replaced before live Jira discovery. Retain that runbook as a clearly superseded methodology artifact, not as evidence of Jira observations.

## Verified Google Drive findings

- Terminal-side Blindly4 works after manual build, self-test and Accessibility permission setup. Codex's execution context did not obtain equivalent desktop access; the operator ran bounded scripts manually.
- My Drive exposes a New AXButton with title and description New, enabled state, AXShowMenu and AXScrollToVisible; AXPress was not advertised.
- AXShowMenu returned success but the captured before/after trees were identical. A later manual observation found no menu; it cannot establish the immediate action result.
- One authorized center click returned success. Its immediate tree was unchanged; the later tree lost browser-window content. This was classified inconclusive, not failed or successful menu opening.
- A later manually opened state exposes focused AXMenu **Folder menu**, including enabled **New folder**, upload entries, Docs, Sheets, Slides, Vids, Forms and More. This positively verifies the open-menu state, not the earlier automated transitions.
- The saved follow-up New folder discovery succeeded: AXMenuItem title `New folder ^C then F`, enabled, with AXCancel, AXScrollToVisible and AXShowMenu. AXPress was not advertised. The redacted detail is included in the capture inventory.
- MacOS `truncated: false` does not rule out depth clipping. Some reads expose only application menus. For the depth-32 single-click case, no node reached the boundary, so depth clipping is not supported as its explanation.

No folder dialog, create submission, validation error or refresh persistence has yet been observed. No folder was created by the capture scripts. Script success and operator reports are labelled separately from raw AX observations.

## Docs and Sheets scope from  feedback

 feedback establishes that the future Drive simulation should represent Docs and Sheets as files. Proposed extension: a common Drive entry identity/name/parent/ownership model with explicit folder, document and spreadsheet kinds, with document/sheet content models added when their workflows are selected. Creation/listing/opening semantics, editing surface, formatting/formulas, sharing and import/export fidelity remain scope decisions. A menu label proves the item is exposed, not that its downstream creation/editor behavior has been captured.

Retain folder creation as the first end-to-end milestone. Do not expand it into a full Docs or Sheets editor before review. Earlier Task 3 already covers Docs and Sheets capabilities; the new feedback connects those capabilities to the Drive file model.

## DFS interpretation

 comparison to exploring a new DFS path is useful framing, not yet a confirmed algorithm requirement. Proposed representation: observed UI states as nodes and attempted authorized actions as edges, each linked to before/after evidence and confidence. Mark edges attempted, verified, inconclusive or blocked; do not identify states solely by volatile numeric AX paths. Revisiting a state and verifying an outcome are distinct from proving a particular traversal strategy.

--Before choosing DFS as an implementation strategy, clarify whether it means conceptual discovery, an actual Sapiens search algorithm, or a benchmark coverage metric. No automatic traversal, backtracking input, retries or state-changing exploration is prescribed by that analogy.

## Actual implementation boundary

This work produced research documents, evidence analysis, redacted derivatives and local capture/diagnostic Python helpers. Blindly4 was built from the existing pinned source; it was not modified. No Google Drive simulator, Docs/Sheets model, Sapiens benchmark adapter, generalized logical clock/event scheduler, isolation deployment or benchmark execution integration was implemented. Existing AutoWFBench and Sapiens source capabilities must be credited as baseline, not this PR's changes.

## Roadmap and review gates

1. Review the research progression, evidence quality and architectural proposals in this documentation PR. Confirm file scope, Docs/Sheets boundaries and the DFS interpretation.
2. Finish controlled Drive discovery: determine a supported New folder interaction, capture dialog/name/commit semantics, obtain separate creation approval, then verify one Project Alpha folder after refresh. Do not automatically retry ambiguous commits.
3. Implement the smallest domain slice and private predicates, then an accessible menu/dialog/list UI informed by captures. Validate through Blindly4 against a dedicated isolated desktop.
4. Add the Sapiens solution adapter and run isolation/lifecycle support needed for repeatable benchmark execution.
5. Expand to Docs/Sheets file types and then selected multi-application workflows; introduce durable resume, logical time, events and faults only with explicit semantics and independent outcome checks.

Readiness is incremental: the current PR can be valuable and reviewable now as research plus partial discovery. It must not be named or described as a completed Google Drive simulator.
