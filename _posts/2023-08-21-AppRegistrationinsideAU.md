---
layout: post
title:  "The Power of Administrative Units"
author: tim
categories: [ Entra Id, Security,]
image: assets/images/AppRegistrationinsideAU\thumbnail.png
description: "Graph API Permission cannot be scoped.  Here is a cool workaround."
featured: false
hidden: false
---

# The Power of Administrative Units

## Introduction

Microsoft Graph API is a powerful tool to automate tasks in your environment. But there is one big problem. You cannot scope the permissions. This means, if you want to use the Graph API, you need to grant the permissions to the whole tenant. This is a big security risk. But there is a cool workaround. You can use Administrative Units to scope the permissions.

## What is an Administrative Unit

Administrative Units are a new feature in Azure AD. You can use them to group your users and devices. This is a great feature for big companies with a lot of users and devices. You can use them to group your users and devices by location, department or other criteria. You can also use them to scope your permissions for the Graph API.

## Use Cases

- You want to use the Graph API to automate tasks in your environment
- App Registration just eligable to a specific group of users/groups/devices

## Usage

### Create an Administrative Unit

First you need to create an Administrative Unit. You can do this in the Azure Portal. Go to Azure Active Directory -> Administrative Units -> New Administrative Unit. Give it a name and a description.

![Create AU](/assets/images/AppRegistrationinsideAU/CreateAU.png)

### Add Members

Now you can add members to your Administrative Unit. You can add users, groups and devices. The Problem is that this action whill happend in the context of you normal/priviledge User Account. If you wanna do this in the context of the App Registration, you need to use the Graph API or Powershell

```powershell

New-AzureADMSAdministrativeUnitMember -Id “<Id of the AU>” -OdataType "Microsoft.Graph.Group" -DisplayName "Example Group" -Description "This is an example group for demonstration purposes" -MailEnabled $True -MailNickname "examplegroup2" -SecurityEnabled $False -GroupTypes @("Unified")

```

![Add Members](/assets/images/AppRegistrationinsideAU/AddMembers.png)

### Create an App Registration

Next step is to create an App Registration. You can do this in the Azure Portal. Go to Azure Active Directory -> App Registrations -> New Registration. Give it a name and a description.

### Apply Permissions

Now the magic happend. Instead of assignment of API Permission, you assign a Entra Id Permission like Group Administrator or User Administrator, but not globally to the whole tenant, just to the Administrative Unit you created before.

![Apply Permissions](/assets/images/AppRegistrationinsideAU/ApplyPermissions.png)

![Apply Permissions2](/assets/images/AppRegistrationinsideAU/ApplyPermissions2.png)

### Test Permissions

Connect with the App Registration from the command line or direct with an API Call. (HOW to: <https://www.azurehero.de/ConnectinToAzure/> )

Then you can managed the devices, users and groups inside of the Administrative Unit.

![Test Permissions](/assets/images/AppRegistrationinsideAU/TestPermissions.png)


