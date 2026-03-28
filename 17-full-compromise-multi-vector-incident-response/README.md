# 🔴 Lab 17 — Multi-Vector Attack → Full Compromise → Incident Response

**Simulated real-world SOC incident involving multi-stage attack, credential compromise, and full system takeover.**

---

## 📌 Scenario

A Linux server was targeted by a multi-vector attack originating from a single attacker IP.

The attacker executed:
- Web enumeration (reconnaissance)
- SSH brute force (initial access)
- Successful authentication
- Privilege escalation to root

The objective was to detect, correlate, and respond to the incident using a SOC analyst approach.

---

## 🖥️ Lab Environment

- Attacker: Kali Linux
- Target: Ubuntu Server
- SIEM: Wazuh
- Log sources:
  - `/var/log/apache2/access.log`
  - `/var/log/auth.log`
  - auditd (EXECVE)

---

## 🌐 Attack Simulation

### Web Enumeration
```
dirb http://192.168.56.107 /usr/share/wordlists/dirb/big.txt
```

- Geração de múltiplas requisições HTTP
- Identificação de diretórios e arquivos

---

## 🔍 Detection & Investigation

### 1. Monitoramento de logs web
```
tail -f /var/log/apache2/access.log
```

- Alto volume de requisições
- Múltiplos códigos 404/403
- Padrão automatizado (não humano)

<img src="images/01-web-enumeration-dirb-attack.png" width="100%">

---

### 2. Correlação com logs SSH
```
grep 192.168.56.103 /var/log/auth.log
```

- Tentativas repetidas de login
- Ataque de brute force identificado

<img src="images/02-ssh-brute-force-from-web-attacker.png" width="100%">

---

### 3. Confirmação de comprometimento
```
grep "Accepted password" /var/log/auth.log
```

- Login bem-sucedido detectado
- Conta comprometida

<img src="images/03-successful-ssh-login-after-brute-force.png" width="100%">

---

### 4. Análise pós-login
```
grep "session opened" /var/log/auth.log
```

- Sessões abertas
- Uso de sudo → acesso root

<img src="images/04-post-login-session-activity.png" width="100%">

---

### 5. Análise de execução de comandos
```
ausearch -m EXECVE
```

- Nenhum comando registrado
- Falha de visibilidade (auditd não configurado)

<img src="images/05-no-command-execution-logged.png" width="100%">

---

## 🧠 Timeline

1. Reconhecimento web (dirb)
2. Enumeração de diretórios
3. Brute force SSH
4. Login bem-sucedido
5. Escalada de privilégio (sudo → root)
6. Atividade pós-comprometimento (sem visibilidade)

---

## 🧬 MITRE ATT&CK

- T1046 — Network Service Discovery
- T1083 — File and Directory Discovery
- T1110 — Brute Force
- T1078 — Valid Accounts
- T1068 — Privilege Escalation
- T1059 — Command Execution

---

## 🚨 Incident Classification

**Critical — Full System Compromise**

- Acesso inicial obtido
- Escalada de privilégio confirmada
- Falta de visibilidade pós-comprometimento

---

## 🛡️ Incident Response

### Decisão SOC
```echo "Host comprometido - isolar imediatamente da rede"```

<img src="images/06-incident-response-isolate-host-decision.png" width="100%">

---

### Contenção
```
sudo ip link set enp0s3 down
```

- Interface de rede desativada
- Comunicação com atacante interrompida

<img src="images/07-host-isolation-network-down.png" width="100%">

---

## ⚠️ Impact Analysis

- Comprometimento total do host
- Acesso root obtido
- Possível persistência e movimentação lateral
- Falha de logging (auditd)

---

## 🔧 Mitigation

- Bloqueio de IP atacante
- Reset de credenciais
- Hardening SSH (fail2ban, disable root login)
- Configuração de auditd
- Monitoramento contínuo via SIEM

---

## 🎯 Skills Demonstrated

- Log Analysis (Apache + SSH)
- Attack Correlation
- Incident Detection
- Timeline Reconstruction
- MITRE ATT&CK Mapping
- Incident Response (Containment)
- SOC Analyst Mindset

---

## 📎 Contact

- LinkedIn: https://www.linkedin.com/in/tiago-krysiaki-b3322b2a7/
- Email: t.krysiaki91@gmail.com








