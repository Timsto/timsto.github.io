---
layout: post
title:  "Control your DNS in a modern way"
author: tim
categories: [ Azure, DNS, AzDnsAsCode]
image: assets/images/AzDnsAsCode\thumbnail.jpg
description: "Its all about DNS. Learn how to control your Domain Name Service in a modern way."
featured: false
hidden: false
---

# Its always DNS

Anytime there is a problem that could not be solved, the Domain Name Service should be check.  Mostly there is a missing or wrong entry, the point not to the right destination.

In a modern world, the process and technic how DNS is handled should be rethink and redesign, because there are some problem with the classic way.

## **Problems**

### Process

Short Words: In most companies there is a dedicated network team that handle the Domain Name Service. And often this team has a full scheduled calender, which means it needs a lot of time until the entries are changed. And in a modern world we don´t have and especially need this time.

### DevOps

In a DevOps world the classic way how to handle DNS is always a problem. Think about deploying a web project over ARM or CI/CD Pipelines and need to change some DNS entries but didn´t had a Endpoint to automatic change the entries. Then the entry need to be change over an process (manually) and this cost time. A lot of time. And thats not DevOps.

## **Solution**

For all of this Problem i write the Powershell Module [**AzDnsAsCode**](https://github.com/Timsto/AzDnsAsCode).
With that module the Azure DNS Service can be configure with simple PowerShell cmdlet. In combination with CI/CD platform, all the problems from above can be solved.
> Yes, with ARM Templates or direct Management Graph Call it will do the same, but the entry in ARM or Graph is for most administrators higher then just start with PowerShell.

That are the Benefits of using **AzDnsAsCode**

- Powershell -> Yes, it is a benefit because most people know how to use powershell and don`t need to learn ARM or Webrequest.
- Authentication, will be handle automatically. It take the Access Token out of your AzContext. For pipeline it means that it works per default and locally it just require **Connect-AzAccount**.
- Configure DNS landscape in text files and protect them with git and access control.

### **How to use it**

First of all it need to be install. Because of the reason, that **AzDnsAsCode** is inside of **PSGallery**, it can be simple installed.

``` powershell
Install-Module AzDnsAsCode
Import-Module AzDnsAsCode
```

Now it is ready to run!

The Module contains different cmdlet for control the Azure DNS Service.

- *Get-AzDnsAsCodeZoneConfig* -> Get current Zone Config
- *New-AzDnsAsCodeZone* -> New DNS Zone in Azure DNS Service
- *Remove-AzDnsAsCodeZone* -> Remove a DNS Zone in Azure DNS Service
- *Set-AzDnsAsCodeConfig* -> Set a single Zone entry
- *Set-AzDnsAsCodeMultiConfig* -> Set **multi** Zone entries
- *Show-AzDnsAsCodeConfiguration* -> Show local configuration of the json file

optional: In the Github repo there is a yaml file for Azure DevOps how a pipeline could look like.
