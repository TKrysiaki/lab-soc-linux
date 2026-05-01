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

![Wazuh Correlation Rule](./images/04_wazuh_bruteforce_success_correlation.png)

- Multiple `Failed password` events identified  
- `Accepted password` confirmed successful access  
- SIEM correlation triggered (Brute Force → Valid Account)

---

### Detection Gap

- SSH password authentication was enabled  
- No brute-force protection initially configured  
- No alert generated prior to successful compromise  

**Root Cause:**  
Lack of preventive controls allowed brute-force attempts to succeed.

---

### Recommendations

- Enforce SSH key-based authentication only  
- Implement Fail2Ban or equivalent protection  
- Configure alerting on authentication anomalies  

---

## 4. Investigation

### Log Sources

- `/var/log/auth.log`
- `auditd`
- `.bash_history`
- Wazuh SIEM alerts

---

### Analyst Hypothesis

The attacker performed a brute force attack to gain initial access, established persistence through account creation and SSH keys, escalated privileges via sudo, and attempted to evade detection by tampering with authentication logs.

---

### Post-Compromise Activity

![Bash History](./images/05_bash_history_post_compromise.png)

- Evidence of system interaction after compromise  
- Indicates manual attacker activity and system exploration  

---

### Log Tampering

![Auth Log Tampered](./images/06_authlog_tampering_evidence.png)

- `auth.log` truncated  
- Attempt to remove authentication traces  

---

### Privilege Abuse

![Auditd Sudo Activity](./images/08_auditd_sudo_activity.png)

- Execution of privileged commands detected  
- Confirms elevated attacker capabilities  

---

### Persistence

![Persistence User](./images/09_persistence_user_suporte.png)

- Malicious user `suporte` created  
- SSH key-based access configured  
- Ensures continued access even after credential reset  

---

## 5. Impact Assessment

- **Access Level:** Unauthorized SSH access with valid credentials  
- **Privilege Level:** Sudo-level command execution (potential root)  
- **Scope:** Single host (192.168.122.102)  
- **Persistence:** Confirmed (user + SSH key)  
- **Log Integrity:** Compromised  
- **Credential Exposure:** Possible (`/etc/shadow` access)

### 🔴 Severity: CRITICAL

**Justification:**
- Successful initial access  
- Persistence established  
- Privileged command execution  
- Evidence of defense evasion  

→ Indicates full system compromise with risk of attacker re-entry.

---

## 6. MITRE ATT&CK Mapping

- T1110 — Brute Force  
- T1078 — Valid Accounts  
- T1098 — Account Manipulation  
- T1070 — Indicator Removal  

---

## 7. CIS Controls Mapping

- CIS Control 5: Account Management  
- CIS Control 6: Access Control Management  
- CIS Control 8: Audit Log Management  
- CIS Control 10: System Hardening  

---

## 8. Classification

- **Incident Type:** Credential Access → Initial Access → Persistence → Defense Evasion  
- **Severity:** CRITICAL  

**Impact Summary:**
- Unauthorized system access  
- Persistence established  
- Log integrity compromised  
- Privileged command execution  
- Potential credential exposure  

---

## 9. NIST Incident Response Mapping (SP 800-61)

- **Preparation:** Logging enabled (auth.log, auditd), SIEM monitoring active  
- **Detection & Analysis:** Brute force identified and correlated with successful login  
- **Containment:** Attacker IP blocked via Fail2Ban  
- **Eradication:** Malicious user and SSH keys removed  
- **Recovery:** Credentials reset, SSH hardened, environment validated  

---

## 10. ISO 27001 Alignment

- A.9 — Access Control  
- A.12 — Operations Security  
- A.16 — Incident Management  
- A.18 — Compliance  

---

## 11. Response Actions

### Containment

![Fail2Ban Blocking Attacker](./images/12_fail2ban_defense_validation.png)

- Attacker IP blocked  
- Active sessions interrupted  

---

### Eradication

- Malicious user `suporte` removed  
- Unauthorized SSH access paths eliminated  

---

### Recovery

- Password reset for user `target`  
- SSH hardened:
  - PasswordAuthentication disabled  
  - Root login disabled  

---

### Defense Validation

- Brute-force attempts successfully blocked  
- No unauthorized access observed after containment  
- Persistence not re-established  
- System integrity restored  

---

## 12. Lessons Learned

- Single log source is insufficient  
- auditd provided critical forensic visibility  
- Defense-in-depth is essential  
- Brute-force attacks can escalate rapidly  
- Preventive controls are critical  

---

## 13. Indicators of Compromise (IoCs)

### Network
- Source IP: 192.168.122.1  
- Destination Port: 22 (SSH)  

### Timeline Indicators
- High volume of failed SSH attempts within short interval  
- Successful login immediately after brute force  

### Accounts
- Compromised user: target  
- Malicious user: suporte  

### Authentication
- Pattern: Multiple failures followed by success  
- Log Source: `/var/log/auth.log`  

### Persistence
- SSH key added for user suporte  
- Path: `/home/suporte/.ssh/authorized_keys`  

### Privilege Abuse
- Sudo command execution  
- Log Source: auditd (EXECVE)  

### Log Tampering
- File: `/var/log/auth.log`  
- Action: Truncated  

### Detection Context
- SIEM: Wazuh  
- Rule ID: 40112  

---

## 14. Conclusion

The incident followed the complete lifecycle:
Detection → Investigation → Classification → Response

The attacker achieved initial access, established persistence, escalated privileges, and attempted to evade detection.

Despite log tampering, multi-source log correlation enabled full incident reconstruction.

All malicious artifacts were removed, defensive controls were implemented and validated, and system integrity was restored.
