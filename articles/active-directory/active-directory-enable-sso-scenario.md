<properties
    pageTitle="Mit Azure Active Directory verwalten | Microsoft Azure"
    description="Dieser Artikel die Vorteile der Integration von Azure Active Directory mit lokalen, Cloud und SaaS-Applikationen."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="10/10/2016"
      ms.author="markvi"/>

# <a name="managing-applications-with-azure-active-directory"></a>Mit Azure Active Directory verwalten

Die eigentliche Workflow oder Inhalt haben Unternehmen zwei Grundvoraussetzungen für alle:

1. Steigerung der Produktivität, sollte Applikationen leicht zu ermitteln und Zugriff

2. Damit Sicherheit und Governance, sollte das Unternehmen die Kontrolle und Überwachung auf die tatsächlich greift jede Anwendung und können

In der Welt von Cloudanwendungen können am besten dies Identität zu steuern, "*wer was*".

Computing Terminologie:

- ** Bekannt als *Identität* - Verwaltung von Benutzern und Gruppen

- *Was* ist *Verwaltung* – die Verwaltung des Zugriffs auf geschützte Ressourcen genannt

Beide Komponenten werden zusammen als *Identity and Access Management (IAM)*definiert die [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) Group als bezeichnet "*Sicherheit nachlässig, das ermöglicht die richtigen Personen die richtigen Ressourcen rechts auf Zeiten aus den richtigen Gründen*".

Okay, so Was ist das Problem? Wenn IAM mit einer integrierten Lösung *nicht verwaltet* wird:

- Identity-Administratoren müssen einzeln erstellen und alle Benutzerkonten einzeln aktualisieren einer Aktivität redundant und zeitaufwändig.

- Speichern mehrerer Anmeldedaten Anträge müssen sie arbeiten muss. Daher eher Benutzern ihre Kennwörter aufzuschreiben oder andere Kennwort-Lösungen andere Datensicherheitsrisiken vorgestellt.

- Redundante aufwändige Aktivitäten reduzieren Benutzer und Administratoren arbeiten auf das Endergebnis des Unternehmens erhöhen.

So wird verhindert, dass was im Allgemeinen Organisationen integrierte IAM Lösungen?

- Die technischen basieren auf Plattformen, die bereitgestellt und jede Organisation ihre eigenen Applikationen angepasst werden.

- Cloudanwendungen wurden häufig stärker als IT-Organisation vorhandenen IAM-Projektmappen integrieren kann.

- Sicherheit und monitoring Tools erfordern zusätzliche Anpassung und Integration zu umfassenden E2E-Szenarien.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directory integriert

Azure Active Directory ist Microsofts umfassende Identität als Service (IDaaS)-Lösung, die:

- IAM aktiviert als Cloud-Dienst 

- Zentrale Verwaltung, Single Sign-On (SSO) und reporting 

- Unterstützt integrierte Verwaltung für [Tausende von Applikationen](https://azure.microsoft.com/marketplace/active-directory/) in der Galerie, einschließlich Salesforce, Google Apps, Feld, Concur und mehr. 


Mit Azure Active Directory alle veröffentlichen für Partner und Kunden (Business oder Consumer) weisen die gleiche Identität Zugriff auf Verwaltungsfunktionen.<br> Dadurch können Sie Ihre Betriebskosten.

Wenn Sie eine Anwendung implementieren, die noch nicht in der Galerie aufgeführt ist benötigen? Zwar etwas mehr Zeit als das Konfigurieren von SSO für Applikationen aus der Galerie, bietet Azure AD einen Assistenten, Sie bei der Konfiguration.

Der Wert von Azure AD geht über "nur" Cloudanwendungen. Sie können Sie mit sicheren remote-Zugriff auch mit lokalen verwenden. Mit sicheren remote-Zugriff können Sie beseitigen der Notwendigkeit VPN oder andere herkömmliche RAS Implementierung.

Azure AD bietet durch zentrale Verwaltung und single Sign-on (SSO) für die Anwendung, die Lösung Daten Sicherheit und Produktivität.

- Benutzer können mehrere Programme mit einem geben mehr Zeit Einnahmen generieren oder geschäftliche Vorgänge Aktivitäten zugreifen.

- Identity-Administratoren können auf Applikationen zentral verwalten.

Der Vorteil für den Benutzer und Ihr Unternehmen ist offensichtlich. Nehmen Sie sich die Vorteile Administrator Identität und der Organisation.

## <a name="integrated-application-benefits"></a>Integrierte Anwendung Vorteile

SSO-Prozess umfasst zwei Schritte:

- Authentifizierung der Prozess die Identität des Benutzers überprüfen.

- Autorisierung der Entscheidung aktivieren oder den Zugriff auf eine Ressource mit einer Zugriffsrichtlinie.

Wenn Sie Azure AD Applikationen verwalten und SSO aktivieren:

- Authentifizierung erfolgt lokal (z. B. AD) oder Azure AD-Konto des Benutzers.

- Autorisierung führt zur Azure AD Zuordnung und Schutz sicherstellen einheitliches Endbenutzererlebnis und Zuweisung, Speicherorte und MFA-Bedingung auf jede Anwendung unabhängig von seiner internen Funktionen hinzufügen können.

Es wichtig zu verstehen, wie die Autorisierung auf Anwendung erlassen hängt davon ab, wie die Anwendung in Azure AD integriert wurde.

- **Anwendung von Dienstanbieter bereits integriert** Office 365 und Azure sind Anträge auf Azure AD erstellt und für die umfassenden Identitäts- und Verwaltungsfunktionen abhängig. Zugriff auf diese Anwendung ist token ausstellen und Verzeichnisinformationen.

- **Programme von Microsoft und andere Programme vorinstalliert** Dies sind unabhängige Cloudanwendungen, die internen Anwendungsverzeichnis abhängig und können unabhängig von Azure AD. Zugriff auf diese Anwendung ist mit einer speziellen Anwendung Anmeldeinformationen ein Anwendungskonto zugeordnet. Abhängig von der Anwendung möglicherweise Anmeldeinformationen eine Föderation Token oder Benutzername und Kennwort für ein Konto, das zuvor in der Anwendung bereitgestellt wurde.

- **Lokale Anwendung** Applikationen durch den Azure AD-Anwendungsproxy hauptsächlich ermöglicht den Zugriff auf lokale Anwendung veröffentlicht. Diesen basieren auf einem zentralen im lokalen Verzeichnis wie Windows Server Active Directory. Zugriff auf diesen wird durch Auslösen des Proxys Anwendungsinhalt an den Endbenutzer anzubieten, und berücksichtigt die lokale Anmeldung Anforderung aktiviert.

Wenn ein Benutzer Ihrer Organisation beitritt, müssen Sie z. B. ein Benutzerkonto für den Benutzer in Azure AD für wichtige Vorgänge anmelden. Benötigt diese Benutzer Zugriff auf eine verwaltete Anwendung wie Salesforce müssen Sie auch ein Konto für diesen Benutzer in Salesforce erstellen und Azure-Konto arbeiten SSO zu verknüpfen. Wenn der Benutzer die Organisation verlässt, ist es ratsam, Azure AD-Konto löschen und alle entsprechenden Konten in der IAM-Anwendung, die der Benutzer Zugriff auf speichert.

## <a name="access-detection"></a>Access-Erkennung

In modernen Unternehmen kennen IT-Abteilung nicht alle verwendeten Cloudanwendungen. In Verbindung mit Cloud App Discovery bietet Azure AD eine Lösung für diese Programme zu erkennen.

## <a name="account-management"></a>Account-management

Traditionell in den verschiedenen Konten verwalten ist ein manueller Prozess durchgeführten IT oder personal in der Organisation. Voll automatisierter Azure AD Account-Management über alle Service Provider integrierte Programme und Anwendungsbereiche von Microsoft unterstützen automatisierte benutzerbereitstellung oder SAML JIT vorinstalliert.

## <a name="automated-user-provisioning"></a>Automatisches Benutzer bereitstellen

Einige Programme bieten Automatisierungsschnittstellen für Erstellung und entfernen oder Deaktivieren von Konten. Wenn ein eine solche Schnittstelle Anbieter wird es von Azure AD genutzt. Dies verringert die Betriebskosten, da administrative Aufgaben automatisch und die Sicherheit Ihrer Umgebung verbessert, weil das Risiko eines unbefugten Zugriffs verringert.

## <a name="access-management"></a>Verwaltung

Mithilfe von Azure AD können Sie Zugriff auf Applikationen mit einzelnen oder Regel gesteuerte Aufgaben verwalten. Sie können auch delegieren an die richtigen Personen in der Organisation gewährleisten optimale Kontrolle und Entlastung Helpdesk Management zugreifen.

## <a name="on-premises-applications"></a>Lokale Anwendung

Die integrierte Anwendung Proxy ermöglicht lokalen Anwendung, wodurch Benutzer veröffentlichen beide konsistent mit modernen Cloud-Anwendung und die Vorteile von Azure AD-Überwachung, Berichterstattung und Sicherheit-Funktionen zugreifen.

## <a name="reporting-and-monitoring"></a>Berichterstattung und Überwachung

Azure AD bietet integrierte Berichts- und Überwachungsfunktionen, mit denen Sie wissen, hat Zugriff auf Applikationen und sie tatsächlich verwendet.

## <a name="related-capabilities"></a>Verwandte Funktionen

Mit Azure können Sie die Anwendung detaillierte Richtlinien mit integrierten MFA sichern. Finden Sie mehr über Azure MFA [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Erste Schritte

Azure AD Applications integrieren zunächst betrachten Sie die [Integration von Azure Active Directory mit erste Schritte](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Siehe auch

[Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
