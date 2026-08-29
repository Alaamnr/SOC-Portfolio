# BTLO-Phishing-Email-Analysis
SOC Investigation &amp; Artifact Extraction Report for BTLO Phishing Challenge.
# 🛡️ Phishing Email Investigation & Artifact Extraction

## 📌 Incident Overview
- **Platform:** Blue Team Labs Online (BTLO)
- **Category:** Phishing Analysis
- **Difficulty:** Easy
- **Role / Perspective:** Security Operations Center (SOC) Analyst Tier 1

---

## 📝 Scenario Summary
A user received a suspicious email and forwarded it to the SOC team for investigation. The objective is to analyze the raw email headers (`.eml`), inspect nested attachments, and extract useful Indicators of Compromise (IoCs) safely without exposing internal systems.

---

## 📊 Summary of Extracted Artifacts (IoCs)

| Artifact Category | Extracted Value | Description / Significance |
| :--- | :--- | :--- |
| **Primary Recipient** | `kinnar1975@yahoo.co.uk` | Target user account |
| **Email Subject** | `Undeliverable: Website contact form submission` | Subject line used in the phishing attempt |
| **Timestamp** | `18 March 2021 04:14` | Time of email dispatch |
| **Originating IP** | `103.9.171.10` | Source IP of the sending SMTP server |
| **Reverse DNS (PTR)** | `c5s2-1e-syd.hosting-services.net.au` | Resolved hostname via Whois lookup |
| **Attachment Name** | `Website contact form submission.eml` | Nested `.eml` file containing the malicious payload |
| **Malicious URL** | `https://35000usdperwwekpodf.blogspot.sg?...` | Phishing URL embedded inside the attachment |
| **Hosting Service** | `blogspot` | Platform hosting the malicious landing page |
| **Page Status** | `Blog has been removed` | Status observed via safe web rendering (`URL2PNG`) |

---

## 🔬 Investigation Methodology & Workflow

### 1. Header Analysis & Text Decoding
- Opened the raw `.eml` file using a standard text editor to inspect MIME structures.
- Analyzed `X-Originating-IP` and `Content-Disposition` headers to trace the sender and locate attached files.

### 2. OSINT & Threat Intelligence
- Conducted Reverse DNS / WHOIS lookup on `103.9.171.10` to identify the network owner.

### 3. Safe URL Inspection (Headless Analysis)
- Avoided direct interaction with the suspicious URL (`blogspot`).
- Leveraged `URL2PNG` / safe headless rendering to capture page screenshots safely without executing potentially malicious scripts locally.

---

## 🛡️ Recommended Mitigation & Action Plan
1. **Perimeter Blocking:** Add IP `103.9.171.10` and domain patterns related to the phishing link to email gateway / firewall blocklists.
2. **Mailbox Sweep:** Query Microsoft 365 / Email Gateway logs for messages containing matching subject lines or originating IPs to purge them from other user mailboxes.
3. **User Awareness:** Remind users to remain cautious when handling `.eml` or unexpected form submission attachments.
