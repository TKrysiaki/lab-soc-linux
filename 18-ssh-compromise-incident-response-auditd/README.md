# 🚨 Detecção de Ataque Brute Force SSH com Análise SOC

---

## 🎯 Cenário

Foi identificado comportamento suspeito no serviço SSH de um servidor Linux, indicando tentativa de acesso não autorizado através de múltiplas autenticações falhadas.

---

## ⚔️ Simulação de Ataque

Foi executado um ataque de força bruta utilizando Hydra a partir de uma máquina atacante (Kali Linux) contra o servidor alvo (Ubuntu):

hydra -l user -P rockyou.txt ssh://192.168.0.5

O objetivo do atacante é descobrir credenciais válidas e obter acesso inicial ao sistema.

---

## 📊 Análise de Logs

O arquivo `/var/log/auth.log` registrou diversas tentativas de login falhadas:

A presença de múltiplos eventos "Failed password" em sequência indica um padrão anômalo de autenticação.

<img src="images/01-ssh-failed-login-kali.png" width="100%">

Esse comportamento não é típico de usuários legítimos e sugere tentativa automatizada de acesso.

---

## 🔍 Investigação

A análise dos eventos mostra:

- Repetição de tentativas em curto intervalo de tempo  
- Mesmo IP de origem realizando diversas tentativas  
- Ausência de sucesso inicial nas autenticações  

Esse padrão é consistente com ferramentas de brute force como Hydra.

<img src="images/02-multiple-attempts-same-ip.png" width="100%">

---

## 🧠 Análise SOC

O evento caracteriza um ataque de força bruta contra o serviço SSH.

Correlação dos eventos:
- Logs de falha de autenticação (auth.log)
- Padrão repetitivo e automatizado
- Origem única (IP atacante)

Detalhes:
- IP atacante: 192.168.0.10  
- Serviço alvo: SSH (porta 22)  
- Técnica: tentativa de adivinhação de credenciais  

Impacto potencial:
Caso o ataque seja bem-sucedido, o invasor pode obter acesso inicial ao sistema, executar comandos remotos, movimentar lateralmente e comprometer outros ativos da rede.

Objetivo do atacante:
Obter acesso inicial (Initial Access)

Conclusão:
Atividade maliciosa confirmada, exigindo ação imediata.

<img src="images/03-wazuh-alert-bruteforce.png" width="100%">

---

## 🚨 Classificação

Malicioso — ataque de força bruta confirmado.

---

## 🛡️ Mitigação

Foi implementado bloqueio automático utilizando Fail2ban, impedindo novas tentativas do IP atacante.

Ações realizadas:

- Bloqueio do IP malicioso  
- Limitação de tentativas de login  
- Reforço na política de autenticação  

Recomendações adicionais:

- Utilizar autenticação via chave SSH  
- Desabilitar login por senha  
- Monitoramento contínuo via SIEM  

<img src="images/04-fail2ban-ban-ip.png" width="100%">

---

## 🧬 MITRE ATT&CK

- T1110 — Brute Force  
- TA0001 — Initial Access  

---

## 🧠 Habilidades Demonstradas

- Análise de logs Linux (auth.log)  
- Detecção de comportamento malicioso  
- Correlação de eventos  
- Uso de SIEM (Wazuh)  
- Resposta a incidente com Fail2ban  
- Classificação de ameaças  

---

## 📌 Conclusão

O laboratório demonstrou um cenário realista de ataque de força bruta, desde a detecção até a resposta.

A análise evidenciou a importância do monitoramento contínuo, correlação de eventos e aplicação de mecanismos automáticos de defesa para proteção de serviços críticos como SSH.
