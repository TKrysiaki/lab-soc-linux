# SOC Linux Lab — Análise de Logs e Investigação de Incidente

Este repositório contém laboratórios práticos de **Cybersecurity** focados em atividades de um **Security Operations Center (SOC)** utilizando ambientes Linux.

O projeto simula um cenário real de ataque **SSH Brute Force** e demonstra todo o processo de investigação através da análise de logs e documentação do incidente.

---

## 🎯 Objetivos

- Praticar o fluxo de investigação SOC Tier 1
- Analisar logs de autenticação Linux
- Identificar comportamento do atacante
- Documentar incidentes de segurança com evidências
- Desenvolver habilidades de defesa (Blue Team)

---

## 🧪 Cenário do Laboratório

- Máquina atacante: Kali Linux
- Máquina alvo: Ubuntu Server 22.04
- Serviço analisado: SSH
- Attack Type: SSH Brute Force
- Fonte dos logs: `/var/log/auth.log`

---

## 🔎 Etapas da Investigação

- Identificação do IP atacante
- Análise da timeline do ataque
- Identificação do usuário alvo
- Verificação de login bem-sucedido
- Conclusão do incidente

---
📂 Etapas do laboratório:

📁 Etapas do laboratório:

> - 01-log-analysis-base.md — Detecção inicial do incidente  
> - 02-ssh-bruteforce-investigation.md — Simulação do ataque  
03-log-analysis-investigation.md — Investigação SOC N1  
04-ssh-bruteforce-detection-and-fail2ban.md — Mitigação do ataque com Fail2ban  
05-ssh-hardening.md — Hardening do serviço SSH  
06-auth-log-deep-analysis.md — Análise avançada de logs  
07-user-activity-investigation.md — Investigação de atividade suspeita  
08-suspicious-login-detection.md — Detecção de login suspeito  
09-privilege-escalation-analysis.md — Análise de escalonamento de privilégio  
10-unauthorized-file-access-investigation.md — Investigação de acesso não autorizado a arquivos  

---

## 🔎 MITRE ATT&CK Mapping
- T1110 - Brute Force
- T1078 - Valid Accounts

---

## 🛠️ Habilidades Praticadas

- Análise de logs Linux
- Investigação de incidentes
- Monitoramento de segurança SSH
- Fundamentos de Blue Team
- Documentação de processos SOC

---

## 🚀 Próximos Passos

- Implementação de defesa ativa com Fail2ban
- Bloqueio automático de IP atacante
- Expansão do fluxo de resposta a incidentes

---

## 👨‍💻 Autor

Estudante de Cybersecurity com foco em Blue Team, análise SOC e segurança defensiva.
