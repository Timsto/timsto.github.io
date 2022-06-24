---
layout: post
title:  "Connection Methods for Azure"
author: tim
categories: [AzureAD, Quick]
image: assets/images/ConnectionToAzure\thumbnail.jpg
description: "Quick and dirty Code snip, this can be used for the programming connection to Microsoft Cloud Workloads"
featured: true
hidden: false
---

# The different Connection Methods

> **_Warning:_** Please remember that some Method will not work because of MFA interactions.

> **_Tested:_**   Posh 5 and Posh 7

# Table of Contents

1. [Connect-AzAccount](#Connect-AzAccount)
2. [Connect-AzureAD](#Connect-AzureAD)
3. [Connect-MgGraph](#Connect-MgGraph)
4. [RestfullApi (Graph + Azure)](#RestfullApi (Graph + Azure))

## Connect-AzAccount

### Connection via UserName + Password

``` powershell
$User = "marta.musterfrau@contoso.com"
$PWord = ConvertTo-SecureString -String "v3ry5tr0n9P@sSwOrd" -AsPlainText -Force
$Credentials = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $User, $PWord
Connect-AzAccount -Credentials $Credentials
```

### Connection via Service Principal + Client Secret

``` powershell
$ApplicationId = '00000000-0000-0000-0000-00000000'
$ClientSecret = 'SuperStrongSecret'
$Credential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $ApplicationId, $ClientSecret
Connect-AzAccount -ServicePrincipal -TenantId $TenantId -Credential $Credential
```

### Connection via Service Principal + Certificate Thumbprint

``` powershell
$Thumbprint = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'
$TenantId = 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyy'
$ApplicationId = '00000000-0000-0000-0000-00000000'
Connect-AzAccount -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -Tenant $TenantId -ServicePrincipal
```

---
## Connect-AzureAD

### Connection via UserName + Password

``` powershell
$User = "marta.musterfrau@contoso.com"
$PWord = ConvertTo-SecureString -String "v3ry5tr0n9P@sSwOrd" -AsPlainText -Force
$Credentials = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $User, $PWord
Connect-AzureAD -Credentials $Credentials
```

### Connection via Service Principal + Client Secret

``` powershell
$ApplicationId = '00000000-0000-0000-0000-00000000'
$ClientSecret = 'SuperStrongSecret'
$TenantId = 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyy'
$creds = [System.Management.Automation.PSCredential]::new($ApplicationId, (ConvertTo-SecureString $ClientSecret -AsPlainText -Force))
Connect-AzAccount -Tenant $TenantId -Subscription $SubscriptionId -Credential $creds -ServicePrincipal
```

### Connection via Service Principal + Certificate Thumbprint

``` powershell
$Thumbprint = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'
$TenantId = 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyy'
$ApplicationId = '00000000-0000-0000-0000-00000000'
Connect-AzureAD -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -Tenantid $TenantId
```

---
## Connect-MgGraph

### Connection via UserName + Password
Connect-MGGraph didnt provide a programmable way to insert Credentials, its needable to fetch an AccessToken via Connect-AzAccount
``` powershell
$User = "marta.musterfrau@contoso.com"
$PWord = ConvertTo-SecureString -String "v3ry5tr0n9P@sSwOrd" -AsPlainText -Force
$Credentials = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $User, $PWord
Connect-AzAccount -Credentials $Credentials

$contextForMSGraphToken = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile.DefaultContext

$newBearerAccessTokenRequest = [Microsoft.Azure.Commands.Common.Authentication.AzureSession]::Instance.AuthenticationFactory.Authenticate($contextForMSGraphToken.Account, $contextForMSGraphToken.Environment, $contextForMSGraphToken.Tenant.id.ToString(), $null, [Microsoft.Azure.Commands.Common.Authentication.ShowDialog]::Never, $null, https://graph.microsoft.com)

$AccessToken = $newBearerAccessTokenRequest.AccessToken

Connect-MGGraph -AccessToken $AccessToken
```

### Connection via Service Principal + Client Secret

``` powershell
$TenantId = 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyy'
$ApplicationId = '00000000-0000-0000-0000-00000000'
$ClientSecret = 'SuperStrongSecret'

$body =  @{
    Grant_Type    = "client_credentials"
    Scope         = "https://graph.microsoft.com/.default"
    Client_Id     = $ApplicationId
    Client_Secret = $ClientSecret
}
 
$connection = Invoke-RestMethod `
    -Uri https://login.microsoftonline.com/$TenantId/oauth2/v2.0/token `
    -Method POST `
    -Body $body

$token = $connection.access_token
 
Connect-MgGraph -AccessToken $token
```

### Connection via Service Principal + Certificate Thumbprint

``` powershell
$Thumbprint = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'
$TenantId = 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyy'
$ApplicationId = '00000000-0000-0000-0000-00000000'
 
Connect-MgGraph -ClientID $ApplicationId -TenantId $TenantId -CertificateThumbprint $Thumbprint
```

---

## RestfullApi (Graph + Azure)

as an Example connecting to Graph an GET my profile information!

### Connection via UserName + Password

``` powershell
$Url = 'https://graph.microsoft.com/beta/me'

$User = "marta.musterfrau@contoso.com"
$PWord = ConvertTo-SecureString -String "v3ry5tr0n9P@sSwOrd" -AsPlainText -Force
$Credentials = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $User, $PWord
Connect-AzAccount -Credentials $Credentials

$contextForMSGraphToken = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile.DefaultContext

$newBearerAccessTokenRequest = [Microsoft.Azure.Commands.Common.Authentication.AzureSession]::Instance.AuthenticationFactory.Authenticate($contextForMSGraphToken.Account, $contextForMSGraphToken.Environment, $contextForMSGraphToken.Tenant.id.ToString(), $null, [Microsoft.Azure.Commands.Common.Authentication.ShowDialog]::Never, $null, https://graph.microsoft.com)

$AccessToken = $newBearerAccessTokenRequest.AccessToken
$meProfile = Invoke-RestMethod -Headers @{Authorization = "Bearer $AccessToken"} -Uri $apiUrl -Method Get
```

### Connection via Service Principal + Client Secret

``` powershell
$Url = 'https://graph.microsoft.com/beta/me'

$body =  @{
    Grant_Type    = "client_credentials"
    Scope         = "https://graph.microsoft.com/.default"
    Client_Id     = $ApplicationId
    Client_Secret = $ClientSecret
}
 
$connection = Invoke-RestMethod `
    -Uri https://login.microsoftonline.com/$TenantId/oauth2/v2.0/token `
    -Method POST `
    -Body $body
$AccessToken = $connection.access_token

$meProfile = Invoke-RestMethod -Headers @{Authorization = "Bearer $AccessToken"} -Uri $apiUrl -Method Get

```

### Connection via Service Principal + Certificate Thumbprint

``` powershell
$Url = 'https://graph.microsoft.com/beta/me'

$Thumbprint = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'
$TenantId = 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyy'
$ApplicationId = '00000000-0000-0000-0000-00000000'
Connect-AzAccount -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -Tenant $TenantId -ServicePrincipal

$contextForMSGraphToken = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile.DefaultContext

$newBearerAccessTokenRequest = [Microsoft.Azure.Commands.Common.Authentication.AzureSession]::Instance.AuthenticationFactory.Authenticate($contextForMSGraphToken.Account, $contextForMSGraphToken.Environment, $contextForMSGraphToken.Tenant.id.ToString(), $null, [Microsoft.Azure.Commands.Common.Authentication.ShowDialog]::Never, $null, https://graph.microsoft.com)

$AccessToken = $newBearerAccessTokenRequest.AccessToken

$meProfile = Invoke-RestMethod -Headers @{Authorization = "Bearer $AccessToken"} -Uri $apiUrl -Method Get
```
