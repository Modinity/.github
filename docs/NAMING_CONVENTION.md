# 📛 Naming Convention

> **Modinity Engineering — Engineering Best Practices**

Consistent naming makes it easy to understand what a repository does, who owns it, and where it lives in our stack — at a glance.

---

## Table of Contents

- [Repository Naming](#-repository-naming)
- [Branch Naming](#-branch-naming)
- [Environment Suffixes](#-environment-suffixes)
- [Quick Reference](#-quick-reference)

---

## 📦 Repository Naming

### Format

```
{department}-{project}-{service}
```

| Segment | Description | Example values |
|---|---|---|
| `department` | The team or business unit that owns the repo | `eng`, `ops`, `data`, `finance`, `pme`, `crs` |
| `project` | The product or initiative the repo belongs to | `platform`, `commerce`, `internal`, `logistics`, `reporting` |
| `service` | The specific service, component, or technical scope | `api`, `web`, `worker`, `bot`, `gateway`, `etl`, `infra` |

### Rules

1. **All lowercase** — no uppercase letters.
2. **Hyphens only** — no underscores, dots, or spaces.
3. **Keep segments short** — prefer abbreviations that are widely understood within the team.
4. **All three segments are required** — do not omit any segment.
5. **Max length: 60 characters** — stay concise.

### Examples

| Repository Name | Meaning |
|---|---|
| `eng-platform-api` | Engineering · Core platform · REST API |
| `eng-commerce-web` | Engineering · Commerce · Web frontend |
| `ops-platform-infra` | Ops · Core platform · GCP infrastructure (Terraform) |
| `data-reporting-etl` | Data team · Reporting · ETL pipeline |
| `eng-platform-auth` | Engineering · Core platform · Auth service |
| `pme-internal-vm` | PME team · Internal tools · VM infrastructure |
| `eng-commerce-worker` | Engineering · Commerce · Background worker |
| `ops-internal-k8s` | Ops · Internal tooling · Kubernetes config |

### ❌ Anti-Patterns

```
# Bad — missing segments, wrong case, wrong separator
PlatformAPI
eng_platform_api
api-service
platform-eng
ENG-platform-API
```

---

## 🌿 Branch Naming

Branch names must reflect the **type of change** and reference the relevant **ticket or issue**.

### Format

```
{type}/{ticket-id}-{short-description}
```

### Branch Types

| Type | When to use |
|---|---|
| `feature` | New feature or enhancement |
| `fix` | Bug fix |
| `hotfix` | Urgent production patch |
| `chore` | Maintenance, dependency updates, refactoring |
| `release` | Preparing a release |
| `docs` | Documentation-only changes |
| `ci` | CI/CD pipeline changes |

### Rules

1. All lowercase, hyphens only.
2. Ticket ID (e.g. Jira, Linear, GitHub Issue) **must** be included when one exists.
3. Short description should be 3–5 words maximum.
4. No personal names (e.g. `john-fix` is not allowed).

### Examples

```bash
feature/ENG-123-add-oauth-login
fix/ENG-456-cart-total-rounding
hotfix/ENG-789-null-pointer-checkout
chore/ENG-321-upgrade-nextjs-15
docs/ENG-111-update-api-readme
release/v1.4.0
ci/ENG-555-add-docker-cache
```

---

## 🌍 Environment Suffixes

When a repository or resource is environment-scoped, use the following suffixes:

| Suffix | Environment |
|---|---|
| `-dev` | Development |
| `-stg` | Staging |
| `-prd` | Production |

> **Note:** These suffixes apply to infrastructure repos and GCP project IDs, **not** to application service repos (which are environment-agnostic and deployed across environments via CI/CD).

### Examples

```
ops-platform-infra-prd   # Production infra repo
ops-platform-infra-stg   # Staging infra repo
platform-internal-prd    # GCP project ID (production)
```

---

## 🗂 Quick Reference

```
Repository  →  {department}-{project}-{service}
Branch      →  {type}/{ticket-id}-{short-description}
GCP Project →  {project}-{scope}-{env}
```

---

*Last updated: May 2026 · Owned by Modinity Engineering*
