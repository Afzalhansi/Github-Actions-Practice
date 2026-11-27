# GitHub Actions Practice

A hands-on repo for learning **GitHub Actions** from scratch. Every topic has its own small workflow file in `.github/workflows/`, built and tested one step at a time. Each file is a working example you can trigger, read, and break on purpose to see what happens.

**Goal:** understand how CI/CD pipelines are built, so I can write real ones for QA automation (Selenium, Cucumber, TestNG) and DevOps work (Docker, AWS, Terraform).

---

## How to use this repo

1. Open the **Actions** tab on GitHub.
2. Pick a workflow from the left sidebar.
3. Click **Run workflow** (for the ones with a manual trigger), or push a change to trigger it.
4. Open the run and read the logs of each step.

---

## Topics covered so far

| # | File | Topic | What it shows |
|---|------|-------|---------------|
| 01 | `01-hello-world.yml` | Anatomy of a workflow | The basic parts: `name`, `on`, `jobs`, `runs-on`, `steps`. Every run starts on a fresh, empty virtual machine. |
| 02 | `02-triggers.yml` | Triggers (events) | `push`, `pull_request`, `schedule` (cron, always in UTC), and `workflow_dispatch` with a manual input form (dropdown and checkbox). |
| 03 | `03-steps-checkout.yml` | Steps, `uses` and checkout | `run` (shell commands) vs `uses` (ready-made actions), `actions/checkout` to get the code onto the runner, `actions/setup-java` to install Java 17, and `working-directory`. |
| 04 | `04-filters.yml` | Workflow filters | Run only for certain `branches`, `tags`, or `paths`. A `paths` filter is ignored when a tag is pushed. |
| 05 | `05-contexts.yml` | Contexts and expressions | Reading run information with `${{ }}`: `github`, `runner`, `env`, plus functions like `contains()`, `format()`, `startsWith()`, `toJSON()`. |
| 06 | `06-conditionals.yml` | Conditionals | `if:` on steps, and the status functions `success()`, `failure()`, `always()`. Combining conditions with `&&` and `\|\|`. |
| 07 | `07-env-vars.yml` | Environment variables | Variables at workflow, job, and step level (the narrowest wins), `$GITHUB_ENV` for later steps, default variables, and why variables never cross between jobs. |

---

## Coming next

- Secrets and variables (`secrets`, `vars`, `GITHUB_TOKEN`)
- Step and job outputs
- Environments and approvals
- Matrix builds
- Job dependencies (`needs`)
- Caching and artifacts
- Concurrency, timeouts, and `continue-on-error`
- Reusable workflows and composite actions
- Service containers and self-hosted runners
- Real pipelines: Selenium/TestNG tests, Docker build and push, AWS login with OIDC, Terraform plan and apply, full CI/CD
- Security basics: least-privilege `permissions`, pinning actions, Dependabot

---

## Lessons learned along the way

- Workflow files must live in `.github/workflows/` and be valid YAML. Indentation matters, and a `: ` inside an unquoted `run:` value breaks the file (use `run: |`).
- The runner starts empty. Add `actions/checkout` to get your code.
- A `pull_request` run uses a temporary merge ref (`refs/pull/N/merge`), not your branch.
- Cron schedules run in UTC. 9:00 AM Dubai is `0 5 * * 1-5`.
- String comparisons must match exactly. One extra space inside quotes silently breaks a condition.
- When a condition doesn't behave, add a debug step that prints the real values.
- Each job gets a fresh machine. Variables set in one job are not visible in another.

---

## Tech used

- GitHub Actions (YAML workflows)
- Ubuntu hosted runners
- Java 17 (Temurin) as the first step toward test automation pipelines