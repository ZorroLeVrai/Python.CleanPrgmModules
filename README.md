# Python.CleanPrgmModules

Recursively scans `E:\Users\Amine\Prgm` and deletes every `node_modules` folder
that sits next to a `package.json` file.

## Usage

```powershell
# Dry run: list what would be deleted, with size in MB, no changes made
python clean_node_modules.py --test

# Real run: asks for confirmation, then deletes and logs each folder with its size
python clean_node_modules.py

# Show help
python clean_node_modules.py --help
```

## Options

| Option         | Description                                              |
|----------------|-----------------------------------------------------------|
| `--test`       | Dry run — only logs what would be deleted, nothing is removed |
| `-h`, `--help` | Show usage help                                           |

Any unrecognised argument makes the script log an error and exit without running.

## Output

Each folder is logged with its size in MB. At the end, the script prints the
total space deleted (or that would be deleted in `--test` mode), formatted
with thousands separators, e.g. `16,352.57 MB`.

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
