# Incident Report — SSH Brute Force with Persistence and Log Tampering

---

## 1. Summary

A successful SSH brute-force attack resulted in unauthorized access to the system, followed by persistence establishment, privilege abuse, and partial log tampering.

The incident was detected through Wazuh SIEM correlation and validated using multiple log sources, including `auth.log`, `auditd`, and command history.

---

## 2. Timeline

- T1 — Multiple SSH authentication failures detected (Brute Force)
- T2 — Successful login for user `target` from 192.168.122.1
- T3 — Post-access activity (command execution)
- T4 — Persistence established (user `suporte` + SSH keys)
- T5 — Log tampering performed (`auth.log` cleared)
- T6 — Privileged commands executed via sudo
- T7 — Persistence identified and removed
- T8 — Attacker IP blocked via Fail2Ban
- T9 — Credentials reset and SSH hardened

---

## 3. Detection

**SIEM:** Wazuh  
- Rule: 40112  
- Description: Multiple authentication failures followed by a success  
- Level: 12 (High)

### Evidence

Evidence of brute force activity followed by successful authentication:

![Wazuh Correlation Rule](./images/04_wazuh_bruteforce_success_correlation.png)

- Multiple `Failed password` events identified  
- `Accepted password` confirmed successful access  
- SIEM correlation triggered (Brute Force → Valid Account)

### Detection Gap

- SSH password authentication was enabled  
- No brute-force protection initially configured  
- No alert generated prior to successful compromise  

**Root Cause:**  
Lack of preventive controls allowed brute-force attempts to succeed.

---

## 4. Investigation

### Log Sources

- `/var/log/auth.log`
- `auditd`
- `.bash_history`
- Wazuh SIEM alerts

---

### Post-Compromise Activity

Commands executed after successful login, confirming attacker interaction:

![Bash History](./images/05_bash_history_post_compromise.png)

- Evidence of system interaction after compromise  
- Indicates enumeration and manual actions by the attacker  

---

### Log Tampering

Evidence of log manipulation to evade detection:

![Auth Log Tampered](./images/06_authlog_tampering_evidence.png)

- `auth.log` cleared using truncate  
- Removal of primary authentication evidence  

---

### Privilege Abuse

Auditd logs capturing privileged command execution:

![Auditd Sudo Activity](./images/08_auditd_sudo_activity.png)

- Commands executed with elevated privileges  
- Confirms attacker escalation capabilities  

---

### Persistence

Persistent access established through account manipulation:

![Persistence User](./images/09_persistence_user_suporte.png)

- Malicious user `suporte` created  
- SSH key-based access configured  
- Ensures continued access even after password changes  

---

## 5. Impact Assessment

- **System Access:** Confirmed  
- **Privilege Level:** User access with sudo capability  
- **Persistence:** Confirmed (user + SSH key)  
- **Credential Exposure:** Possible (`/etc/shadow` accessed)  
- **Log Integrity:** Compromised (auth.log tampered)  

**Conclusion:**  
The attacker achieved persistent access and performed actions consistent with privilege abuse and defense evasion, resulting in a full system compromise scenario.

---

## 6. MITRE ATT&CK Mapping

- T1110 — Brute Force  
  https://attack.mitre.org/techniques/T1110/

- T1078 — Valid Accounts  
  https://attack.mitre.org/techniques/T1078/

- T1098 — Account Manipulation  
  https://attack.mitre.org/techniques/T1098/

- T1070 — Indicator Removal  
  https://attack.mitre.org/techniques/T1070/

---

## 7. Classification

- **Incident Type:** Unauthorized Access + Persistence  
- **Severity:** CRITICAL  

**Impact Summary:**
- Unauthorized system access  
- Persistence established  
- Log integrity compromised  
- Privileged command execution  
- Potential credential exposure  

---

## 8. Response Actions

### Containment

Fail2Ban actively blocking attacker IP:

![Fail2Ban Blocking Attacker](./images/12_fail2ban_defense_validation.png)

- Immediate interruption of attacker access  
- Prevention of further brute-force attempts  

---

### Eradication

- Malicious user `suporte` removed  
- Unauthorized SSH access paths eliminated  

---

### Recovery

- Password reset for user `target`  
- SSH configuration hardened:
  - PasswordAuthentication disabled  
  - Root login disabled  

---

### Rationale

Response actions focused on:

- Immediate containment of attacker access  
- Complete removal of persistence mechanisms  
- Prevention of re-compromise through credential rotation and hardening  

---

## 9. Defense Validation

### Before

- SSH password authentication enabled  
- No brute-force protection  
- High exposure to credential-based attacks  

### After

- Fail2Ban actively blocking malicious IPs  
- SSH restricted to key-based authentication  
- Reduced attack surface and improved access control  

---

## 10. Lessons Learned

- Single log source is insufficient (auth.log was tampered)  
- auditd provided critical forensic visibility  
- Defense-in-depth is essential  
- Brute-force attacks can escalate quickly to full compromise  
- Preventive controls are as important as detection  

---

## 11. Conclusion

This incident demonstrated a complete attack lifecycle, including:

- Initial access (Brute Force)  
- Persistence  
- Privilege abuse  
- Defense evasion  

Despite log tampering, multi-source correlation enabled full incident reconstruction.

Security controls implemented post-incident significantly improved system resilience.

---

## 12. Indicators of Compromise (IoCs)

### Network
- Source IP: 192.168.122.1

### Accounts
- Target User: target
- Malicious User Created: suporte

### Authentication
- Pattern: Multiple SSH authentication failures followed by success
- Log Source: /var/log/auth.log

### Persistence
- SSH Authorized Keys added for user suporte
- Path: /home/suporte/.ssh/authorized_keys

### Privilege Abuse
- Command Execution via sudo
- Log Source: auditd (EXECVE events)

### Log Tampering
- File: /var/log/auth.log
- Action: Truncated (log cleared)
