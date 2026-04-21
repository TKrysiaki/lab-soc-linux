# 📄 Incident Report – SSH Brute Force + Persistence + Log Tampering

---

## 🕒 Timeline

| Time | Event |
|------|------|
| T0 | Múltiplas tentativas de login SSH (Failed password) |
| T1 | Login bem-sucedido (Accepted password) |
| T2 | Criação de usuário `backupsvc` |
| T3 | Adição de chave em `authorized_keys` |
| T4 | Limpeza de logs (`auth.log`) |
| T5 | Detecção via Wazuh |
| T6 | Bloqueio do IP com Fail2Ban |
| T7 | Remoção do usuário malicioso |
| T8 | Hardening do SSH |

---

## 🚨 Incident Summary

Foi identificado um ataque de brute force SSH contra o host `192.168.122.102`, resultando em comprometimento do usuário `target`.

Após o acesso inicial, o atacante estabeleceu persistência através da criação de um novo usuário (`backupsvc`) e uso de SSH Authorized Keys.

Também foi observada tentativa de evasão por meio da limpeza do arquivo `/var/log/auth.log`.

---

## 🔍 Indicators of Compromise (IoCs)

- IP atacante: `192.168.122.1`  
- Host alvo: `192.168.122.102`  
- Usuário comprometido: `target`  
- Usuário malicioso: `backupsvc`  
- Arquivo afetado: `/var/log/auth.log`  
- Persistência: `/home/backupsvc/.ssh/authorized_keys`  

---

## 🧠 Analysis

O ataque seguiu a seguinte cadeia:

1. Brute force SSH (T1110)  
2. Acesso válido com credenciais comprometidas (T1078)  
3. Criação de conta persistente (T1136)  
4. Persistência via chave SSH (T1098.004)  
5. Remoção de evidências (T1070)  

Eventos correlacionados no Wazuh indicaram múltiplas falhas de autenticação seguidas de sucesso, caracterizando comprometimento.

---

## 🚨 Response Actions

- Bloqueio do IP atacante via Fail2Ban  
- Remoção do usuário `backupsvc`  
- Verificação de acessos SSH ativos  
- Análise de arquivos de persistência  

---

## 🛡️ Mitigation & Hardening

- Desativação de autenticação por senha (`PasswordAuthentication no`)  
- Restrição de login root (`PermitRootLogin no`)  
- Adoção de autenticação via chave SSH  
- Rotação de credenciais do usuário comprometido  

---

## 🔎 Validation

- IP atacante bloqueado com sucesso  
- Usuário malicioso removido  
- Arquivo `authorized_keys` limpo  
- Acesso SSH restrito conforme política  

---

## 🧬 MITRE ATT&CK

- T1110 – Brute Force  
- T1078 – Valid Accounts  
- T1098.004 – SSH Authorized Keys  
- T1136 – Create Account  
- T1070 – Indicator Removal  

---

## 🎯 Conclusion

O incidente foi detectado, investigado e contido com sucesso.  
A persistência foi removida e controles de segurança foram aplicados, reduzindo a superfície de ataque e prevenindo recorrência.
