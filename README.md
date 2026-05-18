# Testsigma CI/CD Integration Demo

This repository demonstrates how to integrate [Testsigma](https://testsigma.com) automated tests into a GitHub Actions CI/CD pipeline.

## How It Works

Every code push (to any branch) or pull request targeting `main`/`master` automatically triggers the configured Testsigma Test Plan via the Testsigma REST API. The workflow polls until the run completes and marks the GitHub check as **passed** or **failed** accordingly.

## Setup

### 1. Add GitHub Secrets

Go to **Settings → Secrets and variables → Actions** in this repository and add:

| Secret Name | Value |
|---|---|
| `TESTSIGMA_API_KEY` | Your Testsigma API key |
| `TESTSIGMA_TEST_PLAN_ID` | Your Test Plan ID (e.g. `9697`) |

### 2. Workflow Trigger

The pipeline triggers on:
- Any `push` to any branch
- Any `pull_request` targeting `main` or `master`

### 3. Pipeline Steps

1. **Trigger** — POSTs to `https://app.testsigma.com/api/v1/execution_results` with the test plan ID
2. **Poll** — GETs the run status every 30 seconds (up to 1 hour timeout)
3. **Result** — Exits 0 (success) on `PASSED`, exits 1 (failure) on `FAILED` or `ABORTED`

## Testsigma API Reference

- Trigger: `POST https://app.testsigma.com/api/v1/execution_results`
- Status:  `GET  https://app.testsigma.com/api/v1/execution_results/{run_id}`
- Auth header: `Authorization: Bearer <API_KEY>`

## Finding Your Test Plan ID

In Testsigma: **Test Plans → your plan → CI/CD Integrations** — the execution ID is shown in the REST API trigger call section.
