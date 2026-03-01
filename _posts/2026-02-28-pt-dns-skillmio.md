---
title: Como usar o DNS Público da Skillmio
date: 2026-02-28 05:05:00 +0100
categories: [Tutorial-pt]
tags: [tutorial-pt, dns]     # TAG names should always be lowercase
author: A1
pin: true
description: Este tutorial explica como utilizar os serviços gratuitos da Skillmio DNS para garantir uma navegação mais segura e eficiente
image: /assets/img/SkillmioDNS-visual.jpeg
---

## Skillmio DNS
Skillmio DNS é um serviço gratuito de resolução de DNS com foco em segurança e privacidade, que filtra e bloqueia domínios maliciosos, de phishing e conteúdos indesejados antes de a ligação ser estabelecida, garantindo uma navegação mais segura e eficiente, sem necessidade de instalar software nos dispositivos.

## Benefícios
1. **Segurança** – Navegação mais limpa, rápida e segura em qualquer parte do mundo.
2. **Consumo Local** – Ao usar o DNS da Skillmio, contribui para reforçar os serviços digitais africanos.
3. **Colaboração** – Quanto mais pessoas utilizarem e reportarem sites com publicidade ou conteúdos indesejados, mais eficaz será a proteção para todos.

## Como usar ?

### Dispositivos Android

1. Vá para **Definições** > **Ligações** > **Mais definições de ligação**
2. Toque em **DNS privado**
3. Selecione **Nome de anfitrião do fornecedor DNS privado**
4. Introduza: `dns.skillmio.net`
5. Toque em **Guardar**
   
⏳ Aguarde alguns segundos — se o nome aparecer, significa que está tudo configurado corretamente.

> Para desactivar siga até ao passo 3 e, em vez de selecionar **Nome de anfitrião do fornecedor DNS privado**, escolha **Desativado**.
{:.prompt-info}



### Dispositivos iOS

1. Baixe e guarde o ficheiro de perfil:
   [https://raw.githubusercontent.com/skillmio/CoSec/master/dns-skillmio-unsig.mobileconfig](https://raw.githubusercontent.com/skillmio/CoSec/master/dns-skillmio-unsig.mobileconfig)
2. Vá até ao local onde guardou o ficheiro `dns-skillmio-unsig.mobileconfig`
3. Toque no ficheiro — ele será automaticamente reconhecido como um **perfil de configuração**
4. Surgirá a mensagem **“Perfil pronto a instalar”**
5. Vá para **Definições**
6. Toque em **Perfil Transferido** (no topo das definições)
7. Selecione **Instalar**
8. Introduza o código de desbloqueio do dispositivo, se solicitado
9. Confirme em **Instalar** novamente

Após a instalação, o Skillmio DNS ficará ativo no dispositivo.


### Windows 10/11
1. Abra **Painel de Controlo** > **Rede e Internet** > **Centro de Rede e Partilha**
2. Clique em **Alterar definições do adaptador**
3. Clique com o botão direito na conexão ativa > **Propriedades**
4. Selecione **Protocolo Internet Versão 4 (TCP/IPv4)** > **Propriedades**
5. Marque **Usar os seguintes endereços de servidor DNS** e introduza: `102.213.34.98` e `196.61.76.76`
6. Clique em **OK** e reinicie a conexão  

### Linux
1. Abra o Linux **Terminal**
2. Edita o ficheiro: `vi /etc/resolv.conf`
3. Adicione:
   `nameserver 102.213.34.98`
4. Adicione:
   `nameserver 196.61.76.76`
 
