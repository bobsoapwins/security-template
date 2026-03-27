# Contributing to security-template

Thank you for your interest in contributing to bobsoapwins/security-template — a repository created to test GitHub security measures. Your contributions help make this repo useful, safe, and maintainable. This document explains how to report problems, propose changes, and the expectations for contributions.

- If you find a security vulnerability, follow the steps listed in [ADVISORIES.md](https://github.com/bobsoapwins/security-template/blob/main/docs/ADVISORIES.md)

## Table of contents
- Reporting issues (non-security)
- Reporting security issues (private)
- How to contribute code
- Branches and commit messages
- Pull request process & checklist
- Tests, CI and quality checks
- Coding style
- Code of conduct & licensing
- Contact & acknowledgements

## Feature requests

Open an issue and select the "Feature Request"

## How to contribute code
1. Fork the repository.
2. Create a descriptive branch from main (or the repository's default branch) named like:
   - feature/<short-description>
   - fix/<short-description>
   - chore/<short-description>
3. Run tests and linters locally before committing.
4. Write clear, atomic commits with descriptive messages.
5. Open a Pull Request (PR) from your branch to the repository's default branch.

If your change fixes an open issue, reference it in the PR using "Fixes #<issue-number>" so it gets automatically closed when merged.

## Branches and commit messages
- Branches: keep branches focused and short-lived. Rebase or merge the latest main branch as needed.
- Commit messages: use present-tense, short subject line and an optional body. Example:
  - feat: add validation for user input
  - fix: prevent crash when config is missing

Consider adopting a conventional commits style if your project prefers it.

## Pull request process & checklist
When opening a PR, include:
- A clear description of what the change does and why.
- Link to any related issues.
- Notes about migrations, breaking changes, or backward compatibility.
- Screenshots or logs if relevant.

PR checklist (please complete before requesting review):
- [ ] I have read the CONTRIBUTING.md
- [ ] I have added or updated tests where appropriate
- [ ] I ran all tests locally and they pass
- [ ] I updated documentation if necessary
- [ ] My PR is limited to a single concern / logical change
- [ ] No sensitive data is included in the PR (secrets, private keys, credentials)
- [ ] I followed the repository’s coding style and lint rules

Reviewers will provide feedback and may ask for changes. Maintain open communication and address review comments promptly.

## Tests, CI and quality checks
- This repository uses automated CI checks (configure as appropriate).
- Ensure your changes pass CI before requesting a merge.
- Add unit and integration tests for bug fixes or new features as appropriate.

## Coding style
- Be consistent with existing code.
- Document public functions, modules, and behavior.
- Keep changes minimal and focused for easier review.
- Avoid introducing third-party dependencies without discussion.

## Sensitive data & secrets
- Never commit secrets, credentials, API keys, private keys, or personal data.
- If you accidentally commit a secret, rotate the secret and notify the maintainers immediately. Use a private report channel for details.
- Use environment variables and secrets management for CI.

## Code of conduct
By contributing you agree to follow the project's Code of Conduct. If a CODE_OF_CONDUCT.md exists, follow that. If not, maintain respectful, inclusive, and constructive communication in issues and PRs.

## License & contribution licensing
By submitting a pull request you agree that your contributions will be licensed under the project's license. Confirm the repository license in LICENSE or ask maintainers if unclear.

## Attribution & thanks
We appreciate all contributions—big and small. If you want to be acknowledged, indicate that in your PR and we will credit you in the changelog or contributors list.

## Getting help
If you need help or have questions about contributing, open an issue (non-security) or contact a maintainer.
