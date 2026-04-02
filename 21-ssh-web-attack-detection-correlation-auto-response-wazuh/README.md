# 🔥 Lab 21 — Detecção, Correlação e Resposta Automática a Ataque SSH + Recon Web com Wazuh

---

## 📌 Resumo Executivo

Um ataque coordenado foi identificado a partir do IP **192.168.56.103**, envolvendo brute force SSH e enumeração web.  
O Wazuh detectou padrões anômalos, permitindo a correlação entre eventos e a execução de resposta automatizada via Active Response, bloqueando o atacante em tempo real.

---

## 🎯 Visão Geral

Este laboratório simula um cenário real de SOC, onde um atacante realiza:

- Brute force SSH (Hydra)
- Enumeração web (Gobuster)

---

## 🧱 Ambiente do Lab

| Componente     | Função              |
|---------------|--------------------|
| Kali Linux    | Máquina atacante   |
| Ubuntu Server | Máquina alvo       |
| Wazuh Server  | SIEM / Detecção    |

---

## 🚨 Simulação de Ataque

### 🔴 Brute Force SSH

```bash
hydra -l tiago -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.107
