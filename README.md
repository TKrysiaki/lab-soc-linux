## 🎯 Cenário

Foi identificado um possível comportamento suspeito no serviço SSH de um servidor Linux, indicando tentativa de acesso não autorizado.

---

## ⚔️ Simulação de Ataque

Foi executado um ataque de força bruta utilizando Hydra contra o serviço SSH:

hydra -l user -P rockyou.txt ssh://192.168.0.5

---

## 📊 Análise de Logs

Os logs em /var/log/auth.log apresentaram múltiplas tentativas de login falhadas:

Failed password for user from 192.168.0.10

---

## 🔍 Investigação

- Volume alto de tentativas em curto período
- Mesmo IP repetindo tentativas
- Padrão típico de brute force

---

## 🧠 Análise SOC

O evento caracteriza um ataque de força bruta contra o serviço SSH.

- IP atacante: 192.168.0.10
- Alvo: servidor SSH
- Técnica: tentativa de adivinhação de credenciais

Risco:
Alto — possibilidade de comprometimento da conta

Objetivo do atacante:
Obter acesso inicial ao sistema

---

## 🚨 Classificação

Malicioso — ataque confirmado

---

## 🛡️ Mitigação

- Bloqueio automático com Fail2ban
- Restrição de tentativas de login
- Recomendação de uso de chave SSH

---

## 🧬 MITRE ATT&CK

- T1110 — Brute Force
- TA0001 — Initial Access

---

## 📸 Evidências

<img src="images/01-ssh-failed-login-kali.png" width="100%">
<img src="images/02-wazuh-alert-bruteforce.png" width="100%">
<img src="images/03-fail2ban-ban-ip.png" width="100%">

---

## 🧠 Habilidades Demonstradas

- Análise de logs Linux
- Detecção de ataque
- Uso de SIEM (Wazuh)
- Resposta a incidente
