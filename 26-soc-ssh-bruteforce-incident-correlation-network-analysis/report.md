# Incident Report - SSH Brute Force with Initial Compromise

---

## Summary
Ataque de brute force contra SSH detectado via Wazuh e análise de logs.  
Foi identificado acesso não autorizado ao sistema, sem evidência de persistência.

- Acesso: ✔ Sim  
- Persistência: ❌ Não  
- Severidade: Alta  
- Classificação: True Positive (TP)

---

## Detection
O SIEM (Wazuh) identificou atividade suspeita relacionada a brute force SSH.

**Evidências:**
- Múltiplos eventos "Failed password"
- Regra de alta severidade acionada
- Correlação de tentativas por IP

---

## Investigation
Análise de logs e rede confirmou padrão de ataque e acesso ao sistema.

**Evidências:**
- Padrão de brute force em `/var/log/auth.log`
- Login bem-sucedido ("Accepted password")
- Tráfego SSH identificado (tcpdump)
- Atividade pós-login limitada

---

## Persistence
Nenhuma evidência de persistência foi identificada após o acesso inicial.

---

## Response
Ações realizadas para contenção do incidente:

- Bloqueio de IPs envolvidos
- Implementação de Fail2ban
- Revisão de credenciais
- Monitoramento contínuo

---

## Impact
- Acesso não autorizado ao sistema
- Potencial risco de movimentação lateral
- Necessidade de resposta imediata

---

## MITRE ATT&CK
- T1110 – Brute Force  
- T1078 – Valid Accounts  

---

## Lessons Learned
- Autenticação por senha aumenta risco de brute force
- Correlação entre logs e rede melhora a detecção
- SIEM acelera identificação de padrões maliciosos
- Monitoramento contínuo reduz tempo de resposta

---

## Conclusion
O incidente foi identificado e contido com sucesso.  
Apesar do acesso inicial, não foram observadas ações de persistência ou impacto significativo no sistema.
