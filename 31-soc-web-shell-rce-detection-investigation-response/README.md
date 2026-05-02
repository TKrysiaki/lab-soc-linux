# 🚨 Detection and Response to Web Shell (RCE) Attack (Wazuh + Apache)

---

## 📌 Overview

Simulation of a web shell attack via file upload vulnerability, resulting in remote command execution on the target system.

- Access: ✔ Yes  
- Execution (RCE): ✔ Yes  
- Persistence: ❌ No  
- Evasion: ❌ No  
- Severity: 🔴 9/10 (Critical)  

---

## 📄 Detailed Incident Report

➡️ Full report: [report.md](./report.md)

---

## 🖥️ Environment

- Attacker: 192.168.122.1  
- Target: 192.168.122.171  
- SIEM: Wazuh  
- Web Server: Apache (DVWA)  

---

## 🎯 Attack Scenario

An attacker exploited a vulnerable file upload feature to upload a malicious PHP web shell (`shell.php`) and executed system commands via HTTP requests using the `cmd` parameter.

---

## 🔍 Detection

Suspicious command execution via web application detected by Wazuh:

![Detection](./images/03_wazuh_webshell_detection.png)

- Rule ID: 31514  
- Multiple HTTP requests containing `cmd=`  
- Repeated execution pattern detected  

---

## 🧠 Investigation

Apache logs revealed multiple command executions via HTTP:

![Access Log](./images/02_apache_access_log_webshell.png)

### Evidence:

- Source IP: `192.168.122.1`  
- Endpoint: `/DVWA/hackable/uploads/shell.php`  
- Parameter: `cmd=`  
- Execution confirmed via HTTP requests  

---

## 🔎 Indicators of Compromise (IoCs)

| Category | Indicator | Description | MITRE |
|----------|----------|------------|-------|
| Network | 192.168.122.1 | Attacker IP | T1190 |
| Application | /DVWA/hackable/uploads/shell.php | Web shell endpoint | T1505.003 |
| Application | cmd= | Command execution parameter | T1059 |
| Application | /DVWA/hackable/uploads/shell.php?cmd= | RCE via HTTP request | T1059 |
| Host | /var/www/html/DVWA/hackable/uploads/shell.php | Malicious file | T1505.003 |
| Host | www-data | Execution context | T1059 |
| Behavior | Repeated HTTP command execution | RCE activity | T1059 |
| Detection | Wazuh Rule 31514 | Web shell detection | T1505.003 |

---

## 🔎 Command Execution Evidence

Commands extracted from logs:

![Commands](./images/05_extracted_commands_webshell.png)

- id  
- whoami  
- uname -a  
- pwd  
- ls  
- cat /etc/passwd  

---

## ⚠️ Impact Assessment

- **Access Level:** Remote command execution via web application  
- **Privilege Level:** www-data (web server context)  
- **Scope:** Single host (192.168.122.171)  
- **Exposure:** System enumeration and file access  

### Severity: 🔴 9/10 (Critical)

**Justification:**
- Remote command execution confirmed  
- Ability to execute arbitrary commands  
- Access to system file (`/etc/passwd`) confirmed  

→ Indicates compromise of the web application context.

---

## 🛡️ Response

### Containment

Attacker IP blocked via firewall:

![Response](./images/06_response_eradication_validation.png)

---

### Eradication

- Malicious file (`shell.php`) removed  
- Upload directory access restricted  
- Directory reviewed for additional artifacts  

---

### Recovery

- Web shell no longer accessible (404):

![Validation](./images/07_webshell_access_blocked_404.png)

---

### 🔐 Hardening

- File upload restrictions (block `.php`)  
- MIME type validation  
- Least privilege on web directories  
- Disable PHP execution in upload directories using .htaccess or Apache configuration  

---

### ✅ Defense Validation

- Web shell access blocked (404)  
- No further command execution observed  
- No additional malicious files detected  

→ Environment considered secure after remediation.

---

## 🧬 MITRE ATT&CK

| Technique ID | Technique Name | Description |
|-------------|--------------|------------|
| T1190 | Exploit Public-Facing Application | Exploitation of vulnerable file upload functionality |
| T1505.003 | Web Shell | Malicious PHP file used for command execution |
| T1059 | Command Execution | Remote command execution via HTTP parameter (`cmd=`) |

---

## 🎯 Conclusion

The incident followed the full lifecycle:  
**Detection → Investigation → Classification → Response**

Evidence confirms exploitation of a web application leading to remote command execution.

All malicious artifacts were removed, access was blocked, and defensive measures were validated.

---

## 🧠 Skills Developed

- Web log analysis (Apache)  
- RCE detection via application logs  
- Wazuh alert analysis  
- Log correlation (Apache + Wazuh)  
- Threat hunting (Wazuh + Apache logs)  
- IoC extraction and structuring  
- Incident response (containment + eradication + validation)  
- Web security hardening  

---

## 📞 Contact

LinkedIn: https://www.linkedin.com/in/tiago-krysiaki  
GitHub: https://github.com/TKrysiaki
