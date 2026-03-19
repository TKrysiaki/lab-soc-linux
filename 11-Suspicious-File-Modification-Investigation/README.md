# Lab 11 - Suspicious File Modification Investigation

## 🎯 Objective
Detect and analyze unauthorized modification of `/etc/passwd` using Wazuh.

---

## 🖥️ Lab Environment

- Attacker: Kali Linux
- Target: Ubuntu 24.04 (Wazuh Agent)
- SIEM: Wazuh

---

## 🚨 Attack Simulation

### Command (Ubuntu - Target)
```
echo 'hacker:x:0:0::/root:/bin/bash' | sudo tee -a /etc/passwd
```
### Short explanation (what + why)

> Adiciona um usuário malicioso no /etc/passwd com privilégio root.

### Analysis (SOC)

> Modificação direta em arquivo crítico → forte indicativo de persistência e privilege escalation.

### Screenshot
<img src="images/01-passwd-backdoor-created.png" width="100%">

### Command breakdown

- **echo** → cria conteúdo

- **'hacker:x:0:0::/root:/bin/bash'** → usuário com UID 0 (root)

- **tee -a** → adiciona ao arquivo

- **/etc/passwd** → base de usuários do sistema

---

## 🔍 Verification
### Command (Ubuntu - Target)
```bash
tail -n 5 /etc/passwd
```
### Short explanation (what + why)

> Exibe as últimas linhas para confirmar a alteração.

### Analysis (SOC)

> Confirma que o backdoor foi persistido no sistema.

### Screenshot
<img src="images/02-passwd-backdoor-confirmed.png" width="100%">

### Command breakdown

> **tail** → mostra final do arquivo

> **-n 5**  → últimas 5 linhas

/etc/passwd → arquivo analisado


























