# SOC Linux Lab — Log Analysis and Incident Investigation

This repository contains hands-on cybersecurity labs focused on Security Operations Center (SOC) activities using Linux environments.

The project simulates a real-world SSH brute force attack scenario and demonstrates the full investigation process through log analysis and incident documentation.

---

## 🎯 Objectives

- Practice SOC Tier 1 investigation workflow  
- Analyze Linux authentication logs  
- Identify attacker behavior  
- Document security incidents with evidence  
- Develop defensive (Blue Team) skills  

---

## 🧪 Lab Environment

- Attack Source: Kali Linux  
- Target Machine: Ubuntu Server 22.04  
- Service: SSH  
- Attack Type: SSH Brute Force  
- Log Source: `/var/log/auth.log`  

---

## 🔍 Investigation Steps

- Identify attacker IP address  
- Analyze attack timeline  
- Identify targeted user account  
- Verify successful login  
- Conclude the incident  

---

## 📁 Lab Steps

📁 Lab Steps

- [01-log-analysis-base](./01-log-analysis-base/README.md) — Initial incident detection  
- [02-ssh-bruteforce-investigation](./02-ssh-bruteforce-investigation/README.md) — Attack simulation  
- [03-log-analysis-investigation](./03-log-analysis-investigation/README.md) — SOC investigation (Tier 1)  
- [04-ssh-bruteforce-detection-and-fail2ban](./04-ssh-bruteforce-detection-and-fail2ban/README.md) — Attack mitigation with Fail2ban  
- [05-ssh-hardening](./05-ssh-hardening/README.md) — SSH service hardening  
- [06-auth-log-deep-analysis](./06-auth-log-deep-analysis/README.md) — Advanced log analysis  
- [07-user-activity-investigation](./07-user-activity-investigation/README.md) — Suspicious activity investigation  
- [08-suspicious-login-detection](./08-suspicious-login-detection/README.md) — Suspicious login detection  
- [09-privilege-escalation-analysis](./09-privilege-escalation-analysis/README.md) — Privilege escalation analysis  
- [10-unauthorized-file-access-investigation](./10-unauthorized-file-access-investigation/README.md) — Unauthorized file access investigation  

---

## 🔎 MITRE ATT&CK Mapping

- T1110 - Brute Force  
- T1078 - Valid Accounts  

---

## 🛠️ Skills Developed

- Linux log analysis (auth.log, SSH events)  
- Brute force attack detection  
- Incident investigation (SOC Tier 1)  
- Timeline reconstruction from logs  
- Detection of unauthorized access  
- SSH hardening and mitigation (Fail2ban)  
- Basic privilege escalation analysis  

---

## 🚀 Next Steps

- Implement active defense with Fail2ban  
- Automate attacker IP blocking  
- Expand incident response workflow  

---

## 👨‍💻 Author

Cybersecurity student focused on Blue Team, SOC analysis, and defensive security.
