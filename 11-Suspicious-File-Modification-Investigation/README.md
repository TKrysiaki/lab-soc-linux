# Lab 11 - Suspicious File Modification Investigation

## 🎯 Objective
Detect and analyze unauthorized modification of a critical system file (`/etc/passwd`) using Wazuh SIEM.

---

## 🖥️ Lab Environment

- Attacker: Kali Linux
- Target: Ubuntu 24.04 (Wazuh Agent)
- SIEM: Wazuh Server

---

## 🚨 Attack Simulation

### Command (run on Ubuntu target)
```
echo 'hacker:x:0:0::/root:/bin/bash' | sudo tee -a /etc/passwd
```
### Explanation

- > **echo** → creates a fake user entry

- > **tee -a** → appends content to a file

- > **/etc/passwd** → critical system file (user accounts)

### Analysis

- Direct modification of /etc/passwd is highly suspicious

- Can allow privilege escalation (UID 0 = root)

### Screenshot
<img src="images/01-passwd-backdoor-created.png" width="100%">

---

## 🔍 Local Verification
### Command
```
tail -n 5 /etc/passwd
```
### Explanation

- > **tail -n 5** → shows last 5 lines of the file

- > Used to confirm if malicious entry was added

### Analysis

- Confirms persistence attempt

- Attacker created a new root-level account

### Screenshot
<img src="images/02-passwd-backdoor-confirmed.png" width="100%">

---

## 📊 File Metadata Analysis
### Command
```
stat /etc/passwd
```
### Explanation

- > **stat** → displays file metadata

- > Shows last modification time (mtime)

### Analysis

- Confirms exact moment of change

- Useful for timeline correlation in SOC

Screenshot
<img src="images/03-passwd-malicious-user-detected" width="100%">

---

## 🧠 SIEM Detection (Wazuh)
### Step: Access Wazuh Dashboard

### Analysis

- > Agent must be active to collect logs

- > Confirms telemetry pipeline is working

### Screenshot
<img src="images/04-passwd-file-metadata" width="100%">

---

## 📈 Event Overview
### Step: Filter events in Wazuh

### Analysis

- > Shows multiple security events

- > Includes:

-  > sudo usage

-  > file modifications

### Screenshot
<img src="images/05-current-system-time" width="100%">

---

## 🚨 File Integrity Monitoring Alert

### Detection

- > Rule: Integrity checksum changed

- > Technique: T1565.001 (Data Manipulation)

### Analysis

- > Wazuh detected:

-  > File modification

- > Real-time monitoring trigger

### Screenshot
<img src="images/06-wazuh-fim-detection.png" width="100%">

---

## 🔬 Event Deep Analysis

### Investigation

- > File: /etc/passwd

- > Change detected:

-  > Size

-  > MD5

-  > SHA1

-  > SHA256

### Analysis

- > Confirms tampering of critical file

- > High confidence malicious activity

### Screenshot
<img src="images/07-wazuh-event-details.png" width="100%">

---

## 🧭 Timeline

- > Attacker executes sudo command

- > File /etc/passwd modified

- > Wazuh detects integrity change

- > Alert generated in SIEM

---

## 🧠 MITRE ATT&CK Mapping

- > T1548.003 → Abuse Elevation Control Mechanism (sudo)

- > T1565.001 → Data Manipulation

---

## 🛡️ Mitigation

- > Restrict sudo access

- > Monitor critical files (FIM)

- > Enable alerting for system file changes

- > Implement least privilege

---

## 🧰 Skills Developed

- > Log analysis

- > File integrity monitoring (FIM)

- > Threat detection with SIEM

- > MITRE ATT&CK mapping

- > Incident investigation










































