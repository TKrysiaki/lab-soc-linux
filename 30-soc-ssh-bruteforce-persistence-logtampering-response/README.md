# 🚨 Detection and Response to SSH Brute Force with Compromise, Persistence and Log Tampering (Wazuh + Fail2Ban)

---

## 📌 Overview

Simulation of an SSH brute force attack resulting in full system compromise, persistence, privilege abuse, and log tampering attempts.

- Access: ✔ Yes  
- Persistence: ✔ Yes  
- Evasion (Log Tampering): ✔ Yes  
- Severity: 🔴 Critical  

---

## 📄 Detailed Incident Report

➡️ Full report: [report.md](./report.md)

---

## 🖥️ Environment

- Attacker: 192.168.122.1  
- Target: 192.168.122.102  
- SIEM: Wazuh  
- Defense: Fail2Ban  

---

## 🎯 Attack Scenario

A brute force attack was executed against the SSH service, resulting in unauthorized system access.

---

## 🔍 Detection

Brute force activity detected in Wazuh after multiple failed SSH authentication attempts:

![Detection](./images/01_wazuh_bruteforce_detection.png)

Multiple failed login attempts:

![Failed Password](./images/02_authlog_failed_password.png)

Successful login after brute force:

![Accepted Password](./images/03_authlog_accepted_password.png)

Attack correlation identified:

![Correlation](./images/04_wazuh_bruteforce_success_correlation.png)

---

## 🧠 Investigation

Post-compromise activity identified:

![Bash History](./images/05_bash_history_post_compromise.png)

### Evidence:

- Compromised user: `target`  
- Source IP: `192.168.122.1`  
- Command execution observed  
- Privilege escalation via sudo  

---

## 🔎 Indicators of Compromise (IoCs)

### 🌐 Network
- Source IP: 192.168.122.1  
- Target IP: 192.168.122.102  
- Service: SSH (port 22)  

### 👤 Authentication
- Multiple failed SSH login attempts  
- Successful login for user: target  

### 🧑‍💻 Accounts
- Compromised account: target  
- Malicious account created: suporte  

### 🔐 Persistence
- Creation of new user (suporte)  
- Privilege escalation via sudo  

### 🧼 Log Tampering
- Partial deletion of auth.log  
- Inconsistency between auth.log and auditd logs  

### ⚙️ System Behavior
- Post-compromise command execution  
- Privileged commands confirmed via auditd  

---

## 🧼 Log Tampering

Attempt to remove evidence:

![Log Tampering](./images/06_authlog_tampering_evidence.png)

Evidence preserved via auditd:

![Auditd SSH](./images/07_auditd_ssh_exec_activity.png)

Privileged execution confirmed:

![Sudo Activity](./images/08_auditd_sudo_activity.png)

---

## 🔐 Persistence

Malicious user creation:

![Persistence](./images/09_persistence_user_suporte.png)

---

## 🎯 Impact Assessment

- **Access Level:** Unauthorized SSH access with valid credentials  
- **Privilege Level:** Sudo-level command execution (potential root)  
- **Scope:** Single host (192.168.122.102)  
- **Persistence:** Malicious user created for continued access  
- **Evasion:** Log tampering attempt detected  

### 🔴 Severity: CRITICAL

**Justification:**
- Successful initial access  
- Persistence established  
- Privileged command execution  
- Evidence of log evasion  

→ Indicates full system compromise with risk of attacker re-entry.

---

## 🛡️ Response

### Containment

Attacker IP blocked using Fail2Ban:

![Fail2Ban](./images/12_fail2ban_defense_validation.png)

---

### Eradication

Removal of malicious user:

![User Removed](./images/10_persistence_user_removed.png)

---

### Recovery

Credential reset for compromised account:

![Password Reset](./images/13_password_reset_target.png)

---

### 🔐 Hardening

SSH configuration reinforced:

![SSH Hardening](./images/14_ssh_hardening_config.png)

---

### ✅ Defense Validation

- New brute force attempts successfully blocked  
- No unauthorized access observed after containment  
- Malicious user not recreated  
- No new persistence mechanisms detected  

→ Security controls validated and environment considered stable.

---

## 🧬 MITRE ATT&CK

- T1110 — Brute Force  
- T1078 — Valid Accounts  
- T1098 — Account Manipulation  
- T1070 — Indicator Removal  

---

## 🎯 Conclusion

The incident followed the full lifecycle:
Detection → Investigation → Classification → Response

Evidence confirms successful initial access, persistence, privilege escalation, and attempted log evasion.

All malicious artifacts were removed, defensive measures were implemented and validated, and system integrity was restored.

---

## 🧠 Skills Developed

- SSH log analysis  
- Brute force detection  
- Wazuh correlation  
- Persistence identification  
- Incident response with Fail2Ban  
- SSH hardening  
- Log tampering detection  

---

## 📞 Contact

LinkedIn: https://www.linkedin.com/in/tiago-krysiaki  
GitHub: https://github.com/TKrysiaki
