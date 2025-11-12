---
layout: post
title:  "BreakGlass Accounts"
author: tim
categories: [ Entra Id, Security ]
image: assets/images/BreakGlassAccounts\thumbnail.png
description: "Different ways of Emergancy Access to your Entra ID"
featured: false
hidden: false
---

# BreakGlass Accounts

## Introduction

In every good security concept there should be a way to access your environment in case of emergency. In the context of Entra ID there are different ways to achieve this. In this post I will show you some of the most common ways to implement BreakGlass Accounts in your Entra ID environment.

## User Accounts

This is the most common way to implement a BreakGlass Account. A normal User Account with Global Administrator Permissions. This account must be protected with a random, nobody knows password and Phisining restitant MFA (FIDO2 or Certificate Based Auth). The Fido key must be stored in a safe place like a Tresor, where the access is limited to a few people.

Some best Practices for BreakGlass User Accounts:

- ... must be excluded from any Conditional Access Policies
- ... must be excluded from any Privileged Identity Management Policies
- ... must have a random password which is stored in a safe place
- ... must have Phishing resistant MFA (FIDO2 or Certificate Based Auth)
- ... must be monitored for any SignIn Activity
- ... must be reviewed regularly (at least every 6 months)
- Domain is the default onmicrosoft.com domain (to avoid issues with custom domain unavailability)

![bgpermission](/assets/images/BreakGlassAccounts/bgpermission.png)


## App Registration

Another way to implement a BreakGlass Account is to use an App Registration with Global Administrator or Graph API Permission. This approve make is more resistance if somebody remove all the exclusions from Conditional Access.

Permission: 
    Global Administrator or Directory.ReadWrite.All

Some best Practices for BreakGlass App Registration:
- ... should use Certificate Based Auth which is not coming from a Certificate Authority which is used in the environment
- ... must be monitored for any SignIn Activity
- ... must be reviewed regularly (at least every 6 months)

![graphpermission](/assets/images/BreakGlassAccounts/graphpermission.png)

![bgapp](/assets/images/BreakGlassAccounts/bgapp.png)


## Monitoring

Both version must be monitored. Any access to the BreakGlass Accounts must be logged and alerted. This can be achieved with Azure AD SignIn Logs and Azure Monitor Alerts.

For this export the Logs to a Log Analytics Workspace and create an Alert Rule for any SignIn from the BreakGlass Accounts.

The KQL for this could look like this:

```KQL
SigninLogs
| where UserPrincipalName in ("BreakGlassAccount@domain.onmicrosoft.com", "breakGlassAccount2@domain.onmicrosoft.com")
```

![Alert](/assets/images/BreakGlassAccounts/Alert.png)

Yes there could be multiple conditions added but at the end of the day we want to track any Activity from the Accounts. there should not be a difference for a successful signin or a failed one.

**And please do me a favor. Dont sent the Alerts to a mailbox which is part by the monitored Environment. If somebody gain access to your environment over the BreakGlass Account, he will also have access to the mailbox and can disable the Alerts.**


If you your organization is not using any Azure Ressources and didnt have a subscription you can achive this with Microsoft Defender for Cloud Apps. But the delay is significantly higher.

![MDA](/assets/images/BreakGlassAccounts/MDA.png)
