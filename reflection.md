# Reflection

## 1. Which issues were the easiest to fix, and which were the hardest?

The easiest were the style and naming issues, such as renaming camelCase functions to snake_case, adding docstrings, removing the unused `datetime` import and fixing spacing. These were mechanical changes that the tools explained clearly. The hardest was the SQL injection in `addArtifact` (Bandit B608). It needed a real change in how the query is built, switching from string concatenation to `?` placeholders with a values tuple, and I had to confirm the program still gave the same output.

## 2. Did the static analysis tools report any false positives or warnings that did not need fixing?

Some Bandit warnings were low severity, such as the `subprocess` import (B404) and the `shell=True` call (B602), because the command was a fixed string with no user input. They were still worth fixing, since `shell=True` can become dangerous if the command is later changed to include user input, and `print()` does the same job. The hardcoded password (B105) was also never used, but removing it was correct because secrets should not sit in source code. Overall I did not find clear false positives, although the severity labels did not always match the real risk in this small program.

## 3. How does static analysis improve code quality, security and maintainability?

Static analysis checks code without running it, so it catches problems early and consistently. Bandit found the SQL injection, which is a serious vulnerability that the program's normal output would never reveal. Pylint improved readability and design through docstrings, naming and removal of unused code. Flake8 enforced a consistent PEP 8 style. Together these make the code safer and easier for other developers to read and change. My Pylint score went from 4.85/10 to 10.00/10, Bandit from 5 issues to 0 and Flake8 from 8 issues to 0.

## 4. How would you integrate these tools into a real development workflow?

I would run them automatically in two places. First, as pre-commit hooks, so issues are caught on the developer's machine before code is committed. Second, in a CI pipeline such as GitHub Actions, so every pull request is checked and merging is blocked if Bandit finds issues or the Pylint score drops below a set threshold. Teams can add configuration files to set rules and use `# nosec` comments only with a documented reason, so developers don't ignore the tools. Static analysis does not replace code review or testing, though. It cannot catch everything, such as the missing check for an artifact ID that doesn't exist, which would crash `display_status`.