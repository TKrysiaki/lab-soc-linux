# 🚨 Detecção de Brute Force SSH com Wazuh + Resposta com Fail2ban

---

## 🎯 Cenário

Um servidor Linux foi alvo de tentativas de acesso não autorizado via SSH. O objetivo foi detectar o ataque utilizando Wazuh (SIEM) e implementar resposta automática com Fail2ban.

---

## 📊 Análise de Logs — Tentativas de Login

Foram identificadas falhas de autenticação no SSH:

<img src="images/01-ssh-failed-login-kali.png" width="100%">

Logs locais confirmam tentativas inválidas:

<img src="images/04-ubuntu-auth-log-ssh-failed.png" width="100%">

---

## 📡 Detecção no SIEM (Wazuh)

O Wazuh detectou eventos de autenticação falhada:

<img src="images/02-wazuh-ssh-authentication-failed.png" width="100%">

Detalhes do evento:

<img src="images/03-wazuh-ssh-event-details.png" width="100%">

Múltiplos eventos correlacionados:

<img src="images/07-wazuh-multiple-ssh-failed-events.png" width="100%">

Alerta gerado:

<img src="images/08-wazuh-ssh-alert-details.png" width="100%">

Classificação MITRE no Wazuh:

<img src="images/09-wazuh-mitre-password-guessing.png" width="100%">

---

## ⚔️ Simulação de Ataque

Foi executado brute force com Hydra:

<img src="images/05-hydra-ssh-bruteforce-kali.png" width="100%">

Logs confirmam volume elevado de tentativas:

<img src="images/06-ubuntu-ssh-bruteforce-logs.png" width="100%">

---

## 🧠 Análise SOC

O comportamento identificado caracteriza ataque de força bruta contra o serviço SSH.

Evidências:

- Múltiplas falhas consecutivas de login  
- Detecção correlacionada no SIEM (Wazuh)  
- Origem consistente (mesmo IP)  
- Padrão automatizado (Hydra)  

Correlação:

- auth.log (host)  
- alertas Wazuh (SIEM)  
- atividade do atacante (Kali)  

Impacto:

- Risco de comprometimento do sistema  
- Possibilidade de acesso não autorizado  

Objetivo do atacante:

- Descobrir credenciais válidas  
- Obter acesso inicial ao sistema  

Conclusão:

Atividade maliciosa confirmada.

---

## 🚨 Classificação

Malicioso — tentativa de brute force detectada.

---

## 🛡️ Resposta com Fail2ban

Instalação:

<img src="images/10-install-fail2ban.png" width="100%">

Serviço ativo:

<img src="images/11-fail2ban-service-running.png" width="100%">

Jail SSH configurada:

<img src="images/12-fail2ban-ssh-jail-active.png" width="100%">

Bloqueio após múltiplas tentativas:

<img src="images/13-fail2ban-ssh-bruteforce-block.png" width="100%">

Configuração de limite:

<img src="images/14-fail2ban-ssh-3-attempts-config.png" width="100%">

Teste de ataque:

<img src="images/15-hydra-ssh-test-fail2ban.png" width="100%">

IP bloqueado:

<img src="images/16-fail2ban-ip-blocked.png" width="100%">

---

## 🧬 MITRE ATT&CK

- T1110 — Brute Force  
- TA0001 — Initial Access  

---

## 🧠 Habilidades Demonstradas

- Análise de logs Linux (auth.log)  
- Monitoramento com SIEM (Wazuh)  
- Detecção de ataque brute force  
- Correlação de eventos  
- Implementação de resposta automática (Fail2ban)  
- Hardening de serviço SSH  

---

## 📌 Conclusão

O laboratório demonstrou a detecção eficaz de um ataque de brute force utilizando SIEM (Wazuh) e a implementação de uma resposta automatizada com Fail2ban.

A integração entre monitoramento e defesa permitiu identificar e bloquear o atacante rapidamente, reduzindo o risco de comprometimento.

---

## 📬 Contato

Aberto a oportunidades em SOC / NOC / Cybersecurity Jr.

- LinkedIn: https://www.linkedin.com/in/tiago-krysiaki  
- Email: t.krysiaki91@gmail.com  
