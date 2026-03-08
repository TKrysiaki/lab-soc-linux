# Lab 05 — SSH Hardening

## Objective
Apply basic security hardening to the SSH service to reduce brute-force attack surface.

---

## Environment

Attacker: Kali Linux  
Target: Ubuntu Server  
Service: SSH (22)

---

## Modify SSH Configuration

```bash
sudo nano /etc/ssh/sshd_config
```

Security changes applied:

```
LoginGraceTime 30
PermitRootLogin no
MaxAuthTries 3
```

<img src="images/lab05/ssh-hardening-main.png" width="100%">

---

## Disable Password Authentication

```
PasswordAuthentication no
```

<img src="images/lab05/ssh-disable-password.png" width="100%">

---

## Restart SSH Service

```bash
sudo systemctl restart ssh
```

<img src="images/lab05/ssh-restart-service.png" width="100%">

---

## Test SSH Access

```bash
ssh tiago@192.168.56.105
```

Result:

```
Permission denied (publickey)
```

<img src="images/lab05/ssh-password-blocked.png" width="100%">

---

## Security Outcome

- Root login disabled  
- Password authentication disabled  
- Authentication attempts limited  
- Login grace time reduced  

These changes reduce the attack surface and mitigate brute-force attempts against SSH.
