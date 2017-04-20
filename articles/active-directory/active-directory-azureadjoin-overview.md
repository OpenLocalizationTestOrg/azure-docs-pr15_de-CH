<properties
    pageTitle="Cloud-Funktionen für Windows 10 Geräte Azure Active Directory beitreten erweitern | Microsoft Azure"
    description="Eine detaillierte Übersicht wie Windows 10 Geräte als Azure AD Join Azure Active Directory registrieren nutzen können."
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

# <a name="extending-cloud-capabilities-to-windows-10-devices-through-azure-active-directory-join"></a>Erweitern von Cloud-Funktionen für Windows 10 Geräte Azure Active Directory beitreten

## <a name="what-is-azure-active-directory-join"></a>Was ist Azure Active Directory beitreten?
Azure Active Directory beitreten (Azure AD Join) ist die Funktionalität, die in Azure Active Directory ermöglichen die zentrale Verwaltung des Geräts ein firmeneigenes Gerät registriert. Es ermöglicht Benutzern wie Mitarbeiter und Studenten Enterprise Cloud Azure Active Directory herstellen. Dadurch vereinfacht Windows-Bereitstellung und Zugriff auf Organisationseinheit apps und Ressourcen von jedem Windows-Gerät firmeneigenen sowohl privat genutzte (BYOD).

Azure AD Join ist für Unternehmen, die Cloud-First/Cloud-only - in der Regel kleine und mittlere Unternehmen vorgesehen, die keinen lokalen Windows Server Active Directory-Infrastruktur. Sagte, Azure AD Join kann auch verwendet werden große Organisationen auf Geräten, die nicht traditionelle Domänenbeitritt (z. B. mobile Geräte), oder für Benutzer, die hauptsächlich auf Office 365 oder anderen Azure AD SaaS-apps.

Herkömmliche Domänenbeitritt noch bietet die besten lokalen auf Geräten, die der Domäne beitreten, zwar Azure AD Join für Geräte, die nicht der Domäne beitreten. Azure AD Join eignet sich auch zum Verwalten von Benutzern in der Cloud. Dies geschieht durch mobile Device Management-Funktionen nicht mit herkömmlichen Domänenverwaltungsprogramme wie Gruppenrichtlinien und System Center Configuration Manager (SCCM).

![Überblick über das Unternehmen Geräte und Geräte mit lokalen Active Directory und Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)


## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Warum sollten Unternehmen Azure AD Join verwenden?

* **Unternehmen, die in der Cloud**: wurden verschoben oder verschieben auf ein Modell, in dem Sie Ihre lokalen Speicherbedarf reduzieren und in der Cloud arbeiten möchten, Azure AD Join könnten Sie. Vielleicht haben Sie Azure AD-Konten manuell oder über lokale Active Directory synchronisieren erstellt. In beiden Fällen haben Sie ein Konto in Azure AD und Sie können Windows 10 anmelden. Die Benutzer können ihre Computer Azure AD über entweder die Out-of-Box-Experience (OOBE) oder Menü Einstellungen hinzufügen. Nach dem Beitritt genießen Benutzer anmelden (SSO) Zugriff auf Cloudressourcen wie Office 365 entweder im Browser oder in Office-Programmen.

* **Bildungseinrichtungen**: wir oft hören Szenarien gehört haben Bildungseinrichtungen zwei Benutzertypen: Fakultäten und Studenten. Lehrkräfte werden betrachtet längerfristige Mitglied der Organisation für lokale Konten für sie erstellt werden. Kursteilnehmer sind jedoch kürzere Mitglieder der Organisation und daher in Azure AD verwaltet werden können. Dies bedeutet, dass Verzeichnis Skalierung in die Cloud statt lokal gespeicherten abgelegt werden kann. Es bedeutet auch, dass Schüler Zugriff auf Office 365-Ressourcen in einem Browser oder in Office-Programmen erhalten und Windows mit ihren Azure AD-Konten anmelden können.

* **Handel**: wir einmal von Kunden schon haben ein anderes Szenario ist ihrem Wunsch Saisonarbeiter sind leichter zu verwalten.  Wiederum werden Konten für langfristige, Vollzeit-Mitarbeiter normalerweise erstellt auf Domäne Computer lokal. Aber Saisonarbeiter sind kürzere Mitglied der Organisation verwalten, Benutzerlizenzen leicht bewegt werden, sollten. Erstellen Benutzerkonten in der Cloud mit Office 365-Lizenzen kann Benutzer die Vorteile von Windows und Office-Clientanwendungen mit Azure Konto anmelden. In der Zwischenzeit verwalten Sie mehr Flexibilität mit ihren Lizenzen nach verlassen.
* **Andere Unternehmen**: Obwohl Benutzer in der lokalen Active Directory verwalten, Sie könnte noch profitieren Benutzer Azure Active Directory verbunden. Deshalb Azure AD Azure AD bietet ein einfacheres verknüpfen, effiziente Verwaltung automatische mobile Device Management Registrierung und Funktion zum einmaligen Anmelden und lokalen Ressourcen.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Welche Funktionen bietet Azure AD beitreten?
Mit Azure AD teilnehmen erhalten Sie Folgendes:

* **Automatische Bereitstellung von firmeneigenen Geräten**: Windows 10 Benutzer eine Gerät neue, eingeschweißte im Out-of-Box-Design ohne IT konfigurieren.


* **Unterstützung für moderne Formfaktoren**: Azure AD Join funktioniert auf Geräten, die traditionelle die Domäne beitreten Funktionen verfügen.  


* **Unterstützung für vorhandene Organisationseinheit Konten**: Benutzer müssen nicht mehr zum Erstellen und Verwalten einer persönlichen Microsoft-Konto zu am besten Unternehmen ausgestellt Geräte wie Windows 8. Sie können ihre vorhandenen Geschäftskonten in Azure AD verwenden. Für viele Organisationen bedeutet dies, dass Benutzer einrichten und Windows mit den gleichen Anmeldeinformationen, mit denen sie auf Office 365 anmelden können.


* **Automatische mobile Device Management Registrierung**: Geräte können automatisch in die Verwaltung mobiler Geräte bei Azure AD mit registriert. Dieser Vorgang läuft mit Windows Intune und Partner Mobilgerät. Geräteverwaltung mit Windows Intune erfolgt, können IT-Administratoren Monitor/Azure Active Directory verbunden Geräte neben Domäne Geräte in der SCCM-Verwaltungskonsole verwalten.


* **Einmaliges Anmelden für Unternehmensressourcen**: Benutzer genießen Sie einmaliges Anmelden aus dem Windows-Desktop-apps und Ressourcen in der Cloud, Office 365 und Tausende von Azure AD für Authentifizierung über [Azure AD Connect](active-directory-azureadjoin-deployment-aadjoindirect.md)auf Business-Applications. Firmeneigene Geräte Azure AD angehören können auch SSO für lokale Ressourcen wird das Gerät in einem Unternehmensnetzwerk und überall bei diesen [Azure AD-Anwendungsproxy](https://msdn.microsoft.com/library/azure/Dn768219.aspx).


* **OS Zustand Roaming**: Eingabehilfen, Websites, Wi-Fi-Kennwörter und andere Einstellungen auf firmeneigenen Geräten synchronisiert, ohne persönliches Microsoft-Konto.


* **Unternehmen Windows Store**: der Windows Store app Anschaffung und Azure AD-Konten Lizenzierung unterstützt. Organisationen können Volumenlizenz apps und den Benutzern in ihrer Organisation.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Funktionsweise verschiedene Geräte mit Azure AD Join

| Unternehmens-Gerät (Verbindung zu lokalen Domäne)                                                                                                                                                                                         | Corporate Gerät (Cloud hinzugefügt)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Persönliches Gerät                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Benutzer können Windows mit eigenen Anmeldeinformationen anmelden (wie heute).                                                                                                                                                                        | Benutzer können Windows mit eigenen Anmeldeinformationen anmelden, die in Azure Active Directory verwaltet werden. Dies gilt für Unternehmen Geräte in drei Fällen: 1) die Organisation keinen Active Directory vor (z. B. Kleinunternehmen). 2) die Organisation nicht alle Benutzerkonten in Active Directory erstellen (z. B. Konten für Studenten, Berater oder Saisonarbeiter sind nicht in Active Directory erstellt). 3) die Organisation hat Unternehmen Geräte (lokal) Mitglied einer Domäne, wie Mobiltelefonen oder Tablets Mobile SKU (z. B. eine sekundäre Gerät zur Factory/Einzelhandel) ausgeführt werden können nicht. Azure AD Join unterstützt Unternehmen Geräte für verwalteten und verbundenen Unternehmen verknüpfen. | Benutzer anmelden Windows mit ihren persönlichen Microsoft-Anmeldeinformationen (keine Änderung).                                                |
| Benutzer haben Zugriff auf Roamingeinstellungen und Unternehmen Windows Store. Diese Dienste arbeiten Konten und keine persönliches Microsoft-Konto erforderlich. Dies erfordert die Azure AD für lokale Active Directory herstellen.                                        | Benutzer kann Self-service einrichten. Sie können Durchlaufen der ersten Ausführung (FRX) über ihr Geschäftskonto als Alternative zu IT-Bereitstellung der Geräte, obwohl beide Methoden unterstützt werden.                                                                                                                                                                                                                                                                                                                                                                             | Benutzer können leicht ein Geschäftskonto verwaltet wird in Active Directory oder Azure AD.                                                      |
| Benutzer können SSO vom Desktop-apps, Websites, und Ressourcen, einschließlich der lokalen Ressourcen und Cloud-apps, die Azure AD für Authentifizierung verwenden.                                                                                                            | Geräte werden automatisch im Unternehmensverzeichnis (Azure AD) registriert und automatisch in Verwaltung mobiler Geräte. (Azure AD Premium Feature).                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Benutzer können SSO-apps und Websites/Ressourcen mit dieser Firma arbeiten.                                              |
| Benutzer können ihre persönlichen Konten Microsoft ihre persönlichen Bilder und Dateien ohne Beeinträchtigung der Unternehmensdaten zuzugreifen. (Roamingeinstellungen weiterhin mit ihrer Arbeit.) Das Microsoft-Konto ermöglicht SSO und mehr Laufwerke roaming Settings.  | Benutzer kann das Zurücksetzen eines Kennworts Self-service (SSPR) auf Winlogon, was bedeutet, dass sie ein vergessenes Kennwort zurücksetzen können. (Azure AD Premium Feature).                                                                                                                                                                                                                                                                                                                                                                                                                                   | Benutzer haben Zugriff auf Enterprise Windows Store, so dass sie erwerben und Branchen apps auf ihren persönlichen Geräten verwenden. |                                                               |


## <a name="additional-information"></a>Weitere Informationen
* [Windows 10 für Unternehmen: Verwendungsmöglichkeiten für Geräte für die Arbeit](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern von Cloud-Funktionen für Windows 10 Geräte Azure Active Directory beitreten](active-directory-azureadjoin-user-upgrade.md)
* [Authentifizieren von Identitäten ohne Kennwörter über Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Informationen Sie zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Geräte Domäne Azure AD für Windows 10 Erlebnisse](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)
