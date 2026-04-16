# Audit Log Workflow – Setup & Troubleshooting

The **Audit Log** workflow (`.github/workflows/audit-log.yml`) records every
meaningful repository event as a JSON entry on the orphan `audit-logs` branch.

## Common failure: push rejected by repository rulesets

If the workflow run logs show:

```
remote: error: GH013: Repository rule violations found for refs/heads/audit-logs.
remote: - Cannot create ref due to creations being restricted.
remote: - 6 of 6 required status checks are expected.
```

…the push is blocked by a **repository ruleset** (not classic branch protection).
Choose **one** of the remediation options below.

---

### Option A – Add an `AUDIT_PUSH_PAT` secret (recommended)

A Personal Access Token (PAT) belonging to a user who is permitted to push to
the `audit-logs` branch under the active repository ruleset (typically a repo
admin or a user listed as a **bypass actor** in the ruleset).

> **Important:** A PAT alone does not bypass rulesets. The token's owner must be
> an allowed bypass actor for the ruleset that covers the `audit-logs` branch.
> If no bypass actors are configured, you will also need to add the PAT owner
> (or adjust the ruleset per Option B).

1. Go to **GitHub → Settings → Developer settings → Personal access tokens →
   Fine-grained tokens**.
2. Click **Generate new token**.
3. Set **Repository access** to **Only select repositories** → pick this repo.
4. Under **Permissions → Repository permissions**, grant **Contents: Read and
   write**. No other permissions are needed.
5. Click **Generate token** and copy the value.
6. Ensure the token owner is listed as a **bypass actor** in the repository
   ruleset that covers the `audit-logs` branch (Settings → Rules → Rulesets →
   select the ruleset → Bypass list).
7. In this repository go to **Settings → Secrets and variables → Actions**.
8. Click **New repository secret**, name it **`AUDIT_PUSH_PAT`**, paste the
   token, and save.

The workflow will automatically use `AUDIT_PUSH_PAT` as a fallback whenever the
default `GITHUB_TOKEN` push fails.

### Option B – Exempt the `audit-logs` branch from the ruleset

1. Go to **Settings → Rules → Rulesets**.
2. Open the ruleset that applies to the `audit-logs` branch (look for one
   targeting "all branches" or a pattern that matches `audit-logs`).
3. Either:
   - **Add a bypass actor**: add *GitHub Actions* (the built-in app) so it can
     push to protected branches.
   - **Narrow the branch target**: change the target pattern so that
     `audit-logs` is excluded (e.g. use `refs/heads/main` instead of
     `refs/heads/**`).
4. Make sure the ruleset's "Restrict creations" and "Required status checks"
   rules do not apply to `audit-logs`.

### Option C – Use classic branch protection instead

If using classic branch protection (not rulesets), ensure:

- The `audit-logs` branch does **not** have "Require a pull request before
  merging" enabled, **or**
- The "Allow specified actors to bypass required pull requests" list includes
  `github-actions[bot]`.

---

## Artifact backup

Even when the push fails, every audit entry is uploaded as a **workflow
artifact** named `audit-log-entry-<run_id>` and retained for 90 days. You can
download these from the workflow run's **Artifacts** section and replay them
once the push issue is resolved.

## Diagnostics

When a push fails, the workflow automatically logs:

- `git remote -v`
- `git show-ref`
- `git status --porcelain`
- `git branch -av`
- The raw push error output

Check the **"Push failure diagnostics"** step in the run log for details.
