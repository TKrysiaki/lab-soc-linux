## IoCs (Indicators of Compromise)

### Network IoCs
- [x] IP do atacante identificado
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
