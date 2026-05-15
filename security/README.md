# Codex Mobile Security Audit Subagents

## Overview

This folder defines Codex TOML subagents for Android, iOS, and mixed mobile security audits.

The pack keeps the 10-layer audit model from the Claude Code source material and converts it into the same TOML shape used by this repository's other Codex agents.

## Files

| File | Purpose |
| --- | --- |
| `security_orchestrator.toml` | Coordinates reconnaissance, layer execution, finding aggregation, severity sorting, and report generation. |
| `security_l1_storage.toml` | Data-at-rest security: local storage, databases, key stores, external storage, sensitive resource strings. |
| `security_l2_network.toml` | Transport security: cleartext traffic, ATS, pinning, trust managers, HTTP URLs, mixed-content WebViews. |
| `security_l3_auth.toml` | Authentication and sessions: token handling, JWT validation, biometrics, OAuth flows, logout invalidation. |
| `security_l4_secrets.toml` | Secrets and build security: hardcoded keys, config exposure, CI secrets, gitignore gaps. |
| `security_l5_code.toml` | Code and binary protection: obfuscation, release flags, broad keep rules, debug backdoors, stack traces. |
| `security_l6_runtime.toml` | Runtime and device integrity: root/jailbreak checks, attestation, screenshots, anti-debugging, anti-tamper. |
| `security_l7_ipc.toml` | Component and IPC security: exported components, deep links, pending intents, schemes, entitlements, providers. |
| `security_l8_deps.toml` | Dependency and supply chain security: CVEs, pinning, abandoned libraries, lock files, repository trust. |
| `security_l9_input.toml` | Input validation: SQL injection, WebView exposure, path traversal, parser abuse, unsafe data flows. |
| `security_l10_logging.toml` | Logging and error handling: PII logs, crash-report leakage, BODY logging, verbose errors, analytics leakage. |

## Codex Usage

Recommended prompt:

```text
Use the security_orchestrator subagent.
Audit this Android/iOS project for mobile security vulnerabilities and write SECURITY_AUDIT_REPORT.md.
```

For a focused pass:

```text
Use the security_l4_secrets subagent.
Audit only secrets and build-security risk in this mobile project.
```

## Codex Operating Model

- Prefer `rg` for text search and `find` for file discovery.
- Inspect actual files before reporting. Do not infer vulnerabilities from naming alone.
- Preserve unrelated user changes when writing reports or remediation files.
- Use `spawn_agent` only when the user explicitly authorizes subagents or parallel agent work. Without that authorization, run the layer checks locally in sequence.
- Use web verification for dependency CVEs, advisories, and latest versions because that information changes over time.
- Recommended reasoning: high for all audit agents.

## Supported Platforms

- Android
- iOS
- Mixed Android/iOS repositories

## Final Report Interface

The `security_orchestrator` agent writes `SECURITY_AUDIT_REPORT.md` in the audited project root.

Required report sections:

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

Each finding must use this normalized block:

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

## Layer Summary

- L1 Storage: OWASP MASVS-STORAGE, CWE-312
- L2 Network: OWASP MASVS-NETWORK, CWE-295
- L3 Auth: OWASP MASVS-AUTH, CWE-287
- L4 Secrets: OWASP MASVS-STORAGE, CWE-798
- L5 Code: OWASP MASVS-RESILIENCE, CWE-656
- L6 Runtime: OWASP MASVS-RESILIENCE, CWE-919
- L7 IPC: OWASP MASVS-PLATFORM, CWE-926
- L8 Dependencies: OWASP MASVS-SUPPLY, CVE advisories
- L9 Input: OWASP MASVS-PLATFORM, CWE-89
- L10 Logging: OWASP MASVS-STORAGE, CWE-532

## Limitations

- This is a source and configuration review workflow.
- It does not replace runtime instrumentation, traffic interception, fuzzing, reverse engineering, device-side testing, or a full SBOM review.
- Findings must be based on repository evidence. When evidence is missing, report residual uncertainty instead of claiming security.
