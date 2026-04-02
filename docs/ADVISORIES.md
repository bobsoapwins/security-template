# Security Advisory Reporting

## Purpose

This document explains how to report security vulnerabilities discovered in this project, what information to include, and how we handle reports. It is intended for security researchers, contributors, and anyone who needs to disclose a vulnerability.

## Quick overview

- The preferred, public intake channel in this repository is the GitHub Security Advisories channel.
- Alternative (secure) intake: encrypted email to a dedicated security address (see "Secure contact" below).
- What we expect from reporters: clear, reproducible steps, affected versions, impact assessment, and any PoC or exploit code (if safe to share).
- What we provide in return: acknowledgement within the stated SLA, private triage and patch coordination, and a public advisory once coordinated disclosure is complete.

---

## Accessing GitHub Security Advisories

1. Go to the repository page on GitHub.
2. Click the `Security` tab.
3. Click `Report a vulnerability` (or `Security advisories` → `Create draft security advisory` if you are a repository maintainer).
4. Fill the provided fields on the advisory form. Guidance on the fields is below.

If you prefer not to use GitHub, see "Secure contact" below for an alternative encrypted reporting method.

---

## Filling in the Advisory Details

Below is guidance for each field on the GitHub "Advisory details" page or equivalent reporting form.

### Title
Write a short, descriptive summary of the issue (e.g., "Improper validation in widgets leads to remote code execution").

### Common Vulnerabilities and Exposures (CVE) Identifier
- Select `Not Applicable` if you do not have a CVE.
- Select `I have an existing CVE ID` if you already have a CVE and enter it (e.g., CVE-2026-12345).
- If you do not have a CVE but the issue warrants one, maintainers can help obtain a CVE as part of triage/coordinated disclosure.

When in doubt, leave CVE blank and note it in your submission; maintainers will advise.

### Description
Provide a clear description of the issue:
- What is the bug/vulnerability?
- Where in the codebase does it occur (files, functions, modules)?
- What is the root cause (if known)?
- Any example exploit or proof-of-concept (PoC) code (preferably minimal and non-destructive).
- Steps to reproduce (exact commands, inputs, environment).

Remove any template prompts and replace with your text.

### Affected Products
Provide the following if known. If unknown, state "Unknown":
- Ecosystem (e.g., npm, PyPI, GitHub repository)
- Package or component name (e.g., mylib, module/submodule)
- Affected versions (examples: `< 1.2.3`, `<= 2.0.0`, `1.3.0 - 1.3.7`)
- Patched versions (if you know which versions fix the issue)

Guidance on version notation:
- Use comparisons (`<`, `<=`, `>`, `>=`) when applicable.
- Use ranges to indicate first and last affected versions.
- When a fix is available, specify the minimum patched version (e.g., `>= 1.2.4`).

### Severity
- If you can, provide a CVSS v4 vector and score. Use an official calculator to compute CVSS: [[https://www.first.org/cvss/calculator/3.1](https://www.first.org/cvss/calculator/4.0#)](https://www.first.org/cvss/calculator/4.0#)
- Otherwise, pick Low / Medium / High / Critical and explain the reasoning (impact, complexity, exploitability, scope).

### Weaknesses
If you know an associated CWE identifier, include it (e.g., CWE-79 for XSS). Official CWE list: https://cwe.mitre.org/

### Credits
List names, emails, and handles for any contributors or researchers who should be credited in a public advisory.

### Create Draft
When finished, click `Create Draft Security Advisory`. This will create a private advisory visible only to repository maintainers.

---

## Sample filled advisory (short example)

Title: Improper input validation in `src/parser.go` allows remote command execution

Description:
- Improper sanitization of the `--exec` argument in `cmd/handler.go` leads to shell injection when called with crafted payloads.
- Root cause: using `fmt.Sprintf` to build command lines without escaping user input.
- Reproduction: `./myapp --exec "some; rm -rf /tmp/test"` on Linux, with sample commands:
  1. Build from commit 0ef14b0...
  2. Run `./myapp --exec 'touch /tmp/evil'`
  3. Observe `/tmp/evil` created.
- PoC: minimal script included in attachment.

Affected products:
- Ecosystem: GitHub repository
- Package: bobsoapwins/security-template
- Affected versions: `< 1.2.0`
- Patched versions: `>= 1.2.0`

Severity:
- CVSS v4: 8.8 (AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)

Credits:
- Security researcher: Alice Example (alice@example.com)

---

## Disclosure policy (subject to change depending on repo environment)

- Acknowledgement: We will acknowledge receipt of your report within 72 hours (business days). If we cannot meet this timeframe we will notify you and provide an estimated date.
- Triage: Initial triage will be completed within 14 calendar days. Triage includes reproduction, initial severity assessment, and whether the issue is within scope.
- Coordinated disclosure: We prefer coordinated disclosure. We will work with you to establish a disclosure timeline that allows us to prepare and release a patch and mitigations before public disclosure. Typical coordinated disclosure window is 90 days from the initial report, but this may vary by severity, availability of fixes, and the reporter’s preferences.
- Public advisory: Once a fix and release are available (or disclosure deadline has passed by mutual agreement), maintainers will publish a public advisory in this repository and/or a GitHub Security Advisory entry and will link to any CVE granted.
- Exceptions: If the vulnerability is being actively exploited in the wild and immediate public disclosure is required to protect users, we may shorten the timeline; we will attempt to notify the reporter before doing so.

---

## Secure contact (alternative to GitHub Security Advisories)

Primary intake is GitHub Security Advisories (recommended).

If you cannot use GitHub, contact us securely via encrypted email at [northdunne@gmail.com](mailto:northdunne@gmail.com)

If you prefer a different secure channel (e.g., a third-party vulnerability disclosure platform or Keybase), contact a project maintainer via GitHub to arrange.

---

## Maintainer workflow and responsibilities

- Steps when a report is received:
  1. Acknowledge the reporter within SLA.
  2. Assign a triage owner and create an internal tracking issue (private) if needed.
  3. Reproduce the report, confirm severity, and determine scope.
  4. Develop and test a fix; prepare a release plan and backport strategy for supported versions.
  5. Coordinate with the reporter on disclosure timeline and advisory text.
  6. When ready, publish a public advisory and release patched versions.
- Labels and tracking:
  - Use labels such as: `security`, `security:triage`, `security:fix-in-progress`, `security:patched`, `security:coordinated-disclosure`.
  - Tag the release (e.g., `v1.2.0`) that contains the fix and include the advisory reference in the changelog.
- Backports:
  - Backport policy: we will backport fixes to supported release branches where feasible and secure.
  - If no backport is possible, we will document mitigations and recommended action for affected users.

---

## Guidance on CVE and CVSS

- CVSS guidance: Use CVSS v3.1 to produce a numerical severity score; official calculator: https://www.first.org/cvss/calculator/3.1
- When to request or include a CVE:
  - If the issue affects a widely used library or has cross-project impact, a CVE is appropriate.
  - If unsure, submit the advisory without a CVE and the maintainers will help escalate for CVE assignment if needed.
- Useful resources:
  - GitHub Security Advisories overview: https://docs.github.com/en/code-security/security-advisories/about-security-advisories
  - CVE program: https://cve.mitre.org/
  - CVSS calculator: https://www.first.org/cvss/calculator/3.1
  - CWE list: https://cwe.mitre.org/

---

## Privacy and confidentiality

- By default, all reports submitted via GitHub Security Advisories or our secure contact address will remain private to repository maintainers and GitHub security staff until we coordinate disclosure.
- We will only publish information necessary to explain the vulnerability, its impact, and how to remediate — sensitive details that would materially facilitate exploitation will be withheld until a fix is available, unless required by law or if the reporter requests otherwise.
- If you request anonymity or request we redact specific details in the public advisory, include that request in your report. We will honor such requests when feasible, but in some cases full technical disclosure may be required to protect users.

---

## References and further reading

- GitHub Security Advisories: https://docs.github.com/en/code-security/security-advisories/about-security-advisories
- CVE Program: https://cve.mitre.org/
- CVSS v3.1 specification and calculator: https://www.first.org/cvss/
- CWE — Common Weakness Enumeration: https://cwe.mitre.org/
- Guidance on responsible disclosure: https://www.first.org/ (FIRST resources)

---

## After you report

- You will receive an acknowledgement (within SLA).
- We will keep you informed during triage and patching.
- After coordinated disclosure, we will publish an advisory and credit you based on your stated preferences.

---

## Contact and maintainers

- Placeholder security contact: security@REPLACE_WITH_YOUR_DOMAIN (replace with the project security email).
- For administrative or non-sensitive questions, open an issue on the repository or contact the maintainers via GitHub.

---

Thank you for helping keep this project secure. We appreciate responsible disclosure and will work to resolve reports promptly and transparently.
