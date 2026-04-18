# 🔴 INCIDENT REPORT — SSH Brute Force + Persistência + Resposta a Incidente

---

## 🎯 1. Visão Geral

Foi identificado um ataque de brute force contra o serviço SSH (porta 22) em um servidor Linux, resultando em acesso não autorizado, execução de comandos e criação de persistência no sistema.

- **Alvo:** 192.168.122.102  
- **Atacante:** 192.168.122.50  
- **Serviço afetado:** SSH  
- **Severidade:** Alta  
- **Classificação:** True Positive  

---

## ⚔️ 2. Vetor de Ataque

O atacante realizou múltiplas tentativas de autenticação utilizando brute force até obter sucesso com credenciais válidas.

### 🔍 Descoberta de serviço

<img src="images/01-nmap-scan-ssh-open.png" width="100%">

---

### 💣 Execução do ataque (Hydra)

<img src="images/02-hydra-bruteforce-credenciais.png" width="100%">

---

### 🔓 Acesso inicial

<img src="images/03-ssh-login-acesso-inicial.png" width="100%">

---

## 🔍 3. Detecção

A detecção foi realizada através da análise de logs do sistema.

### 📄 Tentativas falhadas

<img src="images/06-logs-ssh-failed-password.png" width="100%">

---

### 📄 Autenticação bem-sucedida

<img src="images/07-logs-ssh-accepted-password.png" width="100%">

---

### 📊 Quantificação do ataque

<img src="images/08-quantificacao-tentativas-bruteforce.png" width="100%">

---

## 🧠 4. Investigação

Foram correlacionados eventos utilizando auditd para identificar ações pós-comprometimento.

### 🔎 Execução de comandos

<img src="images/09-auditd-execve-ssh.png" width="100%">

---

### 🔎 Uso de privilégios (sudo)

<img src="images/10-auditd-sudo-privilege-escalation.png" width="100%">

---

### 🔎 Alteração de senha

<img src="images/12-auditd-passwd-alteracao-senha.png" width="100%">

---

## 🧬 5. Persistência

Foi identificado que o atacante criou um usuário para manter acesso ao sistema.

<img src="images/11-auditd-useradd-persistencia.png" width="100%">

**Validação adicional:**
- Arquivo: `/etc/passwd`
- Usuário identificado: `suporte`

---

## 🌐 6. Threat Intelligence

<img src="images/14-threat-intel-abuseipdb-ip.png" width="100%">

**Observação:**  
O IP pertence a uma faixa privada (RFC1918), não sendo aplicável para análise de reputação externa.

---

## 🕒 7. Timeline do Incidente

- 10:01 — Início de múltiplas tentativas de login (Failed password)  
- 10:03 — Aumento do volume de tentativas (brute force)  
- 10:05 — Autenticação bem-sucedida (Accepted password)  
- 10:06 — Acesso ao sistema via SSH  
- 10:07 — Criação de usuário (persistência)  
- 10:08 — Execução de comandos com sudo  
- 10:10 — Detecção do incidente  
- 10:12 — Contenção (Fail2ban)  
- 10:15 — Erradicação (remoção do usuário)  
- 10:18 — Hardening aplicado  

---

## 🚨 8. Resposta ao Incidente

### 🔒 Contenção

<img src="images/15-resposta-contencao-fail2ban.png" width="100%">

- Bloqueio do IP atacante via Fail2ban  

---

### 🧹 Erradicação

<img src="images/16-resposta-erradicacao-usuario.png" width="100%">

- Remoção do usuário malicioso (`suporte`)  

---

### 🛡️ Hardening

<img src="images/17-hardening-ssh-config.png" width="100%">

Configurações aplicadas:
- `PermitRootLogin no`  
- `PasswordAuthentication no`  

---

## ⚠️ 9. Impacto

- Acesso não autorizado ao sistema  
- Execução de comandos com privilégio elevado  
- Criação de persistência  
- Risco de movimentação lateral  
- Potencial comprometimento de dados  

---

## 🔎 10. Causa Raiz (Root Cause)

- Uso de credenciais fracas  
- Autenticação por senha habilitada  
- Ausência de proteção contra brute force  
- Falta de hardening no serviço SSH  

---

## 🧩 11. Frameworks

### 🔹 MITRE ATT&CK
- T1110 — Brute Force  

---

### 🔹 NIST Cybersecurity Framework
- Detect → Identificação via logs  
- Respond → Contenção do incidente  
- Recover → Hardening do sistema  

---

### 🔹 CIS Controls
- Control 5 — Account Management  
- Control 6 — Access Control  
- Control 8 — Audit Logs  

---

### 🔹 ISO 27001
- A.9 — Access Control  
- A.12 — Logging and Monitoring  
- A.16 — Incident Management  

---

## ✅ 12. Recomendações

- Implementar autenticação via chave SSH  
- Desabilitar autenticação por senha  
- Utilizar Fail2ban ou equivalente  
- Monitoramento contínuo de logs  
- Aplicar políticas de senha forte  
- Implementar SIEM para correlação de eventos  

---

## 🧠 13. Conclusão

O incidente foi identificado, analisado e tratado com sucesso, seguindo as etapas de detecção, investigação e resposta.  

Foram aplicadas medidas de contenção, erradicação e hardening, reduzindo a superfície de ataque e prevenindo recorrência.

---

## 🚀 14. Lições Aprendidas

- Importância do monitoramento contínuo  
- Necessidade de hardening em serviços expostos  
- Correlação de múltiplas fontes de log  
- Aplicação prática de frameworks de segurança  

---

## 📊 15. Classificação do Incidente

- **Tipo:** Brute Force (SSH)
- **MITRE ATT&CK:** T1110 — Brute Force  
- **Classificação:** True Positive (TP)  
- **Severidade:** Alta  

---

## 🧩 16. Correlação em SIEM (Simulação SOC)

Embora a análise tenha sido realizada manualmente via logs, o comportamento observado corresponde a eventos que seriam correlacionados em um SIEM (ex: Wazuh).

### 🔎 Eventos correlacionados:

- Múltiplos eventos de falha de autenticação (Failed password)
- Origem única (192.168.122.50)
- Alto volume em curto período (brute force)
- Evento de sucesso (Accepted password)
- Execução de comandos com sudo
- Criação de usuário (persistência)

### 🧠 Interpretação:

Esses eventos seriam agrupados como:
> “Multiple SSH authentication failures followed by successful login”

---

## 🧬 17. Pós-Exploração

Após o acesso inicial, foram realizadas ações típicas de pós-exploração:

- Criação de usuário para persistência (`suporte`)
- Execução de comandos com privilégio elevado (sudo)
- Alteração de credenciais

👉 Indica comprometimento ativo do sistema

---

## 🧠 18. Resumo Executivo (1 linha)

Ataque de brute force via SSH resultou em acesso não autorizado, persistência no sistema e execução de ações privilegiadas, sendo detectado, contido e mitigado com sucesso.

---

## 🧠 19. Narrativa Profissional (Entrevista)

> Foi identificado um ataque de brute force contra o serviço SSH, evidenciado por múltiplas falhas de autenticação. Após o comprometimento, o atacante estabeleceu persistência via criação de usuário e executou comandos com privilégio elevado. Realizei a contenção com bloqueio via Fail2ban, erradicação removendo o usuário malicioso e apliquei hardening no serviço para prevenir recorrência.
