
# Incident Report — SSH Brute Force with Persistence and Log Tampering

## 1. Summary
A successful SSH brute-force attack was identified, resulting in unauthorized access, persistence establishment, privilege abuse, and partial log tampering. The incident was detected via Wazuh SIEM and validated through multi-source log correlation.

---

## 2. Timeline (UTC)

- T1 — Multiple SSH authentication failures detected (Brute Force)
- T2 — Successful login for user `target` from 192.168.122.1
- T3 — Post-access activity (command execution)
- T4 — Persistence established (user `suporte` + SSH keys)
- T5 — Log tampering (`auth.log` cleared)
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

**Evidence:**
- Repeated `Failed password`
- `Accepted password` event
- Correlation alert triggered

---

## 4. Investigation

### Log Sources
- `/var/log/auth.log`
- `auditd`
- `.bash_history`
- Wazuh alerts

### Key Findings

- Brute force activity from 192.168.122.1
- Successful login to `target`
- Execution of commands including:
  - system enumeration
  - log manipulation
  - persistence setup

- Persistence:
  - User `suporte` created
  - SSH key-based access configured

- Log Tampering:
  - `auth.log` cleared via truncate

- Privilege Abuse:
  - sudo execution confirmed via auditd

---

## 5. MITRE ATT&CK Mapping

- T1110 — Brute Force  
  https://attack.mitre.org/techniques/T1110/

- T1078 — Valid Accounts  
  https://attack.mitre.org/techniques/T1078/

- T1098 — Account Manipulation  
  https://attack.mitre.org/techniques/T1098/

- T1070 — Indicator Removal  
  https://attack.mitre.org/techniques/T1070/

---

## 6. Classification

- **Incident Type:** Unauthorized Access + Persistence  
- **Severity:** CRITICAL  
- **Impact:**
  - System access obtained
  - Persistence established
  - Logs partially destroyed
  - Privileged command execution
  - Potential credential exposure

---

## 7. Response

### Containment
- Malicious IP blocked via Fail2Ban

### Eradication
- User `suporte` removed
- SSH backdoor eliminated

### Recovery
- Password reset for `target`
- SSH hardened:
  - PasswordAuthentication disabled
  - Root login disabled

---

## 8. Defense Validation

### Before
- SSH password authentication enabled
- No brute-force protection

### After
- Fail2Ban active and blocking attacker IP
- SSH restricted to key-based authentication
- Attack vector mitigated

---

## 9. Lessons Learned

- Single log source is insufficient (auth.log was tampered)
- auditd provided critical forensic visibility
- Defense-in-depth is essential
- Brute force can quickly escalate to full compromise

---

## 10. Conclusion

The incident demonstrated a full attack lifecycle from initial access to persistence and defense evasion. Effective detection, investigation, and response actions successfully contained and remediated the threat while improving the system’s security posture.
