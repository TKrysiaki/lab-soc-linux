# 🚨 Detection and Response to SSH Brute Force Attack (Wazuh + Fail2ban + Auth.log)

---

## 📌 Overview

This lab simulates an SSH brute-force attack against a Linux server, validates telemetry collection through Wazuh, investigates authentication logs, identifies indicators of compromise (IOCs), and performs active containment using Fail2ban.

- **Access:** Successful detection
- **Attack Type:** SSH Brute Force (Hydra)
- **Severity:** 🔴 High (7/10)

---

## 🖥️ Environment

- **Attacker:** `192.168.122.1` (`tiago@lab-soc`)
- **Target:** `WEB-01` (`192.168.122.171`)
- **Operating System:** Ubuntu Linux
- **SIEM:** Wazuh
- **Mitigation:** Fail2ban + UFW

---

## 🎯 Attack Scenario

An SSH brute-force attack was simulated using Hydra and the `rockyou.txt` wordlist against the username `tiago` on the target host `WEB-01`.

---

## 🚀 Attack Execution

![Hydra Execution](./images/01-hydra.png)

Hydra initiated an SSH brute-force password attack against `192.168.122.171` using the user `tiago` and `rockyou.txt`.

---

## 🔍 Detection

![Wazuh Dashboard](./images/02-wazuh.png)

Wazuh dashboard captured authentication failures and categorized the activity under Credential Access (MITRE ATT&CK T1110).

---

## 📊 Alert Investigation

![Alert Details](./images/03-detalhes.png)

Detailed view of the Wazuh alert showing Rule ID `2502` triggered by multiple failed password attempts from the attacker IP.

---

## 📜 Log Analysis

### Failed Authentication Attempts
![Failed Logins](./images/04-grep.png)
Filtering authentication logs exposed continuous failed password attempts from IP `192.168.122.1` targeting the user `tiago`.

### Real-Time Monitoring
![Tail Auth Log](./images/05-detected.png)
Monitoring `/var/log/auth.log` in real time using `tail -f`.

### Attempt Count
![Attempts Count](./images/06-tentativas.png)
A total of **136 failed authentication attempts** were identified using `grep` combined with `wc -l`.

### Session Review
![Session Opened](./images/07-sessionopened.png)
Review of established sessions and system access logs.

### Administrative Commands
![Command History](./images/08-command.png)
Auditing administrative command executions via `sudo` and `pkexec`.

### Process Execution
![Execve Logs](./images/09-execve.png)
Reviewing kernel-level process execution logs (`EXECVE`) for system activity validation.

---

## 🔎 IOCs

| IOC | Value |
|------|-------|
| Source IP | 192.168.122.1 |
| Target Host | WEB-01 |
| Target User | tiago |
| Attack Tool | Hydra |
| Log Source | /var/log/auth.log |
| Wazuh Rule | 2502 |

---

## ⏱️ Timeline

1. Hydra launched the SSH brute-force attack.
2. SSH generated failed authentication events.
3. Wazuh collected and correlated the logs.
4. Rule 2502 was triggered.
5. Authentication logs were investigated.
6. Failed attempts were quantified.
7. Fail2ban banned the attacker's IP.
8. Monitoring confirmed successful containment.

---

## 🧬 MITRE ATT&CK

| Technique ID | Technique | Description |
|--------------|-----------|-------------|
| T1110 | Brute Force | SSH password guessing using Hydra |

---

## 🛡️ Containment

![Fail2ban Action](./images/10-fail2ban.png)

- Fail2ban service started and verified as active.
- Manual ban triggered for attacker IP `192.168.122.1` via `fail2ban-client`.
- Log verification confirmed successful IP block actions.

---

## 📚 Lessons Learned

- Validate SIEM alerts using raw system logs.
- Correlate multiple log sources before escalation.
- Identify attacker infrastructure through IOCs.
- Apply rapid containment using Fail2ban.
- Continue monitoring after mitigation.

---

## 🧰 Skills Demonstrated

- SIEM Monitoring
- Alert Triage
- Linux Log Analysis
- SSH Security
- IOC Identification
- Incident Investigation
- MITRE ATT&CK Mapping
- Fail2ban Administration
- Initial Incident Response

---

## 🎯 Conclusion

This lab reproduces a realistic SOC Level 1 investigation involving an SSH brute-force attack against a Linux server. The attack was detected through Wazuh telemetry, validated using authentication logs, investigated through log analysis, and successfully contained using Fail2ban. The exercise demonstrates practical experience in alert triage, IOC identification, evidence collection, MITRE ATT&CK mapping, and initial incident response.
