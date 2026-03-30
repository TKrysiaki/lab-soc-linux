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

## ⚙️ Preparação do Ambiente

### Auditd ativo

<img src="images/01-auditd-active.png" width="100%">

### Wazuh Agent ativo

<img src="images/02-wazuh-agent-active.png" width="100%">

### Serviço SSH ativo

<img src="images/03-ssh-active.png" width="100%">

**Análise SOC:**  
Ambiente preparado corretamente com coleta de logs ativa.

---

## ⚔️ Fase 1 — Tentativa de Ataque (Brute Force)

<img src="images/04-ssh-bruteforce-detected.png" width="100%">
<img src="images/05-no-successful-login.png" width="100%">

**Análise SOC:**  
Múltiplas tentativas de login falharam, caracterizando brute force.  
Sem sucesso inicial.

---

## ⚠️ Fase 2 — Falha de Defesa

<img src="images/06-fail2ban-inactive.png" width="100%">

**Análise SOC:**  
Fail2ban desativado permitiu continuidade do ataque.

---

## 🚨 Fase 3 — Comprometimento

<img src="images/07-ssh-successful-login.png" width="100%">

**Análise SOC:**  
Login SSH bem-sucedido utilizando credencial válida.  
Indica comprometimento de conta.

---

## 🔎 Fase 4 — Pós-exploração (Auditd)

<img src="images/08-auditd-post-exploitation.png" width="100%">
<img src="images/09-auditd-decoded-commands.png" width="100%">

**Análise SOC:**  
Execução de comandos após acesso.  
Comportamento típico de atacante interagindo com o sistema.

---

## 🧠 Fase 5 — Detecção no SIEM (Wazuh)

<img src="images/10-attacker-commands-detected.png" width="100%">

**Análise SOC:**  
Wazuh detectou atividade suspeita com sucesso.  
Correlação de eventos funcionando.

---

## 🧠 Timeline do Ataque

1. Serviços ativos (auditd, wazuh, ssh)  
2. Tentativas de brute force  
3. Defesa ausente (fail2ban)  
4. Acesso SSH obtido  
5. Execução de comandos  
6. Detecção no SIEM  

---

## 🚨 Classificação do Incidente

- Tipo: Comprometimento de conta  
- Severidade: Alta  
- Técnicas:
  - Initial Access (SSH)
  - Execution
  - Privilege Escalation

---

## 🎯 Impacto

- Acesso não autorizado  
- Possível controle total do sistema  
- Risco de movimentação lateral  

---

## 🛡️ Mitigação

- Ativar Fail2ban  
- Bloquear IP atacante  
- Trocar credenciais  
- Usar autenticação por chave SSH  
- Implementar Active Response no Wazuh  

---

## 💡 Aprendizados

- Defesa desativada facilita ataque  
- Logs são essenciais para investigação  
- auditd dá visibilidade detalhada  
- SIEM permite correlação eficiente  
- Ataques seguem uma cadeia clara  

---

## 📎 Contato

- LinkedIn: https://www.linkedin.com/in/tiago-krysiaki-b3322b2a7/  
- Email: t.krysiaki91@gmail.com  
