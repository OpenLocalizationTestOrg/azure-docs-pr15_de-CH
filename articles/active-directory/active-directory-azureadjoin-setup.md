<properties
    pageTitle="Einrichten von Azure AD Join für Benutzer | Microsoft Azure"
    description="Erklärt, wie Administratoren Azure AD Beitreten zur lokalen Verzeichnis und geräteregistrierung einrichten können."
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
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="setting-up-azure-ad-join-in-your-organization"></a>Einrichten von Azure AD Join in Ihrer Organisation

Bevor Sie Azure Active Directory beitreten (Azure AD Join) einrichten, müssen Sie das lokale Verzeichnis der Benutzer in die Cloud synchronisieren oder manuell verwaltete Konten in Azure AD erstellen.

Detaillierte Informationen zum Synchronisieren von lokalen Benutzer Azure AD fällt in [Ihrer lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).


Verweisen Sie manuell erstellen und Verwalten von Benutzern in Azure AD [Verwaltung in Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Registrierung des Geräts einrichten
1. Melden Sie sich als Administrator bei Azure-Portal an.
2. Wählen Sie im linken Bereich **Active Directory**.
3. Wählen Sie auf der Registerkarte **Verzeichnis** das Verzeichnis ein.
4. Wählen Sie die Registerkarte **Konfigurieren** .
5. Gehen Sie zum Abschnitt **Geräte** .
6. Legen Sie auf der Registerkarte **Geräte** Folgendes ein:  
   * **Maximale Anzahl der Geräte pro Benutzer**: Wählen Sie die maximale Anzahl der Geräte, die ein Benutzer in Azure AD kann.  Ein Benutzer dieses Kontingent erreicht, werden sie nicht zusätzliche Geräte hinzufügen, bis eine oder mehrere ihrer vorhandenen Geräte entfernt sind.
   * **Erfordern MEHRSTUFIGE Authentifizierung, JOIN Geräte**: festlegen, ob Benutzer eine zweite Authentifizierung Faktor um ihr Gerät zu Azure AD hinzuzufügen. Weitere Informationen zu Azure mehrstufige Authentifizierung finden Sie unter [Erste Schritte mit Azure mehrstufige Authentifizierung in der Cloud](..\multi-factor-authentication\multi-factor-authentication-get-started-cloud.md).
   * **Benutzer können AZURE AD JOIN Geräte**: Wählen Sie die Benutzer und Gruppen, die Azure AD Geräte hinzufügen.
   * **Zusätzliche Administratoren bei AZURE Active Directory verbunden Geräte**: Azure AD Premium oder Enterprise Mobility Suite (EMS), Sie können die Benutzer lokale Administratorrechte auf dem Gerät erhalten. Globale Administratoren und gerätebesitzer werden standardmäßig lokale Administratorrechte erteilt.

<center>![Gerät Registrierung einrichten](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center>

Nachdem Sie Azure AD Join für die Benutzer eingerichtet, können sie über ihre persönlichen oder Firmenfirewall Geräte Azure AD verbinden.

Folgende sind die drei Szenarien, mit die Benutzer Azure AD Join einrichten:

- Benutzer fügen eigene Gerät direkt Azure AD.
- Benutzer Domänenbeitritt ein eigenen lokalen Active Directory und dann das Gerät Azure AD.
- Benutzer hinzufügen Geschäfts-oder Windows auf eines Geräts

## <a name="additional-information"></a>Weitere Informationen
* [Windows 10 für Unternehmen: Verwendungsmöglichkeiten für Geräte für die Arbeit](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern von Cloud-Funktionen für Windows 10 Geräte Azure Active Directory beitreten](active-directory-azureadjoin-user-upgrade.md)
* [Informationen Sie zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Geräte Domäne Azure AD für Windows 10 Erlebnisse](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)
