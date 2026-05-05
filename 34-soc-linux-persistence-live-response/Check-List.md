[ ] Network IoCs documentados
    [ ] IP do atacante identificado
    [ ] Portas utilizadas identificadas (4444/4445)
    [ ] Conexões suspeitas registradas via ss -antp

[ ] Host IoCs documentados
    [ ] Nome do serviço malicioso identificado (sys-update.service)
    [ ] Caminho do arquivo persistente documentado
    [ ] Hash do script/binário coletado
    [ ] Chave SSH maliciosa identificada
    [ ] Arquivos alterados registrados

[ ] Authentication IoCs documentados
    [ ] Usuário comprometido identificado
    [ ] Sessões ativas registradas
    [ ] Histórico de login analisado com last

[ ] Persistence IoCs documentados
    [ ] Cron persistence identificada
    [ ] SSH authorized_keys persistence identificada
    [ ] Systemd persistence identificada

[ ] Process IoCs documentados
    [ ] PID malicioso identificado
    [ ] Processo pai identificado
    [ ] Linha de comando capturada via auditd

[ ] Detection Context documentado
    [ ] Regra Wazuh acionada
    [ ] Timestamp do alerta registrado
    [ ] Técnica MITRE associada
