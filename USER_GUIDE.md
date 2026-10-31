# MI Bulk Editor: User Guide
*Bulk editor for Material Instance Constants in Unreal Engine 5*
Publisher: Haishan · Author: Christon Chan · Version 1.0.0 · Supported engines: UE 5.0–5.8 · Platform: Win64 editor

MI Bulk Editor is an **editor-only** plugin. It edits Material Instance Constant assets in your project. It never auto-saves and it never deletes assets: changed Material Instances are only marked as modified, and you decide when to save them with **Save All**.

## 1. Opening MI Bulk Editor
There are three ways to open the **MI Bulk Editor** tab:
- **Tools > MI Bulk Editor**.
- In the Content Browser, select one or more Material Instances, right-click, and choose **Edit in MI Bulk Editor**. The table lists the selected Material Instances.
- In the Content Browser, right-click one or more folders and choose **Open Folder in MI Bulk Editor**. The table lists the Material Instances in those folders, including subfolders.

Inside the tab you can also load assets yourself:
- **Folder** + **Scan Folder**: type a package path (for example `/Game/Materials`) and scan it, including subfolders.
- **Use Content Browser Selection**: load the Material Instances currently selected in the Content Browser.

## 2. The table
Each row is one parameter of one Material Instance.

| Column | Meaning |
|---|---|
| Asset | The Material Instance |
| Parent | Its current Parent material |
| Type | Scalar, Vector, Texture or Switch (Static Switch) |
| Parameter | Parameter name |
| Current | The value the Material Instance uses now |
| State | `override` (set on this Material Instance) or `inherited` (comes from the Parent) |
| New Value | Your pending edit. Empty means no change |
| Note | Warnings, for example for orphaned overrides |

Use **Filter** (asset, parameter or type), **Only overridden** and **Only pending** to narrow the list.

**Supported parameters**
- Scalar, Vector, Texture and Static Switch parameters.
- Parameters exposed through Material Functions are included when they appear on the Material Instance (same table as the parent material's own parameters; not grouped separately).
- Global parameters only. **Layer parameters are not supported**: they can't be edited and are not exported to CSV.

**Orphaned overrides**
An orphaned override is an override stored on a Material Instance for a parameter that its Parent no longer has. It is shown as a **grey read-only row with a warning**. In version 1.0 it can't be edited or cleared, and the plugin leaves it as it is. When a Material Instance with orphaned Static Switch overrides has its static permutation rebuilt, the engine drops those Static Switch overrides; Dry Run warns you about this before you apply.

## 3. Editing values
Type into the **New Value** cell and press Enter. Nothing is written to the asset yet; the edit stays pending until you run **Dry Run** and **Apply**.

| You type | Result on Apply |
|---|---|
| A value | Sets an override with that value |
| (nothing) | No change |
| `#INHERIT` | Clears the override, so the value comes from the Parent again (same as Revert to Parent) |
| `None` (Texture only) | Sets an override with no texture (a None texture override). This is not the same as reverting to the Parent's texture |

Value formats:
- **Scalar:** a number, for example `0.35`. The decimal point is always `.`.
- **Vector:** `(R=1,G=0.5,B=0,A=1)`, `1,0.5,0,1`, or `1,0.5,0` (3 components: Alpha is set to 1).
- **Texture:** an object path, for example `/Game/T/T_A.T_A`, or `None`.
- **Switch:** `true` / `false`, `1` / `0`, `yes` / `no` or `on` / `off`.

`#INHERIT`, `None` and the Switch values are not case-sensitive.

To forget edits, right-click and choose **Clear Pending Edit** for the selected cells, or click **Discard Pending** to drop all pending edits.

## 4. Revert to Parent
**Revert to Parent** clears overrides so the selected parameters inherit from the Parent again. It is available from:
- the **cell right-click** menu,
- the **row right-click** menu,
- the **Revert to Parent** toolbar button (works on the current selection, multi-select allowed).

Revert to Parent marks the selected overridden cells as `#INHERIT`. Cells that are already inherited are not changed. Like any other edit, nothing is written until you run **Dry Run** and **Apply**, and the result can be undone with one **Ctrl+Z**. Orphaned overrides can't be reverted in 1.0.

## 5. Changing the Parent
Select rows of the Material Instances you want to re-parent, type the new Parent path (for example `/Game/Materials/M_Base.M_Base`) and click **Change Parent**. The change stays pending until Apply. You can also change Parents through the `Parent` column of a CSV file (see [CSV_FORMAT.md](CSV_FORMAT.md)).

Dry Run lists the overrides that the new Parent doesn't have, so you can see what would become orphaned before you commit.

## 6. Dry Run
Click **Dry Run** to see exactly what **Apply** would change. Dry Run writes nothing: it does not modify assets and does not mark them as modified.

The report has these sections:
- **Parent changes**: old Parent → new Parent.
- **Value changes**: old value (override or inherited) → new value.
- **Override clears / Revert to Parent**: every cleared override is listed separately here, with a count.
- **Skipped**: edits that will not be applied, with the reason.
- **Warnings**: for example a value in a cell whose Parent doesn't have that parameter.

The summary also shows how many Material Instances will rebuild their static permutation (this triggers shader compilation).

**Apply** is available only after a Dry Run that contains changes. If you edit anything afterwards, run Dry Run again.

## 7. Apply
1. Click **Apply** and confirm.
2. If the assets changed since you reviewed the Dry Run, MI Bulk Editor shows the updated Dry Run instead and asks you to press **Apply** again.
3. All changes are applied as **one transaction**. **One Ctrl+Z undoes the whole batch** (Ctrl+Y redoes it).
4. Every written value is read back and checked. The result report shows how many changes were verified and lists any failures.
5. A JSON change log is written (see section 8).
6. Changed Material Instances are marked as modified. **Nothing is saved automatically: use Save All yourself** when you are happy with the result.

**Static Switches:** when a Static Switch changes, the plugin rebuilds the Material Instance's static permutation so the switch really takes effect. The editor compiles the new shaders on demand, so compilation may start when the material is next rendered or when you save.

## 8. Undo and change logs
**Editor undo:** each Apply (including a revert from a log) is one editor transaction. Press **Ctrl+Z** once to undo the whole batch.

**JSON change log:** every Apply writes a change log to:

`<Project>/Saved/MIBulkEditor/ChangeLogs/`

Each log records, for every change, the asset, the parameter, the value and override state before and after, and whether the change was verified.

**Restoring from a log**
1. Click **Revert Change Log...** and choose a `.json` file from the ChangeLogs folder.
2. MI Bulk Editor prepares the edits that restore the logged "before" state and shows them as a Dry Run. Parameters that were inherited before go back to inherited (the override is cleared; no override with the same value is created). Parents are restored too. Only changes that were verified when they were applied are restored.
3. Review the Dry Run and click **Apply**. The restore is itself one transaction (one Ctrl+Z undoes it) and writes its own change log.

You can restore from a log after restarting the editor (the logs live on disk under your project's `Saved` folder).

## 9. Known limitations
- Only Material Instance Constants (Material Instance assets) are edited.
- Global parameters only. Layer parameters are not supported.
- Orphaned overrides are read-only in 1.0 and can't be cleared.
- The plugin never saves and never deletes assets. Keep your project under source control or make a backup before large batch edits.
- Read-only or source-control-locked files are skipped with a warning (no automatic checkout). Real Perforce workflows have not been re-verified in QA — treat SCC skip as best-effort, not a Perforce guarantee.

## 10. Support
- Fab listing: <!-- Fab listing URL after publish -->
- Support: [chanjc03@gmail.com](mailto:chanjc03@gmail.com)
- CSV reference: [CSV_FORMAT.md](CSV_FORMAT.md) · FAQ: [FAQ.md](FAQ.md)
