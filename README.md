# REDxGRIDxAEV

**REDxGRIDxAEV** is a collection of authorized adversary emulation and validation campaigns conducted in controlled lab environments.

Each campaign documents a scoped adversary emulation workflow, mapped defensive telemetry, curated evidence, and sanitization controls. The repository is intended for defensive validation, detection engineering, security education, and portfolio demonstration.

## CAMPAIGNS

| Campaign | Focus | Tooling | Status |
|---|---|---|---|
| [RG-AE-001](campaigns/rg-ae-001/) | Authorized Windows Post-Compromise Micro Emulations | MITRE CALDERA, PowerShell Script Block Logging, Sysmon, Wazuh | Complete |

## STRUCTURE

```text
campaigns/   Self-contained campaign packages, evidence, and documentation
docs/        Shared methodology and publication standards
```

## RESPONSIBLE USE

All activities documented here were performed only in authorized, controlled lab environments. This repository excludes raw endpoint logs, private configuration, credentials, enrollment keys, production identifiers, and other sensitive material.

See [SECURITY.md](SECURITY.md) for responsible-use and evidence-handling expectations.