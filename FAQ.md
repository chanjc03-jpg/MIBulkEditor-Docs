# FAQ

## Installing and opening
**I installed it, but Tools > MI Bulk Editor doesn't appear.**
Enable the plugin in **Edit > Plugins** (search for "MI Bulk Editor"), tick **Enabled**, and restart the editor. See [INSTALL.md](INSTALL.md).

**How do I open it?**
From **Tools > MI Bulk Editor**, or in the Content Browser: right-click Material Instances and choose **Edit in MI Bulk Editor**, or right-click folders and choose **Open Folder in MI Bulk Editor**.

**Which engine versions are supported?**
UE 5.0–5.8, Windows 64-bit editor. Install the package that matches your engine version.

## Safety
**Does it save my assets?**
No. The plugin never auto-saves. Applied changes mark the Material Instances as modified; review them and use **Save All** yourself.

**Can it delete assets?**
No. MI Bulk Editor never deletes assets. "Clearing an override" only removes the override stored inside the Material Instance; the asset itself stays.

**Does Dry Run change anything?**
No. Dry Run only shows what Apply would do. It does not modify assets or mark them as modified.

**How do I undo a whole batch?**
Press **Ctrl+Z** once. Each Apply is one transaction, so one Ctrl+Z undoes the whole batch.

**Can I undo an edit before I apply it?**
Yes. Pending edits have their own in-memory history in the table: with focus on the table, **Ctrl+Z** undoes the last pending edit and **Ctrl+Y** redoes it. Typing Enter, toggling a checkbox, picking or dragging an asset, **Inherit**, **Revert to Parent**, Clear/Discard Pending, Color Picker **OK**, releasing a Scalar drag, and a CSV import (one step for the whole batch) each add one step. The history clears on close, Reload, successful Apply, or Revert All; after Apply, table-focused Ctrl+Z undoes the Apply in the editor. This doesn't touch your assets or the editor's undo history before Apply. See [USER_GUIDE.md](USER_GUIDE.md) section 3.

**Can I undo after closing the editor?**
Use the JSON change log. Every Apply writes one to `<Project>/Saved/MIBulkEditor/ChangeLogs/`. Click **Revert Change Log...**, choose the log, review the Dry Run and click **Apply**.

## Editing
**What's the difference between a blank cell, `#INHERIT` and `None`?**
- Blank (or no pending edit in the table): no change.
- `#INHERIT`: clears the override, so the value comes from the Parent again. In the table, use the **Inherit** button next to a Scalar, Vector, Switch or Texture cell, or **Revert to Parent**. Both do exactly the same as `#INHERIT`.
- `None` (Texture only): writes a None texture override (choose None in the Texture picker, or write `None` in a CSV file). It does not restore the Parent's texture; use `#INHERIT` or **Revert to Parent** for that.

**Can I type values into the table?**
The **New Value** column uses an editor for each type instead of a text box: a checkbox for Static Switches, a colour swatch that opens the Color Picker (with eyedropper) for Vectors, a number box you can type into or drag for Scalars, and asset pickers for Textures and Parents. See [USER_GUIDE.md](USER_GUIDE.md) section 3. To work with values as text, use CSV export and import.

**How do I enter HDR colours (values above 1)?**
Open the Color Picker from the Vector swatch and type the values into the **R**, **G**, **B** numeric fields, or use the brightness slider. The **Hex** field is always sRGB and can't represent values above 1. HDR values are stored unchanged and appear unchanged in Dry Run and CSV files; only the swatch display is clipped. Colours are stored as linear values, so a Hex colour such as `#808080` shows up as about `0.2159` in a CSV file.

**Can I set negative values?**
- **Scalar:** yes. Type the negative number. Typed values are never clamped: the slider range set on the material parameter only limits dragging, so you can also type values outside that range.
- **Vector:** the Color Picker can't create negative values, but a negative component that a colour already has is kept as long as you don't change that colour. To set a negative component, write it in a CSV file (for example `"(R=-0.5,G=0.2,B=1,A=1)"`) or type it into the **inline R, G, B, A** fields next to the swatch in the cell (not the Color Picker's spin boxes); both give the same result.
- **Static Switch, Texture and Parent:** not applicable.

**Why can't I clear the Parent (set it to None)?**
Every Material Instance needs a Parent, and the Parent is not an override, so there is nothing to "inherit". The Parent picker has no None option and no **Inherit** button. To change the Parent, pick another material or Material Instance. In a CSV file, a blank `Parent` cell means no change.

**Does CSV export include my pending edits?**
No. **Export CSV** exports the applied values only. If pending edits exist, you get a notice that they were not exported. Run **Dry Run** and **Apply** first if you want them in the file.

**Where is Revert to Parent?**
In the cell right-click menu, the row right-click menu, and on the toolbar. It marks the selected overridden cells as `#INHERIT`; the change happens when you Dry Run and Apply.

**Why is Apply greyed out?**
Apply is available only after a **Dry Run** that contains changes. Run Dry Run again after you edit anything.

**Why are some rows grey and read-only?**
They are orphaned overrides: the Material Instance still stores an override for a parameter that its Parent no longer has. They are shown with a warning and can't be edited or cleared in version 1.0.

**Can I edit material layer parameters?**
No. Version 1.0 supports global parameters only. Layer parameters are not supported, are not exported to CSV, and layer columns in an imported CSV are rejected with a warning.

**Are parameters from Material Functions supported?**
Yes. Parameters exposed through Material Functions are listed and edited like any other global parameter on the Material Instance (not grouped separately).

**I changed a Static Switch. When are shaders compiled?**
The plugin rebuilds the static permutation on Apply. The editor compiles the shaders on demand, so compilation may start when the material is next rendered or when you save. The Dry Run summary shows how many Material Instances will rebuild their static permutation.

## CSV
**Why do some cells say `N/A`?**
That Material Instance's Parent doesn't have the parameter. On import, `N/A` cells are ignored. If you write a value into one in the CSV file, you get a warning and the cell is skipped. In the table, `N/A` cells show a disabled editor.

**My import was aborted and nothing changed.**
A parameter column header has a missing or unknown type prefix. Every parameter column must start with `Scalar:`, `Vector:`, `Texture:` or `Switch:`. The error message names the column. Fix the header and import again.

**Are the special values case-sensitive?**
No. `#INHERIT`, `N/A`, `None`, `true` / `false` and parameter names are all case-insensitive.

**How do I write a Vector value in a CSV file?**
`(R=1,G=0.5,B=0,A=1)`, `1,0.5,0,1`, or the 3-component form `1,0.5,0` (Alpha is set to 1). In the CSV file the cell must be in double quotes because it contains commas. See [CSV_FORMAT.md](CSV_FORMAT.md).

## Support
- Fab listing: <!-- Fab listing URL after publish -->
- Support: [chanjc03@gmail.com](mailto:chanjc03@gmail.com)
