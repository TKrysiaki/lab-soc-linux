# 🚨 Detecção e Resposta a Brute Force SSH com Persistência (Wazuh + Fail2ban)
## 📌 Overview
Simulação de ataque SSH brute force com comprometimento e persistência via criação de usuário e SSH key. Detecção realizada com Wazuh e resposta com Fail2ban.

- Acesso: ✔ Sim
- Persistência: ✔ Sim
- Severidade: 🔴 Crítica

---

## 📄 Detailed Incident Report
➡️ [Ver relatório completo](./report.md)

---

## 🖥️ Ambiente
- Atacante: 192.168.56.139
- Alvo: Ubuntu (host: tiago)
- SIEM: Wazuh
- Defesa: Fail2ban

---

## 🎯 Simulação do Ataque

O atacante executou múltiplas tentativas de login via SSH até obter sucesso.

<img src="images/04-first-attack-event.png" width="100%"> <img src="images/07-ssh-failed-login-count.png" width="100%">

---

## 🔍 Detecção e Correlação

O Wazuh identificou comportamento suspeito e correlacionou eventos de falha com login bem-sucedido.

<img src="images/02-filter-ip-attack-wazuh.png" width="100%"> <img src="images/03-accepted-login-correlation.png" width="100%">

---

## 🚨 Confirmação do Comprometimento

Login bem-sucedido após brute force:

<img src="images/08-ssh-successful-login.png" width="100%">

Sessão aberta no sistema:

<img src="images/10-ssh-session-opened-tiago.png" width="100%">

---

## 💥 Ações do Atacante (Pós-Exploração)

Após o comprometimento inicial, foram identificadas ações de pós-exploração visando persistência no host:

- Criação de conta maliciosa (T1136)
- Escalada de privilégio via sudo
- Persistência via SSH Authorized Keys (T1098.004)

<img src="images/11-attacker-commands-executed.png" width="100%">

---

## 🧠 Análise SOC
- Ataque identificado como Brute Force SSH (T1110)
- Comprometimento confirmado via login válido
- Persistência estabelecida via SSH Authorized Keys (T1098.004)
- Uso de privilégios elevados (sudo) após comprometimento inicial, indicando controle total do host.

**👉 Classificação: True Positive (TP)**

**👉 Severidade: Crítica**

---

## 🧬 MITRE ATT&CK

- T1110 – Brute Force
- T1078 – Valid Accounts
- T1098.004 – SSH Authorized Keys
- T1136 – Create Account

---

## 🛡️ Resposta ao Incidente

Bloqueio do IP atacante via Fail2ban:

<img src="images/09-fail2ban-ban-ip.png" width="100%">

---

## 🧹 Remediação
- Remoção do usuário malicioso (backdoor)
- Exclusão de diretórios persistentes
- Reset de senha do usuário comprometido

<img src="images/13-backdoor-user-removed-validation.png" width="100%"> 

<img src="images/14-password-reset-tiago-success.png" width="100%">

---

## 🔎 Validação Pós-Incidente

Auditoria de usuários:

<img src="images/15-users-audit-no-backdoor.png" width="100%">

Monitoramento após mitigação:

<img src="images/16-post-mitigation-monitoring.png" width="100%">

Validação do Fail2ban:

<img src="images/17-fail2ban-status-validation.png" width="100%">

---

## 🎯 Conclusão

O laboratório demonstrou um ciclo completo de atuação SOC:

- Detecção de ataque
- Correlação de eventos
- Confirmação de comprometimento
- Identificação de persistência
- Resposta imediata
- Remediação completa
- Validação pós-incidente

**👉 O ambiente foi restaurado com sucesso, eliminando acesso do atacante.**

---

## 🧠 Skills Desenvolvidas
- Análise de logs SSH
- Correlação de eventos (Wazuh)
- Investigação de incidentes
- Detecção de persistência
- Resposta com Fail2ban
- Remediação de acesso não autorizado

---

## 📞 Contato
- LinkedIn: https://www.linkedin.com/in/tiago-krysiaki
- Email: t.krysiaki91@gmail.com

## 🎯 Buscando oportunidades em SOC / NOC (Segurança da Informação)
