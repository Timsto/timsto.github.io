---
layout: post
title:  "Your MDM is not compliant state ready"
author: tim
categories: [ AzureAD, Intune, ConditionalAccess, Quick]
image: assets/images/collectingSingInsagainstAzureAD\thumbnail.jpg
description: "Using Custom Attributes for Devices to build your compliant State if your MDM is not ready"
featured: true
hidden: false
---

### Just use it if your Mobile Device Management is not ready for sending the compliant state to Azure AD!!!


#szenario: 
Your MDM cannot sent or your MDM is not ready to sent the compliant state to Azure Active Directory, because of different reason. 
That could one solution be that you build an automation around it. 

With the new Device Filter Options in Conditional Access (Still in preview (11.11.2021)) there is an oppotunity to make it simple. 

1. Take the compliant State information out of your current MDM. (CSV, API or whatever it provides you the information)
2. Take this info and update alle the devices with hopefully are already registred in AzureAD (registed or joined make not different) and update the Device.ExtensionAttributes. This can be achived through Graph API https://docs.microsoft.com/en-us/graph/api/device-update?view=graph-rest-1.0&tabs=http#example-2--write-extensionattributes-on-a-device . Plese be careful with Directory.ReadWrite.All permission! 
3. Build an Conditonal Acces Policy like this: 
a. Users -> All | Exclude Emergancy Access Accounts
b. Apps -> All 
c. Conditions: DeviceFilters (preview) -> < device.extensionAttribute10 -ne "compliant" >
d. Controll -> RequireMFA

Then you should have a solution that helps a BUT the goal should be -> MDM sent compliant state to AzureAD like Intune! 
