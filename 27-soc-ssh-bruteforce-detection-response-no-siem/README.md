# 🚨 SOC Lab 27 — Detecção e Resposta a Brute Force (Sem SIEM)

## 🧠 Visão Geral
- **Objetivo do lab**:

  Simular a detecção e resposta a incidentes sem uso de SIEM, realizando análise manual de logs.
- **Tipo de ataque**:

  Brute Force SSH (sem sucesso)
- **Ferramentas utilizadas**:

    `/var/log/auth.log`, Fail2ban, VirusTotal
- **Resultado final**:

  Ataque identificado e contido com sucesso, sem evidência de comprometimento.



  ---

## 🌐 Ambiente
- Máquina atacante: Ubuntu Linux
- Máquina alvo: Ubuntu Linux (SSH habilitado)
- Rede: Ambiente interno (rede privada) via SSH (porta 22)

---

## ⚔️ Simulação de Ataque

O ataque foi realizado por meio de script automatizado, simulando brute force contra o serviço SSH.

Foi observado alto volume de tentativas de login mal-sucedidas em curto período, indicando comportamento automatizado.

<img src="images/01-bruteforce-ssh-failed-login.png" width="100%">

---

## 🔍 Detecção e Análise

A detecção foi realizada via análise manual do arquivo `/var/log/auth.log`.

```
sudo tail -f /var/log/auth.log | grep "Failed password"
```

**Foi identificado**:

- Múltiplas tentativas de login
- Mesmo IP (`192.168.56.108`)
- Tentativas contra usuário `root`

<img src="01-bruteforce-ssh-failed-login.png" width="100%">

---

## 🔎 Verificação de Comprometimento
```
sudo grep "Accepted password" /var/log/auth.log
```
Nenhum login bem-sucedido foi identificado.

<img src="02-no-successful-login.png" width="100%">

---

## 🛡️ Resposta ao Incidente

Mitigação realizada com Fail2ban, bloqueando o IP atacante.

<img src="03-mitigacao-fail2ban-bloqueio-ip.png" width="100%">

---

## ✅ Validação da Mitigação
```
sudo tail -f /var/log/auth.log
```
Nenhuma nova tentativa identificada após bloqueio.

<img src="04-validacao-mitigacao-ataque-bloqueado.png" width="100%">

---

## 🌍 Threat Intelligence

IP analisado no VirusTotal → sem reputação maliciosa (IP privado).

<img src="05-threat-intel-ip-interno-clean.png" width="100%">

---

## 🧬 Pós-Exploração
```
sudo grep "session opened" /var/log/auth.log
```
Nenhuma evidência de atividade maliciosa.

<img src="06-pos-exploracao-nao-identificada.png" width="100%">

---

## 📊 Classificação do Incidente
- Tipo: Brute Force (SSH)
- Severidade: Média
- Status: Contido e encerrado
- MITRE ATT&CK: T1110

---

## 🔗 Kill Chain
- Reconnaissance: Não observado
- Weaponization: Uso de ferramenta automatizada
- Delivery: Conexões SSH (porta 22)
- Exploitation: Tentativas de login
- Installation: Não ocorreu
- Command & Control: Não ocorreu
- Actions on Objectives: Não ocorreu

> **Ataque interrompido na fase de exploitation, sem comprometimento.**

---

## 🧠 Skills Desenvolvidas

- Análise manual de logs (auth.log)
- Detecção de brute force sem SIEM
- Investigação de incidentes (SOC workflow)
- Correlação de eventos (log + comportamento)
- Resposta a incidentes (Fail2ban)
- Validação de mitigação
- Threat Intelligence (VirusTotal)
- Classificação de incidentes (TP / severidade)
- Mapeamento MITRE ATT&CK (T1110)

---

## 🧾 Conclusão

Este laboratório demonstrou a detecção e resposta a um ataque de brute force sem uso de `SIEM`, utilizando análise manual de logs como principal fonte de evidência.

O ataque foi identificado por múltiplas tentativas de login mal-sucedidas via SSH. A investigação confirmou que não houve autenticação bem-sucedida, descartando comprometimento.

A mitigação foi aplicada com `Fail2ban`, bloqueando o IP atacante e interrompendo o ataque. A validação confirmou a eficácia da resposta.

A análise de `Threat Intelligence` indicou tratar-se de um IP privado, e não foram identificadas evidências de pós-exploração.

O incidente foi tratado e encerrado com sucesso no nível N1.

---

## 📬 Contato

LinkedIn: https://www.linkedin.com/in/tiago-krysiaki

Email: t.krysiaki91@gmail.com













