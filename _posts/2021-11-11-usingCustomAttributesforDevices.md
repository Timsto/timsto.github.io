---
layout: post
title:  "Your MDM is not compliant state ready"
author: tim
categories: [ AzureAD, Intune, ConditionalAccess, Quick]
image: assets/images/customattributesDevices\thumbnail.png
description: "Using Custom Attributes for Devices to build your compliant State if your MDM is not ready"
featured: true
hidden: false
---

### Just use it if your Mobile Device Management is not ready for sending the compliant state to Azure AD

## Scenario

Your MDM cannot sent or your MDM is not ready to sent the compliant state to Azure Active Directory, because of different reason.
That could one solution be that you build an automation around it.

With the new Device Filter Options in Conditional Access (Still in preview (11.11.2021)) there is an opportunity to make it simple.

1. Take the compliant State information out of your current MDM. (CSV, API or whatever it provides you the information)
2. Take this info and update alle the devices with hopefully are already registered in AzureAD (registered or joined make not different) and update the Device.ExtensionAttributes. This can be achieved through Graph API [**MSGraph Device Update**](https://docs.microsoft.com/en-us/graph/api/device-update?view=graph-rest-1.0&tabs=http#example-2--write-extensionattributes-on-a-device). Please be careful with Directory.ReadWrite.All permission!
3. Build an Conditional Access Policy like this:
   1. Users -> All | Exclude Emergency Access Accounts
   2. Apps -> All
   3. Conditions: DeviceFilters (preview) -> **device.extensionAttribute10 -ne "compliant"**
   4. Control -> RequireMFA

Then you should have a solution that helps a BUT the goal should be -> MDM sent compliant state to AzureAD like **Intune**!
