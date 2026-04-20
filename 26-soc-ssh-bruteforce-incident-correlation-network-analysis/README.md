# 🚨 Lab 26 — Detecção e Resposta a Brute Force SSH com Correlação Completa
## 📌 Overview
Ataque SSH brute force com comprometimento inicial detectado via Wazuh, correlacionado com logs e tráfego de rede.

- Acesso: ✔ Sim  
- Persistência: ❌ Não  
- Severidade: 🔴 Alta

---

## 📄 Detailed Incident Report
➡️ [Ver relatório completo](./report.md)


---

## ⚔️ Simulação de Ataque

Ataque caracterizado por múltiplas tentativas de autenticação SSH provenientes de diferentes IPs, indicando brute force distribuído.

---

## 🚨 Detecção (SIEM)

O Wazuh identificou atividade suspeita com regra de brute force:

<img src="images/01-wazuh-bruteforce-detected.png" width="100%">

- Regra de alta severidade
- Múltiplas tentativas de login
- Classificação MITRE: T1110

---

## 🔍 Investigação — Logs do Sistema

Análise do /var/log/auth.log revelou padrão claro de ataque:

<img src="images/02-authlog-bruteforce-pattern.png" width="100%">

- Vários usuários inválidos
- Alta frequência de tentativas
- Comportamento automatizado

---

## 🔓 Evidência de Comprometimento

Identificado login bem-sucedido:

<img src="images/03-authlog-success-login.png" width="100%">

- Accepted password for tiago
- Confirma acesso inicial obtido

---

## 💻 Pós-Comprometimento

Nenhuma evidência de persistência ou alteração significativa no sistema após o acesso inicial.

<img src="images/04-history-commands.png" width="100%">


---

## 🔗 Correlação de IP

Eventos agrupados por IP:

<img src="images/05-authlog-ip-correlation.png" width="100%">

- 192.168.56.105 → brute force
- 192.168.56.139 → login bem-sucedido

---

## 🌐 Análise de Rede

Captura de tráfego durante o ataque:

<img src="images/06-tcpdump-capturing-ssh.png" width="100%">

- Monitoramento em tempo real
- Filtro aplicado para SSH

---

## 🔎 Tráfego SSH identificado

<img src="images/07-tcpdump-ssh-traffic.png" width="100%">

- Handshake SSH (SSH-2.0)
- Conexões repetitivas
- Padrão de ataque confirmado

---

## 🧠 Timeline do Incidente
```
Brute force (192.168.56.105)
→ múltiplas falhas
→ ataque distribuído
→ sucesso (192.168.56.139)
→ acesso ao sistema
→ baixa atividade pós-login
```

---

## 🎯 MITRE ATT&CK
- T1110 — Brute Force
- T1078 — Valid Accounts

---

## 🛡️ Resposta

- Bloqueio dos IPs atacantes
- Implementação de Fail2ban
- Revisão de credenciais
- Monitoramento contínuo

---

## 🧾 Conclusão

Foi identificado um ataque de brute force SSH com múltiplas origens, culminando em um login bem-sucedido e comprometimento inicial do sistema.

Apesar da ausência de ações claras de pós-exploração, o acesso não autorizado representa risco elevado e exige resposta imediata.

---

## 📬 Contato

LinkedIn: https://www.linkedin.com/in/tiago-krysiaki

Email: t.krysiaki91@gmail.com






















