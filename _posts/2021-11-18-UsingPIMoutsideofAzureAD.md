---
layout: post
title:  "PIM Workaround for Exchange and Intune"
author: tim
categories: [ AzureAD, PIM, Intune, Exchange, Quick]
image: assets/images/UsingPIMoutsideofAzureAD\thumbnail.jpg
description: "Building a Workaround to use Azure Privileged Identity Management for Role outside of AzureAD Role System"
featured: false
hidden: false
---

# PIM Workaround for Exchange and Intune

## Disclaimer: This is just a workaround until the Product Group fix the issue that some roles are not in scope of Azure AD Right Management

### Problem

Currently there is no way to address Custom roles in Exchange or Intune through Azure Privileged Identity Management. Which means that there are some black spot there is no way to use PIM for this kind of Roles.

### Solution

Use role-assignable groups and PIM for groups.  By that there is a way to address such roles with PIM. Not the normal way but it is a workaround for it.

### How-To

1. Create a Role-assignable group

    ![NewGroup](/assets/images/UsingPIMoutsideofAzureAD/newgroup.png)

2. Assign this group in Exchange or Intune to the specific Role

    ![RoleAssignment](/assets/images/UsingPIMoutsideofAzureAD/roleassignment.png)

3. Enable PIM

    ![EnablePIM](/assets/images/UsingPIMoutsideofAzureAD/enablepim.png)

4. Create PIM Privileged access groups (preview) Config

    ![PIMConfig](/assets/images/UsingPIMoutsideofAzureAD/pimnconfig.png)

Done.

## Some Note´s

- Admin need to go over PIM Groups and not PIM Roles

- there are sometimes sync delays which means the Role is not instead active

- The PIM Report is not covering this aspect and will not shown the additional Role Assignments

- PAG are protected for Roles like "Group Administrator"

- Analyze the audit logs if there is an direct assignment for the new protected Roles
