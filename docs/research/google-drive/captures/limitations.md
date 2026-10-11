# Observation limitations

- Accessibility permissions and desktop visibility differ by execution context. The operator's Terminal could capture; the Codex execution context could not. Build/self-test completion was operator-reported.
- Tree reads are bounded non-atomic observations. MacOS truncated false does not rule out depth clipping; successful exit is not full coverage.
- Some successful reads expose only Chrome's macOS menu bar. A post-action loss of the previously observed web area is inconclusive, not proof of failed interaction.
- New and New folder do not advertise AXPress in captured details. AXShowMenu success did not verify the intended transition.
- The manually opened menu lives inside Drive's existing web area at depth 16, with direct items at 17. It is not an observed separate top-level popup. That location weakens depth-only explanations for the earlier depth-24 attempt without proving its cause.
- No independent screenshot or contemporaneous post-action visual report was collected for automated attempts. Later manual reports cannot establish the immediate prior state.
- Browser-wide captures contain unrelated private content. Only reviewed derivatives belong in Git. Avoid inferring absence of duplicate folders from bounded UI evidence.
- Numeric paths are temporary. Substring find may match New Tab as well as New; verify identity, context, enabled state and actions immediately before any authorized action.
- Menu labels with shortcut suffixes must not be treated as verified keyboard instructions. No unsupported action or extra permission is justified by a role alone.
