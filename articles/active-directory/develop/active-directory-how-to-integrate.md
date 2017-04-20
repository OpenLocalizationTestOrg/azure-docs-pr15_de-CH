<properties
   pageTitle="Integration in Azure Active Directory | Microsoft Azure"
   description="Eine Anleitung zum Vorteile und Ressourcen für die Integration mit Active Directory Azure."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="integrating-with-azure-active-directory"></a>Integration von Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory bietet Unternehmen Unternehmen Identitätsmanagement für Cloudanwendungen.  Azure AD-Integration kann Benutzer optimierte anmelden und unterstützt die Anwendung IT-Richtlinien entsprechen.

## <a name="how-to-integrate"></a>Integration

Es gibt mehrere Methoden für die Anwendung in Azure AD integriert.  Nutzen Sie so viele oder so wenige Szenarien für die Anwendung geeignet ist.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Unterstützung von Azure AD So melden Sie sich bei Ihrer Anwendung

**Reduzieren Sie Reibung anmelden und Supportkosten.** Mithilfe von Azure AD anmelden Ihrer Anwendung keine Benutzer mehr Namen und ein Kennwort merken.  Als Entwickler müssen Sie ein Passwort weniger zu speichern.  Ohne vergessenen Kennwörtern behandeln möglicherweise allein deutlich zu senken.  Azure AD wird für einige der weltweit beliebtesten Cloudanwendungen, einschließlich Office 365 und Microsoft Azure anmelden.  Mit mehreren hundert Millionen Benutzer von Millionen Unternehmen Chancen sind Ihre Benutzer bereits angemeldet Azure AD.  Mehr zum Thema [Hinzufügen von Unterstützung für Azure AD anmelden](../active-directory-authentication-scenarios.md).

**Zeichen für Ihre Anwendung zu vereinfachen.**  Während der Anmeldung für die Anwendung kann Azure AD wichtige Informationen über einen Benutzer senden, damit vor der Anmeldung ausfüllen oder vollständig entfernen.  Benutzer können die Anwendung ihre Azure AD-Konto über eine vertraute Zustimmung Erfahrung wie in soziale Medien und mobile anmelden.  Jeder Benutzer kann registrieren und anmelden, um eine Anwendung in Azure AD integriert, ohne Beteiligung der IT.  Mehr zum Thema [Anmeldung der Anwendung für Azure AD-Konto anmelden](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Suchen Sie nach Benutzern, verwalten Sie Benutzer bereitstellen und steuern Sie den Zugriff auf die Anwendung

**Suchen Sie nach Benutzern im Verzeichnis.**  Graph-API verwenden, um Benutzer suchen und Browsen für andere Personen in ihrer Organisation, wenn andere Personen einladen oder Adressen Zugriff anstatt e-Mail-Adresse eingeben.  Benutzer können über eine vertraute Adressbuch Oberfläche einschließlich Detailansicht der Organisationshierarchie durchsuchen.  Erfahren Sie mehr über die [Graph-API](../active-directory-graph-api.md).

**Wiederverwenden Sie Active Directory-Gruppen und Verteilerlisten, die Ihr Kunde bereits verwalten.**  Azure AD enthält Gruppen, Ihr Kunde ist bereits für e-Mail-Verteilerlisten und Verwalten des Zugriffs.  Mithilfe der Graph-API diese Gruppen anstatt Ihren Kunden erstellen und Verwalten einer separaten Gruppen in Ihrer Anwendung verwenden.  Informationen kann auch zur Anwendung anmelden Token gesendet werden.  Erfahren Sie mehr über die [Graph-API](../active-directory-graph-api.md).

**Verwenden Sie Azure AD steuern, wer auf die Anwendung zugreifen.**  Administratoren und Anwendungsbesitzer in Azure AD können Clientanwendungen auf bestimmte Benutzer und Gruppen Zugriff zuweisen.  Graph-API können Sie dieser Liste lesen und bereitstellen und Aufheben der Bereitstellung von Ressourcen und in Ihrer Anwendung zugreifen können.

**Können Azure AD für Rollen-basierte Zugriffskontrolle.**  Administratoren und Anwendungsbesitzer können Benutzer und Gruppen Rollen zuweisen, die Sie beim Registrieren der Anwendung in Azure AD definieren.  Informationen zu der Anwendung anmelden Token gesendet und kann auch mithilfe der Graph-API gelesen werden.  Weitere Informationen zur [Verwendung von Azure AD für die Autorisierung](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Zugriff auf Profil des Benutzers, Kalender, Kontakte, E-Mail, Dateien und mehr

**Azure AD ist autorisierungsserver für Office 365 und anderen Microsoft Business Services.**  Wenn Unterstützung von Azure AD Zeichen in Ihre Anwendung oder verknüpfen Ihre Benutzerkonten Azure AD-Benutzerkonten mit OAuth 2.0-Unterstützung können Sie Lese- und Schreibzugriff auf ein Benutzerprofil, Kalender, e-Mail, Kontakte, Dateien und andere Informationen anfordern.  Sie nahtlos schreiben Ereignisse in Kalender des Benutzers und lesen oder Dateien auf ihren OneDrive schreiben.  Erfahren Sie mehr über [Office 365-APIs zugreifen](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Ihre Anwendung in Azure und Office 365 Marktplätze

**Förderung der Anwendung auf Millionen von Unternehmen bereits Azure AD verwenden.**  Benutzer Durchsuchen dieser Märkte bereits verwenden oder mehrere Clouddienste zu Cloud Kunden qualifiziert.  Erfahren Sie mehr über Ihre Anwendung in [Azure Marketplace](https://azure.microsoft.com/marketplace/partner-program/).

**Wenn Benutzer bei der Anwendung anmelden, erscheint es Azure AD-Abdeckung und Office 365-Startprogramm.**  Benutzer können der Anwendung schnell und einfach wieder Benutzer Engagement verbessern.  Erfahren Sie mehr über [Azure AD-Abdeckung](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Sichere Kommunikation Gerät Service und Dienst

**Mit Azure AD für Identitätsmanagement von Services und Geräte reduziert den Code schreiben müssen und ermöglicht der IT zum Verwalten des Zugriffs.**  Dienste und Geräte können Azure AD mit OAuth Token erhalten und diese Tokens auf Web-APIs verwenden.  Mit Azure AD vermeiden Sie komplexe Authentifizierungscode schreiben.  Da die Identitäten der Dienste und Geräte in Azure AD gespeichert werden IT Schlüssel und Sperrung zentral anstatt dazu separat in der Anwendung verwalten können.

## <a name="benefits-of-integration"></a>Vorteile der Integration

Integration mit Azure bietet Vorteile, die keine zusätzlichen Code schreiben.

### <a name="integration-with-enterprise-identity-management"></a>Integration mit Enterprise Identitätsmanagement

**Unterstützen der IT-Richtlinien einhalten.**  Organisationen Azure AD ihre Enterprise Identity Managementsysteme integriert eine Person die Organisation verlässt, sie automatisch Zugriff auf die Anwendung ohne verlieren IT müssen zusätzliche Schritte.  IT verwalten können, wer auf die Anwendung zugreifen und bestimmen, welche Richtlinien für Beispiel mehrstufige Authentifizierung - reduzieren müssen Code schreiben, um komplexe Unternehmensrichtlinien halten - erforderlich sind.  Azure AD bietet Administratoren ein detailliertes Überwachungsprotokoll des, Ihre Anwendung so angemeldet IT können leicht nachverfolgen.

**Azure AD erweitert Active Directory in der Cloud, sodass Ihre Anwendung mit Active Directory integrieren kann.**  Viele Organisationen auf der ganzen Welt verwenden Sie Active Directory als principal anmelden und Identity Managementsystem und Arbeiten mit erfordern.  Integration in Azure AD integriert app mit Active Directory.

### <a name="advanced-security-features"></a>Erweiterte Sicherheitsfunktionen

**Mehrstufige Authentifizierung.**  Azure AD bietet systemeigene Mehrfaktor-Authentifizierung.  IT-Administratoren können mehrstufige Authentifizierung Zugriff auf Ihre Anwendung benötigen, sodass Sie keinen code hierfür selbst.  Erfahren Sie mehr über [Mehrstufige Authentifizierung](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Anomale anmelden Erkennung.**  Azure AD verarbeitet mehr als eine Milliarde Anmeldungen täglich mit Algorithmen für maschinelles lernen verdächtigen Aktivitäten erkennen und IT-Administratoren über mögliche Probleme informieren.  Unterstützt Azure AD anmelden, ruft die Anwendung den Vorteil der Schutz. Weitere Informationen über [Bericht anzeigen Azure Active Directory zugreifen](../active-directory-view-access-usage-reports.md).

**Zugangskontrolle.**  Zusätzlich zur mehrstufigen Authentifizierung können Administratoren benötigen diese Kriterien erfüllt werden, bevor Benutzer Ihrer Anwendung anmelden können.  Die festgelegt werden können gehören IP-Adressbereich Clientgeräte Mitgliedschaft in Gruppen und den Status des Geräts für den Zugriff verwendet.  Erfahren Sie mehr über [bedingte Azure Active Directory-Zugriff](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Einfache Entwicklung

**Protokolle nach Industriestandard.**  Microsoft ist bestrebt, Unterstützung von Branchenstandards.  Azure AD unterstützt Authentifizierungsprotokolle SAML 2.0, OpenID verbinden 1.0, OAuth 2.0 und WS-Federation 1.2.  Graph-API ist OData 4.0 kompatibel.  Wenn die Anwendung bereits verbundenen anmelden Protokolle SAML 2.0 oder OpenID verbinden 1.0 unterstützt, kann Hinzufügen von Unterstützung für Azure AD einfach sein.  Erfahren Sie mehr über [Azure AD unterstützten Authentifizierungsprotokolle](../active-directory-authentication-protocols.md).

**Open-Source-Bibliotheken.**  Microsoft bietet vollständig unterstützten open-Source-Bibliotheken für Sprachen und Plattformen zur Beschleunigung der Entwicklung.  Quellcode unter Apache 2.0 lizenziert, und Sie können Verzweigung und zu den Projekten.  Erfahren Sie mehr über [Azure AD-authentifizierungsbibliotheken](../active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Weltweite Präsenz und hohe Verfügbarkeit

**Azure AD in Rechenzentren auf der ganzen Welt bereitgestellt und verwaltet und überwacht rund um die Uhr.**  Azure AD ist das Identity Managementsystem für Microsoft Azure und Office 365 und in 28 Rechenzentren weltweit bereitgestellt.  Verzeichnisdaten unbedingt mindestens drei Rechenzentren repliziert werden.  Globale Lastenausgleich Zugriff die nächstgelegene Kopie von Azure AD mit ihrer Daten gewährleisten und automatisch Anfragen an andere Datencenter zu leiten, wenn ein Problem erkannt wird.

## <a name="next-steps"></a>Nächste Schritte

[Schreiben von Code beginnen](../active-directory-developers-guide.md#getting-started).

[Benutzer mit Azure AD anmelden](../active-directory-authentication-scenarios.md)
