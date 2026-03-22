# 🔐 SSH Brute Force Detection + Active Response (Wazuh)

---

## 🎯 Objective

Simular um ataque de brute force SSH utilizando Hydra, detectar as tentativas de autenticação falha com Wazuh e aplicar resposta automática (bloqueio de IP) utilizando active response.

---

## 🧪 Lab Environment

- Attacker: Kali Linux  
- Target: Ubuntu Server (SSH habilitado)  
- SIEM: Wazuh Manager (Ubuntu)  
- Agent: Wazuh Agent no target  

---

## ⚙️ Architecture

O ambiente segue o modelo clássico de SIEM:

- O agente Wazuh coleta logs do sistema (auth.log)
- Envia para o manager
- O manager analisa e correlaciona eventos
- Gera alertas no dashboard
- Executa active response no endpoint

👉 Esse fluxo representa o funcionamento real de um SOC :contentReference[oaicite:0]{index=0}

---

## 🎯 Scenario

Um atacante (Kali Linux) realiza um ataque de brute force contra o serviço SSH do servidor Ubuntu.

O objetivo do atacante é obter acesso via tentativa massiva de senhas.

Durante o ataque:

- Diversas tentativas de login falham
- Logs são gerados no `/var/log/auth.log`
- Wazuh detecta o comportamento
- Uma regra customizada é acionada
- O IP do atacante é bloqueado automaticamente

---

## ⚔️ Attack Simulation

### Comando executado no Kali

```
hydra -l tiago -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.107
````
<img src="images/01-hydra-attack.png" width="100%">
