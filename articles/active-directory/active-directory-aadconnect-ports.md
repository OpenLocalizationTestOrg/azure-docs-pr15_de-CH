<properties
    pageTitle="Azure AD verbinden: Ports | Microsoft Azure"
    description="Diese Seite ist eine Seite technische Ports für Azure AD-Verbindung geöffnet werden"
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
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-required-ports-and-protocols"></a>Hybrid-Identität erforderlichen Ports und Protokolle
Das folgende Dokument ist eine technische Referenz zu den erforderlichen Ports und Protokolle, die für eine hybride Identität Lösung erforderlich sind. Verwenden Sie die folgende Abbildung und die entsprechende Tabelle.

![Was ist Azure AD verbinden](./media/active-directory-aadconnect-ports/required1.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Tabelle 1: Azure AD verbinden und lokalen AD
Diese Tabelle beschreibt die Ports und Protokolle, die für die Kommunikation zwischen Azure AD Connect Server und lokale AD.

Protokoll | Anschlüsse | Beschreibung
--------- | --------- |---------
DNS|53 (TCP/UDP)| DNS-Abfragen auf der Ziel-Gesamtstruktur.
Kerberos|88 (TCP/UDP)| Kerberos-Authentifizierung für Active Directory-Struktur.
MS-RPC |135 (TCP/UDP)| Während der Erstkonfiguration des Azure AD Connect-Assistenten verwendet, wenn Active Directory-Struktur gebunden wird.
LDAP|389 (TCP/UDP)| Zum Importieren von Daten aus Active Directory. Daten mit Kerberos unterschreiben und verschlüsseln.
LDAP-SSL|636 (TCP/UDP)| Zum Importieren von Daten aus Active Directory. Die Datenübertragung ist signiert und verschlüsselt. Nur verwendet, wenn Sie SSL verwenden.
RPC |49152 bis 65535 (Random hohe RPC-Port)(TCP/UDP)| Während der Erstkonfiguration Azure AD Connect verwendet, wenn die Active Directory-Gesamtstrukturen gebunden. Weitere Informationen finden Sie unter [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017)und [KB224196](https://support.microsoft.com/kb/224196) .

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Tabelle 2: Azure AD verbinden und Azure
Diese Tabelle beschreibt die Ports und Protokolle für die Kommunikation zwischen Azure AD Connect Server und Azure AD erforderlich sind.

Protokoll |Anschlüsse |Beschreibung
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Zum Downloaden von Zertifikatsperrlisten (Certificate Revocation Lists) Zertifikate überprüfen.
HTTPS|443(TCP/UDP)| Mit Azure AD verwendet.

Eine Liste von URLs und IP-Adressen müssen in der Firewall öffnen finden Sie unter [Office 365 URLs und IP-Adressbereiche](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-federation-serverswap"></a>Tabelle 3: Azure AD verbinden und Federation Server/WAP
Diese Tabelle beschreibt die Ports und Protokolle, die für die Kommunikation zwischen Azure AD Connect Server und Verbund-WAP-Servern erforderlich sind.  

Protokoll |Anschlüsse |Beschreibung
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Zum Downloaden von Zertifikatsperrlisten (Certificate Revocation Lists) Zertifikate überprüfen.
HTTPS|443(TCP/UDP)| Mit Azure AD verwendet.
WinRM|5985| WinRM-Listener

## <a name="table-4---wap-and-federation-servers"></a>Tabelle 4: WAP und Verbundserver
Diese Tabelle beschreibt die Ports und Protokolle, die für die Kommunikation zwischen dem Verbundserver und WAP-Servern erforderlich sind.

Protokoll |Anschlüsse |Beschreibung
--------- | --------- |---------
HTTPS|443(TCP/UDP)| Für die Authentifizierung verwendet.

## <a name="table-5---wap-and-users"></a>Tabelle 5: WAP und Benutzer
Diese Tabelle beschreibt die Ports und Protokolle, die für die Kommunikation zwischen Benutzern und WAP-Servern erforderlich sind.

Protokoll |Anschlüsse |Beschreibung
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Für Geräteauthentifizierung verwendet.
TCP|49443 (TCP)| Zertifikat verwendet.

## <a name="table-6a--6b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabelle 6a & 6 - Azure AD Connect Health Agent für (AD FS/Sync) und Azure AD
Die folgenden Tabellen beschreiben die Endpunkte, Ports und Protokolle, die für die Kommunikation zwischen Azure AD Connect Health Agents und Azure AD

### <a name="table-6a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabelle 6a - Ports und Protokolle für Azure AD Connect Health Agent für (AD FS/Sync) und Azure AD
Diese Tabelle beschreibt die folgenden ausgehenden Ports und Protokolle, die für die Kommunikation zwischen Azure AD Connect Health Agents und Azure AD erforderlich sind.  

Protokoll |Anschlüsse  |Beschreibung
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Ausgehende
Azure Servicebus|5671 (TCP/UDP)| Ausgehende

### <a name="6b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>6 - Endpunkte für Azure AD Connect Health Agent für (AD FS/Sync) und Azure AD
Eine Liste der Endpunkte finden Sie [im Abschnitt Anforderungen Azure AD Connect Health Agent](active-directory-aadconnect-health-agent-install.md#requirements).
