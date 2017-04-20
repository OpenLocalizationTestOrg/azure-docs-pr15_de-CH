<properties
    pageTitle="Richten Sie ein neues Gerät mit Azure AD während Setup | Microsoft Azure"
    description="Ein Thema, das beschreibt, wie Benutzer von Azure AD Join während ihrer ersten Ausführung festlegen."
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

# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>Richten Sie ein neues Gerät mit Azure AD während Setup

Windows 10 können Benutzer ihre Geräte Azure Active Directory (Azure AD) in der ersten Ausführung (FRX) hinzufügen. Dies ermöglicht Organisationen eingeschweißt Geräte ihre Mitarbeiter oder Studenten verteilen, oder ihre eigenen Geräte (CYOD) wählen.
Wenn Windows 10 Professional oder Windows 10 Enterprise Edition auf einem Gerät installiert ist, standardmäßig die Erfahrung der Installationsvorgang für Unternehmens-Geräte.

## <a name="to-join-a-device-to-azure-ad"></a>Azure AD ein Gerät hinzufügen


1. Wenn das neue Gerät einschalten und des Installationsvorgangs starten sollten Sie **Immer bereit** Meldung. Folgen Sie Ihr Gerät einrichten.
2. Starten Sie durch Anpassen der Region und Sprache. Akzeptieren Sie Microsoft-Softwarelizenzvertrag.
![Für Ihre Region anpassen](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Wählen Sie das Netzwerk für die Verbindung mit dem Internet verwenden möchten.
4. Wählen Sie, ob Sie persönliche oder eigenen Geräten verwenden. Wenn unternehmenseigene ist Klicken Sie auf **dieses Gerät in meiner Organisation gehört**. Azure AD Join Erfahrung wird gestartet. Es folgt Bildschirm sehen, wenn Sie Windows 10 Professional verwenden.
<center>
![Eigentümer dieser Bildschirm](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)

5.  Geben Sie die Anmeldeinformationen, die Sie von Ihrer Organisation bereitgestellt wurden.
<center>
![Anmeldeseite](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6.  Nach der Eingabe von Benutzername befindet sich entsprechender Mandanten in Azure AD. Sind in einer Föderationsdomäne werden Sie zu Ihrem lokalen Secure Token Service (STS) Server – z. B. Active Directory-Verbunddienste (AD FS) weitergeleitet.
7. Wenn ein Benutzer in einer Domäne ohne Verband sind Anmeldeinformationen Sie direkt auf den Azure AD gehostet. Wenn Unternehmensbranding konfiguriert wurde, außerdem finden Sie Ihr Firmenlogo und Text unterstützt.
8.  Sie sind eine Herausforderung mehrstufige Authentifizierung aufgefordert. Diese Herausforderung wird vom IT-Administrator konfiguriert.
9.  Azure AD prüft, ob diese Benutzergerät Teilnahme Verwaltung mobiler Geräte erfordert.
10. Windows das Gerät im Verzeichnis der Organisation in Azure AD registriert und gegebenenfalls in die Verwaltung mobiler Geräte registriert.
11. Sind verwaltete Benutzer öffnet Windows Desktop durch den automatischen Anmeldevorgang.
12. Sind Föderationsbenutzer richten Sie Ihre Anmeldeinformationen eingeben der Windows-Bildschirm.

> [AZURE.NOTE] Beitreten zu einer Domäne lokale Windows Server Active Directory in Windows Out of Box Experience wird nicht unterstützt. Wenn Sie einer Domäne einen Computer hinzufügen möchten, sollten Sie **Windows mit einem lokalen Konto einrichten** Link daher stattdessen wählen. Sie können dann die Einstellungen auf dem Computer Domäne wie vor.

## <a name="additional-information"></a>Weitere Informationen
* [Windows 10 für Unternehmen: Verwendungsmöglichkeiten für Geräte für die Arbeit](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern von Cloud-Funktionen für Windows 10 Geräte Azure Active Directory beitreten](active-directory-azureadjoin-user-upgrade.md)
* [Authentifizieren von Identitäten ohne Kennwörter über Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Informationen Sie zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Geräte Domäne Azure AD für Windows 10 Erlebnisse](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)
