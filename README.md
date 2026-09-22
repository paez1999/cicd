# cicd

Small Python project (`mathutils`) with a pytest test suite and GitHub Actions CI.

## Setup

```bash
pip install -e .[dev]
```

## Run tests

```bash
pytest
```

## CI/CD pipeline

The workflow at [`.github/workflows/ci.yml`](.github/workflows/ci.yml) has three jobs:

1. **test** — installs deps and runs `pytest`.
2. **build** (simulado) — runs only if `test` passes; writes `image-info.txt` and uploads it as an artifact.
3. **deploy** — runs only on manual dispatch (`workflow_dispatch`) or on pushes to `main`; echoes a simulated deploy to the chosen environment.

It triggers on `push` (to `main`, `develop`, `feature/**`), `pull_request` (to `main`, `develop`), a nightly `schedule`, and `workflow_dispatch`.

### Running the workflow manually

**From the GitHub UI:**

1. Go to the repo's **Actions** tab.
2. Select **CI/CD Pipeline** in the left sidebar.
3. Click **Run workflow**, pick the branch, set the `environment` input (default `staging`), and click **Run workflow**.

**From the CLI (`gh`):**

```bash
gh workflow run ci.yml --ref feature/ci -f environment=staging
```

### Re-running failed jobs

If a run fails, open that run in the **Actions** tab and click **Re-run jobs → Re-run failed jobs** (or `gh run rerun <run-id> --failed` from the CLI). This re-executes only the jobs that failed, using the same commit.

### Finding logs and artifacts

- **Logs**: Actions tab → select the workflow run → select a job → expand each step to see its log output.
- **Artifacts**: open the run's summary page (bottom of the page, or the "Artifacts" section) — the `build` job uploads `image-info.txt` there whenever it runs successfully.
