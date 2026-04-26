# Incident Report — SSH Brute Force with Persistence and Log Tampering

---

## 1. Summary

A successful SSH brute-force attack resulted in unauthorized access to the system, followed by persistence establishment, privilege abuse, and partial log tampering.

The incident was detected through Wazuh SIEM correlation and validated using multiple log sources, including `auth.log`, `auditd`, and command history.

---

## 2. Timeline (UTC)

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

- Repeated `Failed password` events
- `Accepted password` indicating successful login
- Correlation alert triggered in SIEM

### Detection Gap

- SSH password authentication was enabled
- No brute-force prevention mechanism initially in place
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

### Key Findings

- Brute-force activity originating from `192.168.122.1`
- Successful authentication to user `target`
- Post-compromise command execution confirmed

### Persistence

- Malicious user `suporte` created
- SSH key-based access configured

### Log Tampering

- `auth.log` cleared using truncate command
- Loss of primary authentication logs

### Privilege Abuse

- Execution of privileged commands via `sudo`
- Confirmed through `auditd` logs

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
- **Impact:**
  - Unauthorized system access
  - Persistence established
  - Log integrity compromised
  - Privileged command execution
  - Potential credential exposure

---

## 8. Response Actions

### Containment

- Attacker IP blocked via Fail2Ban to prevent further access attempts

### Eradication

- Malicious user `suporte` removed to eliminate persistence
- SSH keys reviewed and unauthorized access paths removed

### Recovery

- Password for user `target` reset to invalidate compromised credentials
- SSH configuration hardened:
  - PasswordAuthentication disabled
  - Root login disabled

### Rationale

Response actions focused on:
- Immediate containment of attacker access
- Complete removal of persistence mechanisms
- Prevention of re-compromise through credential rotation and hardening

---

## 9. Defense Validation

### Before

- SSH password authentication enabled
- No brute-force protection mechanism
- High exposure to credential-based attacks

### After

- Fail2Ban actively blocking malicious IPs
- SSH restricted to key-based authentication
- Reduced attack surface and improved access control

---

## 10. Lessons Learned

- Single log source is insufficient for investigation (auth.log was tampered)
- auditd provided critical forensic visibility
- Defense-in-depth is essential for resilience
- Brute-force attacks can escalate quickly to full compromise
- Preventive controls are as important as detection

---

## 11. Conclusion

This incident demonstrated a complete attack lifecycle, including initial access, persistence, privilege abuse, and defense evasion.

Despite log tampering, the use of multiple data sources enabled successful investigation and response.

The implemented security controls significantly improved the system’s resilience against similar attacks.
