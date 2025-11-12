---
layout: post
title:  "Schrödinger's Config"
author: tim
categories: [ Entra Id, Security, Compliance]
image: assets/images/schroedingersconfig/thumbnail.png
description: "When was the last time you checked your Entra ID/M365 Config"
featured: false
hidden: false
---

# Schrödinger's Config

In one of my last session, if ask the attendes how often they check their Entra ID or M365 Configuration. The answer was mostly "Never" or "Only when there is an incident". And this is not a good approach. It must be checked regulary to make sure to meet security and compliance requirements.

And to be honest. Even if you have a great team and nobody is allowed to change something inside your Tenant, it is always possible that something change without your knowledge.

## Microsoft

Even if nobody in your company is allowed or have the permission to change something inside your M365/Entra ID Tenant, it is always possible that something change without your knowledge. Microsoft is frequently rolling out new features, changing default settings or improving existing functionality. This could have a direct impact on your security and compliance posture.

## Microsoft365DSC

For me the best way to check your configuration is using [Microsoft365DSC](https://www.microsoft365dsc.com). With this Community driven project, you can export, compare and enforce your M365/Entra ID configuration.

![M365DSC](/assets/images/schroedingersconfig/m365dsc.png)

This is an example of how a configuration looks like:

```powershell
AzureAD            = @{
            GroupLifecyclePolicy              = @{
                AlternateNotificationEmails = @("ALERT@kerncloud.net")
            }
            Group                = @(
                @{
                    Description                   = 'Demo Group M365DSC'
                    Owners                        = @(
                        'mio@kerncloud.net'
                    )
                    DisplayName                   = 'DemoGroup-M365DSC'
                    Members                       = @(
                        'mio@kerncloud.net'
                    )
                    Ensure                        = 'Present'
                    IsAssignableToRole            = $True
                    SecurityEnabled               = $True
                    MemberOf                      = @(
                        'mogli@kerncloud.net'
                    )
                    MailEnabled                   = $True
                    MailNickname                  = 'demogroup-m365dsc'
                }
                @{
                    Description        = 'Demo Group M365DSC-2'
                    Owners             = @(
                        'demoOwner@kerncloud.net'
                    )
                    DisplayName        = 'DemoGroup-M365DSC-2'
                    Members            = @(
                        'demo@kerncloud.net'
                    )
                    Ensure             = 'Present'
                    IsAssignableToRole = $True
                    SecurityEnabled    = $True
                    MemberOf           = @(
                        'mogli@kerncloud.net'
                    )
                    MailEnabled        = $True
                    MailNickname       = 'demogroup-m365dsc-2'
                }
            )
            NamedLocationPolicy  = @(
                @{
                    IncludeUnknownCountriesAndRegions = $false
                    Ensure                            = 'Present'
                    IsTrusted                         = $true
                    OdataType                         = '#microsoft.graph.ipNamedLocation'
                    IpRanges                          = @(
                        '10.10.9.0/24'
                    )
                    DisplayName                       = 'kerncloud-Office-PROD'
                }
            )
```

### Compliance Report

With the Compliance Report feature, you can generate a report that shows the differences between your current configuration and a desired configuration. This report can be used to identify any changes that have been made to your configuration and to ensure that your configuration is compliant with your organization's policies.

If you wanna use is **manually** it is enough to use this command:

```powershell
Compare-M365DSCConfiguration -Source "C:\M365DSC\DesiredConfig.ps1" -destination "C:\M365DSC\CurrentConfig.ps1"
```

But the best way is to use it **automated**. For this you can deploy the solution from the provided  [Whitepaper](https://m365dscwhitepaper.azurewebsites.net/Managing%20Microsoft%20365%20with%20Microsoft365Dsc%20and%20Azure%20DevOps.pdf)

It will use Azure Devops Pipelines to export your configuration on a regular basis, compare it with the desired configuration and generate a report.

![compliance](/assets/images/schroedingersconfig/compliance.png)


### Test and Set every single Day your Configuration

Why dont test every single day your configuration in Entra ID or M3656? Its time consuming but with a automation it is possible. With Microsoft365DSC and Azure DevOps you can build a Pipeline which test your configuration every single day. If something change, you will get a notification and can check what change and why.

In my Environment i use this approach to check my configuration every single day. If something change, i get a notification and automatically apply the Configuration that are nessecary to bring my Tenant back to the desired state.

## Approval

With this automation approach it is also possible to build an approval process. If something change, you can review the change and approve or reject it. If you approve the change, it will be applied to your Tenant.

![approval](/assets/images/schroedingersconfig/approval.png)

## Conclusion

Yes this approach is more devops oriented but it is the best way to ensure that your Entra ID/M365 configuration is always compliant with your organization's policies.

The implementation is really simple and with the provided Whitepaper you can get started really fast.

BUT training your clicky admins is the most intense part. Because they need to understand that changes in the Tenant must be done over the desired configuration and not directly in the Tenant.
