# Executive Account Takeover Investigation & Detection Engineering Using Splunk

**Author:** Muhammad Uzair  
**Role:** Aspiring SOC Analyst  
**Platform:** Splunk Enterprise  
**Project Type:** Simulated SOC Investigation / Detection Engineering  
**Environment:** Synthetic Security Data

## Project Overview

This project simulates a real-world SOC investigation involving the suspected compromise of a Cloudora executive account.

Cloudora is a fictional HR software company that handles sensitive employee and business information. The SOC receives suspicious authentication activity associated with the company's CEO account.

The investigation was performed in Splunk Enterprise using synthetic sign-in and audit logs.

The objective was to investigate the complete attack chain, determine whether the account was actually compromised, assess the scope, identify persistence mechanisms, evaluate business impact, develop detections, and document an appropriate response.

## Incident Scenario

The SOC identifies unusual authentication activity involving the CEO account.

Initial telemetry shows authentication attempts originating from Lagos, Nigeria, using an unknown Windows device, while previously observed activity is associated with London-based devices.

The investigation focuses on:

- Was the CEO account actually compromised?
- How did the attacker gain access?
- What happened after successful authentication?
- Was persistence established?
- Was mailbox activity manipulated?
- Were other accounts targeted?
- What is the potential business impact?
- How can similar activity be detected automatically?

## Investigation Environment

| Component | Details |
|---|---|
| SIEM | Splunk Enterprise |
| Operating System | Windows 11 |
| Virtualization | VirtualBox |
| Investigation Data | Synthetic security logs |
| Sign-in Events | 1,498 |
| Audit Events | 166 |
| Log Period | 2026-08-04 to 2026-08-11 |
| Primary Index | `cloudora` |
| Sign-in Sourcetype | `cloudora:signin` |
| Audit Sourcetype | `cloudora:audit` |

All company names, users, IP addresses, and security events are synthetic and intended for educational and portfolio purposes.

## Investigation Workflow

```text
Suspicious Activity
        ↓
Initial Triage
        ↓
User & IP Baseline
        ↓
Authentication Analysis
        ↓
Timeline Reconstruction
        ↓
Audit Log Correlation
        ↓
Persistence Analysis
        ↓
Scope Assessment
        ↓
Business Impact Assessment
        ↓
MITRE ATT&CK Mapping
        ↓
Containment Planning
        ↓
Detection Engineering
        ↓
Incident Reporting
```

## Key Findings

### CEO Account Compromise

Evidence supports compromise of the simulated Cloudora CEO account.

Observed chain:

```text
Password Spraying
      ↓
Successful Authentication
      ↓
Outlook Web Access
      ↓
Security Information Registration
      ↓
Azure Portal Access
      ↓
Mailbox Rule Creation
```

### Persistence / Security Control Modification

A new authenticator/security registration was observed after successful suspicious authentication:

| Field | Evidence |
|---|---|
| Operation | `RegisterSecurityInfo` |
| Source IP | `203.0.113.41` |
| Device | Pixel 6 |
| Target | Authenticator App |
| Result | Success |

### Mailbox Manipulation

A new inbox rule named `RSS subscriptions` was created and affected finance/invoice-related messages by moving them to another folder and marking them as read.

### Scope

The investigation identified **26 unique accounts targeted by failed authentication activity** associated with the suspicious IP range.

Successful suspicious authentication was observed for the CEO account and one additional account. The remaining targeted accounts had no successful authentication observed from the suspicious attacker IPs in the analyzed sign-in telemetry.

> Targeted does not automatically mean compromised.

## Business Impact

### Confirmed

- Executive account compromise
- Successful unauthorized authentication
- New security information registration
- Unauthorized mailbox rule creation
- Finance/invoice-related mailbox filtering
- Multiple additional accounts targeted

### Not Established From Available Telemetry

- Confirmed data exfiltration
- Confirmed financial transaction fraud
- Confirmed customer-data theft
- Confirmed customer account compromise

The report intentionally separates confirmed evidence from potential impact.

## MITRE ATT&CK Mapping

| Technique | Technique Name | Evidence |
|---|---|---|
| T1110.003 | Password Spraying | Repeated failed authentication attempts against multiple accounts |
| T1098.005 | Account Manipulation: Device Registration | New authenticator/security information registered |
| T1564.008 | Email Hiding Rules | Unauthorized inbox rule created to filter finance/invoice messages |

## Detection Engineering

Four reusable Splunk detections were developed:

1. Password spraying
2. Failed authentication followed by successful authentication
3. New security information registration
4. New inbox rule creation

See the `detections/` directory for the individual SPL files.

## Response & Containment

The simulated response plan included:

- Revoke active sessions
- Reset the compromised account password
- Remove the unauthorized authenticator
- Delete the unauthorized inbox rule
- Block observed suspicious IPs
- Review affected accounts
- Continue monitoring

These are documented as simulated response actions and were not performed against a real production environment.

## Evidence Documentation

Investigation steps were maintained using:

```text
Investigation Question
        ↓
SPL Query
        ↓
Observed Result
        ↓
Analyst Interpretation
        ↓
Finding / Status
```

This creates a reproducible link between investigation questions, SPL searches, evidence, and conclusions.

## Repository Structure

```text
splunk-executive-account-takeover-investigation/
│
├── README.md
├── data/
│   ├── cloudora_signin_logs.csv
│   ├── cloudora_audit_logs.csv
│   └── DATASET_README.md
├── spl/
│   ├── investigation_queries.spl
│   └── detection_queries.spl
├── detections/
│   ├── password_spraying.spl
│   ├── failed_to_success_auth.spl
│   ├── security_info_registration.spl
│   └── inbox_rule_creation.spl
├── documentation/
│   ├── incident_report.pdf
│   ├── investigation_evidence_log.docx
│   ├── attack_timeline.md
│   ├── containment_response.md
│   └── mitre_attack_mapping.md
├── dashboard/
│   └── splunk_dashboard.pdf
└── screenshots/
    └── investigation_screenshots/
```

## Limitations

This project uses controlled synthetic data.

- Cloudora is fictional.
- No real credentials were used.
- No real customer information was processed.
- No production systems were accessed.
- IP addresses are documentation/test ranges.
- Containment actions were simulated.
- Static historical logs cannot independently prove the current state of a mailbox or account.
- Findings are limited to the telemetry available in the synthetic dataset.

## Skills Demonstrated

**SOC Investigation**
- Alert triage
- Authentication analysis
- Baseline analysis
- Timeline reconstruction
- Log correlation
- Scope assessment
- Evidence documentation
- Business impact assessment

**Splunk / SPL**
- Searching security data
- Filtering and aggregation
- Statistical analysis
- Timeline reconstruction
- Detection logic
- Dashboard development

**Detection Engineering**
- Password spraying detection
- Failed-to-success authentication detection
- Security information change detection
- Mailbox-rule detection
- Detection tuning

**Incident Response**
- Containment planning
- Session revocation
- Credential reset planning
- Persistence removal
- Monitoring and verification

## Final Assessment

The investigation determined that the simulated Cloudora CEO account was compromised.

The available evidence supports:

```text
Password Spraying
        ↓
Successful CEO Authentication
        ↓
Outlook Web Access
        ↓
Security Information Registration
        ↓
Azure Portal Access
        ↓
Malicious Inbox Rule
```

The broader investigation identified 26 unique targeted accounts, while successful suspicious authentication was observed for the CEO account and one additional account.

No direct evidence of customer-data theft, confirmed exfiltration, or financial fraud was established from the available telemetry.

This project demonstrates an end-to-end SOC workflow from initial suspicious authentication through investigation, evidence correlation, scope assessment, detection engineering, response planning, and incident reporting.

## Author

**Muhammad Uzair**  
BS Software Engineering  
Aspiring SOC Analyst
