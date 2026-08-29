# 🛡️ ATT&CK Threat Intelligence & Framework Operationalization

## 📌 Incident Overview

* **Platform:** Blue Team Labs Online (BTLO)
* **Category:** Threat Intelligence / ATT&CK
* **Difficulty:** Easy
* **Role / Perspective:** Blue Team Specialist / Threat Intelligence Analyst

## 📝 Scenario Summary

You are hired as a Blue Team member for a company assigned to perform threat intelligence operations. The primary objective is to operationalize the MITRE ATT&CK framework by mapping specific attack behaviors, threat actor techniques, software, and tactical IDs to solve scenario-based problems and strengthen defensive strategies.

## 📊 Summary of Key Findings & ATT&CK Mappings

| **Artifact Category**                   | **Extracted Value**  | **Description / Significance**                                                                                       |
| --------------------------------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Technique – Cloud Service Dashboard** | `T1538`              | Discovery technique involving the use of cloud service dashboards to inspect cloud resources.                        |
| **Associated Threat Group**             | `G0099` (APT-C-36)   | Threat group associated with activity involving uncommon network communication over Port 4050.                       |
| **Tactic – Initial Access**             | `TA0001`             | ATT&CK Tactic ID representing techniques used by adversaries to gain initial access.                                 |
| **Malware / Software**                  | `S0372` (LockerGoga) | Ransomware associated with modifying account configurations and restricting user access.                             |
| **Technique – Pass the Hash**           | `T1550.002`          | Lateral movement technique involving the use of password hashes for authentication instead of plaintext credentials. |

## 🔬 Investigation Methodology & Workflow

### 1. Cloud Discovery Defense & Mapping

* Mapped scenario parameters involving public cloud environments such as Azure AD and Microsoft 365 accessed through valid credentials.
* Identified **Cloud Service Dashboard (`T1538`)** under the Discovery tactic as the primary technique used by attackers to inspect cloud resources through web interfaces.

### 2. C2 Port & Threat Actor Correlation

* Investigated C2 communications and custom network activity involving non-standard ports.
* Cross-referenced MITRE ATT&CK Threat Groups and identified **APT-C-36 (`G0099`)** as the associated threat group for the Port 4050 activity described in the scenario.

### 3. Tactic & Software Mapping

* Identified **Initial Access (`TA0001`)** as the relevant ATT&CK tactic representing techniques used to gain an initial foothold.
* Identified **LockerGoga (`S0372`)** as the documented software associated with ransomware activity that can modify account and password configurations and restrict user access.

### 4. Pass the Hash Detection Verification

* Analyzed the detection guidance for **Pass the Hash (`T1550.002`)**.
* Confirmed that monitoring Windows authentication and credential usage is an important part of detecting suspicious Pass the Hash activity.
* Relevant Windows Security events include **Event ID 4624 (Successful Logon)** and **Event ID 4625 (Failed Logon)**, which should be correlated with source hosts, accounts, logon types, and other authentication telemetry rather than analyzed in isolation.

## 🛡️ Recommended Mitigation & Action Plan

1. **Enforce Multi-Factor Authentication (MFA):** Require strong, preferably phishing-resistant MFA for cloud services to reduce the risk of compromised credentials being abused.

2. **Network Monitoring & Egress Filtering:** Monitor unusual outbound connections and investigate unexpected communication over non-standard ports, including Port 4050 when correlated with the activity described in this scenario.

3. **Advanced Logon Auditing:** Enable appropriate Windows Security Event Logging and monitor authentication activity, including Event IDs 4624 and 4625, to identify abnormal credential usage and potential lateral movement.

4. **Cloud Access Monitoring:** Monitor cloud authentication activity for unusual locations, devices, login patterns, and unexpected access to cloud resources.

## 🧰 Tools & Frameworks

* Blue Team Labs Online
* MITRE ATT&CK
* Windows Security Event Logs
* Threat Intelligence
* Network Monitoring

## 🎓 Key Takeaways

* MITRE ATT&CK can be used to map observed adversary behavior to standardized tactics and techniques.
* Threat intelligence can help correlate technical observations with known threat groups and software.
* Cloud environments require monitoring for suspicious use of valid credentials and unusual discovery activity.
* Authentication logs are valuable for detecting abnormal credential usage and potential lateral movement.
* Effective Blue Team analysis combines threat intelligence with detection and mitigation strategies.
