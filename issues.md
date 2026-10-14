# Issues Found and Fixed

| Issue | Type | Line(s) | Description | Fix Approach |
|-------|------|---------|-------------|--------------|
| SQL injection (B608) | Security (Medium) | 11 | `addArtifact` built the INSERT query by string concatenation, so a crafted title could alter the SQL | Used `?` placeholders and passed the values as a tuple |
| `shell=True` (B602, B607) | Security (Low) | 37 | `subprocess.call("echo ...", shell=True)` started a shell for a simple message | Removed subprocess and used `print()` |
| Hardcoded password (B105) | Security (Low) | 7 | `CURATOR_PASSWORD` stored a secret in source code and was never used | Deleted the unused constant |
| Unused import (B404, W0611, F401) | Quality | 2, 3 | `datetime` and `subprocess` were imported but not needed | Removed both imports |
| Unused argument (W0613) | Quality | 15 | `curator` parameter in `displayStatus` was never used | Removed the parameter and updated the call in `main()` |
| Naming (C0103) | Style | 10, 15, 24, 31, 36 | Function names used camelCase instead of snake_case | Renamed to `add_artifact`, `display_status`, `total_value`, `move_to_display`, `backup_note` |
| Missing docstrings (C0114, C0116) | Quality | 1, 10, 15, 24, 31, 36, 40 | Module and functions had no docstrings | Added a docstring to the module and every function |
| Unnecessary else (R1705) | Quality | 17-20 | `else` after `return` was redundant | Removed the `else` and returned directly |
| Line too long (E501, C0301) | Style | 6, 11, 16, 26, 32 | Lines exceeded 79 characters | Wrapped long lines using parentheses and adjacent strings |
| Blank lines and spacing (E303, E225) | Style | 24, 25 | Three blank lines before a function and `total=0` had no spaces | Used two blank lines and `total = 0` |
| No main guard | Bug | 50 | `main()` ran on import | Wrapped it in `if __name__ == "__main__":` |

## Results

| Tool | Before | After |
|------|--------|-------|
| Pylint | 4.85/10 | 10.00/10 |
| Bandit | 5 issues | 0 issues |
| Flake8 | 8 issues | 0 issues |