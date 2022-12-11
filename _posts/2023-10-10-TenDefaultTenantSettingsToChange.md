---
layout: post
title:  "10 böse Tenant Settings"
author: tim
categories: [ Entra, AzureAD, PIM, Intune, Exchange, Teams, Deutsch]
image: assets/images/TenDefaultTenantSettingsToChange\thumbnail.jpg
description: "Einstellungen welche nach der Erstellung einen Tenants geändert werden sollten!"
featured: true
hidden: false
---

# 10 Standard Setting welche geändert werden sollten, nachdem ein Tenant erstellt wurde

Dieses mal schreibe ich in deutscher Sprache da einige der Settings speziel für den Deutschen Raum relevant sind, aufgrund von regulatorischen Einschränkungen.

Nach der Erstellung eines Microsoft Cloud Tenant, sind einige Settings vom Standardwert so vordefiniert, das es schwierig ist, diese mit den deutschen bzw europaweiten Regulatorien zuvereinbaren. Aufgrund dessen gibt es hier eine Liste, welche sich fortlaufend erweitern wird, mit Einstellungen die man erstmal verändern sollte und dann im Nachgang diskutieren und prüfen kann ob man sie wieder verändern darf/kann. 

## Teilen

### Technology

    OneDrive, SharePoint, Teams

### Erklärung

    In einem Tenant darf man alle Datein die in Ondrive und Sharepoint abgelegt sind mit beliebigen Accounts, auch außerbald des Tenants teilen. Auch komplett Anonym.

### Ändern

1. [SharePoint Admin Portal](https://admin.microsoft.com/sharepoint?page=sharing&modern=true)
2. External Sharing runterschieben auf Least permissive sowohl bei Sharepoint als auch bei Onedrive.
3. **"Erweitertes External Teilen"** aufklappen
4. Haken setzen bei **"Gäste müssen sich anmelden mit dem gleichen Account, an den die Einladung versendet wurde"**

-------------------------

## Benutzer Consent

### Technology

### Erklärung

### Ändern

-------------------------

## Gruppen Erstellung

### Technology

### Erklärung

### Ändern

-------------------------

## Teams Datenablage

### Technology

    Teams

### Erklärung

Die Standard Datenablage für Teams ist Sharepoint. Allerdings kann man noch 3rd Party Services wie Dropbox mit dazu aktivieren. Da es sich hierbei aber um nicht Microsoft handelt muss man zusätzliche Datenschutz und Regulatorische Richtlinien klären.

### Ändern

1. [Teams Admin Portal](https://admin.teams.microsoft.com/company-wide-settings/teams-settings)
2. Ausschalten von allen 3rd Party Datenablage Lösungen

    ![TeamsFiles](/assets/images/TenDefaultTenantSettingsToChange/TeamsFiles.png)

-------------------------

## Gäste Einladung

### Technology

    Azure Active Directory

### Erklärung

In einem Default Tenant darf jeder User Daten mit Tenantfremden Accounts, anderen Leute in Teams einladen oder andere aktionen durchführen, welche dazu führen das im Tenant ein **Gäste** erstellt wird. Das sollte nicht komplett unkontrolliert bleiben, weshalb es zu empfehlen ist, diese Funktion erstmal auszuschalten.

### Ändern

1. [Azure Portal](https://portal.azure.com/#view/Microsoft_AAD_IAM/AllowlistPolicyBlade)
2. Auswahl auf **"Nur Benutzer mit bestimmten Administratorrollen können Gastbenutzer einladen"** setzen

    ![GästeEinladung](/assets/images/TenDefaultTenantSettingsToChange/G%C3%A4steEinladung.png)

-------------------------

## App Password

### Technology

    Azure Active Directory, MFA, Office 365

### Erklärung

Leagacy Applicationen (wie Office 2010), können nicht mit Multifaktor-Authentifizierung umgehen, was dazu führt das ein Login nicht möglich ist wenn MFA angefordert wird. Deshalb gibt es den Fallback auf **App Password**, was wie ein normales Password fungiert, aber zusätzlich den MFA request übergeht.

### Ändern

1. [MFA Portal](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx)
2. **"Benutzern das Erstellen von App-Kennwörtern zum Anmelden bei nicht browserbasierten Apps verweigern"** aktivieren

    ![App Password](/assets/images/TenDefaultTenantSettingsToChange/AppPassword.png)

-------------------------

## User AAD / Entra Access

### Technology

    Azure Active Directory, Entra

### Erklärung

    Jeder nicht priviligierte User kann sich im Entra / AAD Portal anmelden, was zu einer Discovery Möglichkeit für Angreifer führt.

### Ändern

1. [Azure Portal](https://portal.azure.com/Microsoft_AAD_UsersAndTenants/UserManagementMenuBlade/~/UserSettings)
2. "Zugriff auf Azure AD-Verwaltungsportal einschränken" -> **JA**

    ![AADPortal](/assets/images/TenDefaultTenantSettingsToChange/AADPortal.png)

------------------

allow user to remember MFA on devices they trust
^

Device Registration

App registration

Self Service Trials