---
layout: post
title:  "Authentication Context with PIM"
author: tim
categories: [ AzureAD, Security, PIM, ConditionalAccess]
image: assets/images/AuthContextwithPim\thumbnail.jpg
description: "More Power and Control with PIM and Authentication Context"
featured: false
hidden: false
---

# Authentication Context with PIM

## Introduction

Microsoft Authentication Context is a sophisticated framework designed to elevate the security and user experience in the realm of digital authentication. At its core, it serves as a powerful tool that enables developers to implement secure access management across a wide array of applications and platforms.

## What is Auth Context

Authentication Context is an additional informations inside of claim which can be targeted by Conditional Access. This add additional security to your environment to control access to your resources like dedicated Sharepoint sides or other applications.

## Use Cases

- Access to a dedicated Sharepoint side only from a managed device
- Fido key only for specific AAD Role activation with PIM
- Access to a protected file only from a managed device + MFA

## Usage inside of PIM

Inside of the PIM configuration you can define the Authentication Context for the activation of a role.

![PIM](/assets/images/AuthContextwithPim/PIM.png)
