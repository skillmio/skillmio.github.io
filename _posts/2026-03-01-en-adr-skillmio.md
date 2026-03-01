---
title: Deploying Services on AlmaLinux 10 Made Easy
date: 2026-03-01 05:05:00 +0100
categories: [Tutorial-en]
tags: [tutorial-en, adr, zabbix, bookstack, wordpress]
author: A1
description: Learn how to deploy services like WordPress, GLPI, and BookStack on AlmaLinux 10 quickly and safely using ADR (Auto-Deploy Role)
image: /assets/img/SkillmioADR-visual.jpeg
---

GitHub repo: https://github.com/skillmio/adr

## **Introduction**

Managing Linux servers can be time-consuming, especially when installing and configuring services like WordPress, GLPI, or BookStack. With **ADR (Auto-Deploy Role)**, you can deploy complete roles with **a single command**, quickly and consistently.


### **Installing ADR**

In the terminal, run:

```bash
curl -fsSL https://raw.githubusercontent.com/skillmio/adr/main/adr.sh -o /tmp/adr
chmod +x /tmp/adr
sudo mv /tmp/adr /usr/local/bin/adr
```

Set the language **before any other action**:

```bash
adr -lg en
```

Check ADR installation and help:

```bash
adr -h
```

> **Tip:** Run ADR on a fresh installation and take a snapshot of the server before applying roles.
> {:.prompt-tip}


### **How to Deploy Roles/Functions**

1. **Install WordPress:**

```bash
adr wordpress
```

2. **Install GLPI:**

```bash
adr glpi
```

3. **Install BookStack:**

```bash
adr bookstack
```

> ADR automatically detects your system, downloads the correct configuration, installs the service, and applies security best practices.
> {:.prompt-info}


### **Listing and Searching Roles**

* **List all available roles:**

```bash
adr -l
```

* **Search for a specific role (fuzzy search):**

```bash
adr --find wordpress
adr -f glpi
```


### **Updates and Diagnostics**

* ADR checks for updates automatically.
* Diagnostics and repair:

```bash
adr -d       # Check installation and configuration files
adr -r       # Fix issues automatically
```


### **Summary**

With ADR, you can deploy roles on AlmaLinux 10 **easily, safely, and consistently**. Each service is installed with **secure configuration defaults**, saving time and reducing errors.


If you want, I can also **adapt the banner and visuals text to English** to match this translation for a fully localized post. Do you want me to do that?
