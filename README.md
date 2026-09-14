# BoardReadyOps demo — a board that gets broken

A two-file KiCad project that is fabricable, and one pull request that quietly ruins it.

`main` carries the clean board and its **Fabrication readiness** check is green.
[Pull request #1](../../pull/1) looks like an ordinary layout change; the check on it goes
red, and its comment says exactly which five things went wrong and where.

This is the case the tool exists for: nothing in the diff itself tells you the board can no
longer be built.

## What the pull request breaks

| Rule | What it catches |
| --- | --- |
| `design.board-outline` | The closed outline on Edge.Cuts is gone, so the fabricator has no board shape. |
| `design.unique-references` | Two components end up sharing a reference designator. |
| `bom.missing-mpn` | A BOM line loses its manufacturer part number and cannot be sourced. |
| `bom.compliance` | A part carries no RoHS/REACH status. |
| `bom.risk-score` | Single-source parts push the assembly's supply risk over the threshold. |

## Running it yourself

```bash
npx @boardreadyops/cli run .
```

No KiCad installation required: `boardreadyops.yml` disables the DRC and ERC rules that
shell out to `kicad-cli`, so the checks above run on the files alone.

The companion repository [boardreadyops-demo-pass](https://github.com/oaslananka/boardreadyops-demo-pass)
runs the same corpus the other way round — a broken board, and a pull request that fixes it.

Licensed MIT. Copy the board, the config, or the workflow into your own project — that is what this
repository is for. BoardReadyOps itself is licensed separately under
[PolyForm Noncommercial](https://github.com/oaslananka/boardreadyops/blob/main/LICENSE); the MIT
grant here covers these demo fixtures, not the tool.
