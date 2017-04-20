<properties
    pageTitle="Azure AD Verbindung synchronisieren: Azure AD-Dienstkonto verwalten | Microsoft Azure"
    description="Dieses Thema beschreibt das Dienstkonto Azure AD wiederherstellen."
    services="active-directory"
    keywords="AADSTS70002, AADSTS50054, Kennwort für Azure AD Connect Sync Connector Service-Konto zurücksetzen"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD Verbindung synchronisieren: Verwalten von Azure AD-Dienstkontos
Azure Active Directory Connector verwendet soll kostenlos. Möchten Sie die Anmeldeinformationen zurückzusetzen, ist in diesem Thema für Sie. Beispielsweise verfügt ein globaler Administrator versehentlich zurückgesetzt das Kennwort für das Dienstkonto mit PowerShell.

## <a name="reset-the-credentials"></a>Anmeldeinformationen zurücksetzen
Wenn das Dienstkonto für Azure Active Directory Connector definiert Azure AD aufgrund Authentifizierung kontaktieren kann, kann das Kennwort zurückgesetzt.

1. Der Synchronisierungsserver Azure AD-Verbindung anmelden und PowerShell starten.
2. Führen Sie `Add-ADSyncAADServiceAccount`.  
![PowerShell-Cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Anmeldeinformationen Sie Azure AD globaler Administrator.

Dieses Cmdlet legt das Kennwort für das Dienstkonto und Azure AD sowohl das Synchronisierungsmodul aktualisiert.

## <a name="known-issues-these-steps-can-solve"></a>Bekannte Probleme lösen diese Schritte
Dieser Abschnitt ist eine Liste der Fehler von Kunden, die eine Anmeldeinformationen für das Dienstkonto Azure AD zurücksetzen behoben wurden.

-----------
Ereignis 6900  
Der Server ist ein unerwarteter Fehler beim Verarbeiten einer Benachrichtigung:  
AADSTS70002: Fehler überprüfen Anmeldeinformationen. AADSTS50054: Kennwort wird für die Authentifizierung verwendet.

----------
Ereignis 659  
Fehler beim Abrufen der Konfiguration Kennwort synchronisieren. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Fehler überprüfen Anmeldeinformationen. AADSTS50054: Kennwort wird für die Authentifizierung verwendet.

## <a name="next-steps"></a>Nächste Schritte

**Themen (Übersicht)**

- [Azure AD Verbindung synchronisieren: verstehen und Synchronisation anpassen](active-directory-aadconnectsync-whatis.md)
- [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
