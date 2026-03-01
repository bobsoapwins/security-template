# 🔒 Secure Repository

A hardened repository for housing **confidential files**. Every action taken in this
repository is monitored, validated, and logged. Unauthorized access or disclosure is
strictly prohibited.

---

## Security Architecture

| Layer | Control | Status |
|-------|---------|--------|
| **Branch Protection** | Rulesets enforce signed commits, required reviews, status checks | See `docs/RULESET_SETUP.md` |
| **Code Ownership** | All files owned by `@bobsoapwins` via `CODEOWNERS` | ✅ Active |
| **Static Analysis** | CodeQL scans every push & PR for vulnerabilities | ✅ Active |
| **Secret Detection** | Gitleaks + TruffleHog scan every push & PR | ✅ Active |
| **Dependency Review** | Every PR dependency change vetted against GitHub Advisory DB | ✅ Active |
| **PR Validation** | Enforces Conventional Commits titles, branch naming, security checklist | ✅ Active |
| **Audit Logging** | All repository events logged to `audit-logs/` | ✅ Active |
| **Access Monitoring** | Collaborator/deploy-key changes trigger immediate security alerts | ✅ Active |
| **Dependabot** | Weekly automated dependency update PRs | ✅ Active |
| **Repository Lockdown** | Emergency manual workflow to freeze the repository | ✅ Active |

---

## Repository Lockdown

In the event of a suspected breach:

1. Go to **Actions → Repository Lockdown → Run workflow**
2. Enter the reason for the lockdown
3. An incident issue will be created, the event will be logged, and an alert will be
   sent via webhook (if `LOCKDOWN_WEBHOOK_URL` is configured)

---

## Workflows

| Workflow | Trigger | Purpose |
|----------|---------|---------|
| `security-scan.yml` | Push, PR, weekly | CodeQL static analysis |
| `secret-scan.yml` | Push, PR, daily | Gitleaks + TruffleHog secret detection |
| `dependency-review.yml` | PR | Vulnerability check for dependency changes |
| `pr-validation.yml` | PR | Title format, branch name, security checklist |
| `audit-log.yml` | All events | Structured event log in `audit-logs/` |
| `access-monitor.yml` | Member/deploy-key/branch-protection events | Alert on access changes |
| `lockdown.yml` | Manual (`workflow_dispatch`) | Emergency repository lockdown |

---

## Contributing

1. Create a branch following the naming convention: `<type>/<slug>`
   - Example: `feature/add-encryption`, `fix/auth-bypass`
2. Open a PR with the security checklist fully completed
3. All status checks must pass before merging
4. At least one CODEOWNER approval is required

See [SECURITY.md](SECURITY.md) for the full security policy and vulnerability
reporting process.
