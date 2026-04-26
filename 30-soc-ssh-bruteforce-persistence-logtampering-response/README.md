# 🚨 Detecção e Resposta a SSH Brute Force com Comprometimento, Persistência e Log Tampering (Wazuh + Fail2Ban)

---

## 📌 Overview

Simulação de ataque SSH brute force com comprometimento completo, persistência, abuso de privilégio e tentativa de evasão via manipulação de logs.

- Acesso: ✔ Sim  
- Persistência: ✔ Sim  
- Evasão (Log Tampering): ✔ Sim  
- Severidade: 🔴 Crítica  

---

## 📄 Detailed Incident Report

➡️ Ver relatório completo: [report.md](./report.md)

---

## 🖥️ Ambiente

- Atacante: 192.168.122.1  
- Alvo: 192.168.122.102  
- SIEM: Wazuh  
- Defesa: Fail2Ban  

---

## 🎯 Attack Scenario

Ataque de brute force realizado contra o serviço SSH, resultando em acesso não autorizado ao sistema.

---

## 🔍 Detection

Atividade de brute force detectada no Wazuh após múltiplas tentativas de autenticação SSH:

![Detection](./images/01_wazuh_bruteforce_detection.png)

Múltiplas falhas de autenticação:

![Failed Password](./images/02_authlog_failed_password.png)

Login bem-sucedido após brute force:

![Accepted Password](./images/03_authlog_accepted_password.png)

Correlação automática do ataque:

![Correlation](./images/04_wazuh_bruteforce_success_correlation.png)

---

## 🧠 Investigation

Atividade pós-comprometimento identificada:

![Bash History](./images/05_bash_history_post_compromise.png)

### Evidências:

- Usuário comprometido: `target`  
- IP de origem: `192.168.122.1`  
- Execução de comandos no sistema  
- Abuso de privilégio via sudo  

---

### 🧼 Log Tampering

Tentativa de remoção de evidências:

![Log Tampering](./images/06_authlog_tampering_evidence.png)

Mesmo após limpeza de logs, evidências mantidas via auditd:

![Auditd SSH](./images/07_auditd_ssh_exec_activity.png)

Execução privilegiada confirmada:

![Sudo Activity](./images/08_auditd_sudo_activity.png)

---

### 🔐 Persistência

Usuário malicioso criado:

![Persistence](./images/09_persistence_user_suporte.png)

---

## 🚨 Classification

- Tipo: Acesso não autorizado + persistência  
- Severidade: 🔴 CRITICAL  
- Impacto: Comprometimento total do sistema  

---

## 🛡️ Response

### Containment

Bloqueio do IP atacante com Fail2Ban:

![Fail2Ban](./images/12_fail2ban_defense_validation.png)

---

### Eradication

Remoção do usuário malicioso:

![User Removed](./images/10_persistence_user_removed.png)

---

### Recovery

Reset de credenciais:

![Password Reset](./images/13_password_reset_target.png)

---

### 🔐 Hardening

Configuração de SSH reforçada:

![SSH Hardening](./images/14_ssh_hardening_config.png)

---

## 🧬 MITRE ATT&CK

- T1110 — Brute Force  
- T1078 — Valid Accounts  
- T1098 — Account Manipulation  
- T1070 — Indicator Removal  

---

## 🎯 Conclusion

O incidente foi detectado, investigado e respondido com sucesso.

O ataque evoluiu de brute force para comprometimento completo, incluindo persistência, execução privilegiada e evasão de logs.

A persistência foi removida e o ambiente protegido com hardening e controle de acesso.

---

## 🧠 Skills Desenvolvidas

- Análise de logs SSH  
- Detecção de brute force  
- Correlação com Wazuh  
- Identificação de persistência  
- Resposta com Fail2Ban  
- Hardening de SSH  
- Detecção de log tampering  

---

## 📞 Contato

LinkedIn: https://www.linkedin.com/in/tiago-krysiaki  
GitHub: https://github.com/TKrysiaki
