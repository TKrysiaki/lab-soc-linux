# 🚨 Detecção e Resposta a Brute Force SSH com Persistência (Wazuh + Fail2ban)

---

## 📌 Overview
Simulação de ataque SSH brute force com comprometimento e persistência via criação de usuário e SSH key.

- Acesso: ✔ Sim  
- Persistência: ✔ Sim  
- Severidade: 🔴 Alta  

---

## 📄 Detailed Incident Report
➡️ [Ver relatório completo](./report.md)

---

## 🖥️ Ambiente
- Atacante: 192.168.122.50  
- Alvo: 192.168.122.102  
- SIEM: Wazuh  
- Defesa: Fail2ban  

---

## 🎯 Attack Scenario
Identificação do serviço SSH exposto e execução de brute force até obtenção de acesso válido.

<img src="images/01-nmap-scan-ssh-open.png" width="100%">
<img src="images/02-hydra-bruteforce-credenciais.png" width="100%">
<img src="images/03-ssh-login-acesso-inicial.png" width="100%">

---

## 🔍 Detection

### 📄 Evidências em logs
Identificação de brute force e acesso:

```
grep "Failed password" /var/log/auth.log
grep "Accepted password" /var/log/auth.log
```
<img src="images/06-logs-ssh-failed-password.png" width="100%"> 
<img src="images/07-logs-ssh-accepted-password.png" width="100%">

## 🧠 Investigation

🔎 Atividade pós-comprometimento

Identificação de comandos executados e uso de privilégios elevados:
```
grep EXECVE /var/log/audit/audit.log
grep sudo /var/log/audit/audit.log
```

<img src="images/09-auditd-execve-ssh.png" width="100%"> 
<img src="images/10-auditd-sudo-privilege-escalation.png" width="100%">

---

## 🧬 Persistence

Persistência identificada após comprometimento inicial:

```
cat /etc/passwd | grep suporte
```
<img src="images/11-auditd-useradd-persistencia.png" width="100%">

- Criação de conta maliciosa (T1136)
- Escalada de privilégio via sudo
- Persistência via SSH Authorized Keys (T1098.004)

---

## 🚨 Response

🔒 Contenção

Bloqueio do IP atacante:
```
sudo fail2ban-client set sshd banip 192.168.122.50
```

<img src="images/15-resposta-contencao-fail2ban.png" width="100%">

---

## 🧹 Erradicação

Remoção do usuário malicioso:

```
sudo userdel -r suporte
```
<img src="images/16-resposta-erradicacao-usuario.png" width="100%">

---

## 🛡️ Hardening

Aplicação de medidas de segurança no SSH:

```
PermitRootLogin no
PasswordAuthentication no
```

<img src="images/17-hardening-ssh-config.png" width="100%">

---

## 🔎 Validation

Validação após resposta:

- Usuário malicioso removido
- Acesso bloqueado
- Monitoramento ativo

---


## 🧬 MITRE ATT&CK

- T1110 – Brute Force
- T1078 – Valid Accounts
- T1098.004 – SSH Authorized Keys
- T1136 – Create Account

---

## 🎯 Conclusion

O incidente foi detectado, investigado e respondido com sucesso.
A persistência foi removida e medidas de hardening foram aplicadas, reduzindo a superfície de ataque.

---


## 🧠 Skills Desenvolvidas

- Análise de logs SSH
- Investigação com auditd
- Correlação de eventos (Wazuh)
- Detecção de persistência
- Resposta com Fail2ban
- Hardening de ambiente Linux

---

📬 Contato

Aberto a oportunidades como SOC Analyst.

LinkedIn: https://www.linkedin.com/in/tiago-krysiaki
GitHub: https://github.com/TKrysiaki
