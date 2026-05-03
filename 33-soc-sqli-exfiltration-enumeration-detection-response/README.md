# 🚨 Detection and Response to SQL Injection (Wazuh + Apache + DVWA)

---

## 📌 Overview

Simulation of a **SQL Injection attack**, resulting in **data exposure and database enumeration**.

- Access: ✔  
- Execution: ✔  
- Persistence: ❌  
- Evasion: ✔  
- Severity: 🔴 9/10 (High)  

---

## 📄 Detailed Incident Report

➡️ Full report: [report.md](./report.md)

---

## 🖥️ Environment

- Attacker: 192.168.122.1  
- Target: 192.168.122.171  
- SIEM: Wazuh  
- Service: Apache (DVWA)  

---

## 🎯 Attack Scenario

An attacker exploited a vulnerable parameter (`id`) in the DVWA application via SQL Injection.  
The attack evolved from authentication bypass (`OR 1=1`) to **data extraction** and **database enumeration** using `UNION SELECT`.

---

## 🔍 Detection

Detection occurred via Wazuh rules analyzing Apache access logs.

![Detection](./images/01_wazuh_sqli_detection_burst.png)

- Rule ID: 31103 / 31106  
- Detection logic: SQL keywords (`UNION`, `OR 1=1`) in HTTP requests  

---

## 🧠 Investigation

Detailed analysis of the alert revealed malicious payloads targeting the SQL parameter.

![Evidence](./images/02_wazuh_sqli_event_details.png)

### Evidence:

- Source IP: `192.168.122.1`  
- Endpoint: `/DVWA/vulnerabilities/sqli`  
- Parameter: `id=`  
- Behavior: SQL Injection with payload escalation  

---

## 🕒 Timeline

Attack duration and progression:

![Start](./images/03_wazuh_sqli_timeline_start .png)  

![End](./images/04_wazuh_sqli_timeline_end.png)

- Start: 03/05/2026 17:28:19  
- End: 03/05/2026 17:52:58  
- Duration: ~24 minutes  

---

## 🔎 Command / Activity Evidence

Clear evidence of SQL Injection exploitation and data extraction:

![Commands](./images/07_exploitation_evidence_access_log.png)

- `UNION SELECT user,password FROM users`  
- `UNION SELECT table_name FROM information_schema.tables`  
- `UNION SELECT column_name FROM information_schema.columns`  

---

## 🔎 Indicators of Compromise (IoCs)

| Category | Indicator | Description | MITRE |
|----------|----------|------------|-------|
| Network | 192.168.122.1 | Attacker IP | T1190 |
| Application | /DVWA/vulnerabilities/sqli | Attack vector | T1190 |
| Application | id= | Injection parameter | T1190 |
| Behavior | UNION SELECT | Data extraction | T1190 |
| Behavior | information_schema | DB enumeration | T1190 |
| Detection | Rule 31103 | SQLi detection | T1190 |

---

## ⚠️ Impact Assessment

- **Access Level:** Application  
- **Privilege Level:** Database read  
- **Scope:** DVWA application  
- **Exposure:** Credentials + database structure  

### Severity: 🔴 9/10 (High)

**Justification:**
- Successful SQL Injection execution  
- Sensitive data exposure (users table)  
- Full database enumeration  
- Adaptive attacker behavior  

→ **Database compromise confirmed (logical level)**

---

## 🧠 Post-Exploitation

Database enumeration confirmed attacker exploration of schema:

![Post Exploitation](./images/08_post_exploitation_db_enumeration.png)

- Tables discovered  
- Columns identified  
- Expansion of attack scope  

---

## 🛡️ Response

### Containment

Firewall rule applied to block attacker IP:

![Containment](./images/09_iptables_rule_validation.png)

- `iptables DROP 192.168.122.1`  

---

### Recovery / Validation

Access attempts blocked successfully:

![Validation](./images/06_defense_validation_connection_timeout.png)

- Connection timeout observed  
- No further interaction possible  

---

### 🔐 Hardening (Lab Context)

- Temporary IP blocking  
- Continued monitoring via Wazuh  
- No permanent changes (lab repeatability maintained)  

---

### ✅ Defense Validation

- Attack blocked ✔  
- No recurrence ✔  
- Environment stable ✔  

---

## 🧬 MITRE ATT&CK

| Technique ID | Technique Name | Description |
|-------------|--------------|------------|
| T1190 | Exploit Public-Facing Application | SQL Injection |
| T1055 | Process Injection (rule mapping) | Detection context |

---

## 🎯 Conclusion

Detection → Investigation → Classification → Response  

A SQL Injection attack was successfully detected using Wazuh and Apache logs.  
The attacker achieved data extraction and database enumeration, confirming impact.  
Containment was applied via firewall rules, and effectiveness was validated through connection blocking.

---

## 🧠 Skills Developed

- Log analysis (Apache access.log)  
- SIEM detection (Wazuh)  
- Threat investigation (timeline + payload analysis)  
- Incident response (containment + validation)  
- MITRE ATT&CK mapping  
- Web attack analysis (SQL Injection)  

---

## 📞 Contact

LinkedIn: https://www.linkedin.com/in/tiagokrysiaki/  
GitHub: https://github.com/TKrysiaki  
