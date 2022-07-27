---
layout: post
title:  "Where are my Users working"
author: tim
categories: [AzureAD, CrossTenant, Quick]
image: assets/images/WhereAreMyUsersWorking\thumbnail.jpg
description: "find out in which Tenants your Users are working!"
featured: false
hidden: false
---

# Where are my Users Working?

Now, that most company have **Azure Active Directory** in place and know how to federate with other companies, to bring there collaboration to the next level, there comes one question up.

**In which Tenants are my users working?**

The answer for this was long time a really difficult question because AzureAD didnt provide the information for the central IT/Admin Persons.
Yes, there was a way to find out, but this just works in the User Context. Not an effective way for more then 10 people!

## How do I found it out!

With the new feature **Cross-Tenant Access Policy**, one of the major improvements is, that every Sign-In into other Tenants, beside your Home Tenant, will be logs in your Home-Tenant Sign-In Logs!

With the launch of XTAP (Cross-Tenant Access Policy), the Product Group create a wonderful Dashboard, which Display all Tenants where my Users are logging in and makes correlation between Apps and Users. And this can be perfectly used to find out in this Tenant my Users are invite as Guest for using Teams as an example.

### Requirements

- Write SignInLogs into Log Analytic Workspace

### Link
<https://portal.azure.com/#view/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/~/Workbooks>

and then choose the Workbook **"Cross-Tenant Access Activity"**.

![Overview](/assets/images/WhereAreMyUsersWorking/cross-tenant-workbook-overview.png)

Here you go. From there you could control your filter like Time range, External Tenant Id or Applications like Microsoft Teams.

![Filters](/assets/images/WhereAreMyUsersWorking/workbook-filters.png)


### Official Microsoft Documentation

<https://docs.microsoft.com/en-us/azure/active-directory/reports-monitoring/workbook-cross-tenant-access-activity>
