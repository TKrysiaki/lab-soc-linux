# 🧠 LAB 14 — Monitoramento e Detecção de Acesso a Arquivos Sensíveis (auditd + Wazuh)

---

## 🎯 Objetivo

Monitorar, detectar e investigar acessos a arquivos sensíveis no Linux (`/etc/passwd` e `/etc/shadow`) utilizando **auditd** e **Wazuh**, evoluindo até detecção customizada **(Detection Engineering)**.

---

## 🧪 Ambiente
- Kali Linux (ataque)
- Ubuntu (alvo)
- Wazuh (SIEM)

---

## 📖 Cenário

Usuário `tiago` eleva privilégio com `sudo` e acessa `/etc/shadow`, simulando coleta de credenciais.

---

## 🔎 Investigação

O usuário `tiago` realizou elevação de privilégio utilizando sudo e acessou o arquivo `/etc/shadow`, que contém hashes de senha.

Esse comportamento é altamente sensível e pode indicar tentativa de coleta de credenciais no sistema.

---

## ⚙️ Etapa 1 — Instalação do auditd
```
sudo apt install auditd -y
```


### 🔎 Explicação
- `apt install` → instala pacote
- `auditd` → serviço de auditoria
- `-y` → confirma automaticamente

📌 Função: registrar eventos de segurança no sistema

```
sudo systemctl enable auditd
```
```
sudo systemctl start auditd
```

### 🔎 Explicação
- `systemctl` → gerencia serviços
- `enable` → inicia no boot
- `start` → inicia agora

📌 Sem isso, não há logs

<img src="images/01-auditd-install-service.png" width="100%">

---

## ⚙️ Etapa 2 — Criar regras de monitoramento
```
sudo auditctl -D
```

### 🔎 Explicação
- `auditctl` → gerencia regras
- `-D` → deleta todas as regras

📌 Garante ambiente limpo


```
sudo auditctl -w /etc/passwd -p r -k passwd-monitor
```

### 🔎 Explicação detalhada
- `-w /etc/passwd` → monitora arquivo
- `-p r` → captura leitura
- `-k passwd-monitor` → chave para busca

📌 Detecta enumeração de usuários

```
sudo auditctl -w /etc/shadow -p r -k shadow-monitor
```

### 🔎 Explicação detalhada
- Monitora arquivo de credenciais
- Acesso aqui = alto risco

<img src="images/02-auditd-rule-passwd-ok.png" width="100%">


---

## ⚙️ Etapa 3 — Gerar evento
```
sudo cat /etc/shadow
```

### 🔎 Explicação
- `cat` → lê conteúdo do arquivo
- `sudo` → executa como root

📌 Simula atacante acessando credenciais

<img src="images/04-shadow-access-event.png" width="100%">

---

## ⚙️ Etapa 4 — Analisar logs
```
sudo ausearch -k shadow-monitor
```

### 🔎 Explicação
- `ausearch` → busca eventos do auditd
- `-k` → filtra pela chave definida

### 📌 Retorna:

- usuário
- comando
- arquivo acessado
- timestamp

<img src="images/05-auditd-shadow-events.png" width="100%">

---

## 🔗 Etapa 5 — Wazuh (detecção inicial)

<img src="images/06-wazuh-detection-shadow.png" width="100%">

## 🔧 Detection Engineering (Melhoria de Detecção)

A regra padrão do Wazuh detecta apenas o uso de sudo, o que gera alertas genéricos.

Foi criada uma regra customizada para identificar especificamente o acesso ao arquivo `/etc/shadow`, aumentando a precisão e a severidade da detecção.

---

### 🔎 Análise
- Regra 5402 → detecta apenas uso de sudo
- ❌ Baixo valor analítico (não contextualiza o comportamento)

➡️ Problema:
Não identifica o acesso a arquivos sensíveis, gerando alertas genéricos.

---

## 🔧 Etapa 6 — Detection Engineering
### 📍 Editar regra
```
sudo nano /var/ossec/etc/rules/local_rules.xml
```

### 🔎 Explicação
- `nano` → editor
- arquivo → regras customizadas do Wazuh

### 📍 Regra criada
```
<rule id="100100" level="12">
  <if_sid>5402</if_sid>
  <match>/etc/shadow</match>
  <description>Alto risco: acesso ao /etc/shadow via sudo</description>
  <mitre>
    <id>T1003</id>
  </mitre>
</rule>
```

### 🔎 Explicação
- `if_sid 5402` → evento base (sudo)
- `match /etc/shadow` → filtra alvo crítico
- `level 12` → alerta alto
- MITRE → credential dumping

📌 Agora a detecção é contextual

<img src="images/07-wazuh-custom-rule-created.png" width="100%">

### 📍 Reiniciar serviço
```
sudo systemctl restart wazuh-manager
```

### 🔎 Explicação
- recarrega regras
- obrigatório após alteração

<img src="images/08-wazuh-restart-ok.png" width="100%">

---

## ⚙️ Etapa 7 — Teste da regra
```
sudo cat /etc/shadow
```

📌 Gera novo evento para validar regra

### 🔥 Resultado final no Wazuh

<img src="images/10-wazuh-custom-alert.png" width="100%">

### 📊 Timeline
```bash
18:55 → auditd iniciado  
18:56 → regras criadas  
18:57 → acesso ao shadow  
19:01 → alerta padrão  
19:32 → alerta customizado (alto risco)
```

---

## 🔍 Análise SOC
- Usuário: tiago
- Ação: leitura de credencial
- Método: sudo + cat
- Resultado: sucesso

➡️ Comportamento suspeito real

---

## ⚖️ Classificação

**True Positive — Atividade Suspeita**

- Acesso a arquivo sensível (`/etc/shadow`)
- Uso de privilégio elevado (`sudo`)
- Comportamento incomum

---

## 💥 Impacto

- Exposição de hashes de senha
- Possível quebra de credenciais offline
- Escalada de privilégio
- Risco de movimentação lateral

---

## 🧬 MITRE ATT&CK
- T1003 → Credential Dumping
- T1548.003 → Sudo abuse
- T1078 → Valid accounts

---

## 🚨 Resposta
- Revisar sudo
- Monitorar usuário
- Hardening
- Melhorar regras

---

## 🧠 Conclusão

Este laboratório demonstra um fluxo completo de investigação em ambiente SOC, incluindo coleta de logs, correlação de eventos e criação de detecção customizada.

A criação da regra no Wazuh permitiu transformar um alerta genérico em uma detecção de alto valor, evidenciando capacidade prática de análise e melhoria contínua de segurança.

---

## 🎯 Habilidades Demonstradas

- Linux
- auditd
- Wazuh
- Log Analysis
- Correlação
- MITRE
- Detection Engineering

---

## 🔥 Resultado Final

👉 Fluxo completo de SOC
👉 Detecção + investigação + melhoria


































