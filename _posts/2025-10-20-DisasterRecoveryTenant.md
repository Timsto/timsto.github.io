---
layout: post
title:  "Zurück in meinen Tenant"
author: tim
categories: [ Entra Id, Security, Compliance, Deutsch]
image: assets/images/DisasterRecoveryTenant\thumbnail.jpg
description: "Disaster Revocery für deinen Entra ID Tenant"
featured: true
hidden: false
---

# Zurück in meinen Tenant



In meinem Job bei Semperis habe ich immer mehr mit dem Thema Disaster Revocery für Entra ID (ehemals Azure AD) zu tun. Dabei geht es häufig um das Thema, wie man wieder zurück in seinen Tenant kommt, sollte ein Angreifer die Kontrolle über den Tenant übernommen haben. In diesem Blogpost möchte ich einige Ansätze und Überlegungen zu diesem Thema teilen.

## Shared Responsibility Model

Microsoft betreibt das Entra ID System und stellt sicher, dass die Infrastruktur sicher und verfügbar ist. Allerdings liegt die Verantwortung für die Verwaltung und den Schutz des eigenen Tenants beim Kunden. Das bedeutet, dass der Kunde sicherstellen muss, dass er über geeignete Sicherheitsmaßnahmen verfügt, um seinen Tenant zu schützen. Dazu gehört ein robustes Identity- und Access-Management, regelmäßige Überprüfungen der Sicherheitsrichtlinien und die Implementierung von Notfallplänen.

Um diese beim Namen zu nennen:

- Starke Authentifizierungsmethoden wie Multi-Faktor-Authentifizierung (MFA) und bedingter Zugriff sollten implementiert werden.
- ein robustes Conditional Access Konzept, damit priviligierte nur dann Zugriff bekommenm, wenn mehrere Faktoren erfüllt sind.
- Regelmäßige Überprüfung und Anpassung von Sicherheitsrichtlinien, um sicherzustellen, dass sie den aktuellen Bedrohungen entsprechen.
- Implementierung von Notfallplänen, um im Falle eines Sicherheitsvorfalls schnell reagieren zu können.

## Disaster Recovery Strategien

Es gibt verschiedene Strategien, die Unternehmen implementieren können, um sicherzustellen, dass sie im Falle eines Angriffs oder einer Kompromittierung ihres Entra ID Tenants wieder Zugriff erhalten. Einige dieser Strategien umfassen:
- **Notfallzugriffskonten**: Es sollten spezielle Konten eingerichtet werden, die nur im Notfall verwendet werden. Diese Konten sollten über starke Authentifizierungsmechanismen verfügen und regelmäßig überprüft werden. Zum Beispiel Emergency Access Accounts (Break Glass Accounts).
Dieser kann entweder ein normaler User Accounts sein oder aber auch eine App Registration mit privilegierten Rechten.

- **Ablaufpläne für privilegierte Konten**: Es sollten klare Ablaufpläne für privilegierte Konten definiert werden, um sicherzustellen, dass sie regelmäßig überprüft und aktualisiert werden. Dies kann durch die Verwendung von Azure PIM (Privileged Identity Management) erreicht werden. 
  
  **Achtung: Der BreakglassAccount sollte nicht in "eligable status" sein, da es zu folgenden Problemen kommen kann:**
  - Sollte ein Approval auf der global Administrator Rolle notwendig sein, kann der Emergency Account nicht genutzt werden, weil kein Approver mehr da ist.
  - Sollte ein Angreifer die P2 Lizenz entfernen, würde der BreakglassAccount keine Rechte mehr haben.

## Worst Case Szenario

Stellt euch nun mal vor, dass alle Maßnahmen die wir jetzt getroffen haben, nicht mehr greifen. Der Angreifer hat alle unsere Notfallzugriffskonten kompromittiert und wir haben keinen Zugriff mehr auf unseren Tenant. In diesem Fall müssen wir auf die Unterstützung von Microsoft zurückgreifen. Microsoft bietet einen Support-Prozess für solche Situationen an, bei dem sie den Kunden helfen, wieder Zugriff auf ihren Tenant zu erhalten. Dieser Prozess kann jedoch zeitaufwändig sein und erfordert in derRegel den Nachweis der Identität des Kunden. 

Daher stellt bite sicher, dass ihr alle notwendigen Informationen und Nachweise bereithaltet, um den Prozess so reibungslos wie möglich zu gestalten. Fragt eueren Microsoft Ansprechpartner nach den genauen Anforderungen und dem Ablauf des Prozesses. Sollten diese erstmal nicht wissen , wie der Prozess abläuft, besteht die Möglichkeit, dass sie dies mit dem Entra ID Produkt Team klären müssen oder eröffnet ein Support Ticket bei Microsoft, um den Ablauf dieses Processes zu erfragen. 

## Fazit

Die Sicherung und Wiederherstellung des Zugriffs auf einen Entra ID Tenant ist eine komplexe Aufgabe, die sorgfältige Planung und Implementierung erfordert. Unternehmen sollten sicherstellen, dass sie über geeignete Sicherheitsmaßnahmen verfügen und Notfallpläne implementieren, um im Falle eines Angriffs schnell reagieren zu können. Im schlimmsten Fall sollten sie bereit sein, die Unterstützung von Microsoft in Anspruch zu nehmen, um wieder Zugriff auf ihren Tenant zu erhalten.

