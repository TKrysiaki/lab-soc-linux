# 🚨 Detecção e Resposta a Ataque Brute Force SSH (SOC Analysis)

---

## 🎯 Cenário

Foi identificado comportamento suspeito no serviço SSH de um servidor Linux, indicando tentativa de acesso não autorizado com possível comprometimento da máquina.

---

## ⚔️ Simulação de Ataque

O atacante utilizou Hydra para executar um ataque de força bruta contra o serviço SSH:

hydra -l user -P rockyou.txt ssh://192.168.0.5

O objetivo é descobrir credenciais válidas e obter acesso inicial ao sistema.

<img src="images/01-hydra-attack-running.png" width="100%">

---

## 📊 Análise de Logs

O arquivo `/var/log/auth.log` registrou múltiplas falhas de autenticação:

Esse padrão indica tentativa automatizada de acesso.

<img src="images/02-ssh-failed-logins.png" width="100%">

---

## 🔍 Investigação

Foi identificado que todas as tentativas são originadas do mesmo IP, caracterizando ataque direcionado.

<img src="images/03-attacker-ip-correlation.png" width="100%">

O comportamento confirma uso de ferramenta automatizada (Hydra).

---

## 🚨 Comprometimento Identificado

Após diversas tentativas, o atacante conseguiu autenticação válida no sistema.

<img src="images/04-ssh-success-login.png" width="100%">

Isso indica que o ataque foi **bem-sucedido**, elevando a gravidade do incidente.

---

## ⚠️ Acesso ao Sistema

Após o login, foi aberta uma sessão SSH no servidor comprometido.

<img src="images/05-session-opened.png" width="100%">

Esse evento confirma **acesso inicial obtido pelo atacante**.

---

## 🧠 Análise de Execução (Pós-comprometimento)

Logs do auditd mostram execução de comandos no sistema após o acesso:

<img src="images/06-auditd-execve-analysis.png" width="100%">

Análise:

- Execução de comandos após login
- Possível enumeração do sistema
- Indício de atividade pós-exploração

---

## 🧠 Análise SOC

O incidente evolui de tentativa de brute force para comprometimento real do sistema.

Correlação completa:

1. Múltiplas falhas de login (auth.log)  
2. Ataque automatizado (Hydra)  
3. Login bem-sucedido  
4. Abertura de sessão SSH  
5. Execução de comandos (auditd)  

Impacto:

- Acesso não autorizado confirmado  
- Possível movimentação lateral  
- Risco de persistência e exfiltração  

Objetivo do atacante:

- Obter acesso inicial  
- Executar comandos no sistema comprometido  

Conclusão:

Incidente crítico com comprometimento confirmado.

---

## 🚨 Classificação

Malicioso — acesso não autorizado com comprometimento do host.

---

## 🛡️ Resposta e Mitigação

Ações executadas:

- Isolamento do host comprometido  

<img src="images/07-host-isolation.png" width="100%">

- Bloqueio do IP atacante  

<img src="images/08-block-attacker-ip.png" width="100%">

Recomendações:

- Desabilitar autenticação por senha  
- Implementar autenticação por chave SSH  
- Monitoramento contínuo via SIEM  
- Revisão de credenciais comprometidas  

---

## 🧬 MITRE ATT&CK

- T1110 — Brute Force  
- T1078 — Valid Accounts  
- TA0001 — Initial Access  
- TA0002 — Execution  

---

## 🧠 Habilidades Demonstradas

- Análise de logs (auth.log, auditd)  
- Detecção de brute force  
- Identificação de comprometimento  
- Correlação de eventos  
- Resposta a incidente (isolamento + bloqueio)  
- Investigação pós-comprometimento  

---

## 📌 Conclusão

O laboratório demonstrou um cenário completo de ataque real:

Brute force → acesso → execução → resposta

Evidenciando a importância de:

- Monitoramento contínuo  
- Correlação de eventos  
- Resposta rápida a incidentes críticos
