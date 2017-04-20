<properties
   pageTitle="Übersicht über die Sicherheit von Azure Identity Management | Microsoft Azure"
   description=" Identitäts- und Management Solutions Hilfe IT schützen Sie den Vorgang in firmeneigenen Rechenzentrum und in der Cloud zusätzliche Validierungsstufen bedingte Richtlinien wie mehrstufige Authentifizierung aktivieren. Dieser Artikel enthält einen Überblick über die wichtigsten Azure Sicherheitsfunktionen Identity Management zum. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-identity-management-security-overview"></a>Übersicht über die Sicherheit von Azure Identity management

Identitäts- und Management Solutions Hilfe IT schützen Sie den Vorgang in firmeneigenen Rechenzentrum und in der Cloud zusätzliche Validierungsstufen bedingte Richtlinien wie mehrstufige Authentifizierung aktivieren. Überwachung verdächtige Aktivitäten über erweiterte Sicherheit reporting, Überwachung und Alarmierung, können potenzielle Sicherheitsprobleme verringern. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) bietet einmaliges Anmelden für Tausende von Cloud apps (SaaS) und Zugriff auf Web-apps lokal ausführen.

Sicherheit Vorteile von Azure Active Directory (AD) können:

- Erstellen und Verwalten einer einzigen Identität für jeden Benutzer unternehmensweit Hybrid, Synchronisieren von Benutzern, Gruppen und Geräte
- Einzelne Anmeldung Zugriff auf Ihre Anwendung einschließlich von integrierten SaaS-apps
- Anwendungssicherheit durch regelbasierte mehrstufige Authentifizierung sowohl lokale erzwingen aktivieren und cloud-Applikationen
- Bereitstellen von sicheren Remotezugriff auf lokale ASP.NET-Webanwendungen durch Azure AD-Anwendungsproxy

Das Ziel dieses Artikels ist eine Übersicht über die wichtigsten Azure Sicherheitsfunktionen bereitstellen, die mit Identitätsmanagement. Wir bieten auch Links zu Artikeln, die alle Details der Features bieten, so informieren.  

Der Artikel behandelt die folgenden Core Azure Identity Management-Funktionen:

- Einmaliges Anmelden
- Reverse proxy
- Mehrstufige Authentifizierung
- Überwachung der Sicherheit, Alarme und Machine Learning-basierte Berichte
- Consumer Identitäts- und Zugriffsmanagement
- Registrierung des Geräts
- Privilegierte Identitätsmanagement
- Schutz
- Hybride Identitätsmanagement

## <a name="single-sign-on"></a>Einmaliges Anmelden

Einzelne anmelden (SSO) bedeutet Zugriff auf alle Programme und Ressourcen, die Geschäfte mit nur einem einzigen Benutzerkonto anmelden müssen. Nach der Anmeldung können Sie alle Anträge ohne Authentifizierung müssen zugreifen (Geben Sie z. B. ein Kennwort) ein zweites Mal.

Viele Unternehmen verlassen sich auf Software als ein Service (SaaS) Anträge auf Office 365 und Feld Salesforce Anwenderproduktivität. In der Vergangenheit IT-Mitarbeiter benötigt einzeln erstellen und Aktualisieren von Benutzerkonten in jeder SaaS-Anwendung und Benutzer für jede Anwendung SaaS Kennwort musste.

Azure AD erweitert lokale Active Directory in die Cloud können organisatorische Hauptkonto melden Sie sich bei ihrer Domäne Geräte und Ressourcen des Unternehmens verwenden, aber auch Web und SaaS-Applikationen für ihre Arbeit.

Nicht nur Benutzer müssen nicht mehrere Benutzernamen und Kennwörter verwalten, Zugriff kann automatisch bereitgestellt oder de bereitgestellte Grundlage Organisationsgruppen und deren Status als Mitarbeiter. Azure AD stellt Sicherheit und Governance-Zugriffskontrolle, die Benutzerzugriff SaaS anwendungsübergreifend zentral verwalten können.

Weitere Informationen:

- [Übersicht über Single Sign-On](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
- [Was ist Zugriff und single Sign-on Azure Active Directory?](../active-directory/active-directory-appssoaccess-whatis.md)
- [Einmaliges Anmelden für Azure Active Directory integrieren Sie SaaS-apps](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Reverse proxy

Azure AD-Anwendungsproxy können Sie lokale Programme wie [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) Sites [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx)und [IIS](http://www.iis.net/)veröffentlichen-apps in Ihrem privaten Netzwerk und bietet sicheren Zugriff für Benutzer außerhalb Ihres Netzwerks. Anwendungsproxy bietet remote-Zugriff und einmaliges Anmelden (SSO) für viele Arten von lokalen ASP.NET-Webanwendungen mit Tausenden von Azure AD unterstützt SaaS-Applikationen. Mitarbeiter können Ihre apps aus anmelden home auf ihren Geräten und über diesen cloudbasierten Proxy authentifizieren.

Weitere Informationen:

- [Aktivieren von Azure AD-Anwendungsproxy](../active-directory/active-directory-application-proxy-enable.md)
- [Veröffentlichen Sie mit Azure AD-Anwendungsproxy Applikationen](../active-directory/active-directory-application-proxy-publish.md)
- [Single-Sign-on Application Proxy](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
- [Arbeiten mit Zugangskontrolle](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Mehrstufige Authentifizierung
Azure mehrstufige Authentifizierung (MFA) ist eine Methode der Authentifizierung, die mehrere Verifizierungsmethode muss Benutzer anmelden und Transaktionen eine wichtige zweite Sicherheitsebene hinzugefügt. MFA hilft Schutz Zugriff auf Daten und Applikationen Benutzer Nachfrage nach einem einfachen Vorgang. Er bietet starke Authentifizierung über eine Reihe von Überprüfungsoptionen, Telefonanruf, SMS oder mobile app Benachrichtigung oder Überprüfung Code und 3. Partei OAuth-Token.

Weitere Informationen:

- [Mehrstufige Authentifizierung](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Was ist Azure mehrstufige Authentifizierung?](../multi-factor-authentication/multi-factor-authentication.md)
- [Funktionsweise von Azure mehrstufige Authentifizierung](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Überwachung der Sicherheit, Alarme und Machine Learning-basierte Berichte

Die Sicherheit überwachen und Alarme und Machine Learning-basierte Berichte, die zum Identifizieren der inkonsistente Zugriffsmuster helfen Ihnen Ihr Unternehmen schützen. Azure Active Directory-Zugriff und Berichte können Einblick in die Integrität und Sicherheit des Verzeichnisses Ihrer Organisation zu. Mit diesen Informationen bestimmen Verzeichnis Admin besser Sicherheitsrisiken liegen kann, damit sie angemessen, diese Risiken planen können.

Berichte werden in der Azure-Verwaltungsportal folgt kategorisiert:

- Anomalien enthalten – Anmeldeereignisse, die zu abweichenden gefunden wurden. Soll Sie solche Aktivitäten und können entscheiden, ob ein Ereignis verdächtige ermöglichen.
- Integrierte Berichte – Einblicke in wie Cloudanwendungen in Ihrer Organisation verwendet werden. Azure Active Directory bietet Integration mit Tausenden von Cloudanwendungen.
- Berichte – angeben Fehler beim externen Applikationen Konten Bereitstellung
- Benutzerspezifische Berichte – anzeigen Gerät-Zeichen Daten für einen bestimmten Benutzer
- Aktivitätsprotokolle – enthalten alle überwachten Ereignisse innerhalb der letzten 24 Stunden, 7 Tagen oder 30 Tagen sowie Gruppe Aktivität ändert und Kennwort zurücksetzen und Registrierung Aktivität.

Weitere Informationen:

- [Ihre Verwendung Berichte anzeigen](../active-directory/active-directory-view-access-usage-reports.md)
- [Erste Schritte mit Azure Active Directory](../active-directory/active-directory-reporting-getting-started.md)
- [Azure Active Directory-Berichte-Handbuch](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Consumer Identitäts- und Zugriffsmanagement

Azure Active Directory B2C ist eine hohe Verfügbarkeit, eine globale Identität Verwaltungsdienst kundenorientierter Anwendungsmöglichkeiten, der Hunderte von Millionen von Identitäten skaliert. Integrierbar in Mobile und Plattformen. Ihre Kunden können den Clientanwendungen anpassbare Erfahrungen mit bestehenden sozialen Gesellschaften oder neue Anmeldeinformationen anmelden.

In der Vergangenheit Anwendungsentwickler und Kunden in ihre Programme anmelden würde ihre eigenen Code geschrieben haben. Und sie würden lokalen Datenbanken oder Systemen zum Speichern von Benutzernamen und Kennwörtern. Azure Active Directory B2C bietet Ihrem Unternehmen ein besseres Applikationen mit Hilfe einer sicheren, standardisierten Plattform und große extensible Richtlinien Consumer Identitätsmanagement integrieren.

Bei Verwendung von Azure Active Directory B2C können Ihre Kunden für Ihre Anwendung ihrer bestehenden sozialen Konten (Facebook, Google, Amazon, LinkedIn) oder neue Anmeldeinformationen (e-Mail-Adresse und Kennwort oder Benutzername und Kennwort) anmelden.

Weitere Informationen:

- [Was ist Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
- [Azure Active Directory B2C-Vorschau: Melden Sie sich oben und Verbraucher in der Anwendung anmelden](../active-directory-b2c/active-directory-b2c-overview.md)
- [Azure Active Directory B2C-Vorschau: Anwendungstypen](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Registrierung des Geräts

Azure AD Geräteregistrierung bildet die Grundlage für Gerät [Zugangskontrolle](../active-directory/active-directory-conditional-access-on-premises-setup.md) Szenarien. Bei einem Gerät bietet Azure Active Directory Gerät mit einer Identität, mit das Gerät authentifiziert werden, wenn sich der Benutzer anmeldet. Authentifizierte Gerät und die Attribute des Geräts können dann verwendet werden, bedingte Richtlinien für Applikationen erzwingen, die in der Cloud und lokal gehostet werden.

In Kombination mit einer mobile Device Management (MDM) Lösung wie Intune werden Geräteattribute in Azure Active Directory mit zusätzlichen Informationen aktualisiert. Dadurch werden bedingte Regeln erstellen, die Zugriff von Geräten der Standards für Sicherheit und Compliance zu erzwingen.

Weitere Informationen:

- [Erste Schritte mit Azure Active Directory Geräteregistrierung](../active-directory/active-directory-conditional-access-device-registration-overview.md)
- [Lokal Zugriff bedingten mit Azure Active Directory Geräteregistrierung](../active-directory/active-directory-conditional-access-on-premises-setup.md)
- [Automatische Anmeldung mit Active Directory für Windows Azure Domäne](../active-directory/active-directory-conditional-access-automatic-device-registration.md)

## <a name="privileged-identity-management"></a>Privilegierte Identitätsmanagement
Azure Active Directory (AD) privilegierten Identity Management können Sie verwalten, steuern und überwachen privilegierten Identitäten und Zugriff auf Ressourcen in Azure AD und andere online-Dienste von Microsoft Office 365 oder Windows Intune.

Manchmal müssen Benutzer in Azure oder Office 365 Ressourcen oder anderen apps SaaS privilegierte Vorgänge ausführen. Oft bedeutet Organisationen haben Ihnen permanenten Systemzugriff in Azure AD. Deswegen wachsenden SRM für Cloud-gehosteten Ressourcen Unternehmen ausreichend überwachen können nicht die Benutzer mit ihren Administratorberechtigungen. Darüber hinaus ist ein Benutzerkonto mit Systemzugriff gefährdet, konnte, eine Verletzung ihrer gesamten Cloud-Sicherheit auswirken. Azure AD Berechtigungen Identitätsmanagement ermöglicht Lösung dieses Risikos.

Azure AD Berechtigungen Identitätsmanagement können Sie:

- Welche Benutzer Azure AD-Admins sind
- Aktivieren Sie on-Demand "just in Time" Administratorzugriff auf Microsoft Online Services wie Office 365 und Windows Intune
- Abrufen von Berichten über Administrator Zugriff Geschichte und Administrator-Aufgaben
- Lassen Sie sich über einen privilegierten Rolle

Weitere Informationen:

- [Azure AD Berechtigungen Identitätsmanagement](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Rollen in Azure AD Berechtigungen Identitätsmanagement](../active-directory/active-directory-privileged-identity-management-roles.md)
- [Azure AD privilegierten Identity Management: Das Hinzufügen oder Entfernen einer Benutzerrolle](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Schutz
Azure AD-Schutz ist eine, die eine konsolidierte Ansicht Risiken und potenzielle Schwachstellen Ihres Unternehmens Identitäten enthält. Identitätsschutz nutzt vorhandene Azure Active Directory Anomalien Erkennungsfunktionen (von Azure AD abweichenden Berichte) und stellt neue Risiken Ereignistypen, die Anomalien in Echtzeit erkannt werden.

Weitere Informationen:

- [Azure Active Directory-Schutz](../active-directory/active-directory-identityprotection.md)
- [Channel 9: Azure AD und Identität: Identität Schutz Vorschau](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Hybride Identitätsmanagement

Microsoft Methode zum Identität umfasst lokale und Cloud nur eine Benutzeridentität für Authentifizierung und Autorisierung für alle Ressourcen, unabhängig vom Standort erstellen.

Weitere Informationen:

- [Hybride Identität Whitepaper](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
- [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
- [Active Directory-Teamblog](https://blogs.technet.microsoft.com/ad/)
