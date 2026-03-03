---
title: Guia de consulta rápida do Proxmox
date: 2026-03-03 17:05:00 +0100
categories: [Tutorial-pt]
tags: [tutorial-pt, proxmox]     # TAG names should always be lowercase
author: A1
pin: true
description: Guia de consulta rápida do proxmox
---


## 1. Informação do Servidor Físico (Host)

| Objectivo                       | Comando                            | Observação                      |
| ------------------------------- | ---------------------------------- | ------------------------------- |
| Ver modelo do servidor físico   | `dmidecode -s system-product-name` | Mostra o modelo (ex: Dell R740) |
| Ver fabricante                  | `dmidecode -s system-manufacturer` | Marca do equipamento            |
| Informação completa do hardware | `dmidecode`                        | Muito detalhado                 |
| Ver CPU                         | `lscpu`                            | Informação do processador       |
| Ver memória RAM                 | `free -h`                          | Resumo                          |
| Ver módulos de RAM              | `dmidecode -t memory`              | Slots e capacidade              |
| Ver discos físicos              | `lsblk`                            | Lista discos                    |
| Informação detalhada dos discos | `smartctl -a /dev/sdX`             | Requer `smartmontools`          |
| Ver versão do Proxmox           | `pveversion -v`                    | Versão completa                 |
| Ver estado geral do sistema     | `uptime`                           | Load average elevado?           |
| Ver utilização CPU              | `top` ou `htop`                    | Processo a consumir recursos    |
| Ver memória                     | `free -h`                          | Swap em uso excessivo           |
| Ver espaço em disco             | `df -h`                            | Partições a 100%                |
| Ver I/O disco                   | `iostat -x 1`                      | Latência alta                   |
| Ver temperatura (se suportado)  | `sensors`                          | Overheating                     |

## 2. Gestão de VMs

| Objectivo              | Comando        |
| ---------------------- | -------------- |
| Listar VMs             | `qm list`      |
| Iniciar VM             | `qm start ID`  |
| Parar VM               | `qm stop ID`   |
| Reiniciar VM           | `qm reboot ID` |
| Ver configuração da VM | `qm config ID` |
| Ver logs da VM         | `journalctl -xe`               |
| Ver tarefas falhadas   | `cat /var/log/pve/tasks/index` |

## 3. Gestão de Containers (LXC)

| Objectivo           | Comando         |
| ------------------- | --------------- |
| Listar containers   | `pct list`      |
| Iniciar container   | `pct start ID`  |
| Parar container     | `pct stop ID`   |
| Aceder ao container | `pct enter ID`  |
| Ver configuração    | `pct config ID` |
| Ver logs            | `journalctl -xe` |

## 4. Armazenamento

| Objectivo                 | Comando        |
| ------------------------- | -------------- |
| Ver storages configurados | `pvesm status` |
| Ver utilização de disco   | `df -h`        |
| Ver pools ZFS             | `zpool status` |
| Ver datasets ZFS          | `zfs list`     |

## 5. Rede

| Objectivo                 | Comando                                               |
| ------------------------- | ----------------------------------------------------- |
| Ver interfaces de rede    | `ip a`                                                |
| Ver bridges               | `brctl show`                                          |
| Ver configuração de rede  | `cat /etc/network/interfaces`                         |
| Ver porta/switch via LLDP | `lldpctl`                                             |
| Ver CDP (Cisco)           | `tcpdump -i vmbr0 -v -n ether host 01:00:0c:cc:cc:cc` |
| Reiniciar rede            | `systemctl restart networking` |


## 6. Cluster

| Objectivo             | Comando        |               |
| --------------------- | -------------- | ------------- |
| Ver estado do cluster | `pvecm status` |               |
| Listar nós do cluster | `pvecm nodes`  |               |
| Ver quorum            | `pvecm status  | grep Quorate` |
| Reiniciar serviço cluster | `systemctl restart pve-cluster` |               |


## 7. Logs Importantes

| Objectivo       | Caminho               |
| --------------- | --------------------- |
| Logs gerais     | `/var/log/syslog`     |
| Logs do Proxmox | `/var/log/pve/`       |
| Logs de tarefas | `/var/log/pve/tasks/` |
| Logs gerais   | `journalctl -xe`            |
| Logs Proxmox  | `journalctl -u pvedaemon`   |
| Logs cluster  | `journalctl -u pve-cluster` |
| Logs corosync | `journalctl -u corosync`    |

 
