---
layout: post
title:  "A modern Zero Trust implementation"
author: tim
categories: [ AzureAD, Security, Zero Trust]
image: assets/images/myfavidentityrepos\thumbnail.jpg
description: "some emample Szenarios for a modern Zero Trust implementation"
featured: true
hidden: false
---

# A modern Zero Trust implementation

In with this post I want to show you some example Szenarios for a modern Zero Trust implementation.

## Szenario 1: Protect your data on external devices

Somedays your allow that unmanaged devices can access your data. In this case you can use the Azure AD Conditional Access Policy to protect your data. This counts for internal users which use Bring-Your-Own-Devices (BYOD) or external users which use their own devices.

### Protect your data on external devices

For this case you can use a Conditional Access Policy which checks if the device is managed or not. If the device is not managed you can protect the access to your data with a MFA challenge and additional block the download of the data. The following screenshot shows the configuration of this policy.

![Conditional Access Policy](/assets/images/modernZeroTrustSzenarios/ConditionalAccessPolicy.png)

This Conditional Access Policy use the following conditions:

- Users and groups: All users
- Applications: All cloud apps
- Conditions:
  - Client apps: Browser
  - Device state: **NOT** Hybrid Azure AD joined devices or **NOT** compliant
- Grant: Require multi-factor authentication
- Session: Use Conditional Access App Control
  - Block downloads

This function use a combination from Conditional Access and Conditional Access App Control. The Conditional Access App Control is a feature from Azure AD Premium P2. This feature allows you to control the session of the user. In this case you can block the download of the data. (Same functionality is available inside of Defender for Cloud Apps)

## Sezenario 2: Self-Services for external partner into your Applications

In this case you want to allow external partners to access your applications. For this case you can use the Azure AD B2B functionality in combination with Entitlement Managenemt (Azure AD Premium P2 Feature). With this functionality you can allow external partners to request access to your applications. The following screenshot shows the configuration of this package.

It can reduce the process time and cost that is currently needed to create a list of allowed partner users, and then to manually add them to the application. It also reduces the risk of granting access to the wrong users, because the approver can review the request before it is granted.

![Access Package](/assets/images/modernZeroTrustSzenarios/AccessPackage.png)

This Access Package use the following configuration:

- Name: Access to Sample Application
- Description: This Access Package allows you to access the Sample Application
- Resources:
  - Sample Application
- Request: 
  - For Users not in your Directory
  - Specific connected organizations
    - Your Partner Organization
- Approval: 
  - Approval required
  - How many stages: 2
  - First approver: Your Partner Organization Product Owner
  - Second approver: Your own Product Owner

## Szenario 3: Just-in-Time Access for onpremises Ressources

In this case you want to allow your users to access onpremises ressources. For this case you can use the Azure AD Privileged Identity Management (PIM) functionality. With this functionality you can allow your users to request access to your onpremises ressources. The following screenshot shows the configuration of this package.

![PIM](/assets/images/modernZeroTrustSzenarios/PIM.png)

- Role Setting for Group which is writeback enabled
- Configure Member and not Owner for the Group
- Approver: Your Security Team

This functionality use Group Writeback v2 to write the group membership to onpremises. This group is used to grant permissions to the onpremises ressources. The following screenshot shows the configuration of the group.

![Group Writeback](/assets/images/modernZeroTrustSzenarios/GroupWriteback.png) 

The following screenshot shows the configuration of the onpremises ressource. 

![Group Writeback2](/assets/images/modernZeroTrustSzenarios/GroupWriteback2.png) 

### WARNING: This feature does not respect the lifetime of Kerberos Tokens. This means that the user can access the ressource until the user logoff from the device. This is a security risk and should be considered before using this feature.
