# Repository Ruleset Setup Guide

This document describes the **branch protection rulesets** that must be configured in
the GitHub repository settings to enforce the security posture of this repository.

GitHub Rulesets are configured in **Settings → Rules → Rulesets** in the repository UI,
or via the GitHub REST API / GitHub CLI.

---

## Ruleset 1 — Main Branch Protection (Required)

**Name:** `main-branch-protection`  
**Enforcement:** `Active`  
**Target:** Branch `main`

| Rule | Setting |
|------|---------|
| Restrict creations | ✅ Enabled |
| Restrict updates | ✅ Enabled — require pull requests |
| Restrict deletions | ✅ Enabled |
| Require linear history | ✅ Enabled |
| Require deployments to succeed | ✅ Enabled (configure target environment) |
| Require signed commits | ✅ Enabled |
| Require a pull request before merging | ✅ Enabled |
| — Required approvals | `1` |
| — Dismiss stale reviews on new commits | ✅ Enabled |
| — Require review from Code Owners | ✅ Enabled |
| — Require last push approval | ✅ Enabled |
| Require status checks to pass | ✅ Enabled |
| — Required checks | `Analyze (actions)`, `Dependency Vulnerability Review`, `Validate PR Title`, `Validate Branch Name`, `Verify Security Checklist`, `Detect Large / Sensitive Files` |
| — Require branches to be up to date | ✅ Enabled |
| Block force pushes | ✅ Enabled |
| Require conversation resolution | ✅ Enabled |

---

## Ruleset 2 — Repository Lockdown (Emergency Use)

**Name:** `repository-lockdown`  
**Enforcement:** `Disabled` by default — `Active` during a lockdown incident  
**Target:** All branches (`~ALL`)

When this ruleset is set to **Active**, it applies the following ultra-restrictive rules:

| Rule | Setting |
|------|---------|
| Restrict creations | ✅ Enabled |
| Restrict updates | ✅ Enabled |
| Restrict deletions | ✅ Enabled |
| Block force pushes | ✅ Enabled |
| Require signed commits | ✅ Enabled |
| Bypass list | Repository admins only |

**To activate lockdown:**  
1. Trigger the `Repository Lockdown` workflow (see [SECURITY.md](../SECURITY.md)).  
2. Then navigate to **Settings → Rules → Rulesets → `repository-lockdown`** and set
   enforcement to **Active**.

**To deactivate after incident resolution:**  
Set enforcement back to **Disabled** once the incident is fully resolved.

---

## Ruleset 3 — Tag Protection

**Name:** `tag-protection`  
**Enforcement:** `Active`  
**Target:** Tags (`~ALL`)

| Rule | Setting |
|------|---------|
| Restrict creations | ✅ Enabled — only admins |
| Restrict deletions | ✅ Enabled |
| Block force pushes | ✅ Enabled |

---

## Configuring via GitHub CLI

```bash
# Install GitHub CLI if not already installed
# https://cli.github.com/

# Activate repository-lockdown ruleset (emergency)
gh api \
  --method PUT \
  -H "Accept: application/vnd.github+json" \
  /repos/{owner}/{repo}/rulesets/<RULESET_ID> \
  -f enforcement=active

# Deactivate after incident is resolved
gh api \
  --method PUT \
  -H "Accept: application/vnd.github+json" \
  /repos/{owner}/{repo}/rulesets/<RULESET_ID> \
  -f enforcement=disabled
```

> Replace `{owner}` and `{repo}` with the actual repository owner and name, and
> replace `<RULESET_ID>` with the numeric ID shown in the URL when viewing the ruleset
> in the repository settings.

---

## Required Repository Secrets

The following secrets must be configured in **Settings → Secrets and variables → Actions**:

| Secret Name | Purpose | Required |
|-------------|---------|----------|
| `MAIL_SERVER` | SMTP server address for email notifications | Optional |
| `MAIL_PORT` | SMTP server port for email notifications | Optional |
| `MAIL_USERNAME` | SMTP username / sender email address | Optional |
| `MAIL_PASSWORD` | SMTP password for sending alert emails | Optional |
| `GITLEAKS_LICENSE` | Gitleaks Enterprise license key (for secret scanning) | Optional |

---

## Additional Recommended Settings

Navigate to **Settings** and enable:

| Setting | Location |
|---------|----------|
| Require contributors to sign commits | Settings → Commits |
| Automatically delete head branches | Settings → Pull Requests |
| Restrict push access | Settings → Branches |
| Enable secret scanning | Settings → Security → Secret scanning |
| Enable push protection | Settings → Security → Push protection |
| Enable Dependabot alerts | Settings → Security → Dependabot |
| Enable Dependabot security updates | Settings → Security → Dependabot |
| Restrict fork creation | Settings → Forks |
| Disable public repository visibility | Settings → Danger Zone → Change visibility |
