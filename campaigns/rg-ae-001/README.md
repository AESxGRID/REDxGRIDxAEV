# REDxGRIDxAEV by AESxGRID

## RG-AE-001: CUSTOM MITRE CALDERA ADVERSARY MICRO EMULATION AND SOC (SECURITY OPERATION CENTER) TELEMETRY VALIDATION

> Custom MITRE CALDERA Adversary Emulation and Wazuh Telemetry Validation in an Isolated Security Validation Range.

## PROJECT SUMMARY

RG-AE-001 is a controlled Adversary Emulation Campaign, built in my REDxGRIDxAEV isolated Lab Environment. 

## WHY?

The project demonstrates how a Red Team Operator can reproduce realistic post-compromise behavior and provide useful evidence to SOC Analysts for investigations.

## USING WHAT EXACTLY?

MITRE CALDERA, hosted on an AthenaOS VM, was used to create custom PowerShell abilities for a Windows 11 Pro target VM (Virtual Machine) and then launch the campaign. 
The SOC VM consists of a Rocky Linux Base and eXtended Threat Management (XTM) stack (OpenAEV, OpenCTI) and Wazuh. 

REDxGRIDxAEV is independently designed and operated end to end. That means that I own the architecture, virtual infrastructure, network segmentation, endpoint configuration, security tooling, telemetry collection, evidence handling, campaign scope, and rollback process. The environment provides a controlled place to test operator behavior and validate defensive visibility without relying on production systems or third-party infrastructure.

## WHAT HAPPENED?

The emulation followed a focused operator workflow that included command execution, host and account discovery, network configuration discovery, transfer of test data, discovery of synthetic lab data, and local collection.

The project was designed to validate more than execution. Windows Event Logging, PowerShell Script Block Logging, Sysmon, and Wazuh were used to determine what activity was recorded on the endpoint and what reached centralized monitoring. The work also documents implementation issues, telemetry gaps, configuration changes, and the results of retesting.

## CURRENT STATUS

**Execution and Initial Telemetry Validation Completed.** The initial campaign and telemetry validation work has been performed in an our isolated lab environment. This repository is being assembled from reviewed, sanitized evidence and will be updated as final documentation and validation artifacts are added.

## WHAT WAS IN SCOPE?

RG-AE-001 was performed only inside the REDxGRIDxAEV isolated Lab Environment. AthenaOS served as the operator platform, selected as an alternative to Kali or Parrot to expand familiarity with operator distribution choices. An authorized Windows 11 Pro VM was the target, and the Rocky Linux SOC VM provided centralized telemetry review.

The campaign assumed a controlled CALDERA foothold on the Windows target and focused on safe, observable post-compromise behavior. Testing covered PowerShell execution, host and account discovery, network configuration discovery, transfer of a harmless staged artifact, discovery of synthetic lab files, and controlled local collection.

All test files were created for this campaign and contained no personal, production, credential, or sensitive information. The campaign excluded initial access, phishing, exploit development, credential theft, privilege escalation, persistence, lateral movement, destructive actions, ransomware simulation, real data access, and data exfiltration.

## THE OPERATOR STORY

The campaign starts from assumed access. Instead of trying to simulate every possible way an attacker could get initial access, I treated the CALDERA agent as an existing foothold and focused on what an operator would do next.

The first priority was getting oriented. I used PowerShell to confirm command execution, identify the host and user context, and check the system's network configuration. Once that basic picture was in place, I transferred a harmless staged file from the operator side, searched only the campaign-created synthetic data directory, and copied one designated document into a controlled local collection location.

I wanted a short, practical chain that made operational sense and produced clear telemetry across execution, discovery, transfer, and collection. What is illuminated performing these steps from start to finish, is key in understanding how these events play out in reality.

## ATT&CK TECHNIQUES TESTED

The following ATT&CK techniques were selected to support the operator workflow described above.

|  ATT&CK&nbsp;ID   | Technique | What I Tested | Why it Mattered |
|-------|------|------|------|
| T1059.001 | PowerShell | Controlled PowerShell execution through a custom CALDERA ability | Tested the available command path and created a clear starting point for endpoint telemetry validation |
| T1082 | System Information Discovery | Basic Windows host and operating system discovery | Helped establish the target's system context before further activity |
| T1087 | Account Discovery | Current user and local account discovery | Showed what account context was available and what local identities existed on the target |
| T1016 | System Network Configuration Discovery | Local adapter, address, gateway, and DNS configuration discovery | Helped identify how the target was connected inside the lab network |
| T1105 | Ingress Tool Transfer | Transfer of a harmless text artifact from AthenaOS to the Windows target | Tested controlled network transfer and file-write telemetry without using an executable payload |
| T1083 | File and Directory Discovery | Discovery limited to the campaign created synthetic data directory | Simulated a search for local data while keeping the activity scoped to lab created files |
| T1005 | Data from Local System | Local copy of one designated synthetic document to a controlled collection directory | Simulated local collection without accessing real data or sending files outside the target |

## REDxGRIDxAEV LAB MAP

RG-AE-001 was executed inside an isolated VMware Workstation environment. The campaign separated the operator platform, Windows target, and SOC telemetry stack so that operator activity and defensive evidence could be reviewed from different systems.

| System | Role in RG-AE-001 | What it handled |
|---|---|---|
| AthenaOS (`REDX-VM-RTOW-01`) | Operator platform | Hosted MITRE CALDERA, stored the custom adversary profile and abilities, and served the benign staged files used during the campaign |
| Windows 11 Pro (`REDX-VM-WKS-01`) | Authorized target | Ran the CALDERA agent, executed the campaign procedures, generated Windows Event Logs and Sysmon telemetry, and contained the synthetic campaign dataset |
| Rocky Linux SOC VM (`REDX-VM-SOC-01`) | Security monitoring platform | Hosted Wazuh for centralized log collection and review, alongside the broader XTM stack used in the REDxGRIDxAEV environment |
| VMware Workstation | Lab virtualization and isolation layer | Hosted the virtual systems and kept campaign activity inside the authorized Lab Environment |

### CAMPAIGN AND EVIDENCE FLOW

```text
AthenaOS
  |-- MITRE CALDERA OPERATION
  `-- Benign staged-file source
        |
        v
Windows 11 Pro Target
  |-- CALDERA Agent executes custom PowerShell abilities
  |-- Windows Event Logging and Sysmon record activity
  `-- Wazuh Agent forwards selected telemetry
        |
        v
Rocky Linux SOC VM
  `-- Wazuh centralizes and presents telemetry for investigation
```

This layout allowed the campaign to be executed from one system while telemetry was reviewed from a separate SOC platform. It also made it possible to compare the planned CALDERA activity with the evidence generated by the Windows target and received by Wazuh.

## BUILDING THE CALDERA CAMPAIGN

I built RG-AE-001 as a custom CALDERA adversary profile rather than relying on a prebuilt campaign. Each campaign step was created as a Windows PowerShell ability and mapped to the ATT&CK technique it was intended to test.

The abilities were grouped into one ordered profile that followed the operator workflow described above. The operation was scoped to the single authorized Windows 11 lab agent and used the atomic planner, plain-text execution, manual approval, and a paused start state. These settings made it possible to review each step before execution and capture evidence in a controlled order.

The campaign was developed through an iterative validation process. Each ability was reviewed and tested to make sure the procedure matched the intended technique, executed safely on the target, and produced usable evidence for later telemetry review.

The final profile provided a reusable starting point for future REDxGRIDxAEV testing. It can be expanded with additional procedures or adapted into a threat-informed scenario once the base workflow and evidence process are fully documented.

## WHAT THE DEFENDERS SAW

The campaign was evaluated from the defender's perspective as well as the operator's perspective. Each procedure was expected to create evidence locally on the Windows target and send relevant telemetry to Wazuh for centralized review.

Windows Event Logging, PowerShell Script Block Logging, and Sysmon were used to record activity on the target. The primary validation points included PowerShell Event ID 4104 for script-block content, Windows Security Event ID 4688 for process creation, and Sysmon Event IDs 1, 3, and 11 for process creation, network connections, and file creation.

Wazuh provided centralized visibility into the Windows target's telemetry. PowerShell Event ID 4104 was confirmed in Wazuh with script-block content preserved, allowing the executed PowerShell activity to be reviewed alongside the target identity, event time, and source event channel.

This validation created a repeatable way to compare CALDERA activity with endpoint and centralized telemetry. It also confirmed that the campaign could be reviewed from both sides: what the operator executed and what the SOC could observe.

## RESULTS, RED TEAM GAPS, AND NEXT STEPS

RG-AE-001 established a working baseline for CALDERA driven adversary emulation and SOC telemetry validation in the REDxGRIDxAEV Lab.

RG-AE-001 was intentionally limited to a post-compromise Windows workflow on one authorized target. The campaign began from assumed access and did not test initial access, privilege escalation, credential access, persistence, lateral movement, domain reconnaissance, command-and-control resilience, or external data transfer.

The next phase will build on this baseline by introducing a second authorized target and a defined operator objective that requires additional discovery, identity-aware decision making, and controlled remote execution. Future iterations can also test how defensive visibility changes when the same core behavior is performed through different procedures or execution paths.

### SOC VALIDATION FOLLOW-UP

The exercise also established that PowerShell Script Block Logging could be collected from the Windows target and reviewed in Wazuh. Additional validation is still planned for centralized visibility of the remaining campaign procedures and relevant Sysmon event types.

The result is a reusable, evidence-focused workflow that can support future custom emulations and threat-informed testing in REDxGRIDxAEV.

## EVIDENCE LOCKER

The evidence locker contains reviewed and sanitized records from the RG-AE-001 Campaign. Each item is selected to show what was planned, what executed, and what defenders could observe.

| Evidence&nbsp;ID | Artifact | What it shows | Repository location |
|---|---|---|---|
| EV-01 | Sanitized CALDERA Campaign Source | Eight custom abilities, ordered adversary profile used for RG-AE-001, including ATT&CK mappings, sanitized staging placeholder | `caldera/` |
| EV-02 | CALDERA Adversary Profile | The configured RG-AE-001 profile and ordered ability chain in the CALDERA interface | `evidence/screenshots/EV-02-caldera-adversary-profile.png` |
| EV-03 | CALDERA Operation Summary | Successful execution records for the scoped Windows 11 campaign operation | `evidence/screenshots/EV-03-caldera-operation-summary.png` |
| EV-04 | Local PowerShell Event ID 4104 | Windows PowerShell Script Block Logging evidence for RG-AE-001 Step 01 / T1059.001 | `evidence/sanitized/EV-04-local-powershell-4104.xml` |
| EV-05 | Local Sysmon Event ID 1 | Sysmon process-creation evidence for RG-AE-001 Step 05, including the controlled staging workflow | `evidence/sanitized/EV-05-local-sysmon-event-1.xml` |
| EV-06 | Wazuh PowerShell Event ID 4104 | Centralized Wazuh visibility of the RG-AE-001 Step 01 script block | `evidence/screenshots/EV-06-wazuh-powershell-4104.png` |
| EV-07 | Wazuh Agent Validation | Active Windows endpoint agent status supporting the central collection path | `evidence/screenshots/EV-07-wazuh-agent-status.png` |
| EV-08 | Architecture and Evidence Flow | Roles and evidence flow across ATHENA-OS, the Windows target, and Wazuh | `diagrams/EV-08-rg-ae-001-architecture.png` |

## WHAT I LEARNED

This project taught me that good adversary emulation needs a clear goal, a limited scope, and a plan for what evidence should appear when the activity runs.

- Building custom CALDERA abilities was more than picking ATT&CK techniques. I had to think through what each command would do, how CALDERA would run it on the target, and what evidence it should leave behind.

- Starting with assumed access kept this first campaign focused. Instead of trying to simulate a full intrusion, I could concentrate on post-compromise activity and make sure the workflow worked from start to finish.

- I also learned that seeing activity on the Windows target is not the same as seeing it in Wazuh. The endpoint can create the evidence, but the collection and visibility path still has to be working for defenders to investigate it centrally.

## CLEANUP AND RESET

RG-AE-001 was conducted in an authorized lab environment. After testing, the Windows target was returned to a known baseline and no persistence mechanisms, scheduled tasks, user accounts, credentials, or tools were intentionally left behind.

- CALDERA operations were stopped after validation, and the target agent was not used for continued activity.
- Test files and other campaign artifacts were reviewed and removed from the Windows target when they were no longer needed for evidence collection.
- Evidence was copied into the project only after review and sanitization. Screenshots, logs, and exports were checked for credentials, tokens, enrollment keys, personal information, and unnecessary system details before being retained.
- The project repository contains documentation and selected sanitized evidence only. It does not contain live operational access, active Command and Control (C2) configuration, or unsafe materials intended for use outside the authorized REDxGRIDxAEV Lab.

## REPOSITORY STATUS

This campaign package was assembled from reviewed and sanitized project artifacts.