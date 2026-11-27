# GitHub Actions Practice

A structured, hands-on learning repository for **GitHub Actions** and CI/CD pipeline design. Each topic is implemented as an isolated, runnable workflow, tested against real trigger scenarios, and documented below.

![01](https://github.com/Afzalhansi/Github-Actions-Practice/actions/workflows/01-hello-world.yml/badge.svg)
![02](https://github.com/Afzalhansi/Github-Actions-Practice/actions/workflows/02-triggers.yml/badge.svg)
![03](https://github.com/Afzalhansi/Github-Actions-Practice/actions/workflows/03-steps-checkout.yml/badge.svg)
![04](https://github.com/Afzalhansi/Github-Actions-Practice/actions/workflows/04-filters.yml/badge.svg)
![05](https://github.com/Afzalhansi/Github-Actions-Practice/actions/workflows/05-contexts.yml/badge.svg)
![06](https://github.com/Afzalhansi/Github-Actions-Practice/actions/workflows/06-conditionals.yml/badge.svg)
![07](https://github.com/Afzalhansi/Github-Actions-Practice/actions/workflows/07-env-vars.yml/badge.svg)
![08](https://github.com/Afzalhansi/Github-Actions-Practice/actions/workflows/08-secrets-vars.yml/badge.svg)
![09](https://github.com/Afzalhansi/Github-Actions-Practice/actions/workflows/09-outputs.yml/badge.svg)

---

## Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Workflow Catalog](#workflow-catalog)
- [Learning Roadmap](#learning-roadmap)
- [Getting Started](#getting-started)
- [Conventions](#conventions)
- [Key Takeaways](#key-takeaways)
- [Tech Stack](#tech-stack)
- [Author](#author)

---

## Overview

This repository documents a progressive path through GitHub Actions, from the anatomy of a workflow to production-style pipelines. It is designed around three principles:

- **One concept per workflow.** Each file isolates a single feature so its behaviour is easy to observe in the run logs.
- **Verified by execution.** Every workflow is triggered in multiple ways (push, pull request, manual dispatch, tags) and the results are checked against expected behaviour.
- **Applied to real use cases.** The later stages target practical pipelines: test automation (Selenium, Cucumber, TestNG), container builds, cloud deployment, and infrastructure as code.

---

## Repository Structure

```
.
├── .github/
│   └── workflows/
│       ├── 01-hello-world.yml
│       ├── 02-triggers.yml
│       ├── 03-steps-checkout.yml
│       ├── 04-filters.yml
│       ├── 05-contexts.yml
│       ├── 06-conditionals.yml
│       ├── 07-env-vars.yml
│       ├── 08-secrets-vars.yml
│       └── 09-outputs.yml
├── src/                  # sample files used to test path filters
├── docs/                 # sample files used to test path filters
└── README.md
```

---

## Workflow Catalog

| # | Workflow | Topic | Key concepts | Triggers |
|---|----------|-------|--------------|----------|
| 01 | `01-hello-world.yml` | Workflow anatomy | `name`, `on`, `jobs`, `runs-on`, `steps`; ephemeral runner | push, manual |
| 02 | `02-triggers.yml` | Event triggers | `push`, `pull_request`, `schedule` (cron, UTC), `workflow_dispatch` with typed inputs | push, pull request, schedule, manual |
| 03 | `03-steps-checkout.yml` | Steps and actions | `run` vs `uses`, `actions/checkout`, `actions/setup-java`, `with`, `working-directory` | push, manual |
| 04 | `04-filters.yml` | Workflow filters | `branches`, `tags`, `paths`; glob patterns; filter behaviour on tag pushes | push, pull request |
| 05 | `05-contexts.yml` | Contexts and expressions | `github`, `runner`, `env` contexts; `contains()`, `format()`, `startsWith()`, `toJSON()` | push, tags, manual |
| 06 | `06-conditionals.yml` | Conditional execution | `if:`, `success()`, `failure()`, `always()`; `&&`, `\|\|`, and grouping | push, manual |
| 07 | `07-env-vars.yml` | Environment variables | workflow, job, and step scope; precedence; `$GITHUB_ENV`; default variables; job isolation | push, manual |
| 08 | `08-secrets-vars.yml` | Secrets and variables | `secrets` vs `vars` contexts; log masking; passing secrets through `env:`; `GITHUB_TOKEN` and `permissions`; GitHub CLI authentication | push, manual |
| 09 | `09-outputs.yml` | Step and job outputs | step `id`, `$GITHUB_OUTPUT`, job `outputs:`, `needs` context; passing data between jobs | push, manual |

The scheduled run in workflow 02 uses `0 5 * * 1-5`, which is 05:00 UTC, or 09:00 in Dubai (UTC+4), Monday to Friday.

Workflow 08 requires a repository secret named `MY_SECRET` and a repository variable named `APP_ENV` (Settings, Secrets and variables, Actions). Without them the values resolve to empty strings.

---

## Learning Roadmap

**Foundations**
- [x] Workflow anatomy
- [x] Triggers and events
- [x] Steps, actions, and checkout

**Control flow**
- [x] Workflow filters
- [x] Contexts and expressions
- [x] Conditionals
- [x] Environment variables

**Configuration and data**
- [x] Secrets and variables, `GITHUB_TOKEN`
- [x] Step and job outputs
- [ ] Environments and approval gates

**Scaling and performance**
- [ ] Matrix strategy
- [ ] Job dependencies (`needs`)
- [ ] Caching
- [ ] Artifacts
- [ ] Concurrency, timeouts, `continue-on-error`

**Reuse and infrastructure**
- [ ] Reusable workflows
- [ ] Composite actions
- [ ] Service containers and container jobs
- [ ] Self-hosted runners

**Real-world pipelines**
- [ ] Selenium / Cucumber / TestNG test pipeline with report artifacts
- [ ] Docker image build and push
- [ ] AWS authentication with OIDC
- [ ] Terraform plan and apply pipeline
- [ ] End-to-end CI/CD to AWS

**Security and maintenance**
- [ ] Least-privilege `permissions`
- [ ] Pinning actions to commit SHAs
- [ ] Dependabot for action updates
- [ ] Debugging and re-running with debug logging

---

## Getting Started

### Prerequisites

- A GitHub account with access to this repository
- Git installed locally
- A code editor with YAML support (for example VS Code with the *GitHub Actions* and *YAML* extensions)

### Clone the repository

```bash
git clone https://github.com/Afzalhansi/Github-Actions-Practice.git
cd Github-Actions-Practice
```

### Configure secrets and variables (required for workflow 08)

In the repository, open **Settings, Secrets and variables, Actions** and create:

| Type | Name | Example value |
|------|------|---------------|
| Repository secret | `MY_SECRET` | any dummy value |
| Repository variable | `APP_ENV` | `practice` |

Secrets and variables are separate stores. A value created in one is not visible through the other's context.

### Run a workflow

1. Open the **Actions** tab of the repository.
2. Select a workflow from the sidebar.
3. Choose **Run workflow** for workflows that support manual dispatch, or push a change that matches the workflow's filters.
4. Open the run and inspect the logs for each step.

Several workflows use a `paths` filter that includes their own file, so editing a workflow and pushing is enough to trigger it.

---

## Conventions

- Workflow files are numbered by topic and named in kebab-case.
- Each workflow has a descriptive `name:` that matches its number, so runs are easy to identify in the Actions tab.
- Third-party actions are pinned to a major version (for example `actions/checkout@v4`).
- Values that appear inside shell scripts, including secrets, are passed through `env:` instead of being interpolated directly with `${{ }}`.
- Multi-word shell commands use `run: |` so that colons inside the command cannot break YAML parsing.
- Commit messages are short, imperative, and describe the change (for example `Add outputs workflow`).

---

## Key Takeaways

- Workflows must live in `.github/workflows/` and be valid YAML. A colon followed by a space inside an unquoted `run:` value breaks parsing, so multi-word commands use `run: |`.
- Every job runs on a fresh runner that starts empty. Use `actions/checkout` to obtain the repository contents.
- A `pull_request` run executes against a temporary merge ref (`refs/pull/<n>/merge`), not the source branch.
- `paths` filters are not evaluated for tag pushes, so a matching tag always triggers a run.
- Cron expressions are evaluated in UTC.
- String comparisons in `if:` conditions must match exactly. A single trailing space inside a quoted value silently prevents a match.
- Expressions in `${{ }}` are resolved by GitHub before the shell executes the script. Shell variables (`$VAR`) are resolved at run time.
- `failure()` and `always()` replace the default `success()` check, which is what makes cleanup and notification steps possible.
- Variables are scoped per job. Data moves between jobs only through explicit outputs.
- Secrets and variables live in separate stores (`secrets.*` and `vars.*`). A missing value resolves to an empty string without an error, so a wrong location fails silently.
- Log masking matches the exact secret string only. A transformed copy of a secret is not masked, so secrets should never be printed.
- Step outputs require a step `id` and are written to `$GITHUB_OUTPUT`. A downstream job reads them through `needs.<job>.outputs.<name>` and must declare `needs`.
- When a condition or value misbehaves, a debug step that prints the actual values is the fastest diagnostic.

---

## Tech Stack

- GitHub Actions (YAML workflows, GitHub-hosted Ubuntu runners)
- GitHub CLI (`gh`) authenticated with the automatic `GITHUB_TOKEN`
- Java 17 (Eclipse Temurin) via `actions/setup-java`
- Planned: Maven, Selenium, Cucumber, TestNG, Docker, AWS, Terraform

---

## Author

**Abdul Basit Hansi**
QA Automation Engineer, building skills toward SDET and DevOps roles.

[LinkedIn](https://linkedin.com/in/abdulhansi) · [GitHub](https://github.com/Afzalhansi)