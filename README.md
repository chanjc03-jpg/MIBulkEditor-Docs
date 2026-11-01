# MI Bulk Editor – Batch Edit Material Instances, Static Switches & CSV
*Bulk editor for Material Instance Constants in Unreal Engine 5*

Publisher: Haishan · Author: Christon Chan · Version 1.0.0 · Supported engines: UE 5.0–5.8 · Platform: Win64 editor

MI Bulk Editor shows the parameters of many Material Instance Constants in one table, so you can change them together and see exactly what will happen before anything is written.

- **Edit in bulk:** Scalar, Vector, Texture and **Static Switch** parameters (including parameters exposed through Material Functions), plus the Parent material, each with an editor that matches its type.
- **Preview first:** **Dry Run** lists every change before you apply it. Cleared overrides are listed in their own section, with a count.
- **One-step undo:** each **Apply** is one editor transaction. One **Ctrl+Z** undoes the whole batch.
- **Change log:** every Apply writes a JSON change log to `Saved/MIBulkEditor/ChangeLogs`, and you can restore from a log.
- **Revert to Parent:** clear overrides from the cell right-click menu, the row right-click menu, or the toolbar button.
- **CSV round-trip:** export to a wide CSV (one row per Material Instance), edit it in a spreadsheet, and import it back through the same preview.
- **Safe:** the plugin never auto-saves (use **Save All** yourself) and never deletes assets.

## Quick start
1. Install the plugin from Fab and enable it in **Edit > Plugins**, then restart the editor. See [INSTALL.md](INSTALL.md).
2. Open **Tools > MI Bulk Editor**, or right-click in the Content Browser: assets > **Edit in MI Bulk Editor**, folders > **Open Folder in MI Bulk Editor**.
3. Set new values in the **New Value** column (a checkbox for Static Switches, a colour swatch with the Color Picker for Vectors, a number box for Scalars, asset pickers for Textures and Parents), or use **Import CSV...**.
4. Click **Dry Run** and review the report.
5. Click **Apply**. Press **Ctrl+Z** if you want the whole batch back.
6. Review the result, then use **Save All**.

## Documentation
| Document | Contents |
|---|---|
| [INSTALL.md](INSTALL.md) | Installing, enabling, uninstalling |
| [USER_GUIDE.md](USER_GUIDE.md) | The table, editing, Revert to Parent, Change Parent, Dry Run, Apply, undo and change logs |
| [CSV_FORMAT.md](CSV_FORMAT.md) | Column headers, cell values, export and import rules |
| [FAQ.md](FAQ.md) | Common questions |
| [CHANGELOG.md](CHANGELOG.md) | Release notes |

Fab **DocsURL** for this product will be this repository's public URL once the empty `MIBulkEditor-Docs` repo is created and these files are pushed (same pattern as [EditorFixPack-Docs](https://github.com/chanjc03-jpg/EditorFixPack-Docs)).

## Scope of version 1.0
- Material Instance Constants (Material Instance assets) only.
- Global parameters only. Layer parameters are not supported.
- Orphaned overrides (overrides whose parameter the parent no longer has) are shown as grey read-only rows with a warning and can't be edited in 1.0.

## Requirements
- Windows 64-bit Unreal Editor, UE 5.0–5.8.
- Editor-only plugin (one Editor module).

## Support
- Fab listing: <!-- Fab listing URL after publish -->
- Support: [chanjc03@gmail.com](mailto:chanjc03@gmail.com)
