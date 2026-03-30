# 🚨 Multi-Vector Attack Analysis: Web Enumeration → SSH Brute Force → Incident Response

---

## 🎯 Cenário

Um servidor Linux exposto apresentou atividade suspeita iniciada por enumeração web, seguida por tentativa de acesso via SSH. A investigação teve como objetivo determinar se houve comprometimento real e definir a resposta adequada.

---

## 🌐 Fase 1 — Enumeração Web

Foi identificada atividade de enumeração utilizando ferramenta de directory brute force (dirb/gobuster).

<img src="images/01-web-enumeration-dirb-attack.png" width="100%">

Análise:

- Múltiplas requisições HTTP  
- Tentativa de descoberta de diretórios  
- Indício de reconhecimento inicial  

👉 Fase de Reconnaissance

---

## ⚔️ Fase 2 — Ataque SSH

Após a enumeração, foi identificado brute force contra o serviço SSH.

<img src="images/02-ssh-brute-force-from-web-attacker.png" width="100%">

👉 Mudança de vetor: Web → SSH

---

## 🚨 Fase 3 — Acesso Inicial

O atacante obteve sucesso na autenticação:

<img src="images/03-successful-ssh-login-after-brute-force.png" width="100%">

👉 Acesso inicial confirmado (Initial Access)

---

## 🧠 Fase 4 — Atividade Pós-Login

Foi observada abertura de sessão no sistema:

<img src="images/04-post-login-session-activity.png" width="100%">

Porém:

<img src="images/05-no-command-execution-logged.png" width="100%">

Análise:

- Sem execução clara de comandos  
- Sem evidência de ações maliciosas imediatas  
- Possível acesso inicial sem exploração ativa  

---

## 🧠 Análise SOC

Correlação dos eventos:

1. Enumeração web (reconhecimento)  
2. Ataque brute force SSH  
3. Login bem-sucedido  
4. Sessão aberta  
5. Ausência de comandos executados  

Interpretação:

- Cadeia de ataque incompleta  
- Comprometimento inicial confirmado  
- Sem evidência de pós-exploração ativa  

Risco:

Alto — acesso válido ao sistema permite execução futura de ações maliciosas

Problema crítico:

- Falta de visibilidade (logs incompletos)  
- Não é possível garantir ausência de atividade  

---

## ⚖️ Decisão do Analista

Mesmo sem evidência de execução de comandos:

👉 O acesso não autorizado já caracteriza incidente de segurança

Decisão:

Isolamento imediato do host para contenção

---

## 🛡️ Resposta a Incidente

Decisão de isolamento:

<img src="images/06-incident-response-isolate-host-decision.png" width="100%">

Execução:

<img src="images/07-host-isolation-network-down.png" width="100%">

---

## 🚨 Classificação

Malicioso — acesso não autorizado com risco de comprometimento ativo.

---

## 🧬 MITRE ATT&CK

- T1595 — Active Scanning  
- T1110 — Brute Force  
- T1078 — Valid Accounts  
- TA0001 — Initial Access  
- TA0007 — Discovery  

---

## 🧠 Habilidades Demonstradas

- Análise multi-vetor (Web + SSH)  
- Correlação de eventos  
- Identificação de ataque em cadeia  
- Tomada de decisão em cenário ambíguo  
- Resposta a incidente (containment)  
- Pensamento analítico de SOC  

---

## 📌 Conclusão

Este laboratório demonstra um cenário realista onde múltiplos vetores são utilizados em sequência para comprometer um sistema.

Mesmo na ausência de evidências claras de pós-exploração, o acesso não autorizado representa um risco crítico, exigindo ação imediata.

A decisão de isolamento foi baseada em:

- Cadeia de ataque identificada  
- Acesso confirmado  
- Falta de visibilidade confiável  

---

## 📬 Contato

- LinkedIn: https://www.linkedin.com/in/tiago-krysiaki  
- Email: t.krysiaki91@gmail.com  
