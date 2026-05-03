# 🚨 Detection and Response to Unauthorized SSH Access & Credential Access Attempt (Wazuh / Linux)

---

## 📌 Overview

Simulation of unauthorized SSH access using valid credentials, followed by attempted credential access and privilege escalation.

- Access: ✔  
- Execution: ✔  
- Persistence: ❌ (not observed)  
- Evasion: ❌  
- Severity: 🔴 8/10 (High)  

---

## 📄 Detailed Incident Report

➡️ Full report: [report.md](./report.md)

---

## 🖥️ Environment

- Attacker: <ATTACKER_IP>  
- Target: <TARGET_IP>  
- SIEM: Wazuh  
- Service: SSH  

---

## 🎯 Attack Scenario

An attacker successfully authenticated via SSH using valid credentials.  
Post-login activity included attempts to access sensitive files (`/etc/shadow`) and execution of privileged commands using `sudo`.

---

## 🔍 Detection

Detection occurred through correlation of:

- Successful SSH login  
- Access attempt to sensitive file  
- Privileged command execution  

![Detection](./images/01_detection.png)

- Rule ID: <RULE_ID>  
- Detection logic: SSH success + sensitive file access + sudo usage correlation  

---

## 🧠 Investigation

![Evidence](./images/02_evidence.png)

### Evidence:

- Source IP: `<ATTACKER_IP>`  
- Service: `SSH`  
- Target file: `/etc/shadow`  
- Behavior observed:
  - Valid login
  - Sensitive file access attempt
  - Privilege escalation activity

---

## 🔎 Indicators of Compromise (IoCs)

| Category | Indicator | Description | MITRE |
|----------|----------|------------|-------|
| Network | <ATTACKER_IP> | Source of SSH login | [T1078](https://attack.mitre.org/techniques/T1078/) |
| Host | /etc/shadow | Credential file access attempt | [T1003](https://attack.mitre.org/techniques/T1003/) |
| Behavior | sudo usage | Privilege escalation attempt | [T1548](https://attack.mitre.org/techniques/T1548/) |
| Host | auth.log | Authentication logs | N/A |
| Detection | <RULE_ID> | Wazuh correlation rule | N/A |
| Response | IP block | Containment action | N/A |

---

## 🔎 Command / Activity Evidence

![Commands](./images/03_commands.png)

- `cat /etc/shadow` (attempt)  
- `sudo <command>`  

---

## ⚠️ Impact Assessment

- **Access Level:** Valid user access  
- **Privilege Level:** User → attempted root escalation  
- **Scope:** Single host  
- **Exposure:** Attempted access to password hashes  

### Severity: 🔴 8/10 (High)

**Justification:**
- Valid account compromise  
- Attempt to access credential storage  
- Privilege escalation behavior observed  

→ High risk of credential exposure and further compromise  

---

## 🛡️ Response

### Containment

![Containment](./images/04_response.png)

- Attacker IP blocked (iptables / Fail2ban)  
- SSH session terminated  
- User account temporarily disabled  

---

### Eradication

- Password reset enforced  
- Review of `~/.ssh/authorized_keys`  
- No malicious persistence artifacts found  

---

### Recovery

![Validation](./images/05_validation.png)

- System integrity validated  
- No further suspicious activity observed  

---

### 🔐 Hardening

- SSH access restricted (Fail2ban enabled)  
- Sudo usage monitored  
- Audit rules for `/etc/shadow` implemented (auditd)  
- Least privilege enforced  

---

### ✅ Defense Validation

- Repeated access attempt blocked  
- Alerts triggered correctly  
- No recurrence observed  

---

## 🧬 MITRE ATT&CK

| Technique ID | Technique Name | Description |
|-------------|--------------|------------|
| [T1078](https://attack.mitre.org/techniques/T1078/) | Valid Accounts | Use of legitimate credentials |
| [T1003](https://attack.mitre.org/techniques/T1003/) | OS Credential Dumping | Attempt to access `/etc/shadow` |
| [T1548](https://attack.mitre.org/techniques/T1548/) | Abuse Elevation Control Mechanism | Use of sudo |

---

## 🎯 Conclusion

Detection → Investigation → Classification → Response  

Unauthorized SSH access using valid credentials led to attempted credential access and privilege escalation.  
The incident was contained, no persistence was identified, and defensive controls were successfully validated.

---

## 🧠 Skills Developed

- Linux log analysis (auth.log)  
- SIEM correlation (Wazuh)  
- Credential access detection  
- Privilege escalation investigation  
- Incident response (containment + validation)  
- MITRE ATT&CK mapping  

---

## 📞 Contact

LinkedIn: https://www.linkedin.com/in/tiagokrysiaki/  
GitHub: https://github.com/TKrysiaki  
