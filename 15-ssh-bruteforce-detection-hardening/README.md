# 🔥 Lab 15 — Detecção de Brute Force em SSH e Hardening

## 📌 Objetivo
Detectar, analisar e responder a um ataque de brute force no serviço SSH, aplicando um fluxo real de investigação SOC.

---

## 🧪 Ambiente do Lab

- Atacante: Kali Linux
- Alvo: Ubuntu Server
- Logs analisados: `/var/log/auth.log`

---

## 🚨 Cenário do Ataque

Foi realizado um ataque de força bruta contra o serviço SSH, gerando múltiplas tentativas de login falhadas contra um usuário válido.

---

## 🔍 Detecção — Tentativas Falhadas

### Comando
```
grep -a "Failed password" /var/log/auth.log
```

## Explicação
- `grep` → busca padrões dentro do log
- `-a` → força leitura como texto (evita erro de binário)
- `"Failed password"` → tentativas falhadas de login SSH
- `/var/log/auth.log` → log de autenticação

## Análise

### Foram identificadas múltiplas tentativas falhadas, caracterizando ataque de brute force.

<img src="images/02-bruteforce-failed-password.png" width="100%">

---

## 📊 Análise de Timeline — Início do Ataque

# Comando
```
grep -a "Failed password" /var/log/auth.log | head
```
# Explicação
- `grep` → filtra tentativas falhadas

- `head` → mostra as primeiras ocorrências


## Análise

### O ataque iniciou em:

- `2026-03-22 20:01:31`

### Eventos em alta frequência indicam automação.


<img src="images/04-attack-start-timeline.png" width="100%">

---

## 🔢 Volume do Ataque

### Comando
```
grep -a "Failed password" /var/log/auth.log | wc -l
```
### Explicação
- `wc -l` → conta o número de linhas
- cada linha representa uma tentativa de login

### Resultado
- 133 tentativas de login falhadas

### Análise

Volume elevado confirma brute force automatizado.

<img src="images/05-bruteforce-attempt-count.png" width="100%">

---

## 🔐 Verificação de Sucesso
### Comando
```
grep -a "Accepted password" /var/log/auth.log
```

### Explicação
- "Accepted password" → indica login SSH bem-sucedido

### Análise

Nenhum login bem-sucedido foi identificado, indicando que o ataque não resultou em comprometimento.

<img src="images/03-no-successful-login.png" width="100%">

---

## 🌐 Origem do Ataque
- IP de origem: `192.168.56.103`
- Tipo: Rede interna (IP privado)

### Análise

Indica ataque interno ou possível movimento lateral dentro da rede.

---

## 🧠 Classificação
- Tipo: Brute Force
- Status: Sem sucesso
- Severidade: Média a Alta
- Confiança: Alta

---

## 💥 Impacto
- Nenhum comprometimento identificado
- Credenciais não expostas
- Ataque ativo detectado

---

## 🛡️ Mitigação — Hardening do SSH
### Etapa 1 — Editar configuração
```
sudo nano /etc/ssh/sshd_config
```

### Explicação

Abre o arquivo de configuração do SSH para ajustes de segurança.

### Etapa 2 — Desabilitar login root
`PermitRootLogin no`

### Explicação

Bloqueia login direto como root, reduzindo a superfície de ataque.

<img src="images/07-ssh-root-login-disabled.png" width="100%">


### Etapa 3 — Aplicar mudanças
```
sudo systemctl restart ssh
```
### Explicação

Reinicia o serviço SSH para aplicar as alterações.

<img src="images/08-ssh-service-restarted.png" width="100%">

---

## 🎯 Conclusão

O ataque de brute force foi detectado e analisado com sucesso através dos logs do sistema.
Apesar de não haver comprometimento, medidas de hardening foram aplicadas para reforçar a segurança do serviço SSH e mitigar ataques futuros.

---

## 🧠 Habilidades Demonstradas
- Análise de logs (auth.log)
- Detecção de brute force
- Correlação de eventos
- Análise de timeline
- Classificação de incidente
- Resposta a incidente
- Hardening de SSH

---

## 🔗 MITRE ATT&CK
- T1110 — Brute Force
- T1078 — Valid Accounts (tentativa)

















