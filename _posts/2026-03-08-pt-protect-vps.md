---
title: Como proteger o seu VPS usando banned_ips.txt (CoSec)
date: 2026-03-08 05:05:00 +0100
categories: [Tutorial-pt]
tags: [tutorial-pt, vps, cosec]     # TAG names should always be lowercase
author: A1
description: Este tutorial explica como proteger o seu VPS usando a lista de bloqueio de IPs do projecto CoSec
---

## Introdução

Servidores VPS expostos à internet são constantemente alvo de **ataques automatizados**, como tentativas de força bruta, scanners de vulnerabilidades e bots maliciosos.  

O projecto **CoSec** mantém uma lista pública de **endereços IP maliciosos identificados em atividades suspeitas**, que podem ser bloqueados automaticamente para aumentar a segurança do seu servidor.

Neste tutorial vamos aprender como:

- Instalar e configurar o **Fail2Ban**
- Aplicar a **lista de bloqueio de IPs do CoSec**
- Automatizar a atualização da lista para manter o servidor protegido

Este método ajuda a **reduzir tráfego malicioso antes mesmo de qualquer ataque atingir os serviços do servidor**.


## 1. Instalar o Fail2Ban

O **Fail2Ban** é uma ferramenta de segurança que monitora logs e bloqueia IPs suspeitos automaticamente.

Instale o Fail2Ban usando:

```bash
sudo dnf install epel-release -y
sudo dnf install fail2ban -y
```

## 2. Criar a configuração de bloqueio

Agora vamos criar uma *jail* personalizada que permitirá adicionar IPs manualmente à lista de bloqueio.

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

### Explicação

* **enabled** – ativa a jail
* **port = all** – bloqueia todas as portas
* **banaction = iptables-allports** – bloqueia completamente o IP no firewall
* **logpath** – utiliza o log do sistema para monitorização

## 3. Iniciar o Fail2Ban

Ative e inicie o serviço:

```bash
sudo systemctl enable --now fail2ban
```

Para verificar se está a funcionar:

```bash
sudo systemctl status fail2ban
```


Para garantir que o processo **continua a correr mesmo que a consola SSH feche**, podemos executar o comando em **background usando `nohup`** e redirecionar a saída para um ficheiro de log.

Aqui está a versão atualizada da secção.


## 4. Importar a lista de IPs bloqueados do CoSec

Agora vamos carregar automaticamente a lista de IPs maliciosos do projecto **CoSec**.

Para evitar que o processo pare caso a sessão SSH seja encerrada, iremos executá-lo **em background**.

```bash
nohup bash -c "
curl -s https://raw.githubusercontent.com/skillmio/CoSec/main/banned_ips.txt \
| grep -vE '^\s*#|^\s*$' \
| xargs -I{} sudo fail2ban-client set banned banip {}
" > /tmp/skillmio-import.log 2>&1 &
```

### O que este comando faz

1. **Descarrega a lista pública de IPs maliciosos**
2. **Remove comentários e linhas vazias**
3. **Adiciona cada IP à lista de bloqueio do Fail2Ban**
4. Executa o processo **em background**
5. Guarda o resultado no log `/tmp/skillmio-import.log`

### Verificar o progresso

Pode acompanhar o processo em tempo real usando:

```bash
tail -f /tmp/skillmio-import.log
```

Após a execução terminar, **todos os IPs da lista passam a estar bloqueados no servidor**, aumentando imediatamente a proteção contra tráfego malicioso.

## 5. Automatizar a atualização da lista

Como a lista de IPs é atualizada regularmente, é recomendável automatizar a sincronização.

### Criar o script de atualização

```bash
sudo tee /usr/local/bin/update-skillmio-bannedip.sh > /dev/null << 'EOF'
#!/bin/bash

curl -s https://raw.githubusercontent.com/skillmio/CoSec/main/banned_ips.txt \
| grep -vE '^\s*#|^\s*$' \
| xargs -I{} sudo fail2ban-client set banned banip {}
EOF
```

Dar permissão de execução:

```bash
sudo chmod +x /usr/local/bin/update-skillmio-bannedip.sh
```

## 6. Criar tarefa automática (cron)

Agora vamos configurar uma tarefa que atualiza a lista **a cada hora**.

```bash
sudo bash -c 'echo "0 * * * * /usr/local/bin/update-skillmio-bannedip.sh" >> /etc/crontab'
```

Assim o servidor irá sincronizar automaticamente os novos IPs maliciosos.

## 7. Verificar IPs bloqueados

Para ver os IPs atualmente bloqueados:

```bash
sudo fail2ban-client status banned
```

Também pode verificar o firewall diretamente:

```bash
sudo iptables -L -n
```

## Conclusão

Com esta configuração, o seu VPS passa a:

* Bloquear **automaticamente IPs maliciosos conhecidos**
* Atualizar a lista **de hora em hora**
* Integrar facilmente com o **Fail2Ban**

Este método ajuda a reduzir:

* Ataques de força bruta
* Scanners automatizados
* Bots maliciosos

Para acompanhar atualizações da lista de IPs ou contribuir para o projecto **CoSec**, visite:

```
https://github.com/skillmio/CoSec
```

Manter listas de bloqueio atualizadas é uma **camada simples mas eficaz de defesa para qualquer servidor exposto à internet**.
