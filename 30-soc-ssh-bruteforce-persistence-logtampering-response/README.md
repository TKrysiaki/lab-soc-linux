# 🚨 Detecção e Resposta a SSH Brute Force com Comprometimento, Persistência e Log Tampering (Wazuh + Fail2Ban)

---

## 📌 Overview

Simulação de ataque SSH brute force com comprometimento real, persistência, abuso de privilégio e tentativa de evasão via manipulação de logs.

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

Ataque de brute force realizado contra o serviço SSH até obtenção de acesso válido:

![Attack Scenario](./images/01_wazuh_bruteforce_detection.png)

---

## 🔍 Detection

Atividade de brute force identificada no Wazuh:

![Detection](./images/01_wazuh_bruteforce_detection.png)

Falhas de autenticação confirmadas no sistema:

![Failed Password](./images/02_authlog_failed_password.png)

Após múltiplas tentativas, login bem-sucedido:

![Accepted Password](./images/03_authlog_accepted_password.png)

Correlação automática do ataque:

![Correlation](./images/04_wazuh_bruteforce_success_correlation.png)

---

## 🧠 Investigation

Atividade pós-comprometimento identificada:

![Bash History](./images/05_bash_history_post_compromise.png)

### Evidências:

- Conta comprometida: `target`  
- IP de origem: `192.168.122.1`  
- Execução de comandos no sistema  
- Abuso de privilégio via `sudo`  

---

### 🧼 Log Tampering

Tentativa de remoção de evidências:

![Log Tampering](./images/06_authlog_tampering_evidence.png)

Mesmo com logs apagados, auditd manteve rastros:

![Auditd](./images/07_auditd_ssh_exec_activity.png)

Execução privilegiada confirmada:

![Sudo](./images/08_auditd_sudo_activity.png)

---

### 🔐 Persistência

Criação de usuário malicioso:

![Persistence](./images/09_persistence_user_suporte.png)

Persistência ativa confirmada no sistema.

---

## 🚨 Classification

- Tipo: Acesso não autorizado + persistência  
- Severidade: 🔴 CRITICAL  
- Impacto: Comprometimento total do host  

---

## 🛡️ Response

### Containment

Bloqueio do IP atacante:

![Fail2Ban](./images/12_fail2ban_defense_validation.png)

---

### Eradication

Remoção do usuário malicioso:

![User Removed](./images/10_persistence_user_removed.png)

Revisão de acessos SSH:

![Authorized Keys](./images/11_authorized_keys_review.png)

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

## 📊 Conclusão

O ataque evoluiu de brute force para comprometimento completo do sistema, incluindo persistência, execução privilegiada e tentativa de evasão.

Mesmo com a remoção de logs, a investigação foi possível através de múltiplas fontes de evidência, destacando a importância de defesa em profundidade.

---

## 📁 Full Report

Veja detalhes completos em: `report.md`
