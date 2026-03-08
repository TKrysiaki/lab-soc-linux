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
- Tipo de ataque: Brute Force
- Fonte dos logs: `/var/log/auth.log`

---

## 🔎 Etapas da Investigação

- Identificação do IP atacante
- Análise da timeline do ataque
- Identificação do usuário alvo
- Verificação de login bem-sucedido
- Conclusão do incidente
- 
---
📂 Etapas do laboratório:

1️⃣ 01-log-analysis-base.md — Detecção inicial do incidente  
2️⃣ 02-ssh-bruteforce-investigation.md — Simulação do ataque  
3️⃣ 03-log-analysis-investigation.md — Investigação SOC N1
4️⃣ 04-ssh-bruteforce-detection-and-fail2ban.md — Mitigação do ataque com Fail2ban

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
