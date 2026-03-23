
# 🔍 Lab 14 — File Access Monitoring & Investigation (auditd + Wazuh)

## 📌 Objetivo

Neste laboratório, o foco será monitorar e investigar acessos a arquivos sensíveis no sistema Linux utilizando o `auditd`, correlacionando os eventos com o Wazuh (SIEM).

O objetivo é evoluir de detecção básica para análise comportamental, entendendo ações realizadas no sistema e identificando atividades suspeitas.

---

## 🧠 Contexto

Após um possível acesso inicial (ex: brute force), um atacante pode tentar acessar arquivos críticos do sistema como:

- `/etc/passwd`
- `/etc/shadow`

Esse tipo de atividade pode indicar:
- Enumeração de usuários  
- Tentativa de coleta de credenciais  
- Movimentação lateral  

---

## 🧱 Ambiente do Lab

- Kali Linux → (opcional, para simulações)  
- Ubuntu → alvo (auditd + logs + Wazuh Agent)  
- Wazuh Server → SIEM (detecção e correlação)  

---

## 🔍 O que será feito

- Configuração do `auditd` para monitorar arquivos críticos  
- Geração de eventos reais de acesso a arquivos  
- Análise de logs (`/var/log/audit/audit.log`)  
- Correlação dos eventos no Wazuh  
- Investigação do comportamento (quem, quando, o que fez)  

---

## 🧠 Abordagem SOC

Este lab será focado em investigação:

- Identificação de comportamento suspeito  
- Análise de contexto (usuário, processo, ação)  
- Classificação da atividade (legítima vs maliciosa)  

---

## 🔁 Fluxo do Lab
Ação → Log (auditd) → Detecção (Wazuh) → Investigação


---

## 🚀 Status

🔜 Em construção


