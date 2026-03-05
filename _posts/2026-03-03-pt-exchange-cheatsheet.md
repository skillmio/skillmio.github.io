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
