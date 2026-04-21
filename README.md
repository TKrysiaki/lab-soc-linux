# 🛡️ SOC Linux Labs – Monitoring, Detection, Investigation, Classification & Response

Hands-on labs focused on **Security Operations Center (SOC)** practices using **Linux, Wazuh (SIEM), auditd, and log analysis**.

---

## 🎯 Objective

Simulate real-world attack scenarios and cover the full SOC workflow:

- Monitoring  
- Detection  
- Investigation (timeline + IoCs)  
- Classification (MITRE ATT&CK)  
- Response (containment + remediation)  

---

## 🧠 SOC Methodology

Monitoring → Detection → Investigation → Classification → Response

---

## 🧰 Tools

- Wazuh (SIEM)  
- auditd  
- auth.log  
- Fail2ban  
- iptables  
- Linux CLI  

---

## 📊 Highlighted Labs

| Lab | Description | Technologies | MITRE |
|-----|------------|-------------|-------|
| [29](https://github.com/TKrysiaki/lab-soc-linux/tree/main/29-soc-ssh-bruteforce-detection-response-persistence-log-tampering) | SSH brute force + persistence + log tampering | Wazuh, auth.log | T1110, T1078 |
| [28](https://github.com/TKrysiaki/lab-soc-linux/tree/main/28-soc-ssh-bruteforce-incident-response-persistence-hardening) | Incident with persistence | Wazuh, Linux | T1110 |
| [27](https://github.com/TKrysiaki/lab-soc-linux/tree/main/27-soc-ssh-bruteforce-detection-response-no-siem) | Detection without SIEM | Linux logs | T1110 |
| [26](https://github.com/TKrysiaki/lab-soc-linux/tree/main/26-soc-ssh-bruteforce-incident-correlation-network-analysis) | Network correlation | Wazuh, logs | T1110 |
| [25](https://github.com/TKrysiaki/lab-soc-linux/tree/main/25-soc-ssh-bruteforce-detection-response-persistence-remediation) | Detection + response + remediation | Wazuh, Fail2ban | T1110 |
| [24](https://github.com/TKrysiaki/lab-soc-linux/tree/main/24-soc-ssh-bruteforce-detection-response-correlation) | Event correlation | Wazuh | T1110 |
| [23](https://github.com/TKrysiaki/lab-soc-linux/tree/main/23-soc-incident-response-bruteforce-correlation-threatintel) | Incident investigation | Wazuh | T1110 |
| [22](https://github.com/TKrysiaki/lab-soc-linux/tree/main/22-soc-bruteforce-detection-response-wazuh-thehive) | SIEM-based detection | Wazuh | T1110 |
| [21](https://github.com/TKrysiaki/lab-soc-linux/tree/main/21-ssh-web-attack-detection-correlation-auto-response-wazuh) | Web attack + correlation | Wazuh | T1190 |
| [20](https://github.com/TKrysiaki/lab-soc-linux/tree/main/20-ssh-bruteforce-detection-response-wazuh-fail2ban) | Wazuh + Fail2ban | Wazuh | T1110 |

---

## 📂 Lab Structure

Each lab includes:

- `README.md` → overview  
- `report.md` → detailed SOC incident report  
- Evidence (logs, alerts, screenshots)  

---

## 🔍 Detection Example (Wazuh)

- Rule: `40112`  
- Event: multiple authentication failures followed by a success  
- Classification:
  - T1110 – Brute Force  
  - T1078 – Valid Accounts  

---

## 🚀 Upcoming Labs

- Web shell detection (Linux)  
- DNS tunneling / exfiltration  
- Advanced Wazuh correlation rules  

---
## 🧑‍💻 Author

Tiago Krysiaki  
Foco: SOC N1 | Blue Team | Threat Detection

LikidIn: https://www.linkedin.com/in/tiago-krysiaki
