<properties
   pageTitle="Wie Applikationen Azure Active Directory hinzugefügt werden."
   description="Dieser Artikel beschreibt, wie eine Instanz von Active Directory Azure Applications hinzugefügt werden."
   services="active-directory"
   documentationCenter=""
   authors="shoatman"
   manager="kbrint"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="shoatman"/>

# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Wie und warum Azure AD Programme hinzugefügt werden

Eines der Beginn rätselhafte Dinge beim Anzeigen einer Liste der Programme in der Instanz von Active Directory Azure verstehen, woher die Anträge und warum sie es.  Dieser Artikel bietet einen allgemeinen Überblick wie im Verzeichnis dargestellt und Kontext, der hilft Ihnen zu verstehen, wie eine Anwendung kam im Verzeichnis enthalten.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Welche Dienste bietet Azure AD Anwendung?

Azure AD nutzen eine oder mehrere der Dienstleistungen Applikationen hinzugefügt.  Diese Dienste umfassen:

* App-Authentifizierung und Autorisierung
* Authentifizierung und Autorisierung
* Einmaliges Anmelden (SSO) mit Föderation oder Kennwort
* Benutzer bereitstellen und Synchronisierung
* Rollenbasierte Zugriffskontrolle; Das Verzeichnis Anwendungsrollen um Aufgaben definieren basierte Autorisierung überprüft in einer Anwendung.
* oAuth Autorisierungsdienste (verwendet von Office 365 und anderen Microsoft-apps zum Autorisieren des Zugriffs auf APIs-Ressourcen.)
* Proxy- & Anwendung veröffentlichen; Veröffentlichen Sie eine Anwendung aus einem privaten Netzwerk mit dem internet

## <a name="how-are-applications-represented-in-the-directory"></a>Wie werden Programme im Verzeichnis dargestellt?

Anwendung in Azure AD mit 2 Objekte dargestellt: ein Application-Objekt und ein Service principal-Objekt.  Gibt ein Application-Objekt "Home" registriert / Verzeichnis "Besitzer" oder "Veröffentlichen" und mindestens service principal-Objekte für die Anwendung in jedem Verzeichnis in dem fungiert.  

Das Application-Objekt beschreibt die app Azure AD (Multi-Tenant-Dienst) und umfassen die folgenden: (*Hinweis*: Dies ist keine vollständige Liste.)

* Name und Logo mit Publisher
* Schlüssel (symmetrische und asymmetrische Schlüssel zur Authentifizierung der app)
* API Abhängigkeiten (oAuth)
* APIs/Ressourcen/Bereiche veröffentlicht (oAuth)
* App-Rollen (RBAC)
* SSO-Metadaten und Konfiguration (SSO)
* Benutzer, die Bereitstellung von Metadaten und Konfiguration
* Proxy-Metadaten und Konfiguration

Der Dienstprinzipalnamen ist ein Datensatz der Anwendung in jedem Verzeichnis die Anwendung handelt einschließlich Basisverzeichnis.  Dienstprinzipalnamen:

* Verweist auf ein Application-Objekt über die app-Id-Eigenschaft
* Datensätze lokaler Benutzer und Gruppe Anwendungsrolle Aufgaben
* Lokale Benutzer und Administrator Berechtigungen Datensätze gewährt der App
    * Beispiel: Berechtigung für die Anwendung Zugriff auf einen bestimmten Benutzer e-Mails
* Lokale Richtlinien Datensätze einschließlich bedingter Richtlinien
* Datensätze lokale alternative lokale Einstellung für eine Anwendung
    * Anspruchsregeln transformation
    * Attribut-Mappings (Benutzer Bereitstellung)
    * Mieter bestimmte Anwendung Rollen (wenn die Anwendung benutzerdefinierte Rollen unterstützt)
    * Name-Logo

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Ein Diagramm der Anwendungsobjekte und Dienstprinzipale auf Verzeichnisse

![Ein Diagramm zur Veranschaulichung, wie Anwendung Objekte und service mit Azure Instanzen Prinzipale.][apps_service_principals_directory]

Wie aus dem Diagramm oben sehen können.  Microsoft bietet zwei Verzeichnisse verwendet intern (links) Anwendung veröffentlichen.

* Für Microsoft-Apps (Microsoft Services Directory)
* Für integrierte 3rd Party Apps (Galerie Verzeichnis)

Anwendung Verleger/Kreditoren in Azure AD integrieren müssen ein Veröffentlichungsverzeichnis haben.  (Einige SAAS-Verzeichnis).

Anträge, die Sie selbst hinzufügen enthalten:

* Apps, die Sie entwickelt (AAD integriert)
* Apps für verbunden Single Sign-On
* Apps mit Azure AD-Anwendungsproxy veröffentlicht.

### <a name="a-couple-of-notes-and-exceptions"></a>Ein paar Notizen und Ausnahmen

* Nicht alle Service-Prinzipale zeigen für Anwendungsobjekte.  Nicht wahr? Azure AD ursprünglich erstellt wurde die Anwendung bereitgestellten Dienste wurden sehr viel begrenzter als die Dienstprinzipalnamen für eine app Identität wurde.  Die ursprüngliche Dienstprinzipal wurde Shape näher Windows Server Active Directory-Dienstkonto.  Aus diesem Grund ist es immer noch möglich, Service Sicherheitsprinzipale mithilfe von Azure AD PowerShell ohne Erstellen eines Application-Objekts erstellt.  Graph-API erfordert eine app-Objekt vor dem Erstellen eines Service principal.
* Nicht alle oben beschriebenen Informationen ist derzeit ausgesetzt programmgesteuert.  Nur sind in der Benutzeroberfläche verfügbar:
    * Anspruchsregeln transformation
    * Attribut-Mappings (Benutzer Bereitstellung)
* Weitere Informationen zu den Dienstprinzipalnamen und Anwendungsobjekte Näheres Azure AD Graph REST API-Referenzdokumentation.  *Hinweis*: der Azure AD Graph API-Dokumentation ist am ehesten mit einem Schema für Azure AD, die derzeit verfügbar ist.  
    * [Anwendung](https://msdn.microsoft.com/library/azure/dn151677.aspx)
    * [Dienstprinzipalnamen](https://msdn.microsoft.com/library/azure/dn194452.aspx)


## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Wie werden meine Azure AD-Instanz apps hinzugefügt?
Es gibt viele Methoden eine Anwendung in Azure AD hinzugefügt werden kann:

* Hinzufügen einer Anwendung in [Azure Active Directory Galerie](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Zeichen/in einem 3. Anwendung in Azure Active Directory integriert (Beispiel: [Smartsheet](https://app.smartsheet.com/b/home) oder [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
    * Während der Anmeldung/Benutzer sollen Berechtigung zur app auf ihr Profil zugreifen und andere Berechtigungen erteilen.  Die erste Person zustimmen verursacht einen Dienstprinzipal Verzeichnis hinzugefügt werden, die die Anwendung darstellt.
* / In Microsoft online Services wie [Office 365](http://products.office.com/) anmelden
    * Wenn Sie Office 365 abonnieren oder eine Testversion oder mehr Service Prinzipale in das Verzeichnis, das die verschiedenen Dienste, die zur Bereitstellung von Office 365 zugeordnete Funktionalität darstellt erstellt.
    * Einige Office 365-Diensten wie SharePoint erstellen Service Prinzipale auf Basis bei Bedarf sichere Kommunikation zwischen Komponenten, einschließlich Workflows zu.
* Eine Anwendung, die Sie entwickeln in der Azure-Verwaltungsportal hinzufügen: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Fügen Sie eine Anwendung entwickeln mit Visual Studio finden Sie unter:
    * [ASP.Net Authentifizierungsmethoden](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
    * [Verbundene Dienste](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Hinzufügen einer Anwendung mit der [Azure AD-Anwendungsproxy](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Eine Anwendung für einmaliges Anmelden mit SAML oder Kennwort SSO-Verbindung
* Viele andere z. B. verschiedene Entwickler in Azure auftritt und in API-Explorer über Developer Center

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Wer meinen Azure AD-Instanz hinzufügen darf?

Nur globale Administratoren können:

* Hinzufügen von apps von Azure AD app Gallery (integrierte 3rd Party Apps)
* Veröffentlichen einer Anwendung mithilfe von Azure AD-Anwendungsproxy

Alle Benutzer im Verzeichnis haben sie developing Applications und Ermessen über die Anwendung sie Freigabe/Zugriff auf ihre Unternehmensdaten ermöglichen.  *Merken Benutzer Zeichen oben in einer App und Erteilen von Berechtigungen kann in einen Dienstprinzipal erstellt wird.*

Diese möglicherweise zunächst sound zu, aber halten Sie Folgendes beachten:

* Apps wurden Windows Server Active Directory für die Benutzerauthentifizierung für viele Jahre nutzen, ohne die Anwendung im Verzeichnis erfasst/aufgezeichnet.  Jetzt haben die Organisation verbessert Sichtbarkeit genau wie viele apps Directory und wofür verwenden.
* Ohne Admin app veröffentlichen/Registrierung gesteuert.  Mit Active Directory Federation Services ist es wahrscheinlich, dass ein Administrator eine Anwendung als vertrauende für Entwickler hinzufügen.  Entwickler können jetzt Self-Service.
* Benutzer Signieren/bis apps über ihre Organisation Konten für geschäftliche Zwecke ist eine gute Sache.  Wenn anschließend die Organisation verlassen verlieren sie Zugriff auf ihr Konto in der Anwendung verwendeten.
* Haben Sie eine Aufzeichnung der Daten der Anwendung eine gute Sache ist freigegeben wurde.  Werden mehr als je zuvor transportable und klare aufgezeichnet, die Daten für die Anwendung freigegeben sind hilfreich.
* Apps mit Azure AD für oAuth entscheiden Sie genau die Berechtigungen, die Benutzer können Programme erteilen und die Berechtigungen ein Administrators zu erfordern.  Es sollte selbstverständlich, dass nur Administratoren größere Bereiche und größere Berechtigungen bekommen können.
* Benutzer hinzufügen und zulassen apps können überwacht Ereignisse, so können Sie die Prüfungsberichte Azure Management Portal um zu bestimmen, wie eine Anwendung zum Verzeichnis hinzugefügt wurde.

**Hinweis:** *Microsoft selbst arbeitet mit der Standardkonfiguration seit vielen Monaten.*

Mit sagte kann verhindern, dass Benutzer im Verzeichnis Programme hinzufügen und von Ausübung über welche Informationen sie freigeben mit Verzeichniskonfiguration im Azure-Verwaltungsportal ändern.  Die folgende Konfiguration in Azure-Verwaltungsportal Register "Konfigurieren" das Verzeichnis möglich.

![Screenshot der Benutzeroberfläche integrierte Anwendung konfigurieren][app_settings]


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Azure AD hinzufügen und Konfigurieren der Dienste für apps.

* Entwickler: [eine Anwendung mit AAD integrieren](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Entwickler: [Überprüfung Beispielcode für apps in Azure Active Directory auf Github integriert](https://github.com/AzureADSamples)
* Entwickler und IT-Profis: [Überprüfung der REST API-Dokumentation für Azure Active Directory Graph API](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* IT-Experten: [Azure Active Directory integrierte Anwendung aus der Galerie erfahren](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* IT-Experten: [Lernprogramme zur Konfiguration eines bestimmten integrierten apps suchen](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* IT-Experten: [Informationen zum Veröffentlichen einer Anwendung mithilfe von Azure Active Directory-Anwendungsproxy](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Siehe auch

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
