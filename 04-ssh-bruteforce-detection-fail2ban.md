# SSH Brute Force Detection and Mitigation with Fail2Ban

## Cenário

Este laboratório simula um ataque de **brute force SSH** contra um servidor Linux e demonstra como um analista SOC pode **detectar e mitigar automaticamente** o ataque utilizando análise de logs e Fail2Ban.

---

## Ambiente do laboratório

Atacante: Kali Linux  
Servidor: Ubuntu Server  
Serviço atacado: SSH  
Logs analisados: `/var/log/auth.log`

---

## Ferramentas utilizadas

- Nmap
- Dirb
- Hydra
- Fail2Ban
- Apache
- UFW

---

## Objetivo

Demonstrar um fluxo realista de análise de segurança:

Reconhecimento → Enumeração → Ataque → Detecção → Mitigação

---

# Passo a passo do laboratório

## 1 - Identificando IP do servidor

No Ubuntu utilizamos o comando abaixo para identificar o endereço IP da máquina.
´´´ip a´´´

![Ubuntu IP](images/02-ubuntu-ip.png)

---

## 2 - Identificando IP da máquina atacante

No Kali Linux utilizamos o mesmo comando para identificar o IP da máquina atacante.

![Kali IP](images/03-kali-ip.png)

---

## 3 - Testando conectividade entre as máquinas

Foi realizado um teste de conectividade entre as máquinas.
´´´ping 192.168.56.105´´´

![Network Connectivity](images/04-network-connectivity.png)

---

## 4 - Reconhecimento de serviços com Nmap

Foi realizado um scan de portas para identificar serviços expostos no servidor.

´´´nmap -sS -sV 192.168.56.105´´´


![Nmap Scan](images/05-nmap-scan.png)

---

## 5 - Servidor web ativo

Foi instalado e iniciado o Apache no servidor Ubuntu.

![Apache Running](images/06-apache-running.png)

---

## 6 - Teste de acesso ao servidor web

A página padrão do Apache foi acessada pelo navegador.

![Apache Webpage](images/07-apache-webpage.png)

Dirb
Hydra
Log analysis
Fail2ban




























