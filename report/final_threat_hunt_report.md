# 🔍 Final Threat Hunt Report: Public Exposure & Brute Force on `windows-target-1`

## 📘 Executive Summary

During a proactive threat hunting exercise on May 22, 2025, the security team identified a virtual machine (`windows-target-1`) that was unintentionally exposed to the public internet. This machine, part of a shared services cluster, had no account lockout policies in place and was receiving multiple failed login attempts from a public IP address. Further investigation confirmed a brute-force attack that resulted in a successful login.

---

## 🎯 Objectives

- Identify any shared services VMs exposed to the internet.
- Investigate potential brute-force login attempts.
- Detect any signs of compromise or post-exploitation activity.
- Map findings to MITRE ATT&CK techniques.
- Document actionable improvements for security posture.

---

## 🧠 Threat Hunting Hypothesis

> If shared services VMs were mistakenly exposed to the public internet and lack proper lockout policies, then a brute-force attack could have occurred — potentially leading to unauthorized access.

---

## 📊 Data Sources

- **DeviceLogonEvents** – for login activity
- **DeviceInfo** – for system metadata and exposure context

---

## 🔍 Key Findings

### 🚨 Brute Force Attack Identified
- **Device**: `windows-target-1`
- **Remote IP**: `185.235.1.44` (Public, Eastern Europe)
- **Attack Window**: May 22, 2025, 03:00–03:52 UTC
- **Failed Logins**: 152 attempts using account `svc-backup`
- **Successful Login**: Occurred at 03:52 UTC from same IP
- **Tools Used**: Remote PowerShell session initiated within 1 minute of login

### 🛠️ Weaknesses
- Public exposure due to firewall misconfiguration
- No account lockout policy configured
- No alert triggered by Defender for failed login pattern

---

## 🔐 MITRE ATT&CK Mapping

| Technique ID | Technique         | Tactic             | Description                                      |
|--------------|-------------------|--------------------|--------------------------------------------------|
| T1110        | Brute Force        | Credential Access  | 150+ failed login attempts from a public IP      |
| T1078        | Valid Accounts     | Persistence        | Compromised account used to gain access          |
| T1021.001    | Remote Services    | Lateral Movement   | Remote PowerShell execution after login          |

---

## 📅 Timeline of Events

| Time (UTC)  | Event                                              |
|-------------|----------------------------------------------------|
| 03:00       | Brute-force login attempts begin                   |
| 03:50       | 152 failed attempts logged                         |
| 03:52       | Successful login with `svc-backup`                 |
| 03:53       | PowerShell session launched                        |
| 04:15       | Threat hunt initiated by security team             |
| 04:45       | VM isolated; incident escalated                    |

---

## 📂 Evidence Artifacts

- KQL queries (`queries/*.kql`)
- Log samples (`logs/*.csv`)
- Visualization (`visuals/failed_login_chart.png`)
- Incident report (`findings/incident_report_windows-target-1.md`)
- MITRE ATT&CK mapping (`findings/MITRE_mapping.md`)

---

## 🧰 Recommendations

### 🔒 Short-Term

- Immediately rotate all credentials on `windows-target-1`
- Remove public IP exposure; restrict with NSG or firewall
- Isolate and reimage the affected VM

### 🛡️ Long-Term

- Enforce account lockout policy via GPO
- Enable geo-fencing for remote logins
- Implement detection rules for:
  - 100+ failed logins in 30 minutes
  - Login success after burst of failures
- Perform audit across other VMs in the same cluster

---

## 🧠 Lessons Learned

- Exposure of internal VMs to public internet must be monitored via continuous scanning
- Brute-force behavior often evades alerts unless patterns are correlated
- Threat hunting should be scheduled routinely — not just post-incident

---

## ✅ Outcome

This hunt successfully identified and contained a brute-force attack on an exposed virtual machine. Security teams now have detection logic in place to prevent recurrence, and hardening measures are being rolled out across the infrastructure.

---

*Report prepared by: Yancarlos Espinosa  
Date: May 30, 2025*
