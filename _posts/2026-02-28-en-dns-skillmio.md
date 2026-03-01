---
title: How to Use Skillmio Public DNS
date: 2026-02-28 05:05:00 +0100
categories: [Tutorial-en]
tags: [tutorial-en, dns]     # TAG names should always be lowercase
author: msaraiva
pin: true
description: This tutorial explains how to use Skillmio DNS free services to ensure safer and more efficient browsing
image: /assets/img/SkillmioDNS-ilustration.png
---

## Skillmio DNS
Skillmio DNS is a free DNS resolution service focused on security and privacy. It filters and blocks malicious domains, phishing attempts, and unwanted content before the connection is established, ensuring safer and more efficient browsing without the need to install software on devices.

## Benefits
1. **Security** – Cleaner, faster, and safer browsing anywhere in the world.
2. **Local Consumption** – By using Skillmio DNS, you contribute to strengthening African digital services.
3. **Collaboration** – The more people use and report websites with intrusive ads or unwanted content, the more effective the protection becomes for everyone.

## How to Use?

### Android Devices

1. Go to **Settings** > **Connections** > **More connection settings**
2. Tap **Private DNS**
3. Select **Private DNS provider hostname**
4. Enter: `dns.skillmio.net`
5. Tap **Save**
   
⏳ Wait a few seconds — if the name appears without error, everything is configured correctly.

> To disable, follow the steps until step 3 and, instead of selecting **Private DNS provider hostname**, choose **Off**.
{:.prompt-info}

### iOS Devices

1. Download and save the profile file:
   [https://raw.githubusercontent.com/skillmio/CoSec/master/dns-skillmio-unsig.mobileconfig](https://raw.githubusercontent.com/skillmio/CoSec/master/dns-skillmio-unsig.mobileconfig)
2. Go to the location where you saved the `dns-skillmio-unsig.mobileconfig` file
3. Tap the file — it will automatically be recognized as a **configuration profile**
4. A message will appear saying **“Profile Ready to Install”**
5. Go to **Settings**
6. Tap **Profile Downloaded** (at the top of Settings)
7. Select **Install**
8. Enter your device passcode if requested
9. Confirm by tapping **Install** again

After installation, Skillmio DNS will be active on the device.

### Windows 10/11

1. Open **Control Panel** > **Network and Internet** > **Network and Sharing Center**
2. Click **Change adapter settings**
3. Right-click the active connection > **Properties**
4. Select **Internet Protocol Version 4 (TCP/IPv4)** > **Properties**
5. Select **Use the following DNS server addresses** and enter: `102.213.34.98` and `196.61.76.76`
6. Click **OK** and restart the connection

### Linux

1. Open the Linux **Terminal**
2. Edit the file: `vi /etc/resolv.conf`
3. Add:
   `nameserver 102.213.34.98`
4. Add:
   `nameserver 196.61.76.76`
