---
layout: post
title: "Your Entra ID Data is by a Big Chance Not in Your Country!"
author: tim
categories: [EntraID, Compliance, DataResidency, Azure]
image: assets/images/RememberDataLocation/thumbnail.png
description: "For EMEA regions: Entra ID data is fixed to Amsterdam and Dublin, regardless of where you deploy your workloads"
featured: false
hidden: false
---

# Your Entra ID Data is by a Big Chance Not in Your Country!

You just deployed your Azure workloads to Switzerland North for compliance, France Central for sovereignty, or Germany for regulatory reasons. Smart move. But here's the thing: **your Entra ID data didn't follow your workloads. It's still sitting in the Netherlands and Ireland.**

This misconception shows up in nearly every customer conversation. Engineers and architects spend weeks selecting the right Azure region for their infrastructure — working through compliance checklists, reviewing data residency requirements, calculating latency. And then nobody asks the obvious question: **where is my Entra ID data actually stored?**

The answer isn't what you think.

## How Entra ID Stores Your Data — The Core Store

Entra ID is not a service you deploy into a region. It's a globally distributed Identity as a Service (IDaaS) platform. Your tenant data lives in something Microsoft calls the Core Store — a set of scale units, each mapped to a geo-location.

When you created your tenant — or when somebody in your company signed up for Microsoft 365 years ago — that decision locked you into a geo-location. There is no "migrate to Switzerland" button. There is no support request that will move it. It's permanent.

For every European customer, the EMEA geo-location is backed by exactly two Azure regions:

- West Europe — Amsterdam, Netherlands
- North Europe — Dublin, Ireland

That's where your users, groups, device objects, application registrations, Conditional Access policies, and most other identity objects live. At rest. In Amsterdam and Dublin.

You can verify this yourself in the Microsoft documentation:

- [Microsoft Entra ID and data residency — Microsoft Learn](https://learn.microsoft.com/en-us/entra/fundamentals/data-residency)
- [Customer data storage for European customers — Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-365/enterprise/o365-data-locations?view=o365-worldwide&preserve-view=true)


**Dont get tricked by this!  The tenant registration country is just metadata. It doesn't determine where your data lives. The geo-location mapping is fixed and permanent.**

![TenantInfo](/assets/images/RememberDataLocation/tenantinfo.png)

## Why is This a Problem?

It isn't necessarily a problem in the security sense. Amsterdam and Dublin are solid, well-operated Microsoft datacenters with excellent resilience between the two regions. Microsoft also operates within the EU Data Boundary for EMEA tenants, which covers most GDPR requirements.

But here's where it becomes a compliance and governance issue:

- A lot of organizations are operating under sector-specific regulations — financial services, healthcare, public sector — where data residency requirements are strict. These organizations spend considerable effort ensuring their Azure SQL databases, their storage accounts, their VM workloads land in the right region.
- And then their entire identity infrastructure — the thing that controls access to all of it — is sitting in the Netherlands and Ireland, and nobody has formally documented that decision.
- That creates a gap in your compliance documentation that auditors will ask about.

## What About Switzerland, France, Germany?

Yes, Azure has regional datacenters in Switzerland, France, and Germany. You can deploy your workloads there. But **Entra ID is not a workload you place in a region.**

The EMEA geo-location mapping is fixed. Permanently. It doesn't matter if your tenant was created with "Germany," "Switzerland," "France," or "Netherlands" as the country — you get Amsterdam + Dublin, period. A tenant registration country is just metadata. The data location is determined by the geo-location, and that never changes.

Germany historically had a special exception for Microsoft 365 sovereign customers, but standard commercial Entra ID tenants follow the EMEA mapping — even German ones.

**The only way to get Entra ID data residency outside Amsterdam/Dublin for European customers:**

1. **US Government Cloud / Sovereign Clouds** — Separate cloud instances with full in-region data residency. Not available for standard commercial customers.
2. **Microsoft Entra External ID with Go-Local add-on** — A paid feature for customer-facing CIAM scenarios, currently only available for Australia and Japan. Irrelevant for your internal corporate directory.

## What Entra Components Are Affected — and Which Are Not

As Microsoft's documentation states, these Entra components store their data in the geo-location — which means Amsterdam + Dublin for EMEA:

- Directory Core Store (users, groups, devices)
- Conditional Access Policies
- App Registrations and Service Principals
- Microsoft Entra Provisioning
- Identity Protection data
- Device Registration Service

### MFA — A Partial Exception

Microsoft Entra multifactor authentication has datacenters in the United States, Europe, and Asia Pacific, and some MFA data is also stored in North America depending on configuration. Worth reading the dedicated MFA data residency page if you need to be precise:

- [MFA Data Residency — Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/authentication/concept-mfa-data-residency)

## What Should You Do With This Information?

Practically speaking: understand it, document it, and don't panic.

Most organizations under GDPR are compliant — Amsterdam and Dublin are EU datacenters, Microsoft has proper DPA agreements in place, and the EU Data Boundary commitment covers the Core Store. This isn't a security breach waiting to happen.

But there are concrete steps you should take:

1. **Audit your compliance documentation.** Does your DPIA mention that identity data is stored in the Netherlands and Ireland? It needs to.
2. **Tell your DPO immediately.** They need to know where your Entra ID data actually lives.
3. **Identify your data residency gaps.** If your compliance framework requires all data in a specific country, you need to document that Entra ID cannot meet that requirement — this is a real gap for some organizations.
4. **Check Azure's roadmap.** Microsoft is expanding local data residency for M365 Advanced Data Residency, but Entra ID Core Store still isn't included. Keep watching.
5. **If you're using Entra External ID** for customer-facing CIAM — the Go-Local add-on may eventually support more regions.

## Backup and Disaster Recovery — A Harder Truth

Here's something else worth understanding: you cannot move Entra ID to a different geo-location. Not for backup, not for compliance, not for any reason.

Some organizations think about this pragmatically: "OK, so our Entra ID is in Amsterdam/Dublin. But what if we back it up locally? Can we rebuild from a local copy if needed?" 

The answer is no. Not practically.

**Entra ID doesn't export to a portable backup format.** You cannot take a backup tape of your Entra ID directory from Switzerland or Germany and restore it somewhere else with the same functionality. Microsoft doesn't support Entra ID "failover" to a different geo-location. There is no restore mechanism that works that way. The Core Store is tightly integrated into Microsoft's infrastructure globally.

More importantly: **if Entra ID goes down in Amsterdam/Dublin, your backup — no matter where you stored it — is useless without the ability to move the service.**

This creates a subtle but serious risk: in a disaster scenario where the EMEA geo-location experiences an extended outage, you don't have a "Plan B" of restoring your identity infrastructure in your local region. You're waiting for Microsoft to recover the service in Amsterdam and Dublin. That's it.

Microsoft's design does provide resilience — the two regions (Amsterdam + Dublin) are geographically separated with network independence and failover capabilities. But that failover happens within Microsoft's infrastructure, not under your control. You cannot opt for "keep my identity backup in Switzerland" as a disaster recovery strategy.

For organizations with strict business continuity requirements, this is a real constraint worth understanding. Your Entra ID recovery depends entirely on Microsoft's ability to restore the EMEA geo-location. You have no alternative data location to recover from.

## TL;DR

Your Entra ID data **is** in Amsterdam and Dublin. It always has been for EMEA customers. It always will be. No exceptions. No migrations. No workarounds (except sovereign clouds, which you maybe don't have access to).

Moving your workloads to Switzerland, France, or Germany doesn't change this.

**Know where your identity data lives. Document it in your compliance records right now. And absolutely make sure your auditors aren't the ones who discover this for you.**

---

**Tags:** EntraID, Compliance, DataResidency, Security, GDPR

**References:**

- [Microsoft Entra ID data residency](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/data-residency)
- [Customer data storage for European customers](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/data-residency-eu)
- [MFA data residency](https://learn.microsoft.com/en-us/entra/identity/authentication/concept-mfa-data-residency)
- [Microsoft 365 data locations](https://learn.microsoft.com/en-us/microsoft-365/enterprise/o365-data-locations)
