# 📄 README — LAB 24
## 📌 24 - Detecção e Resposta a Brute Force SSH com Wazuh, Wireshark e Fail2ban

---

## 🎯 Visão Geral

### Este laboratório simula um ataque de brute force SSH em ambiente controlado, com foco em:

- Detecção via logs e SIEM (Wazuh)
- Análise de tráfego de rede (Wireshark/tcpdump)
- Correlação de eventos
- Classificação SOC (TP/FP + severidade)
- Resposta automatizada (Fail2ban)
- Enriquecimento com threat intelligence

---

## 🧪 Ambiente
- Atacante: Ubuntu (192.168.56.106)
- Alvo: Ubuntu (192.168.56.111)
- SIEM: Wazuh
- Ferramentas:
  - Hydra
  - tcpdump
  - Wireshark
  - Fail2ban
 
---

## 🚨 Simulação do Ataque

### Ataque automatizado utilizando Hydra contra serviço SSH.

<img src="images/01-hydra-ssh-attack.png" width="100%">

> O ataque gerou múltiplas tentativas de autenticação com diferentes usuários, simulando um cenário real de brute force.
