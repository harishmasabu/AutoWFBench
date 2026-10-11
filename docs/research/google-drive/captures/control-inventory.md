# Verified control inventory

| State/control | Observed role and label | State/actions | Evidence |
| --- | --- | --- | --- |
| My Drive page | AXWebArea, My Drive - Google Drive | Enabled; account route verified privately | Baseline tree |
| My Drive navigation | AXRow, My Drive Collapsed | Selected | Baseline tree |
| Location control | AXPopUpButton, My Drive | Enabled | Baseline tree |
| New | AXButton, title/description New | Enabled; AXShowMenu, AXScrollToVisible; AXPress not advertised | New detail |
| Open creation menu | AXMenu, title/description Folder menu | Enabled and focused | Manually opened menu tree |
| New folder | AXMenuItem, New folder ^C then F | Enabled; AXCancel, AXScrollToVisible, AXShowMenu; AXPress not advertised | New folder detail |
| Other menu entries | AXMenuItem: File upload ^C then U; Folder upload ^C then I; Google Docs; Google Sheets; Google Slides; Google Vids; Google Forms; More | Enabled; actions not inspected | Manually opened menu tree |

See [evidence index](README.md) for source files. Presence of a role or advertised action does not prove action behavior. Document and spreadsheet menu entries establish discoverability, not their downstream editor capabilities. The full UI remains primary in the future simApp; selected-workflow fidelity does not require implementing every listed menu item's unrelated workflow immediately.
