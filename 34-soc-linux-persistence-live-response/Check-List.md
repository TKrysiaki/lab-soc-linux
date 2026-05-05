## Attack Scenario

### Initial Access
- [ ] RCE/Web Shell obtido com usuário `www-data`
- [ ] Execução remota de comandos validada
- [ ] Reverse shell estabelecida

---

### Persistence Simulation
- [ ] Persistência via Cron criada em `/etc/cron.d/`
- [ ] Persistência via SSH Key adicionada em `/root/.ssh/authorized_keys`
- [ ] Persistência via Systemd criada em `/etc/systemd/system/`

---

### Threat Activity
- [ ] Comandos suspeitos executados (`curl`, `wget`, `chmod`, `bash`)
- [ ] Arquivos suspeitos criados em `/tmp`
- [ ] Serviço malicioso habilitado com `systemctl enable`
- [ ] Tentativa de reconexão persistente validada

---

## IoCs (Indicators of Compromise)

### Network IoCs
- [ ] IP do atacante identificado
- [ ] Portas utilizadas identificadas (4444/4445)
- [ ] Conexões suspeitas registradas via `ss -antp`

---

### Host IoCs
- [ ] Nome do serviço malicioso identificado (`sys-update.service`)
- [ ] Caminho do arquivo persistente documentado
- [ ] Hash do script/binário coletado
- [ ] Chave SSH maliciosa identificada
- [ ] Arquivos alterados registrados

---

### Authentication IoCs
- [ ] Usuário comprometido identificado
- [ ] Sessões ativas registradas
- [ ] Histórico de login analisado com `last`

---

### Persistence IoCs
- [ ] Cron persistence identificada
- [ ] SSH authorized_keys persistence identificada
- [ ] Systemd persistence identificada

---

### Process IoCs
- [ ] PID malicioso identificado
- [ ] Processo pai identificado
- [ ] Linha de comando capturada via `auditd`

---

### Detection Context
- [ ] Regra Wazuh acionada
- [ ] Timestamp do alerta registrado
- [ ] Técnica MITRE ATT&CK associada
