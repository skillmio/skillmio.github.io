---
title: Implementando Serviços no AlmaLinux 10 Sem Complicação
date: 2026-03-01 05:05:00 +0100
categories: [Tutorial-pt]
tags: [tutorial-pt, adr, zabbix, bookstack, wordpress]     # TAG names should always be lowercase
author: msaraiva
pin: true
description: Aprenda a implementar serviços como WordPress, GLPI e BookStack no AlmaLinux 10 de forma rápida e segura usando ADR (Auto-Deploy Role)
image: /assets/img/SkillmioADR-visual.png
---


github repo: https://github.com/skillmio/adr


## **Introdução**

Gerenciar servidores Linux pode ser trabalhoso, especialmente quando se precisa instalar e configurar serviços como WordPress, GLPI ou BookStack. Com **ADR (Auto-Deploy Role)**, você pode implementar funções completas em **um único comando**, de forma rápida e consistente.


### **Instalação do ADR**

No terminal, execute:

```bash
curl -fsSL https://raw.githubusercontent.com/skillmio/adr/main/adr.sh -o /tmp/adr
chmod +x /tmp/adr
sudo mv /tmp/adr /usr/local/bin/adr
```

Defina o idioma **antes de qualquer outra ação**:

```bash
adr -lg pt
```

Verifique a instalação e a ajuda do ADR:

```bash
adr -h
```

> **Dica:** Execute ADR em uma instalação nova e faça snapshot do servidor antes de aplicar roles.
{:.prompt-tip}


### **Como Implementar Roles/Funções**

1. **Instalar WordPress:**

```bash
adr wordpress
```

2. **Instalar GLPI:**

```bash
adr glpi
```

3. **Instalar BookStack:**

```bash
adr bookstack
```

> ADR detecta o sistema, baixa as configurações corretas, instala o serviço e aplica padrões de segurança automaticamente.
{:.prompt-info}


### **Listar e Procurar Funções**

* **Listar todas as roles disponíveis:**

```bash
adr -l
```

* **Procurar uma função específica (fuzzy search):**

```bash
adr --find wordpress
adr -f glpi
```


### **Atualizações e Diagnóstico**

* ADR verifica atualizações automaticamente.
* Diagnóstico e reparo:

```bash
adr -d       # Verifica instalação e arquivos de configuração
adr -r       # Corrige problemas automaticamente
```



### **Resumo**

Com ADR, você implementa funções no AlmaLinux 10 de forma simples, segura e repetível. Cada serviço é instalado com **padrões de configuração e segurança**, economizando tempo e reduzindo erros.

