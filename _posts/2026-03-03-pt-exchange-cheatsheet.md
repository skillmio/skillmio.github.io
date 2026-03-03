---
title: Guia de consulta rápida do Exchange On-Premises
date: 2026-03-03 20:05:00 +0100
categories: [Tutorial-pt]
tags: [tutorial-pt, exchange]     # TAG names should always be lowercase
author: A1
description: Guia de consulta rápida do Exchange On-Premises
---


## Versões mais comuns

* Microsoft Exchange Server 2016
* Microsoft Exchange Server 2019



#  Serviços Principais

Verificar serviços do Exchange:

```powershell
Get-Service *Exchange*
```

Reiniciar serviços:

```powershell
Restart-Service MSExchangeIS
Restart-Service MSExchangeTransport
```



#  Gerenciamento de Mailbox

## Criar mailbox

```powershell
New-Mailbox -Name "João Silva" -UserPrincipalName joao@dominio.com `
-Alias joao -OrganizationalUnit "Usuarios" `
-Password (ConvertTo-SecureString -String 'Senha@123' -AsPlainText -Force)
```

## Listar mailboxes

```powershell
Get-Mailbox
```

## Ver tamanho da mailbox

```powershell
Get-MailboxStatistics joao | Select DisplayName,TotalItemSize
```

## Desabilitar mailbox

```powersshell
Disable-Mailbox joao
```

## Restaurar mailbox desabilitada

```powershell
Connect-Mailbox -Identity <GUID> -Database DB01 -User usuario
```


#  Banco de Dados (Mailbox Database)

## Listar databases

```powershell
Get-MailboxDatabase
```

## Ver status do database

```powershell
Get-MailboxDatabaseCopyStatus
```

## Montar / Desmontar database

```powershell
Mount-Database DB01
Dismount-Database DB01
```


# DAG (Database Availability Group)

## Ver status DAG

```powershell
Get-DatabaseAvailabilityGroup
Get-DatabaseAvailabilityGroupNetwork
```

## Ver cópias das bases

```powershell
Get-MailboxDatabaseCopyStatus *
```



#  Filas de Email (Queues)

## Ver filas

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



#  Permissões

## Dar permissão Full Access

```powershell
Add-MailboxPermission -Identity joao `
-User maria -AccessRights FullAccess -InheritanceType All
```

## Send As

```powershell
Add-RecipientPermission "joao" -Trustee "maria" -AccessRights SendAs
```



#  Conectores

## Ver Send Connectors

```powershell
Get-SendConnector
```

## Ver Receive Connectors

```powersshell
Get-ReceiveConnector
```


#  Troubleshooting Rápido

## Testar fluxo de email

```powershell
Test-Mailflow
```

## Testar Autodiscover

```powershell
Test-OutlookWebServices
```

## Ver logs de rastreamento

```powershell
Get-MessageTrackingLog -Recipients usuario@dominio.com -Start (Get-Date).AddHours(-2)
```



# Manutenção

## Mover mailbox para outro DB

```powershell
New-MoveRequest -Identity joao -TargetDatabase DB02
```

## Ver status de movimentação

```powersshell
Get-MoveRequest
Get-MoveRequestStatistics joao
```

## Remover move request concluído

```powersshell
Remove-MoveRequest joao
```


# Comandos Úteis Gerais

```powershell
Get-ExchangeServer
Get-ExchangeCertificate
Get-AcceptedDomain
Get-TransportService
```

---
