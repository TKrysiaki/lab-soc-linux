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

Attempt to access sensitive file:
```
cat /etc/shadow
```
## 📸 Evidence
<img src="images/lab10/01-ssh-login-success.png" width="100%">
<img src="images/lab10/02-shadow-access-attempt.png" width="100%">

---

## 🔎 Investigation

### Search for sensitive file access:
```
grep shadow /var/log/auth.log
```
Search for privilege escalation:
```
grep sudo /var/log/auth.log
```
Build attack timeline:
```
grep "2026-03-17" /var/log/auth.log
```
## 📸 Evidence
<img src="images/lab10/03-shadow-log-evidence.png" width="100%">
<img src="images/lab10/04-sudo-commands-log.png" width="100%"> 
<img src="images/lab10/05-attack-timeline.png" width="100%">

---

## 🕒 Timeline of Events

- Successful SSH login detected
- Session opened for user
- Execution of sudo commands
- Suspicious command executed:
  - apt install shadow









