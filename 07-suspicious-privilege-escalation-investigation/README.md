# Suspicious Privilege Escalation Investigation

## Objective
Investigate potential privilege escalation activity on a Linux system by analyzing authentication logs.

## Environment
- Ubuntu (target machine)
- Kali Linux (analyst machine)
- VirtualBox

## Log File Investigated
```bash
/var/log/auth.log
```

## Investigation Steps
### 1. Access log directory

Command:
```bash
cd /var/log
ls
```
<img src="images/01-access-log-directory.png" width="800">

---
## 2. Review authentication logs

Command:
```bash
cat auth.log
```
<img src="images/lab07-02-auth-log-content" width="800">

---
## 3. Identify root sessions

Command:
```bash
grep root auth.log
```
<img src="images/lab07-03-root-session-events" width="800">
---
## 4. Check failed authentication attempts

Command:
```bash
grep "failed" auth.log
```
<img src="images/lab07-04-failed-authentication-check" width="800">

---
## 5. Investigate sudo command history

Command:
```bash
zgrep sudo auth.log.2.gz
```
<img src="images/lab07-05-sudo-commands-history" width="800">
---
## 6. Review login history

Command:
```bash
last
```
<img src="images/lab07/lab07-06-login-history-last-command" width="800">
---
# Findings

- Root sessions were identified in the authentication logs.

- Sudo commands executed by user tiago were recorded.

- No failed authentication attempts were detected.

- Login history confirmed normal system usage.

---

# Conclusion

## The investigation identified legitimate administrative activity using sudo and scheduled cron jobs running as root, with no evidence of unauthorized privilege escalation.






















