# CLAUDE.md

Orbital mechanics simulator — personal physics project (Python/Jupyter, numpy/matplotlib/scipy).

## Layout

| Path | Contents |
|---|---|
| `notebooks/orbit.ipynb` | Main simulation notebook |
| `sandbox_Orbit/` | Scratch copy of the notebook + `Orbit.flowchart.canvas` (Obsidian canvas of the pipeline) |
| `docs/` | `01_pipeline_roadmap.md`, `02_equations_reference.md`, `03_resources.md` |
| `saved_resources/` | Clipped reference articles (markdown) |
| `README-Orbital-Mechanics.md` | Project overview |

## Working-copy rules

- **This vault clone is the primary working copy.** The Desktop checkout at `~/Desktop/Personal Coding Projects/orbital-mechanics-simulator/` syncs via GitHub (push from here, pull there) — never copy files between the two.
- Remote: `github.com/ShaneGilbertie/orbital-mechanics-simulator` (Shane's own repo — safe to push).
- Legacy `orbit/` and `sandbox_Orbit/` Desktop duplicates were archived to `Desktop\Archived or dismantled projects\` on 2026-07-09.

## Running

Open `notebooks/orbit.ipynb` in Jupyter, or run cells via `python` with the standard scientific stack (conda base env has numpy/matplotlib/scipy).
