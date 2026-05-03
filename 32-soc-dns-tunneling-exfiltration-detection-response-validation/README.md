# 🚨 Detection and Response to DNS Data Exfiltration (Suricata + Wazuh)

---

## 📌 Overview

Simulation of a **DNS tunneling/data exfiltration attack**, resulting in **data exfiltration via DNS queries**.

- Access: ❌  
- Execution: ✔  
- Persistence: ❌  
- Evasion: ✔  
- Severity: 🔴 6/10 (Medium)  

---

## 📄 Detailed Incident Report

➡️ Full report: [report.md](./report.md)

---

## 🖥️ Environment

- Attacker: Simulated (`exfil.com`)  
- Target: web-01  
- SIEM: Wazuh  
- IDS: Suricata  
- Service: DNS  

---

## 🎯 Attack Scenario

The host **web-01** generated multiple DNS queries to a controlled domain (`exfil.com`) using sequential subdomains (`data1`, `data2`, ...), simulating **data exfiltration via DNS tunneling**.

---

## 🔍 Detection

Detection was performed through **Suricata DNS logs (eve.json)** by identifying abnormal query frequency and suspicious domain patterns.

![Detection](./images/01_dns_frequency_suspicious_domain.png)

- Rule ID: N/A (Threat Hunting)  
- Detection logic: High-frequency DNS queries + anomalous domain pattern  

---

## 🧠 Investigation

![Evidence](./images/02_dns_exfiltration_payload_pattern.png)

### Evidence:

- Source: `web-01`  
- Domain: `exfil.com`  
- Pattern:
  - `data1.exfil.com`
  - `data2.exfil.com`
- Behavior observed:
  - Sequential DNS queries
  - Data fragmentation via subdomains  

---

## 🔎 Indicators of Compromise (IoCs)

| Category | Indicator | Description | MITRE |
|----------|----------|------------|-------|
| Network | exfil.com | Malicious domain used for exfiltration | T1048.003 |
| Network | data*.exfil.com | Sequential subdomains (data chunks) | T1048.003 |
| Behavior | High DNS frequency | 60 DNS queries to same domain | T1048.003 |
| Detection | Suricata eve.json | Log source for detection | - |

---

## 🔎 Command / Activity Evidence

![Commands](./images/03_dns_exfiltration_volume.png)

- 60 DNS queries identified  
- Sequential pattern (`data1`, `data2`, ...)  
- Consistent with DNS tunneling behavior  

---

## ⚠️ Impact Assessment

- **Access Level:** N/A  
- **Privilege Level:** user  
- **Scope:** web-01  
- **Exposure:** Potential data exfiltration via DNS  

### Severity: 🔴 6/10 (Medium)

**Justification:**
- Stealth exfiltration channel (DNS)
- Confirmed data transfer behavior
- Limited scope (single host)

→ Controlled data exfiltration with moderate impact  

---

## 🛡️ Response

### Containment

![Containment](./images/04_dns_containment_iptables.png)

- Implemented egress filtering using iptables to block DNS queries containing `exfil.com`

---

### Eradication

![Eradication](./images/06_eradication_no_malicious_process.png)

- No malicious process identified  
- No persistence mechanism detected  
- Activity classified as simulated  

---

### Recovery

![Validation](./images/05_dns_no_recurrence_validation.png)

- No further DNS queries observed  
- Traffic successfully contained  

---

### 🔐 Hardening

- DNS domain blocking (egress filtering)  
- DNS traffic monitoring  
- Baseline creation for DNS behavior  

---

### ✅ Defense Validation

- DNS queries to `exfil.com` blocked  
- No recurrence observed in real-time logs  
- Environment stable  

---

## 🧬 MITRE ATT&CK

| Technique ID | Technique Name | Description |
|-------------|--------------|------------|
| T1048.003 | Exfiltration Over Unencrypted Non-C2 Protocol | Data exfiltration via DNS |

---

## 🎯 Conclusion

DNS exfiltration activity was detected through anomaly-based analysis, investigated via log correlation, and successfully contained using egress filtering, with no recurrence after mitigation.

---

## 🧠 Skills Developed

- DNS traffic analysis  
- Threat hunting (Suricata logs)  
- Log correlation (SIEM + IDS)  
- Incident response (containment + validation)  
- Network-based detection  

---

## 📞 Contact

LinkedIn: [https://www.linkedin.com/tiago-krysiaki](https://www.linkedin.com/in/tiagokrysiaki/)

GitHub: https://github.com/TKrysiaki/lab-soc-linux
