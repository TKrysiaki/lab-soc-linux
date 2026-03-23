# 🔍 Lab 14 — Monitoramento e Investigação de Acesso a Arquivos (auditd + Wazuh)

## 📌 Objetivo

Neste laboratório, o foco será monitorar e investigar acessos a arquivos sensíveis no sistema Linux utilizando o `auditd`, correlacionando os eventos com o Wazuh (SIEM).

O objetivo é evoluir de uma detecção simples para uma análise comportamental, entendendo ações realizadas no sistema e identificando atividades suspeitas.

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

- Kali Linux → (opcional, para simulação de ações)  
- Ubuntu → alvo (auditd + logs + Wazuh Agent)  
- Wazuh Server → SIEM (detecção e correlação)  

---

## 🔍 O que será feito

- Configurar o `auditd` para monitorar arquivos críticos  
- Gerar eventos reais de acesso a arquivos  
- Analisar logs (`/var/log/audit/audit.log`)  
- Correlacionar eventos no Wazuh  
- Investigar o comportamento (quem, quando e o que foi feito)  

---

## 🧠 Abordagem SOC

Este laboratório será focado em investigação:

- Identificação de comportamento suspeito  
- Análise de contexto (usuário, processo, ação)  
- Classificação da atividade (legítima ou maliciosa)  

---

## 🔁 Fluxo do Lab
Ação → Log (auditd) → Detecção (Wazuh) → Investigação


---

## 🚀 Status

🔜 Em construção
