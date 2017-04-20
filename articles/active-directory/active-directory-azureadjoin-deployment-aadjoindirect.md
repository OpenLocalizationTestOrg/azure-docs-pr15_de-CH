<properties
    pageTitle="Verwendungsszenarios und Bereitstellungsaspekte für Azure AD | Microsoft Azure"
    description="Erklärt, wie Administratoren Azure AD Join für Endbenutzer (Mitarbeiter, Studenten, andere Benutzer) einrichten. Darüber hinaus erläutert die realen Szenarien für die Verwendung von Azure AD Join."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

< tags "Active Directory" ms.service= ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="09/27/2016"

    ms.author="femila"/>

# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Verwendungsszenarios und Bereitstellungsaspekte für Azure AD

## <a name="usage-scenarios-for-azure-ad-join"></a>Verwendungsszenarien für Azure AD
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Szenario 1: Unternehmen in der cloud

Azure Active Directory beitreten (Azure AD Join) profitieren Sie derzeit betreiben und Verwalten von Identitäten für Ihr Unternehmen in der Cloud oder bald in die Cloud verschieben. Sie können ein Konto, die in Azure AD Anmelden bei Windows 10 erstellt haben. Schrittweise [Ausführung Erfahrung (FRX) erste](active-directory-azureadjoin-user-frx.md)oder bei Azure AD Menü [Einstellungen](active-directory-azureadjoin-user-upgrade.md)können Benutzer ihre Computer in Azure AD verknüpfen.  Benutzer genießen auch einmaliges Anmelden (SSO) Zugriff auf Cloudressourcen wie Office 365 entweder im Browser oder in Office-Programmen.

### <a name="scenario-2-educational-institutions"></a>Szenario 2: Bildungseinrichtungen

Bildungseinrichtungen haben in der Regel zwei Benutzertypen: Fakultäten und Studenten. Mitglieder gelten längerfristige Mitglieder der Organisation. Es ist wünschenswert, für lokale Konten für sie erstellt. Kursteilnehmer sind kürzere Mitglieder der Organisation und ihre Konten in Azure Active Directory verwaltet werden. Dies bedeutet, dass Verzeichnis Skalierung in der Cloud anstatt lokal gespeicherten abgelegt werden kann. Es bedeutet auch, dass Kursteilnehmer können Windows mit ihren Azure AD-Konten anmelden und Zugriff auf Office 365-Ressourcen in Office-Programmen.

### <a name="scenario-3-retail-businesses"></a>Szenario 3: Einzelhandel

Einzelhandel haben Saisonarbeiter und langjährige Mitarbeiter. Sie normalerweise für lokale Konten erstellen und Domäne beigetretenen Computer für längerfristige Vollzeitmitarbeiter. Saisonarbeiter sind kürzere Mitglieder der Organisation und sollte ihre Konten verwalten, Benutzerlizenzen leicht bewegt werden können. Beim Erstellen von Benutzerkonten in der Cloud mit Office 365-Lizenzen profitieren Sie diese Benutzer der Anmeldung bei Windows und Office mit Azure Konto, und Sie mehr Flexibilität mit ihren Lizenzen nach verlassen.

### <a name="scenario-4-additional-scenarios"></a>Szenario 4: Zusätzliche Szenarien

Zusammen mit der zuvor beschriebenen Vorteile Sie profitieren die Benutzer ihre Geräte Azure AD ein einfacheres verknüpfen, effizientes Device-Management, automatische mobile Device Management Registrierung und einmaliges Anmelden in Azure AD beitreten und lokalen Ressourcen.  


## <a name="deployment-considerations-for-azure-ad-join"></a>Aspekte der Bereitstellung Azure AD Join

### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Anwender mit firmeneigenen Gerät direkt in Azure AD


Unternehmen bieten Cloud nur Firmen, Unternehmen und Organisationen. Diese Partner können dann problemlos Unternehmen apps und Ressourcen mit Vorzeichen zugreifen. Dieses Szenario ist für Benutzer, die hauptsächlich in der Cloud, wie Office 365 oder SaaS-apps zugreifen, die auf Azure AD für Authentifizierung.

### <a name="prerequisites"></a>Erforderliche Komponenten
**Unternehmensebene (Administrator)**

*   Azure-Abonnement Azure Active Directory  

**Auf Benutzerebene**

*   Windows 10 (Professional und Enterprise Edition)

### <a name="administrator-tasks"></a>Aufgaben des Administrators
* [Registrierung des Geräts einrichten](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Aufgaben
* [Richten Sie ein neues Windows 10 Gerät mit Azure AD während setup](active-directory-azureadjoin-user-frx.md)
* [Einrichten eines Geräts 10 Windows Azure AD Menü Einstellungen](active-directory-azureadjoin-user-upgrade.md)
* [Verknüpfen eines Geräts Windows 10 für Ihr Unternehmen](active-directory-azureadjoin-personal-device.md)



## <a name="enable-byod-in-your-organization-for-windows-10"></a>BYOD in Ihrer Organisation für Windows 10 aktivieren
Sie können Ihre Benutzer und Mitarbeiter einrichten ihrer persönlichen Windows-Geräte (BYOD) auf Unternehmens-apps und Ressourcen. Die Benutzer können ihre Azure AD-Konten (Konten Arbeits- oder Schulcomputer) ein Windows personal Zugriff auf Ressourcen sicher und kompatible hinzufügen.

### <a name="prerequisites"></a>Erforderliche Komponenten
**Unternehmensebene (Administrator)**

*   Azure AD-Abonnement

**Auf Benutzerebene**

*   Windows 10 (Professional und Enterprise Edition)


### <a name="administrator-tasks"></a>Aufgaben des Administrators

* [Registrierung des Geräts einrichten](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Aufgaben
* [Verknüpfen eines Geräts Windows 10 für Ihr Unternehmen](active-directory-azureadjoin-personal-device.md)


## <a name="additional-information"></a>Weitere Informationen
* [Windows 10 für Unternehmen: Verwendungsmöglichkeiten für Geräte für die Arbeit](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern von Cloud-Funktionen für Windows 10 Geräte Azure Active Directory beitreten](active-directory-azureadjoin-user-upgrade.md)
* [Authentifizieren von Identitäten ohne Kennwörter über Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Informationen Sie zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Geräte Domäne Azure AD für Windows 10 Erlebnisse](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)
