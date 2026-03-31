# 🔥 SSH Brute Force Detection & Response (Wazuh + Fail2ban)

## 📌 Visão Geral

Este laboratório simula um **ataque real de força bruta via SSH** e demonstra o fluxo completo de um SOC:

- Simulação de ataque com Hydra  
- Coleta e análise de logs via Wazuh (SIEM)  
- Detecção de múltiplas falhas de autenticação  
- Resposta automática com Fail2ban (bloqueio de IP)  

---

## 🧪 Ambiente do Lab

- Atacante: Kali Linux (`kali-attacker`)  
- Alvo: Ubuntu (`ubuntu-target`)  
- SIEM: Wazuh (`wazuh-siem`)  

---

## 🚨 Simulação de Ataque

Foi executado um ataque de brute force utilizando Hydra:

```
hydra -l tiago -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.107
```

---

## 🔎 Resultado do Ataque

O ataque realizou diversas tentativas até encontrar uma credencial válida:
```login: tiago password: 123456```

<img src="images/06-hydra-ssh-bruteforce-success.png" width="100%">

---

## 🔍 Detecção no SIEM (Wazuh)

O Wazuh detectou múltiplas falhas de autenticação:

- sshd: authentication failed
- Correlação de eventos de brute force
- Alerta crítico disparado (Level 10)


## 🧬 MITRE ATT&CK
-  T1110 → Brute Force
-  T1021.004 → Remote Services (SSH)
  
<img src="images/07-wazuh-ssh-bruteforce-detected.png" width="100%">

---

## 🧠 Análise SOC

Sequência observada:

- Múltiplas tentativas de login falhadas
- Pico de eventos em curto intervalo
- Disparo de regra de brute force
- Identificação de comportamento malicioso

👉 Classificação: Ataque de força bruta confirmado

---

## 🛡️ Resposta Automática (Fail2ban)

Foi implementado o Fail2ban para bloqueio automático:

```
[sshd]
enabled = true
port = ssh
logpath = /var/log/auth.log
maxretry = 3
bantime = 600
```

---

## 🚫 Resultado da Mitigação

Após nova tentativa de ataque:

```Connection refused```

👉 O IP atacante foi bloqueado na camada de rede.

<img src="images/08-fail2ban-ssh-block-success.png" width="100%">

---


## 🔎 Interpretação Técnica

- Antes: tentativas atingiam o serviço SSH (application layer)
- Depois: conexão bloqueada antes do serviço (network layer)

👉 Indica atuação efetiva de mecanismo de defesa (IPS)

---

## ⚠️ Classificação do Incidente

- Tipo: Brute Force Attack
- Severidade: Alta
- Impacto: Comprometimento de credenciais
- Status: Mitigado automaticamente

---

## 🔐 Medidas de Segurança
- Implementação de Fail2ban
- Limitação de tentativas de login
- Uso de autenticação por chave SSH
- Restrição de acesso por IP

---

## 🎯 Habilidades Demonstradas

- Análise de logs (auth.log)
- SIEM (Wazuh)
- Detecção de ameaças
- Correlação de eventos
- MITRE ATT&CK
- Resposta a incidentes
- Hardening de serviços

---

📬 Contato
LinkedIn: https://www.linkedin.com/in/tiago-krysiaki
Email: t.krysiaki91@gmail.com
