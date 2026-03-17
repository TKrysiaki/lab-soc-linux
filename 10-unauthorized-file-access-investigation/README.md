# 🔍 Lab 10 - Unauthorized File Access Investigation

## 🎯 Objective
Detect and investigate unauthorized access attempts to sensitive files in a Linux environment using log analysis.

---

## 🖥️ Lab Environment

- Attacker: Kali Linux
- Target: Ubuntu Server
- Access method: SSH
- Logs analyzed: /var/log/auth.log

---

## ⚔️ Attack Simulation

SSH access from attacker machine:

```
ssh tiago@192.168.18.237
```
<img src="images/01-ssh-login-success.png" width="100%">

---


Attempt to access sensitive file:
```
cat /etc/shadow
```

<img src="images/02-shadow-access-attempt.png" width="100%">

---

## 🔎 Investigation

### Search for sensitive file access:
```
grep shadow /var/log/auth.log
```
<img src="images/03-shadow-log-evidence.png" width="100%">

---

### Search for privilege escalation:
```
grep sudo /var/log/auth.log
```

<img src="images/04-sudo-commands-log.png" width="100%"> 

---

### Build attack timeline:
```
grep "2026-03-17" /var/log/auth.log
```
<img src="images/05-attack-timeline.png" width="100%">

---

## 🕒 Timeline of Events

- Successful SSH login detected
- Session opened for user
- Execution of sudo commands
- Suspicious command executed:
  - apt install shadow

 ---

## 🚨 Findings

- Attempt to access sensitive file (/etc/shadow)
- Use of privilege escalation via sudo
- Execution of commands related to authentication system
- Indicators of suspicious activity

---

## 🧠 Conclusion

### Suspicious activity was identified following SSH access, including privilege escalation and interaction with sensitive system components.
The behavior suggests a potential attempt to access or manipulate authentication data.

---

## 🛡️ Mitigation

- Restrict sudo privileges
- Monitor access to sensitive files
- Implement alerting for suspicious commands
- Review authentication logs regularly

---

## 🧰 Skills Demonstrated

- Linux Log Analysis
- SSH Investigation
- Privilege Escalation Detection
- Threat Analysis
- Timeline Correlation



























