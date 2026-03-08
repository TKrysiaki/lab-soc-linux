# SSH Brute Force Detection and Mitigation with Fail2Ban

## Cenário

Este laboratório simula um ataque de **brute force contra o serviço SSH** e demonstra como detectar o ataque através da análise de logs e como mitigá-lo automaticamente utilizando **Fail2Ban**.

O fluxo do laboratório segue o modelo utilizado em operações de **Security Operations Center (SOC)**:

Reconhecimento → Ataque → Detecção → Resposta

---

## Ambiente

Atacante: Kali Linux  
Servidor: Ubuntu Server  
Serviço atacado: SSH

---

## Ferramentas utilizadas

- Nmap
- Dirb
- Hydra
- Fail2Ban
- UFW
- Apache

---

# Passo a passo do laboratório

---

## 1 - Identificando o IP do servidor

No Ubuntu utilizamos o comando:
```bash
ip a
```

Esse comando permite identificar o endereço IP da máquina alvo.

![Ubuntu IP](images/02-ubuntu-ip.png)

---

## 2 - Testando conectividade entre as máquinas

No Kali realizamos um teste de conectividade para verificar se as máquinas conseguem se comunicar.
```bash
ping 192.168.56.105
```

![Network connectivity](images/04-network-connectivity.png)

---

## 3 - Reconhecimento com Nmap

Foi realizado um scan para identificar portas abertas e serviços ativos no servidor.
```bash
nmap -sS -sV 192.168.56.105
```

O scan identificou o serviço **SSH ativo na porta 22**.

![Nmap Scan](images/05-nmap-scan.png)

---

## 4 - Instalação e verificação do servidor web

No Ubuntu foi instalado o Apache para simular um servidor web.
```bash
sudo apt install apache2
```

Depois verificamos se o serviço está ativo.

![Apache Running](images/06-apache-running.png)

---

## 5 - Enumeração Web

No Kali utilizamos o **Dirb** para descobrir diretórios no servidor web.
```bash
dirb http://192.168.56.105
```

![Dirb Enumeration](images/08-dirb-enumeration.png)

---

## 6 - Teste de acesso SSH

Antes do ataque confirmamos que o serviço SSH está acessível.
```bash
ssh tiago@192.168.56.105
```

![SSH Access](images/09-ssh-access-test.png)

---

## 7 - Ataque brute force com Hydra

Foi realizado um ataque de brute force utilizando a wordlist **rockyou**.
```bash
hydra -l tiago -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.105
```

![Hydra Attack](images/11-hydra-bruteforce.png)

---

## 8 - Detecção através de análise de logs

As tentativas de login falhadas podem ser identificadas no arquivo:
```bash
/var/log/auth.log
```
Monitoramento em tempo real:
```bash
sudo tail -f /var/log/auth.log
```

![Attack Logs](images/12-ssh-attack-logs.png)

---

## 9 - Mitigação automática com Fail2Ban

O Fail2Ban foi configurado para monitorar tentativas de login falhadas e bloquear automaticamente o IP atacante após múltiplas tentativas.

Verificação do status da jail SSH:
```bash
sudo fail2ban-client status sshd
```

![Attacker IP Banned](images/16-attacker-ip-banned.png)

---

## Conclusão

Este laboratório demonstra um fluxo completo de análise de segurança utilizado em ambientes SOC:

- Reconhecimento de serviços expostos
- Simulação de ataque brute force
- Análise de logs de autenticação
- Detecção de comportamento malicioso
- Resposta automática utilizando Fail2Ban





























































































































