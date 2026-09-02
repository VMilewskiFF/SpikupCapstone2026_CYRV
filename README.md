# CYRV capstone project

Analysis of the CYRV e-commerce dataset (Brazilian online marketplace orders).
See [`data/README.md`](data/README.md) for what each data file contains.

## Repo structure

- `data/` — the dataset used for analysis. See its README for details.
- `src/cyrv/` — shared code, packaged as `cyrv` so it can be imported anywhere with `import cyrv`. Optional — you can also just write standalone scripts here and run them directly, no import needed.
- `notebooks/` — Jupyter notebooks for exploration and analysis.
- `pyproject.toml` — project dependencies, managed with `uv`.

## Setup with uv

[`uv`](https://docs.astral.sh/uv/) manages the Python version, virtual
environment, and dependencies for this project. Install it first if you don't
have it (see the uv docs), then from the project root:

```bash
uv sync
```

This creates a `.venv/` folder and installs everything listed in
`pyproject.toml`. Run any script or notebook kernel through `uv`, so it uses
that environment:

```bash
uv run src/my_script.py
uv run jupyter lab
```

## Managing dependencies

Add a new package:

```bash
uv add pandas
```

Remove one:

```bash
uv remove pandas
```

This updates `pyproject.toml` and the lock file (`uv.lock`) automatically —
don't edit dependencies by hand. Commit both files so everyone uses the same
versions.
