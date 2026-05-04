# 🛠️ Development Guide

> **Modinity Engineering — Engineering Best Practices**

This guide covers the day-to-day engineering workflow at Modinity: from writing commits to opening a Pull Request and getting it merged. Following these conventions keeps our history clean, our reviews fast, and our deployments safe.

---

## Table of Contents

- [Git Workflow](#-git-workflow)
- [Commit Messages](#-commit-messages)
- [Pull Request Conventions](#-pull-request-conventions)
- [PR Checklist](#-pr-checklist)
- [Code Review Standards](#-code-review-standards)
- [Definition of Done](#-definition-of-done)
- [Protected Branches & Merging Rules](#-protected-branches--merging-rules)

---

## 🔀 Git Workflow

We follow a **trunk-based development** model with short-lived feature branches.

```
main (protected)
  └── feature/ENG-123-add-oauth-login
  └── fix/ENG-456-cart-total-rounding
  └── hotfix/ENG-789-null-pointer-checkout
```

### Flow

```
1. Pull the latest main
   git pull origin main

2. Create a branch (see Naming Convention)
   git checkout -b feature/ENG-123-add-oauth-login

3. Make small, focused commits (see Commit Messages)

4. Push and open a Pull Request against main

5. Get at least 1 approval → Squash & Merge

6. Delete the branch after merge
```

> **Keep branches short-lived.** Aim to merge within 1–3 working days.  
> Long-running branches cause merge conflicts and slow down the team.

---

## ✍️ Commit Messages

We use **[Conventional Commits](https://www.conventionalcommits.org/)** for all commit messages.

### Format

```
{type}({scope}): {short summary}

[optional body]

[optional footer: BREAKING CHANGE / closes #issue]
```

### Commit Types

| Type | When to use |
|---|---|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation only changes |
| `style` | Formatting, missing semicolons — no logic change |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `perf` | Performance improvement |
| `test` | Adding or fixing tests |
| `chore` | Build process, dependency updates, tooling |
| `ci` | CI/CD configuration changes |
| `revert` | Reverts a previous commit |

### Rules

1. **Imperative mood** — write "add feature" not "added feature" or "adds feature".
2. **Lowercase** — the summary line is all lowercase (after the type prefix).
3. **No period** at the end of the summary line.
4. **Max 72 characters** on the summary line.
5. **Scope is optional** but recommended — use the service or module name.

### Examples

```bash
feat(auth): add google oauth login
fix(cart): correct rounding on total price calculation
docs(api): update webhook endpoint documentation
chore(deps): upgrade next.js to 15.1.0
ci(docker): add layer caching to build pipeline
perf(db): add index on orders.created_at column
refactor(shipping): extract rate calculation into utility
test(auth): add unit tests for token refresh logic

# With breaking change
feat(api)!: rename /v1/orders to /v2/orders

BREAKING CHANGE: the /v1/orders endpoint is removed.
Clients must migrate to /v2/orders.
```

---

## 📬 Pull Request Conventions

### PR Title Format

```
{type}({scope}): {short description}
```

The PR title **must follow the same Conventional Commits format** as commit messages. This ensures our merge commits and changelogs are consistent.

#### PR Title Examples

```
feat(auth): add google oauth single sign-on
fix(modiship): correct weight calculation for heavy parcels
chore(infra): upgrade cloud run memory to 1GB
docs(api): document rate limiting headers
ci(deploy): add staging deployment on PR merge
hotfix(checkout): resolve null pointer on empty cart
```

#### ❌ Bad PR Titles

```
fix stuff
WIP
john's changes
updated file
ENG-123
```

### PR Description Template

Every PR must include a description. Use the following structure:

```markdown
## 📋 Summary
<!-- What does this PR do? Why is this change needed? -->

## 🔗 Related Issue / Ticket
<!-- Link to the Jira / Linear / GitHub issue -->
Closes #[issue-number]

## 🧪 How to Test
<!-- Steps for the reviewer to verify the change works -->
1. 
2. 
3. 

## 📸 Screenshots / Recordings
<!-- For UI changes, include before/after screenshots -->

## ✅ Checklist
- [ ] Code follows our [naming conventions](./NAMING_CONVENTION.md)
- [ ] Self-reviewed my own code
- [ ] Added or updated tests where applicable
- [ ] Updated documentation if needed
- [ ] No secrets or credentials are committed
- [ ] Verified the change works in a local / dev environment
```

### PR Size Guidelines

| Size | Lines changed | Policy |
|---|---|---|
| 🟢 **Small** | < 200 lines | Ideal — fast to review |
| 🟡 **Medium** | 200–500 lines | Acceptable — add extra context in description |
| 🔴 **Large** | > 500 lines | Must be split unless it's auto-generated code |

> **Large PRs slow down reviews and increase the risk of missed bugs.**  
> If you cannot split the work, add a detailed description and request a pairing session.

---

## ✅ PR Checklist

Before marking a PR as **Ready for Review**, ensure:

- [ ] Branch is up to date with `main` (rebased or merged)
- [ ] All CI checks pass (lint, tests, build)
- [ ] No `console.log`, `debugger`, or temporary code left in
- [ ] Environment variables are documented (not hardcoded)
- [ ] Secrets are never committed — use Secret Manager or `.env` (gitignored)
- [ ] Database migrations (if any) are backward-compatible
- [ ] Breaking changes are clearly flagged with `!` and a `BREAKING CHANGE` footer

---

## 👀 Code Review Standards

### For Authors

- **Self-review first** — read your own diff before requesting a review.
- **Keep PRs focused** — one PR, one purpose.
- **Respond to all comments** — either resolve with a code change or reply with reasoning.
- **Don't merge your own PR** — unless it's a solo project or an emergency hotfix.

### For Reviewers

- **Review within 1 business day** — respect your teammates' time.
- **Be constructive, not critical** — suggest, don't demand.
- **Use comment prefixes** to set expectations:

| Prefix | Meaning |
|---|---|
| `nit:` | Minor style concern — author's discretion |
| `suggestion:` | Improvement idea — not blocking |
| `question:` | Seeking understanding — not blocking |
| `blocker:` | Must be addressed before merge |

#### Example Review Comments

```
nit: consider renaming `data` to `orderData` for clarity

suggestion: this could be extracted into a shared utility function

blocker: this will throw a null pointer if `user` is undefined
```

- **Approve explicitly** only when you are satisfied — don't approve just to unblock.
- **Request changes** if any blockers exist.

---

## 🏁 Definition of Done

A task is considered **Done** when:

1. ✅ The code is merged into `main`
2. ✅ All CI/CD checks pass
3. ✅ The feature is deployed to the **staging** environment
4. ✅ A smoke test has been performed in staging
5. ✅ The related ticket is updated and closed
6. ✅ Documentation is updated if user-facing behavior changed

---

## 🔒 Protected Branches & Merging Rules

| Branch | Protection | Merge Strategy |
|---|---|---|
| `main` | ✅ Protected | Squash & Merge only |
| `release/*` | ✅ Protected | Merge Commit |
| `feature/*` | ❌ Open | — |
| `fix/*` | ❌ Open | — |
| `hotfix/*` | ❌ Open | — |

### Why Squash & Merge on `main`?

- Keeps `main` history **linear and readable** — one commit per feature/fix.
- The squashed commit title comes directly from the **PR title**, which is why the PR title must follow Conventional Commits.
- Makes `git bisect` and `git log` much easier to navigate.

### Hotfix Process

For urgent production issues:

```
1. Branch off main:
   git checkout -b hotfix/ENG-999-fix-checkout-crash

2. Fix, test, and open a PR immediately
3. Request review from at least 1 senior engineer
4. Merge → triggers production deployment
5. Tag the release:
   git tag -a v1.3.1 -m "hotfix: fix checkout null pointer"
   git push origin v1.3.1
```

---

*Last updated: May 2026 · Owned by Modinity Engineering*
