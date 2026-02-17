# Cerber-Ransomware-Triage
Reconstruction of a multi-stage Cerber ransomware attack involving brute-force, steganography, and C2 beaconing.

# Incident Report: Cerber Ransomware Investigation & Triage

## 🛡️ Executive Summary
This report reconstructs a 14-day intrusion where an attacker compromised a Joomla CMS via brute-force, staged a malicious payload, and eventually deployed Cerber ransomware using steganography to bypass signature-based detections.

| Feature | Details |
| :--- | :--- |
| **Disposition** | True Positive |
| **Severity** | 🔴 Critical |
| **Dwell Time** | 13 Days, 19 Hours |
| **Tools Used** | Splunk (SIEM), Suricata (IDS), Sysmon |
| **Final Impact** | Large-scale encryption of .txt and .pdf files |

## 📈 Attack Lifecycle Timeline (UTC)
The attack consisted of an initial compromise followed by a two-week "dormant" period before final ransomware execution.

| Time | Action | Phase |
| :--- | :--- | :--- |
| **2016-08-10 21:30** | Acunetix Vulnerability Scan targeting Joomla CMS | Reconnaissance |
| **2016-08-10 21:45** | 412 Brute-force attempts from IP `23.22.63.114` | Credential Access |
| **2016-08-10 21:46** | Successful admin login (Password: `batman`) | Initial Access |
| **2016-08-10 21:52** | Malicious binary `3791.exe` uploaded to web server | Persistence |
| **2016-08-24 16:48** | `wscript.exe` pulls `mhtr.jpg` (Steganography) | Execution |
| **2016-08-24 16:49** | C2 beaconing and widespread file encryption | Impact |

## 🔍 Technical Indicators & TTPs

### MITRE ATT&CK® Mapping
* **Brute Force (T1110.001):** 412 failed attempts before success.
* **Obfuscated Files: Steganography (T1027.003):** Encryptor hidden within `mhtr.jpg`.
* **Data Encrypted for Impact (T1486):** 650+ documents encrypted with `.cerber` extension.

### 🎯 Post-Incident Response & SOC Recommendations

#### **Immediate Actions Taken**
- **Isolation:** Quarantined host `we8105desk` to stop C2 communication.
- **Credential Reset:** Enforced immediate password rotation for all Joomla admin accounts.
- **Forensics:** Captured memory images to extract hex-encoded strings from the `wscript.exe` process.

#### **Future Detection & Prevention**
- [ ] **Account Lockout:** Implement a policy to lock accounts after 5-10 failed attempts.
- [ ] **MFA Deployment:** Require Multi-Factor Authentication for CMS administrative panels.
- [ ] **New SIEM Correlation:** Alert when a "Successful Login" occurs from an IP previously flagged for scanning/brute-force activity within 24 hours.

