# Changelog

## 1.0.0
- New: MI Bulk Editor tab. Lists the parameters of many Material Instance Constants in one table. Open it from **Tools > MI Bulk Editor**, or from the Content Browser with **Edit in MI Bulk Editor** (assets) and **Open Folder in MI Bulk Editor** (folders).
- New: bulk editing of Scalar, Vector, Texture and Static Switch parameters, including parameters exposed through Material Functions. Static Switch changes rebuild the static permutation so they take effect.
- New: typed **New Value** editors in the table: checkbox for Static Switches, colour swatch with Unreal's Color Picker and eyedropper for Vectors, drag-to-adjust number box for Scalars, and asset pickers for Textures (drag in from the Content Browser, or None) and Parents. An **Inherit** button on Scalar, Vector, Switch and Texture cells restores the inherited value (same as `#INHERIT`). Editing a selected row applies to every selected row with the same parameter name and type; mixed values show as `Multiple Values`, and the status bar reports `Edited N rows.` (with a `Skipped` count for rows of another type). Pending edits can be undone and redone in the table (Ctrl+Z / Ctrl+Y) before Apply. Scalar values can be negative and typed values are not clamped to the slider range. Hex colours are sRGB; values are stored as linear, and HDR values are kept. CSV rules are unchanged; CSV export contains applied values only.
- New: optional preview thumbnails in the table (**Thumbnails** toolbar toggle, off by default, remembered per project). Contiguous rows for the same Material Instance share one thumbnail with a collapse/expand control; when sorting scatters a Material Instance, each visible row shows its own thumbnail. Collapse controls hide when Thumbnails are off.
- New: Change Parent for many Material Instances at once — **Use CB Parent** (select a Material or Material Instance in the Content Browser, then click the toolbar button) or the Parent asset picker / **Change Parent**. Pending Parent changes show as `old -> new` until Apply.
- New: **Revert to Parent** from the cell right-click menu, the row right-click menu, and the toolbar.
- New: **Dry Run** preview. Writes nothing. Cleared overrides are listed separately, with a count.
- New: **Apply** as one transaction. One Ctrl+Z undoes the whole batch. Every written value is read back and checked.
- New: JSON change log for every Apply in `Saved/MIBulkEditor/ChangeLogs`, and **Revert Change Log...** to restore from a log.
- New: CSV export and import (wide table, one row per Material Instance, typed column headers). Blank = no change, `#INHERIT` clears an override, `None` writes a None texture override, `N/A` marks parameters the Parent doesn't have.
- Orphaned overrides are shown as grey read-only rows with a warning.
- Never auto-saves; never deletes assets.
- Limitations: global parameters only; Layer parameters are not supported. Orphaned overrides can't be edited in 1.0.
- Supported: UE 5.0–5.8, Win64 editor.
