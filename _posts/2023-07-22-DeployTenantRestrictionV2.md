---
layout: post
title:  "Deploy Tenant Restriction V2"
author: tim
categories: [ AzureAD, Security, Intune, Quick]
image: assets/images/DeployTenantRestrictionV2\thumbnail.jpg
description: "Deploy Tenant Restriction V2 with Intune"
featured: false
hidden: false
---

# Introduction

Microsoft Tenant Restriction v2 is a feature that allows you to limit what your users can access when they use an external account to sign in from your networks or devices. With the Tenant restrictions settings included with cross-tenant access settings, you can control the external apps that your Windows device users can access when they're using external accounts. For example, let's say a user in your organization has created a separate account in an unknown tenant, or an external organization has given your user an account that lets them sign in to their organization. You can use tenant restrictions to prevent the user from using some or all external apps while they're signed in with the external account on your network or devices.

The Tenant restrictions V2 is now available as a public preview and it offers more features than its predecessor. It allows an organization admin to control whether your users can access external applications from your network or devices using externally issued identities, including accounts issued by external organizations and accounts created in unknown tenants.


## How to deploy with Intune?

Sadly there is no default Intune Policy for this, so we need to create a workaround with the Microsoft provided admx files.

### Step 1: Download the admx files

Go to the [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=104042) and download the admx Template for Windows 10 November 2021 Update v2.0

### Step 2: Upload the admx files to Intune

Go to the Intune Portal -> Devices -> Configuration Profiles -> Import ADMX -> select the downloaded admx file -> Upload

![Intune admx](/assets/images/DeployTenantRestrictionV2/Intune2.png)

### Step 3: Create a new Configuration Profile

 1. Go to the Intune Portal
 2. Devices
 3. Configuration Profiles
 4. Create Profile -> Windows 10 and later
 6. Setting Catalog -> search for "Tenant Restriction" -> select Cloud Policy Details
    1. Azure AD Directory ID
    2. Enable Firewall protection of Microsoft Endpoint
    3. Policy GUID
 7. Next
 8. Enabled "Cloud Policy Details"
 9. Enter Informations for:
    1. Azure AD Directory ID
    2. Enable Firewall protection of Microsoft Endpoint
    3. Policy GUID

! Informations got from the [Cross-tenant access settings](https://entra.microsoft.com/#view/Microsoft_AAD_IAM/CompanyRelationshipsMenuBlade/~/CrossTenantAccessSettings/menuId/IdentityProviders)

![Intune admx](/assets/images/DeployTenantRestrictionV2/XTAP.png)

9.  Next
10. Scope Tags -> Next
11. Assignments -> Next
12. Review + Create -> Create

![Intune admx](/assets/images/DeployTenantRestrictionV2/Intune3.png)

## Result

Really easy, if you try to login with an Account from another Tenant you will get the following error message:

![Error](/assets/images/DeployTenantRestrictionV2/Error.png)
