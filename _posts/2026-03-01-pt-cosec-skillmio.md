---
title: Como usar as listas CoSec para reforçar a segurança de TI
date: 2026-03-01 05:05:00 +0100
categories: [Tutorial-pt]
tags: [tutorial-pt, cosec, blocked_domains, banned_ips]     # TAG names should always be lowercase
author: msaraiva
pin: true
description: Aprenda a utilizar as listas blocked_domains e banned_ips da CoSec para reforçar a proteção da sua infraestrutura de TI.
image: /assets/img/SkillmioCoSec-visual.jpeg
---

github repo: https://github.com/skillmio/CoSec

## O que é uma lista de bloqueio?

Uma lista de bloqueio (blocklist) é um conjunto de domínios ou endereços IP identificados como maliciosos, suspeitos ou indesejados. Estas listas são utilizadas para impedir comunicações com ameaças conhecidas, reduzindo riscos de malware, phishing, botnets e publicidade abusiva.

## O que são a blocked_domains.txt e a banned_ips.txt?

- **blocked_domains.txt** – Lista de domínios maliciosos ou indesejados que devem ser bloqueados ao nível do DNS.
- **banned_ips.txt** – Lista de endereços IP associados a atividades suspeitas ou maliciosas, recomendados para bloqueio ao nível de firewall ou sistemas de filtragem.

Estas listas são mantidas no âmbito da iniciativa **CoSec**, desenvolvida pela Skillmio, com foco no reforço contínuo da segurança digital.

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

#### 1. Editar ficheiro de configuração

Debian / Ubuntu
```bash
sudo vi /etc/bind/named.conf.options
````
CentOS / RHEL

```bash
sudo vi /etc/named.conf
```

#### 2. Adicionar ou ajustar o bloco `options`:

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

#### 3. Validar configuração

```bash
sudo named-checkconf
```

#### 4. Reiniciar serviço

Debian / Ubuntu

```bash
sudo systemctl restart bind9
```

CentOS / RHEL

```bash
sudo systemctl restart named
```

Após reiniciar o serviço, o seu servidor DNS passará a utilizar a filtragem baseada na lista `blocked_domains.txt` através dos servidores Skillmio.

## Como usar a banned_ips.txt?

A lista `banned_ips.txt` pode ser integrada diretamente na firewall da sua infraestrutura para bloquear endereços IP maliciosos ao nível da rede.

Abaixo seguem exemplos para **fail2ban** .

### Fail2ban

#### 1. Instalar o fail2ban 

Ubuntu / Debian

```bash
sudo apt update
sudo apt install fail2ban -y
````

AlmaLinux / Rocky

```bash
sudo dnf install epel-release -y
sudo dnf install fail2ban -y
```

#### 2. Criar uma Jail `banned` (usa Heredoc EOF)

```bash
sudo tee /etc/fail2ban/jail.d/banned-ips.conf > /dev/null << 'EOF'
[banned]
enabled = true
port    = all
filter  = sshd
banaction = iptables-allports
logpath = /var/log/secure
EOF
```

**Como funciona**

* Os IPs adicionados via `fail2ban-client set banned banip <IP>` são incluídos no ipset
* O firewalld bloqueia automaticamente esses IPs em todas as portas

Sem necessidade de logs, filtro ou tempo de banimento. Os IPs permanecem bloqueados até serem removidos manualmente.

#### 3. Ativar e iniciar o Fail2Ban

Ativar Fail2Ban:

```bash
sudo systemctl enable --now fail2ban
```

Ver estado do serviço Fail2Ban:

```bash
sudo systemctl status fail2ban
```

Outros comandos úteis:

Ver estado do Fail2Ban e jails:

```bash
sudo fail2ban-client status
```

Reiniciar o serviço Fail2Ban:

```bash
sudo systemctl restart fail2ban
```


#### 4. Baixar a Lista de IPs da Skillmio e aplicar bloqueio

```bash
curl -s https://raw.githubusercontent.com/skillmio/CoSec/main/banned_ips.txt \
| grep -vE '^\s*#|^\s*$' \
| xargs -I{} sudo fail2ban-client set banned banip {}
```

Este ficheiro contém os IPs a bloquear automaticamente.


#### 5. Verificar o estado da Jail `banned`

```bash
sudo fail2ban-client status banned
```

Deve mostrar o número de IPs atualmente bloqueados.


#### 6. Atualizar automaticamente os IPs bloqueados (opcional mas recomendado)

Criar um script de atualização:

```bash
sudo tee /usr/local/bin/update-skillmio-bannedip.sh > /dev/null << 'EOF'
#!/bin/bash

curl -s https://raw.githubusercontent.com/skillmio/CoSec/main/banned_ips.txt \
| grep -vE '^\s*#|^\s*$' \
| xargs -I{} sudo fail2ban-client set banned banip {}
EOF
```

Tornar o script executável:

```bash
sudo chmod +x /usr/local/bin/update-skillmio-bannedip.sh
```

Adicionar uma tarefa cron:

```bash
sudo bash -c 'echo "0 * * * * /usr/local/bin/update-skillmio-bannedip.sh" >> /etc/crontab'
```

Isso verifica atualizações **a cada hora**.

Depois de criar o script e torná-lo executável, pode executá-lo manualmente assim:

```bash
update-skillmio-bannedip.sh
```

Após a execução, verifique os IPs atualmente bloqueados:

```bash
sudo fail2ban-client status banned
```

#### 7. Resumo

Com esta configuração:

* O Fail2Ban está instalado e protege o seu servidor
* A lista de IPs bloqueados da Skillmio bloqueia ativamente IPs maliciosos
* É possível atualizar automaticamente a blacklist
