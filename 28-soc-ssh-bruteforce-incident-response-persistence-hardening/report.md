# Incident Report - SSH Brute Force with Persistence

---

## Summary
Ataque de brute force contra SSH resultou em acesso não autorizado ao host e criação de persistência.

- Acesso: ✔ Sim  
- Persistência: ✔ Sim  
- Severidade: Alta  
- Classificação: True Positive (TP)

---

## Detection
Foram identificadas múltiplas tentativas de autenticação falhadas seguidas de login bem-sucedido.

**Evidências:**
- Eventos "Failed password" (brute force)
- Evento "Accepted password" (acesso confirmado)

---

## Investigation
Análise de logs indicou atividade pós-comprometimento no sistema.

**Evidências:**
- Execução de comandos (auditd - EXECVE)
- Uso de privilégios elevados (sudo)
- Alteração de credenciais

---

## Persistence
Foi identificada persistência após o acesso inicial.

**Evidências:**
- Criação de conta maliciosa
- Manutenção de acesso via SSH (authorized_keys)

---

## Response
Ações realizadas para contenção e erradicação:

- Bloqueio do IP atacante (Fail2ban)
- Remoção de usuário malicioso
- Exclusão de mecanismos de persistência
- Reset de credenciais comprometidas

---

## Impact
- Acesso não autorizado ao sistema
- Execução de comandos privilegiados
- Criação de persistência
- Risco de comprometimento adicional

---

## MITRE ATT&CK
- T1110 – Brute Force  
- T1078 – Valid Accounts  
- T1098.004 – SSH Authorized Keys  
- T1136 – Create Account  

---

## Lessons Learned
- Autenticação por senha aumenta risco de brute force
- Falta de proteção (rate limiting) facilita ataque
- Monitoramento contínuo reduz tempo de resposta
- Hardening é essencial para prevenção

---

## Conclusion
O incidente foi identificado, contido e remediado com sucesso.  
Medidas de segurança foram aplicadas para prevenir recorrência.
