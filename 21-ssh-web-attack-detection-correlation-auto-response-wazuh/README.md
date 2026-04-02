# 🔥 Lab 21 — Detecção, Correlação e Resposta Automática a Ataque SSH + Recon Web com Wazuh

---

## 📌 Resumo Executivo

Foi identificado um ataque coordenado originado do IP **192.168.56.103**, combinando brute force SSH e enumeração web.  
A correlação de eventos permitiu identificar comportamento malicioso e aplicar bloqueio automático via Wazuh, interrompendo o ataque em tempo real.

---

## 🎯 Visão Geral

Este laboratório simula um cenário real de SOC, onde um atacante tenta:

- Obter acesso via força bruta (SSH)
- Mapear a aplicação web (recon)

O objetivo é executar o ciclo completo de resposta a incidente:
**Detecção → Investigação → Correlação → Resposta**

---

## 🧱 Ambiente

| Máquina | Função |
|--------|--------|
| Kali   | Atacante |
| Ubuntu | Alvo |
| Wazuh  | SIEM |

---

## 🚨 Ataque

### 🔴 Hydra (Brute Force SSH)

```bash
hydra -l tiago -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.107

---
xxxx
