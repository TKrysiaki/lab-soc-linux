# 🚨 Análise de Cadeia de Ataque: SSH Compromise + Privilege Escalation (SOC Analysis)

---

## 🎯 Cenário

Um servidor Linux apresentou comportamento suspeito no serviço SSH, indicando tentativa de acesso não autorizado. A investigação revelou não apenas comprometimento inicial, mas também escalonamento de privilégios até root.

---

## 📊 Análise de Logs — Tentativas de Acesso

O arquivo `/var/log/auth.log` registrou múltiplas falhas de autenticação via SSH:

Esse padrão indica tentativa de brute force.

<img src="images/01-ssh-failed-password.png" width="100%">

---

## 🔍 Investigação — Correlação de IP

Foi identificado que todas as tentativas são originadas do mesmo IP, caracterizando ataque automatizado.

<img src="images/02-ip-correlation-attack.png" width="100%">

---

## 🚨 Comprometimento Inicial

Após diversas tentativas, o atacante conseguiu autenticação válida:

<img src="images/03-ssh-accepted-password.png" width="100%">

👉 Isso confirma acesso inicial ao sistema (Initial Access)

---

## ⚠️ Atividade Suspeita — Uso de Sudo

Logs mostram execução de comandos com privilégios elevados:

<img src="images/04-sudo-activity.png" width="100%">

Análise:

- Usuário comprometido executando sudo  
- Indício de tentativa de elevação de privilégio  

---

## 🔥 Escalonamento de Privilégio

Foi identificado alteração de senha do usuário root:

<img src="images/05-password-change-root.png" width="100%">

👉 Isso indica comprometimento crítico do sistema

---

## 🧠 Acesso Root Confirmado

Sessão root foi aberta após a alteração de credenciais:

<img src="images/06-session-opened-root.png" width="100%">

👉 Controle total do sistema pelo atacante

---

## 🧠 Análise SOC

O incidente demonstra uma cadeia completa de ataque:

1. Brute force SSH (tentativas falhadas)  
2. Acesso inicial obtido (valid credentials)  
3. Execução de comandos com sudo  
4. Alteração de credenciais do root  
5. Acesso root confirmado  

Correlação:

- Mesmo IP em todas as etapas  
- Sequência lógica de ataque  
- Evolução de acesso → privilégio total  

Impacto:

- Comprometimento total do host  
- Controle administrativo completo  
- Alto risco de persistência e movimentação lateral  

Objetivo do atacante:

- Obter acesso inicial  
- Escalar privilégios  
- Assumir controle total do sistema  

Conclusão:

Incidente crítico com comprometimento total do sistema.

---

## 🚨 Classificação

Malicioso — ataque completo com privilege escalation bem-sucedido.

---

## 🛡️ Mitigação

- Desabilitar autenticação por senha no SSH  
- Implementar autenticação por chave  
- Restringir uso de sudo  
- Monitorar alterações em contas privilegiadas  
- Resetar credenciais comprometidas  
- Auditoria completa do sistema  

---

## 🧬 MITRE ATT&CK

- T1110 — Brute Force  
- T1078 — Valid Accounts  
- T1548 — Abuse Elevation Control Mechanism  
- TA0001 — Initial Access  
- TA0004 — Privilege Escalation  

---

## 🧠 Habilidades Demonstradas

- Análise de logs Linux (auth.log)  
- Detecção de brute force  
- Identificação de acesso não autorizado  
- Investigação de privilege escalation  
- Correlação de eventos  
- Classificação de incidente crítico  

---

## 📌 Conclusão

Este laboratório demonstra uma cadeia completa de ataque real, onde um invasor evolui de tentativas de acesso até controle total do sistema.

A análise evidencia a importância de:

- Monitoramento contínuo  
- Controle de privilégios  
- Correlação de eventos  
- Resposta rápida a incidentes críticos  

---

## 📬 Contato

- LinkedIn: https://www.linkedin.com/in/tiago-krysiaki-b3322b2a7/  
- Email: t.krysiaki91@gmail.com  
