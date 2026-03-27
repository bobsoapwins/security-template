# Security Policy

## Purpose

This repository is designed to be secure. All access, changes, and
contributions are continuously monitored.

## Supported Versions

| Version | Supported |
|---------|-----------|
| `main`  | ✅ Yes    |
| All others | ❌ No |

Only the `main` branch is maintained and monitored.

## Reporting a Security Issue

If you discover a security vulnerability in this repository, you have 2 options. You can either create a public Github issue in this repository, or you can submit a confidential vulnerability report.

<details><summary>Submitting a public issue</summary>
<p>

**Only use this option if you are fully certain that there is a viable fix for the issue. This is to prevent people from taking advantage of vulnerabilitues**

1. Go to [https://github.com/bobsoapwins/security-template/issues](https://github.com/bobsoapwins/security-template/issues)
2. Fill out the provided fields with the best knowledge you have of the incident

</p>
</details> 

<details><summary>Submitting a confidential advisory</summary>
<p>

1. Go to [https://github.com/bobsoapwins/security-test-repository/security/advisories/new](https://github.com/bobsoapwins/security-test-repository/security/advisories/new)
2. Fill out the provided fields with the best knowledge you have of the incident (for more info see [ADVISORIES.md](https://github.com/bobsoapwins/security-template/blob/main/docs/ADVISORIES.md))
3. You will receive a response within **48 hours**.

</p>
</details> 


## Security Controls in Place

| Control | Description |
|---------|-------------|
| **Branch Protection Rulesets** | Direct pushes to `main` are blocked; PRs require ≥ 1 approved review from a CODEOWNER, all status checks must pass, and conversation threads must be resolved before merging. |
| **CODEOWNERS** | Every file is owned by `@bobsoapwins`; no merge can bypass owner review. |
| **CodeQL Scanning** | Static analysis runs on every push and PR to detect security vulnerabilities. |
| **Secret Scanning** | Automated scanning on every push to detect accidentally committed credentials or keys. |
| **Dependency Review** | All dependency changes in PRs are evaluated against the GitHub Advisory Database. |
| **Audit Logging** | All repository events (pushes, PR merges, collaborator changes, etc.) are logged automatically. |
| **Repository Lockdown** | An emergency workflow can be triggered manually to immediately freeze the repository. |
| **Access Monitoring** | Any change to collaborators or deploy keys triggers an immediate alert. |
| **Dependabot** | Automated pull requests keep dependencies up-to-date and vulnerability-free. |
| **PR Security Checklist** | Every PR must include a completed security checklist before it can be merged. |

## Repository Lockdown

In the event of a suspected breach or unauthorized access:

1. Navigate to **Actions → Repository Lockdown** in the GitHub UI.
2. Click **Run workflow** and confirm the action.
3. The workflow will immediately:
   - Create an incident report issue
   - Post an alert listing the triggering user and timestamp
   - Optionally notify administrators via a webhook (configure `LOCKDOWN_WEBHOOK_URL`
     in repository secrets)

## Secret Handling

- **Never** commit secrets, credentials, API keys, or tokens.
- Use [GitHub Encrypted Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
  for all sensitive values required by workflows.
- All secrets referenced in this repository are documented in `docs/RULESET_SETUP.md`.

## Compliance

All contributors must read and agree to this policy before making any contribution.
