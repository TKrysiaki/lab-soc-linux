# Lab 06 — SSH Brute Force Incident Investigation

## Scenario

A SOC alert indicates possible brute force activity against an SSH service.

The objective is to investigate authentication logs and identify the attacker.

---

## Target Identification

Ubuntu server IP:

```
192.168.56.105
```

<img src="images/lab06/target-ip.png" width="100%">

---

## Verify SSH Service

```
sudo systemctl status ssh
```

<img src="images/lab06/ssh-service-running.png" width="100%">

---

## Attack Simulation

Brute force attack executed from Kali Linux using Hydra.

```
hydra -l tiago -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.105
```

<img src="images/lab06/hydra-attack-start.png" width="100%">

---

## Log Investigation

Monitoring SSH authentication logs.

```
sudo tail -f /var/log/auth.log
```

Detected multiple failed authentication attempts.

<img src="images/lab06/auth-log-bruteforce.png" width="100%">

---

## Count Failed Attempts

```
sudo grep -a "Failed password" /var/log/auth.log | wc -l
```

Result:

```
79 failed login attempts detected
```

<img src="images/lab06/failed-password-count.png" width="100%">

---

## Identify Attacker IP

```
sudo grep -a "Failed password" /var/log/auth.log | grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' | sort | uniq -c
```

Result:

```
99 192.168.56.104
```

<img src="images/lab06/attacker-ip-analysis.png" width="100%">

---

## Mitigation

Blocking attacker IP using UFW.

```
sudo ufw deny from 192.168.56.104
```

<img src="images/lab06/ip-blocked-ufw.png" width="100%">

---

## Incident Summary

Type: SSH Brute Force Attack  
Target user: tiago  
Attacker IP: 192.168.56.104  
Attempts detected: 99  
Log source: /var/log/auth.log

---

## Conclusion

The attack was detected through log analysis and mitigated by blocking the attacker IP using the firewall.
