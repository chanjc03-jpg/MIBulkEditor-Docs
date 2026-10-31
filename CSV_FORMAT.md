# CSV Format

MI Bulk Editor exports and imports a **wide table**: **one row per Material Instance**, one column per parameter.

- Encoding: export writes UTF-8 with BOM, so Excel opens non-ASCII asset names correctly.
- Standard CSV (RFC 4180), comma-separated. A cell that contains a comma must be in double quotes.
- The decimal point is always `.`, whatever your system's regional settings.

## 1. Columns
| Column | Required | Meaning |
|---|---|---|
| `Asset` | Yes | The Material Instance, as an object path (`/Game/Env/MI_Rock_01.MI_Rock_01`) or package path (`/Game/Env/MI_Rock_01`) |
| `Parent` | No | Export: the current Parent. Import: if the cell is not empty and differs from the current Parent, the Parent is changed |
| `Scalar:<Name>` | No | A Scalar parameter, for example `Scalar:Metal` |
| `Vector:<Name>` | No | A Vector parameter, for example `Vector:Tint` |
| `Texture:<Name>` | No | A Texture parameter, for example `Texture:BaseColor` |
| `Switch:<Name>` | No | A Static Switch parameter, for example `Switch:UseTint` |

The type prefix keeps parameters with the same name apart: `Scalar:Roughness` and `Vector:Roughness` are two different columns.

## 2. Cell values
| Type | Format | Example |
|---|---|---|
| Scalar | Number | `0.35` |
| Vector | `(R=…,G=…,B=…,A=…)`, `r,g,b,a`, or `r,g,b` (3 components: Alpha is set to 1). In double quotes in the CSV file, because it contains commas | `"(R=1,G=0.5,B=0,A=1)"`, `"1,0.5,0,1"` or `"1,0.5,0"` |
| Texture | Object path, or `None` | `/Game/T/T_A.T_A` |
| Switch | `true` / `false`, `1` / `0`, `yes` / `no` or `on` / `off` | `true` |

## 3. Special cells
| Cell | On export | On import |
|---|---|---|
| A value | The Material Instance overrides this parameter | Sets an override with that value |
| Blank | The value is inherited from the Parent | **No change** (even if the Material Instance currently overrides it) |
| `#INHERIT` | (not exported) | Clears the override, so the value comes from the Parent again |
| `None` (Texture) | The Material Instance has a None texture override | Writes a None texture override (this is not the same as inheriting the Parent's texture) |
| `N/A` | The Material Instance's Parent doesn't have this parameter | Left as `N/A`: ignored. If you type a value into an `N/A` cell, you get a warning and the cell is skipped |

**Case:** `#INHERIT`, `N/A`, `None`, the Switch values (`true` / `false`, `yes` / `no`, `on` / `off`) and parameter names are all case-insensitive. `#inherit`, `n/a`, `NONE`, `TRUE`, `Yes`, `OFF` and `Scalar:roughness` all work.

## 4. Import rules
- **Import never writes directly.** Importing loads the edits and runs a **Dry Run**. Nothing is written until you click **Apply**.
- **Blank means no change.** Deleting a value or leaving cells empty can't clear your overrides by accident. To clear an override on purpose, write `#INHERIT`.
- **Missing or unknown type prefix: the whole import is aborted.** If any parameter column header has no type prefix or an unknown one (for example `Metal` or `Scaler:Metal`), the import stops, **nothing is written**, and the error message names the column.
- A file without an `Asset` column is rejected, and nothing is written.
- **Layer columns are rejected with a warning.** Columns such as `LayerIndex`, `Association`, or layer-qualified parameter columns (for example `Scalar:Metal@Layer0`) are ignored with a warning; the other columns are imported normally. Layer parameters are not supported in 1.0.
- **A value for a parameter the Parent doesn't have** (including a value typed into an `N/A` cell) gives a warning and that cell is skipped. The rest of the row is still imported.
- If the `Parent` column changes a Material Instance's Parent, parameter cells are checked against the **new** Parent.
- Orphaned overrides (read-only) and Layer parameters are skipped and listed under **Skipped** in the Dry Run.
- Values that can't be read (for example text in a Scalar column, or a texture path that doesn't exist) are reported as warnings and are not written.
- Export, then import with no edits = zero changes in the Dry Run.

## 5. Export rules
- Overridden parameters: the override value.
- Inherited parameters: blank.
- Parameters the Material Instance's Parent doesn't have: `N/A`.
- No Layer columns are exported.

## 6. Example
```csv
Asset,Parent,Scalar:Metal,Scalar:Rough,Vector:Tint,Texture:BaseTex,Switch:UseTint
/Game/Env/MI_Rock_01.MI_Rock_01,/Game/Env/M_Rock.M_Rock,0.8,,"(R=0.2,G=0.4,B=0.6,A=1)",,true
/Game/Env/MI_Rock_02.MI_Rock_02,/Game/Env/M_Rock.M_Rock,#INHERIT,0.7,,None,
/Game/Props/MI_Crate_01.MI_Crate_01,/Game/Props/M_Crate.M_Crate,,0.5,N/A,N/A,N/A
```
On import, this example:
- `MI_Rock_01`: sets `Metal` to 0.8, `Tint` to (0.2, 0.4, 0.6, 1) and `UseTint` to true; `Rough` and `BaseTex` are not changed.
- `MI_Rock_02`: clears the `Metal` override (inherits from `M_Rock` again), sets `Rough` to 0.7 and sets `BaseTex` to a None texture override.
- `MI_Crate_01`: sets `Rough` to 0.5. Its Parent `M_Crate` has no `Tint`, `BaseTex` or `UseTint`, so those cells are `N/A` and ignored.

## 7. Tips for spreadsheets
- Keep the header row exactly as exported. A changed type prefix aborts the import.
- Always check the Dry Run before you click Apply.
- When saving from Excel, choose **CSV UTF-8 (Comma delimited)** (or equivalent). Version 1.0 expects UTF-8 (with or without BOM). Non-UTF-8 encodings such as GBK / ANSI are not supported in 1.0.
