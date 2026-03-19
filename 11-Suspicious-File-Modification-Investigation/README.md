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

> **/etc/passwd** → arquivo analisado

---

## 🕒 Timeline Reference
### Command (Ubuntu - Target)
```
date
```
### Short explanation (what + why)

> Obtém horário atual para correlação com logs.

### Analysis (SOC)

> Permite alinhar evento com logs do SIEM.

### Screenshot
<img src="images/05-system-time.png" width="100%">

### Command breakdown

- **date** → mostra data e hora do sistema

---

## 🧠 Wazuh - Access
### Short explanation (what + why)

> Acessar dashboard para análise dos eventos.

### Analysis (SOC)

### Confirma que o agente está enviando logs corretamente.

### Screenshot
<img src="images/10-wazuh-dashboard-access.png" width="100%">

## 📈 Events Analysis
### Short explanation (what + why)

> Visualizar eventos relacionados ao /etc/passwd.

## Analysis (SOC)

### Identificação de atividade suspeita no sistema.

### Screenshot
<img src="images/11-wazuh-security-events-passwd.png" width="100%">

---

## 🚨 Detection

### Short explanation (what + why)
> Analisar alertas gerados pelo Wazuh.

### Analysis (SOC)

> Eventos mostram uso de sudo e modificação crítica.

### Screenshot
<img src="images/17-wazuh-passwd-events-detected.png" width="100%">

---

## 🔬 File Integrity Monitoring (FIM)
### Short explanation (what + why)

> Monitoramento de integridade detecta alteração no arquivo.

### Analysis (SOC)

> Alteração de hash confirma modificação real → alta severidade.

### Screenshot
<img src="images/20-wazuh-fim-passwd-modified.png" width="100%">

---

## 🧭 Timeline

- Execução do ataque

- Modificação do /etc/passwd

- Detecção via Wazuh

- Geração de alerta

---

## 🧠 MITRE ATT&CK

- T1548.003 → Abuse Elevation Control Mechanism (sudo)

- T1565.001 → Data Manipulation

---

## 🛡️ Mitigation

- Restringir sudo

- Monitorar arquivos críticos

- Implementar FIM

- Aplicar least privilege

---

🧰 Skills Developed

- Log analysis

- SIEM investigation

- File Integrity Monitoring

- Incident response

- MITRE mapping














































