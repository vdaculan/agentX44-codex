# Mobile Security Audit Agents

`security/` contains Codex subagent definitions for Android, iOS, and mixed mobile security audits.

The pack uses one orchestrator plus ten focused audit layers. The orchestrator coordinates reconnaissance, layer execution, finding aggregation, severity sorting, and report generation. The layer agents focus on one security domain each.

Each audit agent is a standalone `.toml` file. Use the agent by referencing its `name` in your prompt.

## Agents

| Agent | File | Purpose | Sandbox |
| --- | --- | --- | --- |
| `security_orchestrator` | `security_orchestrator.toml` | Coordinates a full Android/iOS source-and-configuration audit and writes `SECURITY_AUDIT_REPORT.md`. | `workspace-write` |
| `security_l1_storage` | `security_l1_storage.toml` | Audits local storage, databases, keystores, external storage, sensitive resources, and data-at-rest risk. | `read-only` |
| `security_l2_network` | `security_l2_network.toml` | Audits transport security, cleartext traffic, ATS, pinning, trust managers, HTTP URLs, and WebView mixed content. | `read-only` |
| `security_l3_auth` | `security_l3_auth.toml` | Audits authentication, session handling, token storage, JWT handling, biometrics, OAuth, and logout invalidation. | `read-only` |
| `security_l4_secrets` | `security_l4_secrets.toml` | Audits hardcoded keys, sensitive config, CI secrets, gitignore gaps, keystores, and local secret files. | `read-only` |
| `security_l5_code` | `security_l5_code.toml` | Audits obfuscation, release flags, broad keep rules, debug backdoors, release hardening, and stack trace exposure. | `read-only` |
| `security_l6_runtime` | `security_l6_runtime.toml` | Audits root/jailbreak checks, attestation, screenshot controls, anti-debugging, anti-tamper, and device integrity. | `read-only` |
| `security_l7_ipc` | `security_l7_ipc.toml` | Audits exported components, deep links, pending intents, URL schemes, entitlements, providers, and IPC exposure. | `read-only` |
| `security_l8_deps` | `security_l8_deps.toml` | Audits dependency and supply-chain risk, CVEs, version pinning, lock files, abandoned libraries, and repository trust. | `read-only` |
| `security_l9_input` | `security_l9_input.toml` | Audits input validation, SQL injection, WebViews, path traversal, parser abuse, and unsafe data flows. | `read-only` |
| `security_l10_logging` | `security_l10_logging.toml` | Audits PII logs, crash-report leakage, BODY logging, verbose errors, analytics leakage, and sensitive error handling. | `read-only` |

## Full Audit Usage

Use `security_orchestrator` when you want a complete audit across all layers.

```text
Use the security_orchestrator subagent.
Audit this Android/iOS project for mobile security vulnerabilities and write SECURITY_AUDIT_REPORT.md.
```

Expected behavior:

- inventory the project files
- detect Android, iOS, or mixed platform scope
- inspect relevant build, manifest, source, config, and dependency files
- coordinate L1-L10 audit coverage
- aggregate duplicate findings
- sort findings by severity
- write `SECURITY_AUDIT_REPORT.md` in the audited project root

The orchestrator is allowed to write the final report. It should not fix code during the audit unless a separate remediation task explicitly asks for fixes.

## Focused Audit Usage

Use a layer agent when only one security area needs review.

Secrets audit:

```text
Use the security_l4_secrets subagent.
Audit only secrets and build-security risk in this mobile project.
Redact secret values in any evidence.
```

Auth audit:

```text
Use the security_l3_auth subagent.
Audit authentication, session handling, token storage, biometrics, OAuth flows, and logout invalidation.
```

Dependency audit:

```text
Use the security_l8_deps subagent.
Audit mobile dependency and supply-chain risk.
Check Gradle, CocoaPods, Swift Package Manager, lock files, package repositories, CVEs, advisories, and version pinning.
```

Network audit:

```text
Use the security_l2_network subagent.
Audit transport security, cleartext traffic, certificate handling, pinning, trust managers, ATS, HTTP URLs, and mixed-content WebViews.
```

## Expected Report

The full audit report is written to:

```text
SECURITY_AUDIT_REPORT.md
```

Required report structure:

```text
# Mobile Security Audit Report
## Summary Table (severity x count)
## CRITICAL Findings
## HIGH Findings
## MEDIUM Findings
## LOW Findings
## INFO Findings
## Remediation Handoff
```

Each finding should use this shape:

```text
### [FINDING-###]
- Layer     : [L1-L10 name]
- Platform  : Android | iOS | Both
- Severity  : CRITICAL | HIGH | MEDIUM | LOW | INFO
- File      : <exact path>
- Line(s)   : <numbers>
- Issue     : <one sentence>
- Risk      : <attacker impact>
- Evidence  : <exact code snippet>
- Fix       : <complete corrected code or config>
- Reference : <OWASP MASVS, CWE, or CVE ID>
```

Focused layer agents return their own layer-specific finding IDs, such as `FINDING-L4-001`. The orchestrator normalizes and renumbers final report findings globally as `FINDING-001`, `FINDING-002`, and so on.

## Operating Rules

- Inspect actual files before reporting.
- Prefer `rg` for text search and `find` for file discovery.
- Every finding must include exact file paths and line numbers when tied to code or configuration.
- Findings must be evidence-backed; do not report vulnerabilities from naming alone.
- Use web verification for dependency CVEs, advisories, package freshness, and latest stable versions because that information changes over time.
- Redact actual secrets in findings.
- Do not fix code during an audit unless the user explicitly asks for remediation.
- Preserve unrelated user changes when writing reports or remediation files.
- State residual uncertainty when evidence is incomplete.
- Do not claim the app is secure; report observed risks and what was not verified.

## Layer Map

| Layer | Agent | Main Standard References |
| --- | --- | --- |
| L1 Storage | `security_l1_storage` | OWASP MASVS-STORAGE, CWE-312 |
| L2 Network | `security_l2_network` | OWASP MASVS-NETWORK, CWE-295 |
| L3 Auth | `security_l3_auth` | OWASP MASVS-AUTH, CWE-287 |
| L4 Secrets | `security_l4_secrets` | OWASP MASVS-STORAGE, CWE-798 |
| L5 Code | `security_l5_code` | OWASP MASVS-RESILIENCE, CWE-656 |
| L6 Runtime | `security_l6_runtime` | OWASP MASVS-RESILIENCE, CWE-919 |
| L7 IPC | `security_l7_ipc` | OWASP MASVS-PLATFORM, CWE-926 |
| L8 Dependencies | `security_l8_deps` | OWASP MASVS-SUPPLY, CVE advisories |
| L9 Input | `security_l9_input` | OWASP MASVS-PLATFORM, CWE-89 |
| L10 Logging | `security_l10_logging` | OWASP MASVS-STORAGE, CWE-532 |

## Limitations

This is a source-and-configuration review workflow. It does not replace:

- runtime instrumentation
- traffic interception
- fuzzing
- reverse engineering
- device-side testing
- full SBOM review
- penetration testing

When evidence is missing, report the uncertainty instead of treating absence of evidence as proof of safety.
