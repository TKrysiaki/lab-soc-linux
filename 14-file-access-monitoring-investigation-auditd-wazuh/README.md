# 🚨 Monitoramento de Acesso a Arquivos Sensíveis com auditd + Detecção Customizada no Wazuh

---

## 🎯 Cenário

Foi implementado monitoramento de arquivos sensíveis em um servidor Linux utilizando auditd, com integração ao Wazuh para detecção de acessos não autorizados ao arquivo `/etc/shadow`.

---

## ⚙️ Configuração do Monitoramento (auditd)

Instalação e ativação do auditd:

<img src="images/01-auditd-install-service.png" width="100%">

Criação de regra para monitorar `/etc/passwd`:

<img src="images/02-auditd-rule-passwd-ok.png" width="100%">

Criação de regra para monitorar `/etc/shadow`:

<img src="images/03-auditd-rule-shadow-ok.png" width="100%">

👉 Agora qualquer acesso a esses arquivos será registrado

---

## 🚨 Evento Detectado — Acesso ao Shadow

Foi realizada tentativa de acesso ao `/etc/shadow`:

<img src="images/04-shadow-access-event.png" width="100%">

Eventos registrados pelo auditd:

<img src="images/05-auditd-shadow-events.png" width="100%">

---

## 📡 Detecção no SIEM (Wazuh)

O Wazuh identificou o evento:

<img src="images/06-wazuh-detection-shadow.png" width="100%">

---

## 🛠️ Criação de Regra Customizada

Foi criada uma regra personalizada no Wazuh para detectar acesso ao `/etc/shadow`:

<img src="images/07-wazuh-custom-rule-created.png" width="100%">

Reinício do serviço:

<img src="images/08-wazuh-restart-ok.png" width="100%">

---

## 🔥 Validação da Detecção

Novo acesso ao `/etc/shadow` disparou alerta customizado:

<img src="images/09-shadow-access-trigger-custom.png" width="100%">

Alerta gerado no Wazuh:

<img src="images/10-wazuh-custom-alert.png" width="100%">

---

## 🧠 Análise SOC

O acesso ao arquivo `/etc/shadow` representa um comportamento altamente sensível e potencialmente malicioso.

Evidências:

- Evento capturado pelo auditd  
- Logs detalhando acesso ao arquivo crítico  
- Detecção e alerta no SIEM (Wazuh)  
- Regra customizada validada  

Correlação:

- Host (auditd) → evento bruto  
- SIEM (Wazuh) → detecção e alerta  

Impacto:

- Possível exposição de hashes de senha  
- Risco de credential dumping  
- Potencial escalonamento de privilégio  

Objetivo do atacante:

- Coletar credenciais  
- Escalar privilégios  
- Comprometer o sistema  

Conclusão:

Atividade altamente suspeita com alto impacto potencial.

---

## 🚨 Classificação

Malicioso — tentativa de acesso a credenciais sensíveis.

---

## 🧬 MITRE ATT&CK

- T1003 — OS Credential Dumping  
- TA0006 — Credential Access  

---

## 🧠 Habilidades Demonstradas

- Configuração de auditd  
- Monitoramento de arquivos críticos  
- Integração com SIEM (Wazuh)  
- Criação de regras customizadas  
- Detecção de comportamento sensível  
- Análise de eventos de segurança  

---

## 📌 Conclusão

O laboratório demonstrou a implementação de monitoramento ativo de arquivos críticos e a criação de detecção personalizada no SIEM.

A abordagem permite identificar rapidamente tentativas de acesso a credenciais, fortalecendo a capacidade de resposta a incidentes.

---

## 📬 Contato

Aberto a oportunidades em SOC / NOC / Cybersecurity Jr.

- LinkedIn: https://www.linkedin.com/in/tiago-krysiaki  
- Email: t.krysiaki91@gmail.com  
