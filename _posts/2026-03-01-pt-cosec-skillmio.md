---
title: Como usar as listas CoSec para reforçar a segurança de TI
date: 2026-03-01 05:05:00 +0100
categories: [Tutorial-pt]
tags: [tutorial-pt, cosec, blocked_domains, banned_ips]     # TAG names should always be lowercase
author: msaraiva
pin: true
description: Aprenda a utilizar as listas blocked_domains e banned_ips da CoSec para reforçar a proteção da sua infraestrutura de TI.
image: /assets/img/SkillmioCoSec-visual.png
---

## O que é uma lista de bloqueio?

Uma lista de bloqueio (blocklist) é um conjunto de domínios ou endereços IP identificados como maliciosos, suspeitos ou indesejados. Estas listas são utilizadas para impedir comunicações com ameaças conhecidas, reduzindo riscos de malware, phishing, botnets e publicidade abusiva.

## O que são a blocked_domains.txt e a banned_ips.txt?

- **blocked_domains.txt** – Lista de domínios maliciosos ou indesejados que devem ser bloqueados ao nível do DNS.
- **banned_ips.txt** – Lista de endereços IP associados a atividades suspeitas ou maliciosas, recomendados para bloqueio ao nível de firewall ou sistemas de filtragem.

Estas listas são mantidas no âmbito da iniciativa **CoSec**, com foco em reforçar a segurança digital.

## Como usar a blocked_domains.txt?

###  Método 1 – Upstream (Recomendado)

A forma mais rápida e simples de utilizar a `blocked_domains.txt` é configurar os servidores DNS da Skillmio como *upstream* no seu servidor DNS interno.

Isso permite que todo o tráfego DNS da sua organização beneficie automaticamente da filtragem.

Servidores DNS Skillmio:
- `102.213.34.98`
- `196.61.76.76`


### Configuração no Active Directory (Windows Server)

1. Abra o **DNS Manager**
2. Clique com o botão direito no servidor DNS
3. Selecione **Properties**
4. Vá até ao separador **Forwarders**
5. Adicione:
   - `102.213.34.98`
   - `196.61.76.76`
6. Clique em **OK**

Após a configuração, todas as consultas externas passarão pelos servidores da Skillmio.


### Configuração no BIND (Linux)

Editar ficheiro de configuração

#### Debian / Ubuntu
```bash
sudo vi /etc/bind/named.conf.options
````
#### CentOS / RHEL

```bash
sudo vi /etc/named.conf
```

#### Adicionar ou ajustar o bloco `options`:

```bash
options {
    directory "/var/cache/bind";

    forwarders {
        102.213.34.98;   // Skillmio DNS 1
        196.61.76.76;    // Skillmio DNS 2
    };

    recursion yes;
};
```

#### Validar configuração

```bash
sudo named-checkconf
```

#### Reiniciar serviço

##### Debian / Ubuntu

```bash
sudo systemctl restart bind9
```

##### CentOS / RHEL

```bash
sudo systemctl restart named
```
Após reiniciar o serviço, o seu servidor DNS passará a utilizar a filtragem baseada na lista `blocked_domains.txt` através dos servidores Skillmio.

## Como usar a banned_ips.txt?

A lista `banned_ips.txt` pode ser integrada diretamente na firewall da sua infraestrutura para bloquear endereços IP maliciosos ao nível da rede.

Abaixo seguem exemplos para **fail2ban** e **firewalld**.



