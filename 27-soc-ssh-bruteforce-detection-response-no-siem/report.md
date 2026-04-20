# Incident Report - SSH Brute Force (No Compromise)

---

## Summary
Ataque de brute force contra SSH identificado via análise manual de logs.  
Não houve acesso bem-sucedido nem evidência de comprometimento.

- Acesso: ❌ Não  
- Persistência: ❌ Não  
- Severidade: Média  
- Classificação: True Positive (TP)

---

## Detection
Foram identificadas múltiplas tentativas de autenticação falhadas em curto intervalo de tempo.

**Evidências:**
- Eventos repetidos "Failed password"
- Origem única: IP [IP_ATACANTE]
- Alvo: usuário root

---

## Verification
Nenhum login bem-sucedido foi identificado.

**Evidências:**
- Ausência de eventos "Accepted password"
- Nenhuma sessão ativa registrada

---

## Response
Ação de contenção realizada com Fail2ban.

- Bloqueio do IP atacante
- Interrupção das tentativas de acesso

---

## Impact
- Nenhum acesso não autorizado
- Nenhuma execução de comandos
- Nenhuma alteração no sistema

---

## MITRE ATT&CK
- T1110 – Brute Force

---

## Lessons Learned
- SSH exposto com autenticação por senha aumenta risco
- Monitoramento contínuo permite detecção precoce
- Fail2ban reduz efetivamente tentativas automatizadas

---

## Conclusion
O ataque foi identificado e contido com sucesso, sem impacto ao ambiente.  
Medidas de proteção foram eficazes na mitigação da ameaça.
