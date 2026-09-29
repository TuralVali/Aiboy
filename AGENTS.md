# Python Environment Setup Agent

## Purpose

This agent prepares a Python virtual environment for a repository provided by the user.

The user only needs to provide the repository location. The agent should:

1. Validate the repository path.
2. Ask for the virtual environment name if it was not supplied.
3. Detect the available Python executable.
4. Create the virtual environment.
5. Upgrade `pip`, `setuptools`, and `wheel`.
6. Detect `requirements.txt`.
7. Install all dependencies from `requirements.txt`.
8. Optionally install additional packages requested by the user.
9. Validate that the environment can import the installed packages.
10. Produce a concise setup report.

## Required Inputs

### Repository location

The user provides a local repository/folder path.

Example:

```text
C:\Projects\my-risk-model
```

or:

```text
D:\Git\credit-risk-model
```

### Virtual environment name

If the user does not provide a name, ask:

```text
What should I name the virtual environment?
```

Recommended default:

```text
.venv
```

### Additional packages

Ask whether the user wants packages installed in addition to `requirements.txt`.

Example:

```text
Additional packages (optional, comma-separated):
pyarrow, pandera, plotly
```

If the user says none, continue without additional packages.

## Execution Workflow

### Step 1 — Validate repository

Confirm that the supplied path exists and is a directory.

If it does not exist:

- Stop.
- Clearly report the invalid path.
- Ask the user to provide a valid repository path.

### Step 2 — Detect Python

Check for Python using the following priority:

1. `python`
2. `python3`
3. `py`

Prefer a Python version compatible with the repository.

If a repository contains a configuration file such as:

- `pyproject.toml`
- `.python-version`
- `Pipfile`
- `setup.py`
- `setup.cfg`

inspect it for Python-version requirements before creating the environment.

Do not silently choose an incompatible Python version.

### Step 3 — Create virtual environment

Create the environment inside the repository unless the user explicitly specifies another location.

Default:

```text
<repository>/.venv
```

Example:

```text
C:\Projects\my-risk-model\.venv
```

If the requested environment already exists:

- Do not delete it automatically.
- Ask whether to reuse it or recreate it.

### Step 4 — Activate/use the environment

Do not rely on shell activation for subsequent commands.

Instead, call the Python and pip executables directly from the environment.

Windows:

```text
<repo>\.venv\Scripts\python.exe
<repo>\.venv\Scripts\pip.exe
```

Linux/macOS:

```text
<repo>/.venv/bin/python
<repo>/.venv/bin/pip
```

This avoids shell-specific activation problems.

### Step 5 — Upgrade packaging tools

Run:

```bash
python -m pip install --upgrade pip setuptools wheel
```

Use the Python executable belonging to the newly created environment.

### Step 6 — Install requirements

If `requirements.txt` exists:

```bash
python -m pip install -r requirements.txt
```

If multiple requirement files exist, inspect them and ask before installing multiple files.

Examples:

```text
requirements.txt
requirements-dev.txt
requirements-test.txt
```

Do not automatically install development/test requirements unless the user asks for them.

If no `requirements.txt` exists:

- Report that it was not found.
- Continue if additional packages were provided.
- Otherwise finish with a warning rather than pretending the dependency installation was completed.

### Step 7 — Install additional packages

If the user supplied additional packages, install them after `requirements.txt`.

Example:

```bash
python -m pip install pyarrow pandera plotly
```

Never silently add arbitrary packages.

### Step 8 — Validate installation

Run:

```bash
python -m pip check
```

Then perform a lightweight Python validation.

For packages explicitly installed during setup, attempt imports where practical.

Example:

```python
import pandas
import numpy
import scipy
```

Do not fail the whole setup merely because a package's import name differs from its pip package name. Report the package separately if import validation cannot be inferred safely.

### Step 9 — Generate setup report

Report:

```text
Repository:
Environment:
Python version:
Python executable:
Requirements file:
Requirements installed:
Additional packages:
pip check:
Status:
```

Example:

```text
Repository: C:\Projects\credit-risk-model
Environment: C:\Projects\credit-risk-model\.venv
Python: 3.12.6
Requirements: requirements.txt
Additional packages: pyarrow, pandera
pip check: PASS
Status: SUCCESS
```

## Safety Rules

- Never delete an existing `.venv` without explicit confirmation.
- Never modify source code as part of environment setup.
- Never modify `requirements.txt` unless explicitly requested.
- Never upgrade arbitrary project dependencies unless requested.
- Do not install packages unrelated to the user's request.
- Do not use global Python when the virtual environment can be used.
- Use `python -m pip` rather than a potentially unrelated global `pip`.
- Preserve the repository's existing files.
- If installation fails, report the exact command/category that failed and continue only with independent validation steps where appropriate.

## Optional Enhancements

If the user asks, the agent can additionally:

- Create/update `.gitignore` with `.venv/`.
- Select a VS Code Python interpreter.
- Generate a `requirements.txt` from an existing environment.
- Create a `requirements-dev.txt`.
- Run project tests after setup.
- Run a smoke test.
- Generate an environment report.
- Detect whether Poetry, uv, Pipenv, or Conda is already being used.
- Offer to use the repository's existing package manager instead of plain pip.

## Recommended User Prompt

```text
Set up the Python environment for this repository.

Repository:
<repository path>

Environment name:
<name, or ask me>

Additional packages:
<optional packages>

Follow the Python Environment Setup Agent instructions.
Do not modify application source code.
Do not delete an existing environment without asking me.
```

## Expected Result

At the end, the repository should contain:

```text
repository/
├── .venv/
├── requirements.txt
├── ...
```

and the agent should clearly state whether the environment was successfully created and dependencies installed.
