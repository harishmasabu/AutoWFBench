# Google Drive capture evidence

Start with [control inventory](control-inventory.md), [transition findings](transitions.md) and [limitations](limitations.md). The files here are selected **redacted derivatives**, not raw account-bearing dumps. Original raw output is retained privately outside the repository. Recorded source hashes identify original bytes where available; earlier manual commands did not record timestamps.

| Stage | Findings | Included evidence |
| --- | --- | --- |
| Permission and coverage setup | [Read-only attempt](read-only-attempt.md), [initial capture](initial-capture-analysis.md), [foreground capture](foreground-capture-analysis.md) | Reports only; redundant menu-bar-only structures retained privately |
| My Drive baseline | [Baseline analysis](my-drive-baseline-analysis.md) | [Tree](my-drive-depth20.redacted.json) |
| New action inventory | [New analysis](new-button-analysis.md) | [Detail](new-button-detail.redacted.json) |
| AXShowMenu attempt | [Findings](new-menu-attempt/findings.md), [diagnostic reasoning](new-menu-attempt/diagnostic-assessment.md) | One representative [before tree](new-menu-attempt/002-before.redacted.json), action result, redacted commands and raw hashes; after trees were identical and duplicate derivatives are omitted |
| Single-click attempt | [Findings](new-single-click/findings.md) | [Before](new-single-click/003-before.redacted.json), [later loss of coverage](new-single-click/010-after-settled.redacted.json), empty find result, command records and hashes; immediate tree equalled before |
| Manual menu open | [Findings](manual-menu-open/findings.md) | [Menu tree](manual-menu-open/tree.redacted.json) and [New folder detail](manual-menu-open/new-folder-detail.redacted.json) |

All original artifacts, including omitted duplicate derivatives, were preserved outside the repository before consolidation. No evidence was replaced by a fabricated observation. Redaction removes account/file/tab text, local executable paths, PIDs and geometry where appropriate. Tree derivatives retain ordered child topology, semantic roles/states and only reviewed navigation/menu strings. Attribute-name lists are capability metadata, not attribute values. Paths in findings identify historical snapshots and must not be reused for live input.

This collection verifies selected states, not a complete successful folder workflow. The New folder follow-up is a successful read-only find with one exact contextual match, enabled AXMenuItem title `New folder ^C then F`, actions AXCancel, AXScrollToVisible and AXShowMenu. AXPress was not advertised. No item activation, dialog, creation or persistence is yet validated. Shortcut text is reproduced as observed, not recommended input.
