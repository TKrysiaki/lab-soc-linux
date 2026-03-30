# 🔐 Lab 19 - SSH Compromise Detection & Response (Auditd + Wazuh)

## 🎯 Objetivo

Simular um comprometimento via SSH, analisar logs no host (Linux) e no SIEM (Wazuh), correlacionar eventos e conduzir uma investigação completa no estilo SOC.

---

## 🧱 Ambiente do Lab

- Kali Linux (Atacante)
- Ubuntu (Alvo)
- Wazuh (SIEM)
- Ferramentas:
  - SSH
  - auditd
  - Wazuh

---

## ⚔️ Simulação do Ataque

Foi realizado um acesso SSH válido a partir da máquina atacante (Kali), utilizando credenciais do usuário `tiago`.

Após o acesso, foram executados comandos com privilégio elevado (`sudo`), simulando comportamento de pós-exploração.

---

## 🔎 Investigação

### 📌 Evidência 1 — Acesso SSH

<img src="images/01-ssh-accepted-login.png" width="100%">

- Log: `/var/log/auth.log`
- Evento: `Accepted password`
- Usuário: `tiago`
- Origem: `192.168.56.103`

**Análise SOC:**
Autenticação bem-sucedida indica uso de credencial válida. Em contexto real, isso pode representar credenciais comprometidas.

---

### 📌 Evidência 2 — Elevação de Privilégio

<img src="images/02-sudo-access.png" width="100%">

- Evento: `sudo`
- Usuário: `tiago`
- Acesso como: `root`

**Análise SOC:**
Usuário comum executando comandos como root caracteriza **Privilege Escalation**.

**MITRE ATT&CK:**
- T1548 – Abuse Elevation Control Mechanism

---

### 📌 Evidência 3 — Execução de Comandos (auditd)

<img src="images/03-auditd-exec.png" width="100%">

- Log: `/var/log/audit/audit.log`
- Evento: `EXECVE`
- Comandos executados:
  - `whoami`
  - `ls`
  - `tail`

**Análise SOC:**
Execução interativa de comandos indica atividade manual após acesso, típico de exploração real.

---

### 📌 Evidência 4 — Detecção no Wazuh

<img src="images/04-wazuh-alert.png" width="100%">

- Evento: `Successful sudo to ROOT executed`

**Análise SOC:**
O SIEM correlacionou a atividade e gerou alerta de alto risco, confirmando comportamento suspeito.

---

## 🧠 Timeline do Ataque

1. Acesso SSH realizado com sucesso  
2. Sessão autenticada  
3. Execução de `sudo`  
4. Execução de comandos (auditd)  
5. Detecção e alerta no Wazuh  

---

## 🚨 Classificação do Incidente

- Tipo: Comprometimento de conta  
- Severidade: Alta  
- Técnica: Acesso válido + elevação de privilégio  

---

## 🎯 Impacto

- Acesso root obtido  
- Possibilidade de controle total do sistema  
- Risco de persistência e movimentação lateral  

---

## 🛡️ Mitigação

- Bloquear IP atacante  
- Forçar troca de senha do usuário  
- Implementar autenticação por chave SSH  
- Configurar Fail2ban  
- Criar Active Response no Wazuh para bloqueio automático  

---

## 💡 Aprendizados

- Acesso válido não significa atividade legítima  
- Correlação entre logs é essencial em SOC  
- auditd fornece visibilidade detalhada de execução  
- SIEM (Wazuh) é fundamental para detecção centralizada  

---

## 📎 Contato

- LinkedIn: https://www.linkedin.com/in/tiago-krysiaki-b3322b2a7/  
- Email: t.krysiaki91@gmail.com  
