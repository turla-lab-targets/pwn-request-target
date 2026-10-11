# pwn-request-target

Intentionally vulnerable lab target for [TurlaFSB/cicd-compromise-lab](https://github.com/TurlaFSB/cicd-compromise-lab) scenario 01 — **pwn request**.

**Do not use real secrets here. This repo is disposable. Every secret below is a dummy value.**

## The vulnerability

`.github/workflows/ci.yml` triggers on `pull_request_target`. That event runs with the base repo's `GITHUB_TOKEN` and secrets even for a PR opened from a fork — but the workflow then checks out the **PR's own head commit** (`github.event.pull_request.head.sha`) and runs `npm ci && npm test` against it. Since the PR author controls every file in their own PR, including `tests/`, they control what code runs — with this workflow's token and secret access.

This is the "pwn request" pattern described in the [OWASP Top 10 CI/CD Security Risks](https://owasp.org/www-project-top-10-ci-cd-security-risks/) (CICD-SEC-4, Poisoned Pipeline Execution) and is a real-world recurring GitHub Actions misconfiguration.

## Required repo secret

For the attack/evidence script to demonstrate secret exposure, this repo needs one dummy secret set under **Settings → Secrets and variables → Actions**:

| Name | Value |
|---|---|
| `DUMMY_CLOUD_KEY` | any placeholder string, e.g. `dummy-cloud-key-not-real-12345` |

This is never a real credential. It exists only to prove the attack path can reach a secret.

## Status

Vulnerable baseline for scenario 01. See the main repo for the attack script, evidence, and hardened fix.
