# Threat Hunting Scenario: Exposed VM + Brute-Force Investigation

This lab simulates a real-world threat hunting scenario using Microsoft Defender for Endpoint and Kusto Query Language (KQL). The objective is to identify VMs unintentionally exposed to the internet and analyze potential brute-force login attempts or compromises.

## 🎯 Scenario Overview

**Situation**: During routine maintenance, a security team discovered that several VMs in the shared services cluster were mistakenly exposed to the internet.

**Goal**: Investigate those machines for brute-force login attempts, particularly on older VMs without account lockout policies.

## 🧠 Hypothesis

> If a VM is exposed to the public internet and lacks lockout policies, then attackers may have been able to brute-force into it by trying many passwords without restriction.

## 📊 Data Sources

- `DeviceLogonEvents`
- `DeviceInfo`

## 🔍 Threat Hunting Steps

1. **Preparation** – Defined a hypothesis based on known misconfigurations and external exposure.
2. **Data Collection** – Used Defender logs (`DeviceLogonEvents`) to gather failed login attempts.
3. **Analysis** – Queried for devices receiving many failed logins, especially from public IPs.
4. **Investigation** – Identified potential successful brute-force logins followed by suspicious actions.
5. **Response** – Documented indicators and recommended actions (account lockout policy, firewall rules).
6. **Documentation** – Full report written and mapped to MITRE ATT&CK TTPs.
7. **Improvement** – Suggested detection rule improvements and post-incident hardening.

## 🛠️ Tools & Skills

- Microsoft Defender for Endpoint
- KQL (Kusto Query Language)
- MITRE ATT&CK Mapping
- Incident documentation
- Data analysis and pattern recognition

## 📂 Contents

- `/queries` – All KQL used for detection
- `/findings` – Key incidents and IOC documentation
- `/report` – Final write-up of the threat hunt

## ✅ Key Findings

- `windows-target-1` received over 150 failed logins from public IPs in under 1 hour.
- A successful login occurred shortly after from the same IP – likely brute-force success.
- Mapped to ATT&CK technique: **T1110 – Brute Force**.

## 🔒 Prevention Steps

- Implement account lockout policy
- Enable geofencing on login attempts
- Create alert rules for brute-force patterns

---

## 📸 Screenshots

![failed_logins](visuals/failed_login_chart.png)
