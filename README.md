# blue-team-siem-lab
Build a small SOC environment using Windows VM, Ubuntu VM, and Elastic SIEM VM. All systems run locally to ensure safety and legality. Goal: collect logs, detect attacks, and investigate alerts.
# SIEM Lab: Log Monitoring, Detection & Triage

## Overview

This project demonstrates a small Security Information and Event Management (SIEM) environment built for blue team and SOC analyst practice. It focuses on log collection, detection engineering, alert investigation, and incident response documentation.

The lab simulates real-world security monitoring using Windows and Linux endpoints connected to a centralized SIEM platform.

---

## Architecture

```text
Windows VM
  ├── Security Logs
  ├── Sysmon Logs
  └── Winlogbeat / Elastic Agent

Linux VM (Ubuntu)
  ├── /var/log/auth.log
  └── Filebeat / Elastic Agent

SIEM VM (Elastic Stack)
  ├── Elasticsearch
  ├── Kibana
  └── Detection Rules + Dashboards
```

---

## Tools Used

* VMware Workstation / VirtualBox
* Windows 11 VM
* Ubuntu Server VM
* Elastic Stack (Elasticsearch, Kibana, Elastic Agent)
* Sysmon (Windows logging enhancement)

---

## Lab Design Goals

* Centralize security logs from Windows and Linux systems
* Detect common attack patterns
* Build SOC-style dashboards
* Investigate and triage alerts
* Document incident response workflows

---

## Log Sources

### Windows

* Security Event Logs
* Sysmon Events

Key Event IDs:

* 4624 → Successful logon
* 4625 → Failed logon
* 4720 → User creation
* 4732 → Group membership changes

### Linux

* /var/log/auth.log
* SSH authentication events

---

## Attack Simulations (Controlled)

All simulations are performed in an isolated lab environment.

### 1. Brute Force Login Attempts

* Repeated failed login attempts
* Generates Event ID 4625

### 2. User Account Creation

```powershell
net user labuser Password123! /add
```

* Generates Event ID 4720

### 3. Privilege Escalation

```powershell
net localgroup administrators labuser /add
```

* Generates Event ID 4732

### 4. Suspicious PowerShell Execution

```powershell
powershell -EncodedCommand <base64>
```

### 5. Linux SSH Brute Force Simulation

```bash
ssh fakeuser@localhost
```

---

## Dashboards

### Authentication Monitoring Dashboard

* Failed logins over time
* Successful logins
* Top source IPs
* User activity trends

### Endpoint Activity Dashboard

* Process execution tracking
* PowerShell activity
* Network connections

### Administrative Actions Dashboard

* User creation events
* Group membership changes
* Privilege escalation events

---

## Detection Rules

### Brute Force Detection

Trigger when:

* > 5 failed login attempts within 5 minutes from same source

### New User Detection

Trigger on:

* Event ID 4720 (user creation)

### Privilege Escalation Detection

Trigger on:

* Additions to administrator group

### Suspicious PowerShell Detection

Trigger on:

* Encoded PowerShell commands
* Obfuscated command execution

### Linux SSH Brute Force Detection

Trigger on:

* Multiple failed SSH authentication attempts

---

## Investigation & Triage Process

When an alert triggers:

### 1. Identify the Event

* What happened?
* Which rule fired?

### 2. Gather Context

* Username
* Host machine
* Source IP
* Timestamp

### 3. Analyze Behavior

* Is this expected activity?
* Does it match known attack patterns?

### 4. Determine Severity

* Low: failed logins only
* Medium: suspicious behavior without compromise
* High: confirmed malicious activity

### 5. Recommended Response

* Block IP address
* Reset credentials
* Isolate endpoint
* Escalate to Tier 2 SOC

---

## Example Incident Report

### Alert: Suspicious PowerShell Execution

**Evidence:**

* Encoded PowerShell command detected

**Analysis:**

* Potential obfuscated script execution
* Matches known malware behavior patterns

**Impact:**

* Possible system compromise attempt

**Response:**

* Investigate parent process
* Check persistence mechanisms
* Review network connections

---

## Key Skills Demonstrated

* SIEM configuration and log ingestion
* Windows and Linux log analysis
* Detection rule creation
* Incident investigation and triage
* Security monitoring and alert validation

---

## Cost

| Component                 | Cost |
| ------------------------- | ---- |
| VMware Workstation Player | Free |
| Ubuntu Server             | Free |
| Windows Evaluation VM     | Free |
| Elastic Stack (Basic)     | Free |
| Sysmon                    | Free |

**Total: $0**

---

## Resume Summary

Built a SIEM lab using Elastic Stack with Windows and Linux log sources; configured log ingestion and detection rules; simulated attacks such as brute force and privilege escalation; investigated alerts and documented incident response procedures.

---

## Future Improvements

* Add Active Directory domain simulation
* Integrate MITRE ATT&CK mapping
* Add alert severity scoring
* Automate log ingestion pipelines
* Deploy cloud-based SIEM version (Azure / AWS)
