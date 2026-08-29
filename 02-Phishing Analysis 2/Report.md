# 🛡️ Phishing Email Investigation & Artifact Extraction (Part 2)

## 📌 Incident Overview
- **Platform:** Blue Team Labs Online (BTLO)
- **Category:** Phishing Analysis
- **Difficulty:** Easy
- **Role / Perspective:** Security Operations Center (SOC) Analyst Tier 1

---

## 📝 Scenario Summary
A suspicious inbound email impersonating **Amazon Support** was submitted to the SOC team for investigation. The email claimed that the recipient's account was limited due to unusual activity and required urgent action within 72 hours. The goal is to safely dissect the raw email structure, decode obfuscated HTML payloads, and extract critical Indicators of Compromise (IoCs).

---

## 📊 Summary of Extracted Artifacts (IoCs)

| Artifact Category | Extracted Value | Description / Significance |
| :--- | :--- | :--- |
| **Body Encoding Scheme** | `base64` | Obfuscation technique used to bypass standard email filters |
| **Call-to-Action (CTA) URL** | `https://amaozn.zzyuchengzhika.cn/?mailtoken=saintington73@outlook.com` | Primary malicious landing page embedded in the button |
| **Target Domain** | `amaozn.zzyuchengzhika.cn` | Typosquatting domain impersonating Amazon |
| **Logo Resource URL** | `https://images.squarespace-cdn.com/content/.../amazon-logo...` | External legitimate image used for brand spoofing |
| **Attacker Social Profile** | `amir.boyka.7` | Anomalous Facebook username extracted from email footer |
| **Headless Page Status** | `This web page could not be loaded.` | Status returned during URL2PNG automated rendering |

---

## 🔬 Investigation Methodology & Workflow

### 1. Structure & Payload Decoding
- Opened the `.eml` file using a standard text editor and identified a multipart MIME structure.
- Located the core email body encoded in **Base64** (`Content-Transfer-Encoding: base64`).
- Decoded the Base64 payload using **CyberChef** (`From Base64`) to expose the hidden raw HTML layout.

### 2. Obfuscation & SafeLinks Triage
- Inspected the `<A href="...">` and `originalSrc` attributes within the decoded HTML.
- Identified Microsoft Outlook SafeLinks wrapping around the malicious destination domain `amaozn.zzyuchengzhika.cn`.
- Extracted external image links used to spoof Amazon’s brand identity.

### 3. Headless Analysis & Anomaly Detection
- Analyzed the phishing domain via headless rendering services (`URL2PNG`), observing server timeouts / non-responsive page states.
- Flagged an anomalous social media link in the signature section pointing to a personal Facebook account (`amir.boyka.7`).

---

## 🛡️ Recommended Mitigation & Action Plan
1. **Perimeter & SWG Blocking:** Block `zzyuchengzhika.cn` and all related subdomains on the Secure Web Gateway (SWG) and DNS sinkholes.
2. **Mail Gateway Rules:** Implement pattern-matching rules on the Email Gateway to flag external emails containing typosquatted brand domains (e.g., `amaozn`).
3. **Credential Reset & Containment:** Invalidate active sessions and initiate password resets for any user who interacted with the link within the 72-hour window.