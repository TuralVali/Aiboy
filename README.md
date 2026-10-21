# Merged PR Adversarial Review

This folder contains a reusable instruction file for GitHub Copilot coding agents.

## Setup
Copy `AGENTS.md` to the **root of the repository** you want to review. GitHub Copilot commonly recognizes repository-level `AGENTS.md` as agent instructions. Keep your existing `README.md` intact.

## Start a review
Open GitHub Copilot Chat in that repository and send:

```text
Review the already merged PR represented by commit hash <COMMIT_HASH>.
Follow AGENTS.md strictly. Determine the correct diff/range, analyze the changed code,
run applicable static checks and adversarial DataFrame/Parquet tests, and write all
review-only outputs to review_artifacts/<SHORT_HASH>/.
Do not modify production code. Ask before installing dependencies or running
expensive/destructive operations. Provide confirmed, reproducible findings only.
```

For a multi-commit PR, provide its merge/squash commit or the full commit range.

## Outputs
Expect `REPORT.md`, `test_matrix.csv`, review-only `tests/`, synthetic `fixtures/`, and `run_log.txt` under `review_artifacts/<SHORT_HASH>/`.

## Notes
The agent needs access to repository history, a working Python environment, and permission to run tests. Some checks may be unavailable; the report must state limitations. This instruction set does not guarantee every defect will be found.
