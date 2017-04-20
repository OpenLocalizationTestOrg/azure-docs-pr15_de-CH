<properties
    pageTitle="Hybrid-Identität: Verzeichnisintegration tools Vergleich | Microsoft Azure"
    description="Diese Seite bietet eine umfassende Tabelle, die die verschiedenen Directory Integrationstools vergleicht, die Directory-Integration verwendet werden können."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Hybride Identität Directory Integration Vergleich

Im Laufe der Jahre Integrationstools Directory gewachsen entwickelt und.  Dieses Dokument soll bieten eine konsolidierte Ansicht dieser Tools und einen Vergleich der Funktionen, die in jedem verfügbar sind.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

>[AZURE.NOTE] Azure AD Connect umfasst Komponenten und zuvor als Dirsync- und AAD Sync veröffentlicht. Diese Tools werden nicht mehr einzeln veröffentlicht und alle aktualisieren in Azure AD Connect-Updates enthalten, sodass Sie immer wissen, wo die neuesten Funktionen.
>
>Dirsync- und Azure AD Sync sind veraltet. Weitere Informationen finden [hier](active-directory-aadconnect-dirsync-deprecated.md).


Verwenden Sie den folgenden Schlüssel für jede Tabelle.

● = Jetzt verfügbar  
FR = zukünftigen Versionen  
PP = Public Preview  

## <a name="on-premises-to-cloud-synchronization"></a>Lokale Synchronisierung Cloud

| Funktion  | Azure Active Directory herstellen  | Azure Active Directory Synchronization Services (AAD Sync) | Azure Active Directory-Synchronisierungstools (DirSync)| Forefront Identity Manager 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Verbindung zu lokalen AD-Gesamtstruktur | ● | ● | ● | ● |● |
| Verbinden mit mehreren lokalen Active Directory-Gesamtstrukturen |●  | ● |  | ● |● |
| Verbinden Sie mit mehreren lokalen Exchange-Organisationen | ● |  |  |  | |
| Verbinden Sie mit einzelnen lokalen LDAP-Verzeichnis | FR |  |  | ● |● |
| Verbinden Sie mit mehreren lokalen LDAP-Verzeichnissen |FR  |  |  | ● |● |
| Verbinden mit lokalen Active Directory und LDAP-Verzeichnissen auf lokalen |FR  |  |  | ● |● |
| Verbinden Sie mit benutzerdefinierten Systemen (z. B. SQL, Oracle, MySQL, etc.) | FR |  |  | ● |● |
| Synchronisieren von benutzerdefinierten Attributen (Directory Extensions) | ● |  |  |  |  |
|Verbinden Sie mit lokalen HR (z.B. SAP, Oracle eBusiness PeopleSoft)| FR |  |  | ● |● |
|FIM Regeln und Anschlüsse unterstützt für die Bereitstellung auf lokalen Systemen.|  |  |  | ● |● |

## <a name="cloud-to-on-premises-synchronization"></a>Lokale Synchronisierung Cloud

| Funktion  | Azure Active Directory herstellen  | Azure Active Directory Synchronization Services | Azure Active Directory-Synchronisierungstools (DirSync) | Forefront Identity Manager 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Rückschreiben von Geräten | ● |  | ● |  ||
| Attribut Rückschreiben (für Exchange Hybrid-Bereitstellung) | ● | ● | ● | ● |● |
| Rückschreiben von Benutzern und Gruppen |  ●|  | |  ||
| Rückschreiben von Kennwörtern (von Self-service-Kennwort zurücksetzen (SSPR) und Kennwort ändern) |  ● | ● |  |  ||



## <a name="authentication-feature-support"></a>Authentifizierung Unterstützung

| Funktion  | Azure Active Directory herstellen | Azure Active Directory Synchronization Services | Azure Active Directory-Synchronisierungstools (DirSync) | Forefront Identity Manager 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Kennwortsynchronisierung für einzelne lokale AD-Gesamtstruktur | ● | ● | ● |  ||
| Kennwortsynchronisierung für mehrere lokale Active Directory-Gesamtstrukturen |  ●| ● |  |  ||
| Einmaliges Anmelden mit | ● | ● | ● | ● |● |
| Rückschreiben von Kennwörtern (von SSPR und das Kennwort ändern) |●  | ● |  |  ||



## <a name="set-up-and-installation"></a>Einrichtung und Installation

| Funktion  | Azure Active Directory herstellen  | Azure Active Directory Synchronization Services | Azure Active Directory-Synchronisierungstools (DirSync) | Microsoft Identity Manager 2016 (MIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Unterstützt die Installation auf einem Domänencontroller | ● | ● | ● |  |
| Installation mit SQL Express unterstützt | ● | ● | ● |  |
| Einfaches Upgrade von DirSync |● |  |  |  |
| Lokalisierung von Admin UX Windows Server-Sprachen | ● | ● | ● |  |
|Lokalisierung von Endbenutzern UX Windows Server-Sprachen| |  |  |● |
| Unterstützung für WindowsServer 2008 und Windows Server 2008 R2 | ● Synchronisierung nicht für die Föderation| ● | ●  | ● |
| Unterstützung für Windows Server 2012 und Windows Server 2012 R2 | ● | ● | ● | ● |

## <a name="filtering-and-configuration"></a>Filtern und Konfiguration

Funktion  | Azure Active Directory herstellen | Azure Active Directory Synchronization Services | Azure Active Directory-Synchronisierungstools (DirSync) | Forefront Identity Manager 2010 R2 (FIM)| Microsoft Identity Manager 2016 (MIM)
:-------- |:--------:|:--------:|:--------:|:--------:|:--------:|
Filter für Domänen und Organisationseinheiten | ● | ● | ● | ●  | ●
Attributwerte Objekte filtern | ● | ● | ● | ●| ●
Lassen Sie minimalen Satz von Attributen (MinSync) synchronisiert zu | ● | ● |  ||
Verschiedene Servicevorlagen Datenflüsse Attribut angewendet werden kann |●  | ● |  ||
Lassen Sie Entfernen von Attributen aus, die aus AD Azure AD zu | ● | ● |  |  |
Erweiterte Anpassung Attribut Datenflüsse zulassen | ● | ● |  | ●  | ●

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
