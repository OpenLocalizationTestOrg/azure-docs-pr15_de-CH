<properties
   pageTitle="Azure Active Directory Graph API | Microsoft Azure"
   description="Eine Übersicht und Schnellstart-Handbuch für Graph-API ermöglicht den programmgesteuerten Zugriff auf Azure AD über REST API-Endpunkte."
   services="active-directory"
   documentationCenter=""
   authors="PatAltimore"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API

> [AZURE.IMPORTANT] Azure AD Graph-API-Funktion ist auch über [Microsoft Graph](https://graph.microsoft.io/)eine einheitliche API, die APIs von anderen Microsoft-Diensten wie Outlook, OneDrive, OneNote, Planer und Office Graph Zugriff durch einen einzelnen Endpunkt und mit einem einzigen Token enthält.

Azure Active Directory Graph API ermöglicht programmgesteuerten Zugriff auf Azure AD über REST API-Endpunkte. Clientanwendungen können die Graph-API ausführen erstellen, lesen, aktualisieren und löschen böte Objekte und Daten. Graph-API unterstützt beispielsweise folgende allgemeine Vorgänge für ein Benutzerobjekt:

- Erstellen eines neuen Benutzers in einem Verzeichnis

- Detaillierte Eigenschaften des Benutzers wie ihre zu erhalten

- Aktualisieren Sie Eigenschaften eines Benutzers wie Standort und Telefonnummer oder ändern Sie ihres Kennworts

- Überprüfen der Gruppenmitgliedschaft des Benutzers für den rollenbasierten Zugriff

- Ein Benutzerkonto deaktivieren oder vollständig löschen

Benutzer-Objekte können Sie ähnliche Vorgänge für andere Objekte wie Gruppen und Programme ausführen. Aufrufen der Graph-API in einem Verzeichnis die Anwendung muss bei Azure AD registriert und Zugriff auf das Verzeichnis konfiguriert werden. Dies wird normalerweise durch einen Benutzer oder Administrator Zustimmung Fluss erreicht.

Zunächst mit Azure Active Directory Graph API finden Sie im [Diagramm API Schnellstart-Handbuch](active-directory-graph-api-quickstart.md)oder der [interaktiven Diagramm API-Referenzdokumentation](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).


## <a name="features"></a>Funktionen

Graph-API bietet die folgenden Features:

- **REST-API-Endpunkte**: die Graph-API ist eine RESTful-Dienst besteht aus Endpunkten erfolgt über standard-HTTP-Anfragen. Graph-API unterstützt XML oder Javascript Object Notation (JSON) Inhaltstypen Anfragen und Antworten. Weitere Informationen finden Sie in [Azure AD Graph REST-API-Referenz](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- **Authentifizierung mit Azure**: jeder Anforderung an die Graph-API durch Anhängen einer JSON Web Token (JWT) in der Authorization-Header der Anforderung authentifiziert werden muss. Dieses Token wird abgerufen, indem eine Anforderung für Azure AD token Endpunkt und gültige Anmeldeinformationen. OAuth 2.0 Client Anmeldeinformationen Flow verwenden oder der Autorisierungscode Flow ein Token zum Diagramm aufrufen zu gewähren. Weitere Informationen, [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

- **Rollenbasierte Autorisierung (RBAC)**: Sicherheitsgruppen RBAC in Graph-API ausgeführt wird. Beispielsweise möchten Sie bestimmen, ob ein Benutzer Zugriff auf eine bestimmte Ressource, kann die Anwendung Vorgang [Gruppenmitgliedschaft überprüfen (transitiv)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) aufrufen, der True oder False zurückgibt.

- **Differenzielle Abfrage**: Wenn Sie Änderungen in einem Verzeichnis zwischen zwei Zeiträume ohne häufige Abfragen der Graph-API überprüfen möchten, können Sie eine differenzielle Abfrage. Diese Anforderung wird nur die Änderungen zwischen der letzten differenziellen abfrageanforderung und die aktuelle Anforderung zurückgegeben. Weitere Informationen finden Sie in [Azure AD Graph-API differenzielle Abfrage](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).

- **Directory Extensions**: Wenn Sie eine Anwendung, die zum Lesen oder Schreiben von Eigenschaften für Verzeichnisobjekte entwickeln, können Sie registrieren und erweiterungswerte mithilfe der Graph-API verwenden. Beispielsweise wenn die Anwendung eine Eigenschaft Skype-ID für jeden Benutzer erfordert, registrieren die neue Eigenschaft in das Verzeichnis und auf jedem Benutzerobjekt zur Verfügung. Weitere Informationen finden Sie in [Azure AD Graph-API Directory Schema Extensions](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).

- **Gesichert durch berechtigungsbereiche**: AAD Graph-API macht berechtigungsbereiche, die Zugriff auf AAD Daten sichern/zugestimmt und unterstützen eine Vielzahl von app Clienttypen:
 - mit einer Benutzeroberfläche erhalten delegiert Zugriff auf Daten über Autorisierung von angemeldeten Benutzers)
  - Verwenden Sie die definieren Application-rollenbasierte Zugriffskontrolle wie Service-Daemon (app Rollen)

    Sowohl delegiert und app Rolle berechtigungsbereiche darstellen ein Recht von Graph-API verfügbar gemacht und können von Clientanwendungen durch Registrierung Berechtigungen [Funktionen in Azure-Verwaltungsportal](https://manage.windowsazure.com)angefordert werden. Clients können überprüfen, berechtigungsbereiche gewährt Ihnen indem Bereich ("scp") Anspruch im Zugriffstoken für delegierte Berechtigungen erhalten und Rollen ("Rollen") für Rollenberechtigungen anfordern. Erfahren Sie mehr über [Azure AD Graph-API Berechtigungsbereiche](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).


## <a name="scenarios"></a>Szenarien

Graph-API ermöglicht vielen Anwendungsszenarios. Die folgenden Szenarien sind die häufigsten:

- **Line of Business (einzelne Mandanten) Anwendung**: In diesem Szenario funktioniert ein für eine Organisation mit Office 365-Abonnement. Der Entwickler erstellt einer Anwendung, die mit Azure AD Aufgaben dieser Lizenz einen Benutzer zuweisen. Diese Aufgabe erfordert Zugriff auf Graph-API, damit Entwickler registriert die Anwendung in Azure AD Mieter und konfiguriert Lese- und für die Graph-API Schreibberechtigungen. Dann wird die Anwendung konfiguriert seine eigenen Anmeldeinformationen oder des Benutzers anmelden derzeit zu Token Graph-API aufrufen.

- **Software als Service (Multi-Tenant)**: In diesem Szenario entwickelt ein unabhängiger Softwareanbieter (ISV) gehostete Multi-Tenant-Web-Anwendung, die Managementfunktionen für andere Organisationen bereitstellt, die Azure AD verwenden. Diese Features erfordern den Zugriff auf Verzeichnisobjekte und die Anwendung muss also die Graph-API aufzurufen. Entwickler die Anwendung in Azure AD registriert, konfiguriert erfordern gelesen und Schreibberechtigungen für die Graph-API und ermöglicht es externen Zugriff, sodass andere Organisationen können die Anwendung im Verzeichnis zustimmen. Wenn die Anwendung zum ersten Mal ein Benutzer in einer anderen Organisation authentifiziert, werden Zustimmungsdialogfeld mit Berechtigungen angezeigt, die Anwendung erfordert.  Gewährung Zustimmung geben dann die Anwendung die angeforderten Berechtigungen an die Graph-API im Verzeichnis des Benutzers. Weitere Informationen zu Rahmen Zustimmung finden Sie unter [Übersicht über die Zustimmung](active-directory-integrating-applications.md).

## <a name="see-also"></a>Siehe auch

[Azure AD Graph API Schnellstart-Handbuch](active-directory-graph-api-quickstart.md)

[AD-Diagramm REST-Dokumentation](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Azure Active Directory developer's guide](active-directory-developers-guide.md)
