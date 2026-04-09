# 🚨 Detecção e Resposta a Brute Force SSH com Correlação e Threat Intelligence

## 📌 Visão Geral

Este laboratório simula um ataque real de brute force SSH em ambiente interno, seguido de investigação completa em um cenário SOC, incluindo:

- Detecção via logs e SIEM (Wazuh)
- Correlação de eventos
- Enriquecimento com Threat Intelligence
- Classificação do incidente
- Resposta e mitigação

---

## 🧪 Lab Environment
- Atacante: Ubuntu (script automatizado)
- Alvo: Ubuntu (SSH ativo)
- SIEM: Wazuh
- Defesa: Fail2ban
- Logs: `/var/log/auth.log`

---

## ⚔️ Simulação de Ataque

O ataque foi executado via script automatizado simulando comportamento stealth.

```
python3 adversary_simulator_v2.py
```

### 🧠 Explicação
- Executa recon + brute force
- Baixa taxa de tentativas → evasão de detecção
- Simula atacante real (não ruidoso)

<img src="images/01-attack-script-running.png" width="100%">

---

## 🔎 Investigação — Logs Linux

Monitoramento em tempo real:

```
sudo tail -f /var/log/auth.log
```

### 🧠 Análise SOC
- múltiplos Failed password
- invalid user → enumeração
- mesmo IP repetidamente

👉 Indício claro de brute force

<img src="images/02-failed-password-logs.png" width="100%">

---

## 🛡️ Detecção no SIEM (Wazuh)

### Alerta identificado:
- Regra: 5710
- Técnica: T1110.001 (Password Guessing)

### 🧠 Análise
- Tentativa com usuário inexistente
- Ataque em fase inicial

<img src="images/03-wazuh-ssh-invalid-user.png" width="100%">

---

## 🔍 Verificação de Comprometimento
```
sudo grep -a "Accepted password" /var/log/auth.log
```

### 🧠 Resultado
- ❌ Nenhum acesso bem-sucedido
- ✔️ Apenas tentativas falhas

👉 Ataque não comprometeu o sistema

<img src="images/04-no-successful-login.png" width="100%">

---

## 🌐 Threat Intelligence

### IP analisado:

```192.168.56.107```

Consulta em AbuseIPDB:

### 🧠 Resultado
- IP não listado
- Classificado como IP privado (interno)

<img src="images/05-threat-intel-private-ip.png" width="100%">

---

## ⚠️ Análise Crítica

> Ataque não é externo → origem interna

### Isso indica:

- possível máquina comprometida
- movimento lateral
- risco elevado

---

## 📊 Correlação de Eventos

- Logs Linux → brute force
- Wazuh → alerta confirmado
- Threat Intel → IP interno

👉 Correlação multi-fonte validada

<img src="images/06-wazuh-alerts-overview.png" width="100%">

---

## 🚨 Classificação do Incidente
- Tipo: Brute Force SSH
- MITRE:
  - T1110 — Brute Force
  - T1110.001 — Password Guessing
  - T1046 — Network Discovery
- Origem: Interna
- Status: Sem sucesso
- Classificação: ✅ True Positive
- Severidade: 🔴 Alta

---

## 🛡️ Resposta (Containment)

### Bloqueio do atacante:
```
sudo fail2ban-client set sshd banip 192.168.56.107
```

### Verificação:
```
sudo fail2ban-client status sshd
```
<img src="images/07-fail2ban-ip-banned.png" width="100%">

---

## 🔐 Mitigação (Hardening SSH)

### Configuração aplicada:
```
PasswordAuthentication no
PermitRootLogin no
```

### Reinício do serviço:
```
sudo systemctl restart ssh
```

<img src="images/08-ssh-hardening-config.png" width="100%">

---

## 🧠 Análise Final (SOC)
- Ataque detectado em estágio inicial
- Sem comprometimento
- Origem interna aumenta criticidade
- Resposta rápida evitou impacto

---

## 🎯 Conclusão

Este laboratório demonstra um fluxo completo de atuação em SOC:

- Detecção → Investigação → Correlação → Classificação → Resposta

O diferencial não está apenas em identificar o ataque, mas em:

> correlacionar dados, validar impacto e tomar decisão baseada em evidência

---

## 🧠 Skills Desenvolvidas
- Análise de logs Linux
- Investigação de brute force
- Uso de SIEM (Wazuh)
- Threat Intelligence
- Correlação de eventos
- Resposta a incidentes
- Hardening de serviços

---

## 📬 Contato

LinkedIn: https://www.linkedin.com/in/tiago-krysiaki

Email: t.krysiaki91@gmail.com





