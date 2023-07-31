---
layout: post
title:  "Welcome Global Secure Access"
author: tim
categories: [ AzureAD, Security, ZTNA]
image: assets/images/WelcomeGSA\thumbnail.png
description: "Overview of Use Cases for Global Secure Access"
featured: true
hidden: false
---

# A warm Welcome to Global Secure Access

For me this will be a GameChanger for the next view years!

With Microsoft Global Secure Access, Microsoft will close some really important gaps and more important will give us the possibility to implement a Zero Trust Network Access (ZTNA) solution.

## What is Global Secure Access

Lets see how AI will describe it:

"Microsoft Global Secure Access is a comprehensive and cutting-edge suite of tools, technologies, and protocols crafted by Microsoft to fortify digital connectivity across organizations of all sizes. At its core, this framework aims to enhance security while ensuring a seamless user experience for accessing networks, resources, and services from anywhere in the world." - ChatGPT

In my own Words:

**More Security and control on the full road from Client to the Service.**

Inside of the Microsoft Global Secure Access (GSA) you will find two new services:

- Microsoft Entra Private Access
- Microsoft Entra Public Access

And additionally a function to bring more Thread Detection into the Access to M365 Services.

![GSA](/assets/images/WelcomeGSA/thumbnail.png)


## Microsoft Entra Private Access

Microsoft Entra Private Access is a new service which will give you the possibility to connect to your internal resources without the need of a VPN.
It also brings for me one of the most important features:

**Transparent traffic passthrough with Application Proxy**

and this brings us some really important possibilities:

- No more need for a VPN
- no more need for a RDP Gateway
- no more need for a Jump Host
- Public Access to internal Ressources like SMB Shares but protected with MFA and Conditional Access
- Mobile Access to internal Applications


## Microsoft Entra Public Access

Microsoft Entra Public Access is a new service, that let you control the access to the Internet from your Clients. It will give you the possibility to control the access to the Internet from your Clients and also to protect your Clients from malicious Websites.

Yes, its additional to Defender to Cloud Apps, but both will work together in the future and bring a great improvement in the security of your Clients.

Here you get also some really important possibilities: 

- no more self-hosted Out-boound Proxy
- next-Gen Internet Access Control and filtering



## Use Cases

For me there are currently 3 really cool Use Cases:

1. native RDP Access to internal Server without the need of a VPN, but protected with MFA and CA
2. Block Access to M365 Services from outside of the GSA Network
3. Entry into the GSA Networt must be evaluated with every Signal change like IP, Location, Device, User, App, Risk, etc.

In the next view weeks i write some articles for this impressive features. **Stay tuned!**


### Additional Links

[Official Entra Blog](https://techcommunity.microsoft.com/t5/microsoft-entra-azure-ad-blog/microsoft-entra-expands-into-security-service-edge-with-two-new/ba-p/3847829)
