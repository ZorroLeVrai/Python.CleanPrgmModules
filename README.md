# Python.CleanPrgmModules

Recursively scans a root folder and deletes:
- every `node_modules` folder that sits next to a `package.json` file
- every `.venv` folder that sits next to a `requirements.txt`, `requirements-dev.txt`, or `pyproject.toml` file

The root folder is read from the `ROOT_DIR` constant in the script. If `ROOT_DIR`
is left empty, it falls back to the environment variable named by the
`ROOT_DIR_FROM_ENV` constant (currently `prgm`).

## Usage

At least one target option (`--all`, `--js`, or `--py`) is required.

```powershell
# Dry run: list what would be deleted, with size in MB, no changes made (both node_modules and .venv)
python clean_node_modules.py --test --all

# Real run: asks for confirmation, then deletes and logs each folder with its size
python clean_node_modules.py --all

# Only target node_modules folders
python clean_node_modules.py --js
python clean_node_modules.py --test --javascript

# Only target .venv folders
python clean_node_modules.py --py
python clean_node_modules.py --test --python

# --js and --py together are equivalent to --all
python clean_node_modules.py --test --js --py

# Show help
python clean_node_modules.py --help
```

## Options

| Option                | Description                                                      |
|------------------------|------------------------------------------------------------------|
| `--test`               | Dry run — only logs what would be deleted, nothing is removed    |
| `--js`, `--javascript` | Only remove `node_modules` folders                                |
| `--py`, `--python`     | Only remove `.venv` folders                                      |
| `--all`                | Remove both `node_modules` and `.venv` folders (same as `--js --py`) |
| `-h`, `--help`         | Show usage help                                                   |

At least one of `--all`, `--js`/`--javascript`, or `--py`/`--python` must be
given, otherwise the script logs an error and exits without running. Any
unrecognised argument does the same. Outside of `--test` mode, the script
always asks for a `y`/`N` confirmation before deleting anything.

## Output

Each folder is logged with its size in MB. At the end, the script prints the
total space deleted (or that would be deleted in `--test` mode), formatted
with thousands separators, e.g. `16,352.57 MB`.

## Tests

Unit tests live in `test_clean_node_modules.py` and use `pytest-mock` to
simulate directory trees (`os.walk`, `os.path.getsize`, `shutil.rmtree`,
`input`) — no real files or folders are touched.

```powershell
.\.venv\Scripts\Activate.ps1
pip install -r requirements-dev.txt
pytest -v
```

## Reminder: using the .venv

```powershell
# Activate the virtual environment
.\.venv\Scripts\Activate.ps1

# Install/check dependencies (e.g. mypy)
pip install -r requirements-dev.txt

# Run mypy type checking
mypy clean_node_modules.py

# Deactivate when done
deactivate
```

### Recreating the .venv from scratch

If `.venv` has been deleted (e.g. by this script's own `--py` cleanup), rebuild
it from the pinned dependencies in `requirements.txt` / `requirements-dev.txt`:

```powershell
# 1. Create a fresh virtual environment
python -m venv .venv

# 2. Activate it
.\.venv\Scripts\Activate.ps1

# 3. Install runtime dependencies (if requirements.txt exists)
pip install -r requirements.txt

# 4. Install dev dependencies (mypy, etc.)
pip install -r requirements-dev.txt
```

This project currently has no runtime dependencies, so step 3 can be skipped —
only `requirements-dev.txt` (containing `mypy`) is needed.
