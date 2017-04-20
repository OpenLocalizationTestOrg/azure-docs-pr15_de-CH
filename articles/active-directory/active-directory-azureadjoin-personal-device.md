

<properties
    pageTitle="Verbinden ein Geräts mit Ihrer Organisation | Microsoft Azure"
    description="Erläutert, wie Benutzer ihre persönlichen Windows 10 Geräte in das Unternehmensnetzwerk anmelden können, sowie Bereitstellungsschritte für ein BYOD Szenario."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="join-a-personal-device-to-your-organization"></a>Verbinden eines Geräts mit Ihrer Organisation

## <a name="to-join-a-windows-10-device-to-your-organization"></a>Ihre Organisation ein Windows 10 hinzufügen

1.  **Wählen Sie im Menü **Start** .**
2.  Wählen Sie **Konten aus**und dann auf **Ihr Konto**.
3.  Klicken Sie auf **Hinzufügen oder schulkonto**, und geben Sie Konto Ihrer Organisation.
4.  Geben Sie auf der Anmeldeseite für Ihre Organisation Benutzernamen und Kennwort, und klicken Sie auf **OK**.
5.  Sie werden aufgefordert, eine mehrstufige Authentifizierung Herausforderung. (Hierfür wird vom IT-Administrator konfiguriert.)
6.  Azure Active Directory (Azure AD) überprüft, ob das Gerät mobile Device Management Registrierung benötigt.
7.  Windows das Gerät im Verzeichnis der Organisation in Azure AD registriert und gegebenenfalls in die Verwaltung mobiler Geräte registriert.
8.  Verwaltete Benutzer sind, nimmt Windows Sie auf dem Desktop durch das automatische anmelden.
9.  Wenn Föderationsbenutzer sind ein Windows-Bildschirm gelangen Sie Ihre Anmeldeinformationen eingeben.

## <a name="additional-information"></a>Weitere Informationen
* [Windows 10 für Unternehmen: Verwendungsmöglichkeiten für Geräte für die Arbeit](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern von Cloud-Funktionen für Windows 10 Geräte Azure Active Directory beitreten](active-directory-azureadjoin-user-upgrade.md)
* [Authentifizieren von Identitäten ohne Kennwörter über Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Informationen Sie zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Geräte Domäne Azure AD für Windows 10 Erlebnisse](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)
