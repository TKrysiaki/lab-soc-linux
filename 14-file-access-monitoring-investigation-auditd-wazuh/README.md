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
- 
<img src="images/02-auditd-rule-passwd-ok.png" width="100%">
<img src="images/03-auditd-rule-shadow-ok.png" width="100%">


































