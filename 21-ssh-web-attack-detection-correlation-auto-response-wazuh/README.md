# 🔐 Detecção e Resposta a Brute Force SSH com Wazuh + TheHive

---

## 🎯 Visão Geral

Este laboratório simula um cenário real de SOC (Security Operations Center), onde um atacante realiza um ataque de força bruta contra um serviço SSH.

O objetivo é demonstrar como um analista SOC:

- Detecta atividade maliciosa
- Correlaciona eventos
- Investiga alertas
- Responde ao incidente
- Valida a contenção

---

## 🧠 Por que este Lab é importante

Este não é apenas um laboratório de análise de logs.

Ele demonstra:

- Simulação de ataque real
- Detecção com SIEM
- Processo de investigação
- Resposta a incidente
- Validação técnica

➡️ Representa o fluxo real de um SOC

---

## 🖥️ Arquitetura do Laboratório

- Atacante: Ubuntu (script automatizado)
- Alvo: Windows (SSH habilitado)
- SIEM: Wazuh
- Resposta a Incidentes: TheHive

---

## ⚔️ Simulação de Ataque

O ataque foi executado com um script automatizado contendo:

- Hydra (brute force SSH)
- Nmap (reconhecimento)
- Feroxbuster (enumeração web)
- Netcat (banner grabbing)

### Execução

```
python3 adversary_simulator.py
```
<img src="images/01-attack-hydra-bruteforce.png" width="100%">

### 🔍 Análise
- Múltiplas tentativas de login
- Alta frequência
- Comportamento automatizado

➡️ Indica ataque de brute force

---

## 🔎 Detecção (Wazuh)

O Wazuh detectou múltiplas falhas de autenticação:

- Event ID: 4625
- Regra: authentication_failed
- Correlação por frequência (firedtimes)
<img src="images/02-wazuh-ssh-failed-login.png" width="100%">


### 🧠 Análise SOC
- Falhas repetidas
- Usuário inválido
- Curto intervalo entre tentativas

➡️ Classificado como brute force

---

## 🚨 Geração de Alerta (TheHive)

O alerta foi enviado automaticamente ao TheHive:

- Severidade: HIGH
- Fonte: Wazuh
- Tipo: SSH Attack
<img src="images/03-thehive-alerts-ssh-bruteforce.png" width="100%">

---

## 🧠 Análise do Incidente

Detalhamento do alerta:

<img src="images/04-thehive-alert-details.png" width="100%">

### 📌 Classificação
- Tipo: Brute Force SSH
- Severidade: HIGH
- Status: Malicioso

### 📌 MITRE ATT&CK
- T1078 – Valid Accounts

### 📌 Evidências
- Tentativas > 10
- Falhas de autenticação
- Padrão automatizado

---

## 🛡️ Resposta (Contenção)

Bloqueio do IP atacante via firewall do Windows:
```
New-NetFirewallRule -DisplayName "Block Attacker" -Direction Inbound -RemoteAddress 192.168.56.110 -Action Block
```
<img src="images/05-firewall-block-ip.png" width="100%">

### 🎯 Resultado
- IP bloqueado
- Ataque interrompido

---

## 📋 Verificação da Regra
```
Get-NetFirewallRule | findstr Block
```
<img src="images/06-firewall-rules-list.png" width="100%">

---

## ✅ Validação

Teste de conectividade após o bloqueio:
```
nc -vz 192.168.56.110 22
```
<img src="images/07-block-test-timeout.png" width="100%">

### ✔️ Resultado
- Timeout na conexão
- Firewall realizando DROP

➡️ Contenção validada com sucesso

---

## 🧠 Conclusão

Este laboratório demonstra um fluxo completo de operação SOC:

1. Simulação de ataque
2. Detecção via SIEM
3. Correlação de eventos
4. Análise do incidente
5. Resposta ativa
6. Validação técnica

---

## 🚀 Habilidades Demonstradas
- Análise de logs (Windows / SSH)
- SIEM (Wazuh)
- Resposta a incidentes (TheHive)
- Detecção de ameaças
- Gestão de firewall
- Fluxo de investigação SOC

---

## 📬 Contato

LinkedIn: https://www.linkedin.com/in/tiago-krysiaki

Email: t.krysiaki91@gmail.com














