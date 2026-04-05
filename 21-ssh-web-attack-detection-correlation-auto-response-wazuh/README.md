# 🔐 Detecção, Correlação e Resposta a Ataques SSH e Web com Wazuh

---

## 🎯 Visão Geral

Este laboratório simula um cenário real de SOC onde múltiplos vetores de ataque são executados contra um ambiente monitorado.

O objetivo é demonstrar como um analista SOC:

- Detecta diferentes tipos de ataque
- Correlaciona eventos distintos
- Identifica padrões maliciosos
- Executa resposta automatizada

---

## 🧠 Por que este Lab é importante

Este laboratório eleva o nível ao demonstrar:

- Correlação de múltiplos eventos
- Detecção de ataques simultâneos
- Resposta automatizada baseada em regras
- Visão mais próxima de ambiente real

➡️ Não é apenas detecção isolada — é **correlação de ataque**

---

## 🖥️ Arquitetura do Laboratório

- Atacante: Ubuntu
- Alvo: Linux (SSH + Web)
- SIEM: Wazuh
- Defesa: Fail2ban

---

## ⚔️ Simulação de Ataque

Foram executados múltiplos ataques:

- Brute Force SSH (Hydra)
- Scan de portas (Nmap)
- Enumeração web (Feroxbuster)

### Execução

```
hydra -L users.txt -P rockyou.txt ssh://IP-ALVO
```
<img src="images/01-hydra-attack.png" width="100%">
