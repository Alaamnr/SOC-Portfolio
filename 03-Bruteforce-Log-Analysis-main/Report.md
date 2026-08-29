# 🛡️ RDP Brute-Force Log Analysis & Artifact Extraction

## 📌 Incident Overview
* Platform: Blue Team Labs Online (BTLO)
* Challenge Name: Bruteforce
* Category: Log Analysis / Incident Response
* Difficulty: Medium
* Role / Perspective: SOC Analyst (Tier 1)

---

## 📝 Scenario Summary
A system administrator detected an unusually high volume of Audit Failure events in the Windows Security Event Log, suggesting an automated brute-force attack against the Remote Desktop Protocol (RDP) service. 

The objective of this investigation is to analyze the extracted log artifacts, trace the attacker's activity, identify targeted local accounts, and determine network indicators (Source IP and Port range) without running untrusted code on local systems.

---

## 📊 Summary of Extracted Artifacts (IoCs)

| Artifact Category | Extracted Value | Description / Significance |
| :--- | :--- | :--- |
| Windows Event ID | 4625 | Standard Event ID for failed logon attempts |
| Total Failure Events | 3128 | Total count of failed authentication attempts detected |
| Targeted Account | administrator | Local privileged account targeted by the brute-force attack |
| Failure Reason | Unknown user name or bad password | Sub-status description indicating wrong credentials |
| Attacker Source IP | 172.16.17.18 | Source IP address conducting the logon requests |
| IP Network Scope | LAN (Private IP) | Private IP address space (RFC 1918) |
| Source Port Range | 49162-65534 | Ephemeral port range used by the attacker to randomize requests |

---

## 🔬 Investigation Methodology & Workflow

### 1. File Inspection & String Matching
Loaded BTLO_Bruteforce_Challenge.txt into Command Prompt (cmd) and queried for Audit Failure strings to calculate the exact event count accurately:

find /c "Audit Failure" BTLO_Bruteforce_Challenge.txt

### 2. Log Parsing & Event Identification
* Inspected Event ID 4625 properties using Windows Event Viewer / Excel filtering.
* Identified the targeted account name under Account For Which Logon Failed and extracted the exact Failure Reason string.

### 3. Network Information & Port Parsing
Isolated network fields (Source Network Address and Source Port). Executed a PowerShell script to parse and extract the minimum and maximum source ports used during the brute-force attempt (Result: Range 49162 to 65534).

---

## 🛡️ Recommended Mitigation & Action Plan

* Account Lockout Policy: Configure Windows Account Lockout policies (e.g., lock the account after 5 failed attempts for 15-30 minutes) to render automated brute-force attacks ineffective.
* Network Segmentation & RDP Hardening: Restrict direct internet/untrusted exposure of RDP port 3389. Enforce access via a secure Gateway/VPN with Multi-Factor Authentication (MFA).
* SIEM / SOC Alerting: Create a SIEM detection rule that triggers an alert when more than 20 failed logon attempts (Event ID 4625) occur within a 5-minute window from the same source IP.
