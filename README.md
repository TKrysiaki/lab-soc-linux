# 🛡️ SOC Linux Labs – Detection, Investigation & Response

Laboratórios práticos focados em operações de **SOC (Security Operations Center)** utilizando **Linux, Wazuh (SIEM), auditd e análise de logs**.

---

## 🎯 Objetivo

Simular cenários reais de ataque e resposta, cobrindo:

- monitoramento
- Detection
- Investigation (timeline + IoCs)
- Classification (MITRE ATT&CK)
- Response (containment + remediation)

---

## 🧠 Metodologia SOC
Monitoring → Detection → Investigation → Classification → Response


---

## 🧰 Ferramentas

- Wazuh (SIEM)
- auditd
- auth.log
- Fail2ban
- iptables
- Linux CLI

---

## 📊 Labs em Destaque

| Lab | Descrição | Tecnologias | MITRE |
|-----|----------|------------|-------|
| [29](https://github.com/TKrysiaki/lab-soc-linux/tree/main/29-soc-ssh-bruteforce-detection-response-persistence-log-tampering) | SSH brute force + persistence + log tampering | Wazuh, auth.log | T1110, T1078 |
| [28](https://github.com/TKrysiaki/lab-soc-linux/tree/main/28-soc-ssh-bruteforce-incident-response-persistence) | Incidente com persistência | Wazuh, Linux | T1110 |
| [27](https://github.com/TKrysiaki/lab-soc-linux/tree/main/27-soc-ssh-bruteforce-detection-response-no-siem) | Detecção sem SIEM | Linux logs | T1110 |
| [26](https://github.com/TKrysiaki/lab-soc-linux/tree/main/26-soc-ssh-bruteforce-incident-correlation-network) | Correlação de rede | Wazuh, logs | T1110 |
| [25](https://github.com/TKrysiaki/lab-soc-linux/tree/main/25-soc-ssh-bruteforce-detection-response-persistence-remediation) | Detecção + resposta + remediação | Wazuh, Fail2ban | T1110 |
| [24](https://github.com/TKrysiaki/lab-soc-linux/tree/main/24-soc-ssh-bruteforce-detection-response-correlation) | Correlação de eventos | Wazuh | T1110 |
| [23](https://github.com/TKrysiaki/lab-soc-linux/tree/main/23-soc-incident-response-bruteforce-correlation) | Investigação de incidente | Wazuh | T1110 |
| [22](https://github.com/TKrysiaki/lab-soc-linux/tree/main/22-soc-bruteforce-detection-response-wazuh) | Detecção com SIEM | Wazuh | T1110 |
| [21](https://github.com/TKrysiaki/lab-soc-linux/tree/main/21-ssh-web-attack-detection-correlation) | Ataque web + correlação | Wazuh | T1190 |
| [20](https://github.com/TKrysiaki/lab-soc-linux/tree/main/20-ssh-bruteforce-detection-response-wazuh-fail2ban) | Wazuh + Fail2ban | Wazuh | T1110 |

---

## 📂 Estrutura dos Labs

Cada lab contém:

- `README.md` → visão geral
- `report.md` → relatório SOC detalhado
- evidências (logs, alertas, prints)

---

## 🔍 Exemplo de Detecção (Wazuh)

- Rule: `40112`
- Evento: múltiplas falhas de login + sucesso
- Classificação:
  - T1110 – Brute Force
  - T1078 – Valid Accounts

---

## 🚀 Próximos Labs

- Web shell detection (Linux)
- DNS tunneling / exfiltration
- Correlação avançada no Wazuh

---

## 📎 Repositório

👉 https://github.com/TKrysiaki/lab-soc-linux

---

## 🧑‍💻 Autor

Tiago Krysiaki  
Foco: SOC N1 | Blue Team | Threat Detection

LikidIn: https://www.linkedin.com/in/tiago-krysiaki
