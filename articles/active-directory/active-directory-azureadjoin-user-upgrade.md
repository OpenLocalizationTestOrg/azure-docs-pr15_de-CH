<properties
    pageTitle="Einrichten eines Geräts 10 Windows Azure AD aus | Microsoft Azure"
    description="Erläutert, wie Benutzer in Azure AD im Menü Einstellungen beitreten können."
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

# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Einrichten eines Geräts 10 Windows Azure AD aus
Wenn Sie bereits Windows 7 oder Windows 8, und dem Computer oder Gerät auf Windows 10 aktualisiert wurde, können Sie über das Menü Einstellungen Azure Active Directory (Azure AD) verknüpfen.

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>Azure AD Menü Einstellungen hinzufügen


1. Klicken Sie im **Startmenü** auf **den Charm** .
2. Wählen Sie **Einstellungen** **System**->**über**->**Azure AD beitreten**.
<center>
![Verknüpfen von Azure AD Menü Einstellungen](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>

3. Klicken Sie auf **Weiter** im Fenster Azure AD beitreten.
<center>
![Nachrichtenfenster Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Anmeldeinformationen Sie Ihre anmelden. Diese kann gehören alle, die vollständige Authentifizierung erforderlich sind. Sind Teil des föderierten Mieter bieten Systemadministrator Föderation Erfahrung, die von Ihrer Organisation gehostet wird.
<center>
![Bereitstellen von Anmeldeinformationen](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Wenn Ihre Organisation Azure mehrstufige Authentifizierung Dank Azure AD konfiguriert, bieten des zweiten Faktors, bevor Sie fortfahren.
6. Klicken Sie auf **das Gerät zu verwaltende** **annehmen** .
7. Die sollte Meldung "das Gerät ist jetzt der Organisation in Azure AD Mitglied".


## <a name="additional-information"></a>Weitere Informationen
* [Informationen Sie zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Geräte Domäne Azure AD für Windows 10 Erlebnisse](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)
* [Authentifizieren von Identitäten ohne Kennwörter über Microsoft Passport](active-directory-azureadjoin-passport.md)
