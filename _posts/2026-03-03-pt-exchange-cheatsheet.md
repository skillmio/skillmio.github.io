---
title: Guia de consulta rápida - Exchange On-Premises
date: 2026-03-03 20:05:00 +0100
categories: [Tutorial-pt]
tags: [tutorial-pt, exchange]     # TAG names should always be lowercase
author: A1
description: Guia de consulta rápida - Exchange On-Premises
---


### Versões mais comuns

* Microsoft Exchange Server 2016
* Microsoft Exchange Server 2019


## Verificações

### Verificar a versão do Microsoft Exchange Server On-Premises

Abrir o Exchange Management Shell e executar:
```powershell
Get-ExchangeServer | Select Name,Edition,AdminDisplayVersion
```

## Logs 

### Ver todas as actividades de emails processadas no último 1 hora

>Dica: para ver se:
>
>* o email chegou ao servidor
>* foi entregue
>* foi rejeitado
>* ficou preso na fila
{:.prompt-tip}


```powershell
Get-MessageTrackingLog -Start (Get-Date).AddHours(-1)
```

`Get-MessageTrackingLog`

Este comando lê os **Message Tracking Logs**, que registam eventos como:

* envio de email
* recepção de email
* entrega na mailbox
* rejeição ou bloqueio
* encaminhamento (forward)
* spam filtering
* erros de transporte
* `Get-Date` → obtém a data e hora actual.
* `.AddHours(-1)` → subtrai **1 hora**.
  
Ou seja, mostra **o caminho que um email percorreu dentro do Exchange**.

### Procurar um email por endereço:
```powershell
Get-MessageTrackingLog -Recipients user@dominio.com
```

### Procurar um email por assunto:
```powershell
Get-MessageTrackingLog -MessageSubject "teste"
```

### Procurar um email por assunto:
```powershell
Get-MessageTrackingLog -MessageSubject "teste"
```

### Ver fila 
```powershell
Get-Queue
```

## Ver mensagens na fila

```powersshell
Get-Message -Queue "NOME_DA_FILA"
```

## Reprocessar fila

```powershell
Retry-Queue "NOME_DA_FILA"
```


### Procurar por intervalo de horas:
Exemplo: entre 10h e 12h
```powershell
Get-MessageTrackingLog -Start "02/15/2026 10:00:00" -End "02/15/2026 12:00:00"
```

### Procurar por data + destinatário
```powershell
Get-MessageTrackingLog -Recipients user@empresa.com -Start "01/15/2026 10:00:00" -End "01/15/2026 12:00:00"
```

## Ver cópias das bases

```powershell
Get-MailboxDatabaseCopyStatus *
```
