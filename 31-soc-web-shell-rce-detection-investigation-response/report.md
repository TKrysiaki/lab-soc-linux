# Incident Report — Web Shell (RCE) via DVWA Upload

---

## 1. Summary

A web shell attack was successfully executed through a vulnerable file upload mechanism, resulting in remote command execution (RCE) on the web server.

The attacker uploaded a malicious PHP file (`shell.php`) and executed system commands via HTTP requests using the `cmd` parameter.

The incident was detected by Wazuh SIEM and validated through Apache access logs.

---

## 2. Timeline

| Time (UTC-3)        | Event | Source |
|---------------------|------|--------|
| 02/May/2026 10:47:00 | Access to upload functionality | Apache access.log |
| 02/May/2026 10:47:00 | Malicious file uploaded (POST) | Apache access.log |
| 02/May/2026 10:58:40 | First execution (`cmd=id`) | Apache access.log |
| 02/May/2026 10:58:45 | Execution (`cmd=whoami`) | Apache access.log |
| 02/May/2026 10:58:50 | Execution (`cmd=uname -a`) | Apache access.log |
| 02/May/2026 10:58:55 | Execution (`cmd=pwd`) | Apache access.log |
| 02/May/2026 10:59:00 | Execution (`cmd=ls`) | Apache access.log |
| 02/May/2026 10:59:05 | Sensitive file access (`/etc/passwd`) | Apache access.log |
| 02/May/2026 11:00:00 | Detection triggered | Wazuh |
| 02/May/2026 11:05:00 | Web shell removed | Host |
| 02/May/2026 11:06:00 | Attacker IP blocked | Firewall |
| 02/May/2026 11:07:00 | Validation (404 response) | Apache access.log |

---

## 3. Detection

**SIEM:** Wazuh  

- Rule: Web shell command execution detection  
- Rule ID: Wazuh Rule 31514 
- Level: High  

### Evidence

![Wazuh Detection](./images/03_wazuh_webshell_detection.png)

- Multiple HTTP requests with `cmd=` parameter  
- Repeated command execution pattern  
- MITRE mapping applied  

---

### Detection Gap

- File upload allowed execution of `.php` files  
- No validation of file type or content  
- No restriction on upload directory execution  

**Root Cause:**  
Insecure file upload configuration enabled arbitrary code execution.

---
### Recommendations

- Block execution of `.php` files in upload directories  
- Implement MIME type validation  
- Restrict upload directory permissions  
- Add SIEM rules for query-based execution patterns  
- Disable execution of PHP files in upload directories  

---

## 4. Investigation

### Log Sources

- Apache: `/var/log/apache2/access.log`
- SIEM: Wazuh alerts (Rule ID: 31514)

---

### Analyst Hypothesis

The attacker exploited a vulnerable file upload functionality to deploy a web shell (`shell.php`) and executed system commands via HTTP requests using the `cmd=` parameter.

---

### Access Log Evidence

![Access Log](./images/02_apache_access_log_webshell.png)

- Repeated requests to `/DVWA/hackable/uploads/shell.php`
- Use of parameter `cmd=`
- Source IP: `192.168.122.1`
- High-frequency requests (interactive behavior)

---

### Command Execution Evidence

![Commands](./images/05_extracted_commands_webshell.png)

Commands executed:

- id  
- whoami  
- uname -a  
- pwd  
- ls  
- cat /etc/passwd  

---

### Execution Context

- User: `www-data`
- Execution within web server context (limited privileges)

---

### Key Findings

- Remote command execution confirmed via HTTP  
- Consistent attacker behavior from single IP  
- System enumeration and file access observed  
- No persistence or privilege escalation identified  

---

## 5. Impact Assessment

### Severity
🔴 Severity: 9/10 (Critical)

### Scope
- Single host affected: 192.168.122.171 (web-01)
- No lateral movement observed

### Compromise

- **Initial Access:** Vulnerable file upload  
- **Execution:** Remote command execution (`cmd=`)  
- **Privilege Level:** `www-data`  
- **Persistence:** Not observed  
- **Data Exposure:** Access to `/etc/passwd` confirmed  

### Summary

Remote command execution was achieved via web shell, enabling system enumeration and file access. Despite limited privileges, the ability to execute arbitrary commands represents a critical impact.

---

## 6. MITRE ATT&CK Mapping

- T1190 — Exploit Public-Facing Application  
- T1505.003 — Web Shell  
- T1059 — Command Execution  

---

## 7. CIS Controls Mapping

- CIS Control 4: Secure Configuration  
- CIS Control 6: Access Control  
- CIS Control 8: Audit Logs  
- CIS Control 16: Application Security  

---

## 8. Classification

- **Incident Type:** Initial Access → Execution  
- **Severity:** CRITICAL  

---

## 9. NIST Incident Response

- Detection → Wazuh alerts  
- Analysis → Log correlation  
- Containment → IP blocked  
- Eradication → Web shell removed  
- Recovery → Environment validated  

---

## 10. ISO 27001 Alignment

- A.9 — Access Control  
- A.12 — Operations Security  
- A.14 — Secure Development  
- A.16 — Incident Management  

---

## 11. Response Actions

### Containment

- IP 192.168.122.1 blocked (UFW)
- Upload directory restricted  

![Containment](./images/06_response_eradication_validation.png)

---

### Eradication

- `shell.php` removed  
- Directory reviewed  
- Permissions adjusted  

![Eradication](./images/06_response_eradication_validation.png)

---

### Validation

- 404 response confirmed  
- No new `cmd=` activity  
- No new Wazuh alerts  

![Validation](./images/07_webshell_access_blocked_404.png)

---

### Outcome

- Attack contained and eradicated  
- No persistence  
- Environment secure  

---

## 12. Lessons Learned

- Apache logs + Wazuh correlation enabled detection within minutes  
- File upload functionality must enforce strict validation  
- Execution in upload directories should be disabled by default  
- Even low-privilege execution (`www-data`) can result in critical impact  

---

## 13. Indicators of Compromise (IoCs)

| Category | Indicator | Description | MITRE |
|----------|----------|------------|-------|
| Network | 192.168.122.1 | Attacker IP | T1190 |
Application | /DVWA/hackable/uploads/shell.php | Web shell endpoint | T1505.003
| Application | /DVWA/hackable/uploads/shell.php?cmd= | RCE via HTTP request | T1059 |
| Host | shell.php | Malicious file | T1505.003 |
| Host | www-data | Execution context | T1059 |
| Behavior | Repeated HTTP command execution | RCE activity | T1059 |
| Detection | Rule 31514 | Wazuh detection | T1505.003 |

---

## 14. 🧠 Lessons Learned

- Correlation between Apache logs and Wazuh alerts enabled rapid detection of RCE activity  
- Repeated HTTP requests with parameters (`cmd=`) are strong indicators of web shell usage  
- Even limited execution context (`www-data`) can lead to critical impact  
- Lack of upload validation allows direct exploitation of web applications  
- Disabling execution in upload directories is essential to prevent RCE  
- Detection based on behavior (patterns) is more reliable than single alerts

----

## 15. Conclusion

The incident followed the full lifecycle:  
Detection → Investigation → Response

The attacker exploited a web vulnerability to achieve remote command execution.

All malicious activity was contained, the root cause addressed, and the environment validated as secure.
