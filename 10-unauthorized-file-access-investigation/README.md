# 🚨 Investigação de Acesso Não Autorizado e Tentativa de Leitura de Credenciais (SOC Analysis)

---

## 🎯 Cenário

Foi identificado acesso bem-sucedido via SSH em um servidor Linux. A investigação teve como objetivo analisar possíveis ações maliciosas realizadas após o login.

---

## 🚨 Acesso Inicial

Login SSH realizado com sucesso:

<img src="images/01-ssh-login-success.png" width="100%">

👉 Indica acesso válido ao sistema

---

## ⚠️ Tentativa de Acesso Sensível

Foi detectada tentativa de acesso ao arquivo `/etc/shadow`, que contém hashes de senha:

<img src="images/02-shadow-access-attempt.png" width="100%">

Evidência nos logs:

<img src="images/03-shadow-log-evidence.png" width="100%">

Análise:

- Acesso a arquivo crítico do sistema  
- Indicativo de tentativa de coleta de credenciais  
- Comportamento altamente suspeito  

---

## 🧠 Uso de Privilégios Elevados

Execução de comandos com sudo:

<img src="images/04-sudo-commands-log.png" width="100%">

Análise:

- Usuário elevando privilégios  
- Possível tentativa de obter acesso root  
- Indicativo de exploração pós-login  

---

## 📊 Linha do Tempo do Ataque

Sequência dos eventos:

<img src="images/05-attack-timeline.png" width="100%">

Fluxo identificado:

1. Login SSH bem-sucedido  
2. Tentativa de acesso ao `/etc/shadow`  
3. Execução de comandos com sudo  

---

## 🧠 Análise SOC

O comportamento indica possível comprometimento de conta seguido de tentativa de coleta de credenciais e elevação de privilégios.

Correlação:

- Login válido (auth.log)  
- Tentativa de acesso a arquivo sensível  
- Uso de sudo após login  

Impacto:

- Risco de exposição de credenciais  
- Possível escalonamento de privilégio  
- Comprometimento potencial do sistema  

Objetivo do atacante:

- Obter hashes de senha  
- Escalar privilégios  
- Manter persistência  

Conclusão:

Atividade maliciosa altamente suspeita, com indícios de pós-exploração.

---

## 🚨 Classificação

Malicioso — tentativa de acesso a credenciais sensíveis com uso de privilégios elevados.

---

## 🧬 MITRE ATT&CK

- T1003 — OS Credential Dumping  
- T1078 — Valid Accounts  
- T1548 — Abuse Elevation Control Mechanism  
- TA0001 — Initial Access  
- TA0004 — Privilege Escalation  
- TA0006 — Credential Access  

---

## 🧠 Habilidades Demonstradas

- Análise de logs Linux  
- Identificação de acesso suspeito  
- Detecção de tentativa de credential dumping  
- Investigação de uso de sudo  
- Construção de timeline de ataque  
- Correlação de eventos  

---

## 📌 Conclusão

O laboratório demonstra um cenário de pós-comprometimento, onde um invasor utiliza credenciais válidas para acessar o sistema e tenta extrair informações sensíveis e elevar privilégios.

A análise reforça a importância de:

- Monitoramento de arquivos críticos  
- Controle de privilégios  
- Correlação de eventos após login  

---

## 📬 Contato

Aberto a oportunidades em SOC / NOC / Cybersecurity Jr.

- LinkedIn: https://www.linkedin.com/in/tiago-krysiaki  
- Email: t.krysiaki91@gmail.com  
