<properties
    pageTitle="Liste der verfügbaren Anschlüsse und API-Apps | Microsoft Azure App Service"
    description="Informieren Sie sich über Connectors und API-Apps in Azure App Service"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>


# <a name="list-of-connectors-and-api-apps-to-use-in-your-logic-apps"></a>Liste der Connectors zu API-Apps in Ihren Apps Logik
>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2014-12-01-Vorschau-Schemaversion. Version Logik Apps allgemeine Verfügbarkeit finden Sie in der [Liste Connectors](../connectors/apis-list.md).

Informationen Sie über die verfügbaren Anschlüsse und API-Apps von Microsoft zur Verwendung in Logik-Apps erstellt.

Preisinformationen und eine Liste der Service-Tier enthalten, finden Sie unter [Azure App Preisen](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Gehen Sie zunächst Logik Apps vor der Anmeldung für ein Azure-Konto [versuchen Logik](https://tryappservice.azure.com/?appservice=logic)App. Sie können sofort eine kurzlebige Starter Logik-app in App Service erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="core-connectors"></a>Core-Anschlüsse
In der folgenden Tabelle listet alle verfügbaren Anschlüsse und API-Apps von Microsoft erstellt, die als Core verfügbar sind:

Name | Beschreibung
--- | ---
[Bing Translator](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Verwenden Sie Bing Übersetzen von Text in einer anderen Sprache.
[HTTP](app-service-logic-connector-http.md) | Der HTTP-Listener öffnet einen Endpunkt, der als HTTP-Server und HTTP oder HTTPS-Anfragen überwacht. Die HTTP-Aktion erfordert eine API App und Logik Apps nativ unterstützt.
[Pufferzeit](app-service-logic-connector-slack.md) | Verbindung mit Puffer und Pufferzeit Kanäle senden.


## <a name="enterprise-integration-connectors"></a>Enterprise-Integration Connectors
Die folgende Tabelle listet alle verfügbaren Anschlüsse und API-Apps von Microsoft als Enterprise Integration Connectors erstellt:

Name  | Beschreibung
------------- | -------------
[BizTalk-Regeln](app-service-logic-use-biztalk-rules.md) | Verwenden Sie BizTalk-Regeln zu definieren, der Geschäftslogik innerhalb einer Organisation. Unternehmens-Policies können ohne Erneutes Kompilieren oder erneutes Bereitstellen der zugehörigen Anwendung aktualisiert werden.
[BizTalk XPath-Extractor](app-service-logic-xpath-extract.md) | Sucht und extrahiert Daten aus dem XML-Inhalt auf Grundlage einer XPath-Auswahl.
[DB2-Connector](app-service-logic-connector-db2.md) | Verbindung zu IBM DB2-Datenbank lokal und auf Azure virtuellen Computer ein Windows-Betriebssystem ausführen. Informix strukturierte Abfragesprache Befehle können Web API und OData-API Vorgänge zuordnen. <br/><br/>Keine Trigger. Aktionen auswählen, einfügen, aktualisieren, löschen und benutzerdefinierte Anweisung<br/><br/>Dieser Connector auch Microsoft Client für DRDA Informix-Server über ein Netzwerk herstellen.
[Datei](app-service-logic-connector-file.md) | Mit diesem Connector können Sie lokale Dateisystem oder Netzwerk und vollständige Datei verschiedene Aufgaben hochladen, löschen, Auflisten von Dateien und mehr.
[Informix](app-service-logic-connector-informix.md) | Ein IBM Informix-Datenbank für lokale und auf einem auf Windows Azure virtuellen Computer verbunden. Informix strukturierte Abfragesprache Befehle können Web API und OData-API Vorgänge zuordnen.<br/><br/>Keine Trigger. Aktionen auswählen, einfügen, aktualisieren, löschen und benutzerdefinierte Anweisung.<br/><br/>Bei lokalen VPN oder Azure ExpressRoute dienen. Dieser Connector auch Microsoft Client für DRDA Informix-Server über ein Netzwerk herstellen.
[Microsoft SQL Server](app-service-logic-connector-sql.md) | SQL Server oder einer Azure SQL-Datenbank lokal angeschlossen. Sie können erstellen, aktualisieren, abrufen und löschen auf einer SQL-Datenbanktabelle.
MQ | IBM WebSphere MQ Server, Version 8, lokalen und auf einem auf Windows Azure virtuellen Computer verbunden. Bei lokalen VPN oder Azure ExpressRoute dienen. Der Connector enthält außerdem Microsoft Client für MQ.<br/><br/>Keine Trigger. Keine Aktionen.<br/><br/>**Hinweis** Derzeit kann mit Logik-Apps verwendet werden.

## <a name="connectors-as-triggers"></a>Connectors als Trigger
Mehrere Connectors bieten Triggern Logik Apps. Diese Trigger werden zwei Typen:

1. Umfrage Trigger: Trigger Abfragen an einer bestimmten Häufigkeit für neue Daten zu überprüfen. Wenn neue Daten verfügbar sind, wird eine neue Instanz der Logik-App Daten als Eingabe ausgeführt. Um zu verhindern, dass dieselben Daten verbraucht mehrmals Trigger kann bereinigen lesen und Logik App übergebenen Daten. Beispiele für solche Connectors sind Datei und SQL Azure-Speicher.
2. Push-Auslöser: Trigger überwachen, Daten auf einen Endpunkt oder ein Ereignis auftritt. Eine neue Instanz einer Anwendung Logik dann auslösen. Beispiele für solche Connectors sind HTTP-Listener und Twitter.

## <a name="connectors-as-actions"></a>Connectors Aktionen
Connectors können auch Aktionen innerhalb der App Logik verwendet werden. Aktionen sind nützlich für die Suche nach Daten in der Logik App, die dann bei der Ausführung verwendet. Beispielsweise müssen Sie zum Nachschlagen von Daten aus einer SQL-Datenbank für zusätzliche Informationen zu einem Kunden bei der Verarbeitung eines Auftrags. Oder Sie müssen schreiben, aktualisieren oder Löschen von Daten in einem Ziel. Sie können dazu die Aktionen der Connectors. Aktionen zuordnen Operationen in API-Apps (definiert durch ihren stolz Metadaten).

## <a name="create-your-own-connectors-and-api-apps"></a>Anschlüsse und API-Apps erstellen
[Anschlüsse und API-Apps Referenz](http://aka.ms/appservicesconnectorreference)  
[Azure App Service API-app-Triggern](../app-service-api/app-service-api-dotnet-triggers.md)  
[Logik App Verweis](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## <a name="more-on-connectors-and-api-apps"></a>Anschlüsse und API-Apps
[Was sind Connectors und BizTalk API-Apps](app-service-logic-what-are-biztalk-api-apps.md)  
[Verwenden der Hybrid-Verbindungs-Manager in Azure App Service](app-service-logic-hybrid-connection-manager.md)  
[Verwalten Sie und überwachen Sie die integrierte API-Apps und Connectors](app-service-logic-monitor-your-connectors.md)
