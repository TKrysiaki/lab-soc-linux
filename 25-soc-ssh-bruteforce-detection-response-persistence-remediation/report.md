# Incident Report - SSH Brute Force with Persistence

## Summary
Ataque de brute force SSH resultou em acesso não autorizado e persistência no host.

- Acesso: ✔ Sim  
- Persistência: ✔ Sim  
- Severidade: Alta  

---

## Key Events
- Múltiplas tentativas de login (Failed password)
- Login bem-sucedido (Accepted password)
- Criação de conta maliciosa
- Persistência via SSH Authorized Keys
- Bloqueio do IP (Fail2ban)

---

## IoCs
- IP: [192.168.56.139]
- Porta: 22/TCP
- Usuários alvo: root, admin, ubuntu
- Arquivo: /home/tiago/.ssh/authorized_keys
- Padrão: múltiplos "Failed password"

---

## MITRE ATT&CK
- T1110 – Brute Force
- T1078 – Valid Accounts
- T1098.004 – SSH Authorized Keys
- T1136 – Create Account

---

## Response
- IP bloqueado (Fail2ban)
- Usuário malicioso removido
- Chave SSH removida
- Senhas resetadas

---

## Hardening
- PermitRootLogin no
- PasswordAuthentication no
- Fail2ban ativo

---

## Conclusion
Ataque contido, persistência removida e ambiente protegido.
