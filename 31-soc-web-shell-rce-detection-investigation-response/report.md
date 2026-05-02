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
- Description: Suspicious execution via `shell.php`  
- Level: High  

### Evidence

![Wazuh Detection](./images/03_wazuh_webshell_detection.png)

- Multiple HTTP requests with `cmd=` parameter  
- Repeated command execution pattern  
- MITRE mapping automatically applied  

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

---

## 4. Investigation

### Log Sources

- `/var/log/apache2/access.log`
- Wazuh SIEM alerts  

---

### Analyst Hypothesis

The attacker exploited a vulnerable file upload feature to deploy a web shell and executed system commands via HTTP requests, enabling remote control of the server.

---

### Command Execution Evidence

![Commands](./images/05_extracted_commands_webshell.png)

Commands observed:

- id  
- whoami  
- uname -a  
- pwd  
- ls  
- cat /etc/passwd  

---

### Access Log Evidence

![Access Log](./images/02_apache_access_log_webshell.png)

- Repeated requests to `/shell.php?cmd=`  
- Same source IP  
- Short interval execution  

---

### Execution Context

- User: `www-data`  
- Confirms execution within web server context  

---

## 5. Impact Assessment

- **Access Level:** Remote command execution via web application  
- **Privilege Level:** www-data (web server context)  
- **Scope:** Single host (192.168.122.171)  
- **Persistence:** Not observed  
- **Log Integrity:** Intact  
- **Data Exposure:** Possible access to `/etc/passwd`

### 🔴 Severity: CRITICAL

**Justification:**
- Remote command execution confirmed  
- Arbitrary command execution possible  
- Access to sensitive system files  

→ Indicates compromise of web application layer with potential escalation risk.

---

## 6. MITRE ATT&CK Mapping

- T1190 — Exploit Public-Facing Application  
- T1505.003 — Web Shell  
- T1059 — Command Execution  

---

## 7. CIS Controls Mapping

- CIS Control 4: Secure Configuration  
- CIS Control 6: Access Control Management  
- CIS Control 8: Audit Log Management  
- CIS Control 16: Application Software Security  

---

## 8. Classification

- **Incident Type:** Initial Access → Execution  
- **Severity:** CRITICAL  

**Impact Summary:**
- Remote command execution  
- Exposure of system-level data  
- Application compromise  

---

## 9. NIST Incident Response Mapping (SP 800-61)

- **Preparation:** Logging enabled (Apache, Wazuh)  
- **Detection & Analysis:** Web shell activity identified via SIEM and logs  
- **Containment:** Attacker IP blocked  
- **Eradication:** Web shell removed  
- **Recovery:** Upload control reviewed, environment validated  

---

## 10. ISO 27001 Alignment

- A.9 — Access Control  
- A.12 — Operations Security  
- A.14 — System Acquisition, Development and Maintenance  
- A.16 — Incident Management  

---

## 11. Response Actions

### Containment

![Response](./images/06_response_eradication_validation.png)

- Attacker IP blocked  
- Upload directory access restricted  

---

### Eradication

- Malicious file `shell.php` removed  
- Upload directory inspected  
- Execution permissions reviewed  

---

### Recovery

- Web shell no longer accessible (404):

![Validation](./images/07_webshell_access_blocked_404.png)

---

### Defense Validation

- No further command execution observed  
- No new malicious files detected  
- Environment stabilized  

---

## 12. Lessons Learned

- Web applications are critical attack vectors  
- File upload must be strictly validated  
- Application logs are essential for detection  
- SIEM correlation improves visibility  
- Even low privilege (www-data) can lead to impact  

---

## 13. Indicators of Compromise (IoCs)

| Category       | Indicator | Description |
|---------------|----------|-------------|
| 🌐 Network     | 192.168.122.1 | Attacker source IP |
| 🌐 Network     | HTTP requests to `/DVWA/hackable/uploads/shell.php` | Malicious web shell access |
| 🌍 Application | `/DVWA/hackable/uploads/shell.php` | Web shell endpoint |
| 🌍 Application | Parameter `cmd=` | Command execution via HTTP |
| 🖥️ Host        | `/var/www/html/DVWA/hackable/uploads/shell.php` | Malicious file on server |
| 🖥️ Host        | User: www-data | Execution context (web server user) |
| 🖥️ Host        | SHA256: <HASH_DO_ARQUIVO> | Hash of malicious web shell |
| ⚙️ Behavior    | Repeated HTTP requests | Interactive command execution pattern |
| ⚙️ Behavior    | Commands: id, whoami, uname, pwd, ls | System enumeration |
| ⚙️ Behavior    | Access to `/etc/passwd` | Sensitive file access |
| 📡 Detection   | Wazuh Rule ID: 31514 | Web shell command execution detection |
| 📡 Detection   | Log source: Apache access.log | Evidence of HTTP-based execution |

---

## 14. Conclusion

The incident followed the complete lifecycle:  
Detection → Investigation → Classification → Response

The attacker successfully exploited a web application vulnerability to achieve remote command execution.

Although no persistence was established, the impact was significant due to the ability to execute arbitrary commands.

All malicious artifacts were removed, defensive measures were implemented, and the environment was validated as secure.
