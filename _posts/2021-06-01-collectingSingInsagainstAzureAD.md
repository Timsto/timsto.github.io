---
layout: post
title:  "Collecting SignIns against Azure Active Directory"
author: tim
categories: [ AzureAD, SignInLogs]
image: assets/images/collectingSingInsagainstAzureAD\thumbnail.jpg
description: "How to collect SignIns against Azure Active Directory"
featured: true
hidden: false
---
In this post i wanna give you an overview how you can collect SignIn Information against Azure Active Directory. 

### Azure AD Portal
First and the easiest way is to gether the SignIn Information over the [Azure AD Portal](https://aad.portal.azure.com). Just scroll down to section **Monitoring**, select **Sign-In** and found all of your SignIns activity from all Application that sign in against Azure Active Directory. 

![Azure Ad Portal](/assets/images/collectingSingInsagainstAzureAD/portal.png)

### PowerShell 
With powershell its also really easy but need some requirements. First of all you need the PowerShell Module **AzureADPreview** which contains the function **Get-AzureADAuditSignInLogs**. 

    Import-Module AzureADPreview -AllowClobber
    Connect-AzureAD
    Get-AzureADAuditSignInLogs

Tip: Use the parameter -Top 10 or save the output from Get-AzureADAuditSignInLogs inside a variable, otherwise your terminal will work some time and the outcomes brings you nothing.
![PowerShell](/assets/images/collectingSingInsagainstAzureAD/powershell.png)

### Graph API

Over Graph API it is quiet simple but requires a few prerequisites. First you need a program language and knowledge inside it, how to handle Web Request. In my example i use Powershell to send the Webrequest against the Microsoft Graph endpoint. 

'https://graph.microsoft.com/beta/users?$top=999&$select=displayName,userPrincipalName,signInActivity'

With the parameter 'top=999' you give directly a higher score on how many entitlements you will receive per Request. The default is 100 users, which means that you need to do a lot or more paging and more time. 
Most important is **signInActivity**. With this parameter you get the signin information, we wanna collect.  Yes, you can use the **/auditLogs/signIns** endpoint, but in most governance project is more worth to use **/users** endpoint. One important information. The SignIn activity from /users Endpoint just redirect internally to /auditLogs/signIns, which means there are limitations about how fast it will provide returns. in big environments and fast computing machine, it is necessary to handle some pauses. 

Last but not least. Dont forget handling your Access Token. For this use [AzApiCall](https://github.com/JulianHayward/AzAPICall). With this little powershell module you didn´t need to thing about all of the points from the top and just use this: 

    AzApiCall -url 'https://graph.microsoft.com/beta/users?$top=999&$select=displayName,userPrincipalName,signInActivity' -Method Get 

### Don´t forget Azure DevOps
Finally. Don´t forget Azure DevOps. I see in many projects that the identity teams did a great job to collecting and analyze SignIn information over the different possibilities but forget the User Activity from Azure DevOps. 
The problem is that using **Private Access Token (PAT)** or other ways to interact with DevOps, didn´t shows up in the Azure Active Directory SignIn log.  Nether as interactive or non-interactive.  Because of this some identity governance project will fall behind because there is always some developer (mostly external -> they didn´t use M365) which will be prevent to work over long time. 

Solution for this is to get the *user activity* from Azure DevOps. 
From a simple way just log in to your DevOps **Organization -> Users**. From here, a list of all Users is displayed, which you can sort by various aspects. 
Most company/projects need to get those information programmatic which mean you need an function to get those information. During my last project, we reverse engineer this problem and found this **url** to get the log information: 
**'https://vsaex.dev.azure.com/"DevOpsOrg"/_apis/userentitlements?api-version=5.0-preview.2'**

But be careful. This endpoint had some challenges: 
1. You need a PAT from a **DevOps Organization Admin** with the right scope to reach the Endpoint
2. The endpoint return an **CSV** outcome and you need to handle this. 

Don`t forget to do this for every AzDevOps Organization you have! 




