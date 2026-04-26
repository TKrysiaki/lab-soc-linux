# 🛡️ SOC Linux Labs – Monitoring, Detection, Investigation, Classification & Response
![SIEM](https://img.shields.io/badge/SIEM-Wazuh-blue)
![Detection](https://img.shields.io/badge/Focus-Threat%20Detection-red)
![OS](https://img.shields.io/badge/OS-Linux-black)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

Practical SOC labs demonstrating real-world detection, investigation, and response using Wazuh (SIEM).

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

## 🚨 Key Skills Demonstrated

- SIEM monitoring and alerting (Wazuh)
- Brute force detection and correlation
- Log analysis (auth.log, auditd)
- Incident investigation (timeline + IoCs)
- Threat classification (MITRE ATT&CK)
- Response and remediation (Fail2ban, hardening)

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
| [30](./30-soc-ssh-bruteforce-persistence-logtampering-response) | SSH brute force + valid access + persistence + log tampering + response validation | Wazuh, auth.log, auditd, Fail2ban | T1110, T1078, T1098, T1070 |
| [29](./29-soc-ssh-bruteforce-persistence-logtampering) | SSH brute force + valid access + persistence + log tampering | Wazuh, auth.log | T1110, T1078, T1098, T1070 |
| [28](./28-soc-incident-persistence) | SSH brute force + valid access + persistence (no SIEM correlation) | Linux logs, auth.log | T1110, T1078, T1098 |
| [27](./27-soc-detection-without-siem) | SSH brute force detection (manual log analysis, no SIEM) | Linux logs, auth.log | T1110 |
| [26](./26-soc-network-correlation) | SSH brute force detection + network correlation | Wazuh, logs | T1110 |
| [25](./25-soc-detection-response-remediation) | SSH brute force detection + response + remediation (Fail2ban) | Wazuh, Fail2ban | T1110 |
| [24](./24-soc-event-correlation) | SSH brute force detection + event correlation (SIEM rules) | Wazuh | T1110 |
| [23](./23-soc-incident-investigation) | SSH brute force detection + investigation (timeline + IoCs) | Wazuh | T1110 |
| [22](./22-soc-siem-detection) | SSH brute force detection using SIEM (Wazuh rules) | Wazuh | T1110 |
| [21](./21-soc-web-attack-correlation) | Web attack detection + correlation (HTTP logs + SIEM) | Wazuh, web logs | T1190 |
| [20](./20-soc-wazuh-fail2ban) | Brute force detection + automated blocking (Fail2ban integration) | Wazuh, Fail2ban | T1110 |

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
Focus: SOC N1 | Blue Team | Threat Detection  

LinkedIn: https://www.linkedin.com/in/tiago-krysiaki

---

![GitHub Stats](https://github-readme-stats.vercel.app/api?username=TKrysiaki&show_icons=true&include_all_commits=true)
