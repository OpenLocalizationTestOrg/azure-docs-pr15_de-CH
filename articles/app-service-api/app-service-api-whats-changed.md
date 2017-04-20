<properties
    pageTitle="App Service API Apps - Änderungen | Microsoft Azure"
    description="Informationen Sie für API-Apps in Azure App Service neu."
    services="app-service\api"
    documentationCenter=".net"
    authors="mohitsriv"
    manager="wpickett"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps---whats-changed"></a>App Service API Apps – was geändert wird

Das Ereignis Connect() November 2015 waren Verbesserungen Azure App Service [angekündigt](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Diese Verbesserungen zugrunde liegenden API-Apps Mobile und Web Apps ausgerichtet, Konzept Anzahl reduzieren und Verbesserung der Bereitstellung und Leistung zur Laufzeit geändert. 30. November 2015 neue API-apps mit Azure-Verwaltungsportal erstellen oder die neueste Tools diese Änderungen. Dieser Artikel beschreibt diese Änderungen, sowie vorhandene apps die Leistungsfähigkeit erneut bereitstellen.

## <a name="feature-changes"></a>Feature ändert
Die wichtigsten Funktionen der API-Apps – Authentifizierung, CORS und API-Metadaten wurden direkt in App Service verschoben. Mit dieser Änderung sind die Features Web, Mobile und API-Apps verfügbar. Alle drei Teilen tatsächlich **Microsoft.Web/sites** Ressource dasselbe in Ressourcen-Manager. API-Apps-Gateway ist nicht mehr erforderlich oder mit API-Apps angeboten. Dies erleichtert auch die Azure API Management verwenden, denn nur das Management-API-Gateway.

![Übersicht über die API-Apps](./media/app-service-api-whats-changed/api-apps-overview.png)

Ein wichtiges Entwurfsprinzip mit API-Apps-Updates ist Ihre API ist in einer Sprache Ihrer Wahl zu können.  Wenn Ihre API bereits als Web App oder Mobile-Anwendung bereitgestellt wird, müssen Sie keinen erneut Ihre Anwendung um neuen Funktionen nutzen. Wenn Sie API-Apps Vorschau besitzen, ist Migration Hinweise unten.

### <a name="authentication"></a>Authentifizierung
Die vorhandenen schlüsselfertige API-Apps, Mobile Dienste/Apps und Web Apps-Authentifizierungsfunktionen wurden vereinheitlicht und stehen in einem einzigen Azure App Service-Authentifizierung Blade im Verwaltungsportal. Einführung in Authentifizierungsdienste in App Service finden Sie unter [Erweitern App-Authentifizierung / Autorisierung](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

API-Szenarien sind eine Reihe von relevanten neue Funktionen:

- **Unterstützung für Active Directory von Azure direkt**ohne Clientcode zu AAD Token für einen Sitzungstoken exchange: der Client nur zählen AAD Token in der Authorization-Header nach der Träger token-Spezifikation. Daher auch keine App dienstspezifische SDK Client oder Server erforderlich ist. 
- **Dienst- oder "Internal" Zugriff**: haben Sie ein Hintergrundprozess oder einem anderen Client Zugriff auf APIs ohne Benutzeroberfläche können Sie ein Token mit einem AAD Dienstprinzipalnamen anfordern und App-Dienst für die Authentifizierung mit Ihrer Anwendung übergeben.
- **Verzögerte Autorisierung**: vielen gelten unterschiedliche Zugriff für verschiedene Teile der Anwendung. Möglicherweise möchten Sie einige APIs öffentlich verfügbar sein, während andere anmelden. Die ursprüngliche Authentifizierung war nichts, mit der gesamten Website anmelden müssen. Diese Option noch vorhanden, aber Sie können alternativ Anwendungscode Zugriff Beschlüsse Rendern nach der Benutzerauthentifizierung App Service.
 
Weitere Informationen zu den neuen Authentifizierungsfeatures finden Sie unter [Authentifizierung und Autorisierung für API-Apps in Azure App Service](app-service-api-authentication.md). Informationen zum Migrieren von vorhandenen API-apps das Vorgängermodell API-apps in die neue finden Sie unter [Migrieren von vorhandenen API-apps](#migrating-existing-api-apps) in diesem Artikel.
 
### <a name="cors"></a>CORS
Anstatt eine durch Kommas getrennte **MS_CrossDomainOrigins** app gibt es jetzt eine-Blade in Azure-Verwaltungsportal für CORS konfigurieren. Alternativ können sie mit konfiguriert werden Ressourcenmanager oder Azure PowerShell, CLI [-Explorers](https://resources.azure.com/)Werkzeuge. Legen Sie die **Cors** -Eigenschaft auf **Microsoft.Web/sites/config** Ressourcentyp für die ** &lt;Sitenamen&gt;/web** Ressource. Zum Beispiel:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>API-Metadaten
API-Definition Blade ist jetzt über Web, Mobile und API-Apps. Im Verwaltungsportal können Sie eine relative Url oder eine absolute Url auf einen Endpunkt dieser Hosts Swagger 2.0 Darstellung der API. Alternativ können sie konfiguriert werden mit Ressourcen-Manager-Tools. Legen Sie die **ApiDefinition** -Eigenschaft auf **Microsoft.Web/sites/config** Ressourcentyp für die ** &lt;Sitenamen&gt;/web** Ressource. Zum Beispiel:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

Zu diesem Zeitpunkt muss der Endpunkt zugänglich ohne Authentifizierung für viele downstream-Clients (z. B. Visual Studio REST API Client Generation und PowerApps "Add API" Fluss) zu verwenden. Dies bedeutet, wenn App-Authentifizierung verwenden und die API-Definition innerhalb der app selbst verfügbar machen möchten, müssen Sie die Option Authentifizierung zurückgestellt beschriebenen damit stolz Metadaten öffentlich ist.

## <a name="management-portal"></a>Verwaltungsportal
Auswählen von **Neu > Web + Mobile > API App** im Portal erstellen API-apps, die im Artikel beschriebenen neuen Funktionen widerspiegeln. **Durchsuchen > API-Apps** zeigt nur die neuen API-apps. Sobald Sie eine API-App durchsuchen, teilt das Blade das gleiche Layout und Funktionen wie Web- und Mobile Apps. Der einzige Unterschied sind Quickstart Inhalt und Reihenfolge der Einstellungen.

Vorhandene API-apps (oder Marketplace API-apps Logik Apps erstellt) die vorherige Vorschau Funktionen werden im Designer Logik Apps und wenn alle Ressourcen in einer Ressourcengruppe.

## <a name="visual-studio"></a>Visual Studio

Die meisten Werkzeuge Web Apps funktioniert mit neuen API-apps, da sie den gleichen zugrunde liegenden **Microsoft.Web/sites** Ressourcentyp teilen. Die Visual Studio Azure-Werkzeuge, sollten jedoch aktualisierte Version 2.8.1 oder höher da eine Reihe von Funktionen für APIs macht. Das SDK von [Azure Downloadseite](https://azure.microsoft.com/downloads/)herunterladen.

Rationalisierung der Typen App veröffentlichen auch unter unified **Veröffentlichen > Microsoft Azure App Service**:

![API-Apps veröffentlichen](./media/app-service-api-whats-changed/api-apps-publish.png)

Lesen Sie Informationen zum SDK 2.8.1 [Blogbeitrag](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/)Ankündigung.

Alternativ können Sie das Veröffentlichungsprofil manuell aus dem Verwaltungsportal veröffentlichen aktivieren importieren. Cloud Explorer Codeerzeugung und API-app Auswahl/Erstellung werden jedoch SDK 2.8.1 oder höher.

## <a name="migrating-existing-api-apps"></a>Migrieren von vorhandenen API-apps
Wenn Ihre benutzerdefinierte API die früheren Vorabversion von API-Apps bereitgestellt wird, bitten Sie bis zum 31. Dezember 2015 in das neue Modell für API-Apps migrieren. Da sowohl die alten und neuen Modell auf Web-APIs in App-Dienst gehostet wird, kann die meisten vorhandenen Code wiederverwendet werden.

### <a name="hosting-and-redeployment"></a>Hosting und erneute Bereitstellung
Die Schritte zum erneuten Bereitstellen entsprechen alle vorhandenen Web API App Service bereitstellen. Schritte:

1. Erstellen einer leeren API-Anwendung. Dies kann im Portal mit dem neuen > API-App in Visual Studio veröffentlichen oder Ressourcen-Manager-Tools. Wenn Ressourcenmanager Werkzeuge oder Vorlagen, eingestellt **Art** **-API** auf **Microsoft.Web/sites** Ressourcentyp zu den Schnellstarts und im Verwaltungsportal-API-Szenarios ausgerichtet.
2. Verbinden und die leere API-app mit Bereitstellungsmechanismen App Service unterstützt Ihr Projekt bereitstellen. Lesen Sie weitere [Bereitstellungsdokumentation Azure App Service](../app-service-web/web-sites-deploy.md) . 
  
### <a name="authentication"></a>Authentifizierung
Die App Service-Authentifizierungsdienste unterstützen dieselben Funktionen, die mit der vorherigen API-Apps verfügbar waren. Wenn Sie Sitzungstoken und SDKs erforderlich, verwenden Sie folgenden Client- und SDKs:

- Client: [Azure Mobile Client SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- Server: [Microsoft Azure Mobile Anwendung .NET Authentifizierungserweiterung](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Verwenden Sie stattdessen das App Service Alpha SDKs wurden, sind diese jetzt veraltet:

- Client: [Microsoft Azure AppService SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
- Server: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

Insbesondere ist in Azure Active Directory jedoch keine App dienstspezifische AAD Token direkt arbeiten.

### <a name="internal-access"></a>Interner Zugriff
API-Apps Vorgängermodell enthalten eine integrierte interne Zugriffsebene. Verwenden des SDK wird für Anfragen Signierung erforderlich. Wie zuvor beschrieben mit der neuen API-Apps AAD Service Prinzipale als Alternative für die Dienst-Authentifizierung dienen ohne eine App dienstspezifischen SDK. Erfahren Sie mehr in [Service principal Authentifizierung für API-Apps in Azure App Service](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Entdeckung
API-Apps Vorgängermodell hatte APIs für andere API-apps zur Laufzeit in derselben Ressourcengruppe hinter demselben Gateway ermitteln. Dies ist besonders nützlich in Architekturen, die Microservice Muster implementiert. Dies nicht direkt unterstützt wird, werden eine Reihe von Optionen zur Verfügung:

1. Verwenden der Azure-Ressourcen-Manager-API für die Erkennung.
2. Setzen Sie Azure API Management vor APIs App Dienst gehostet. Azure API Management dient als Fassade und bieten eine stabile vor externen Url selbst wenn interne Netzwerktopologie ändert.
3. Erstellen Sie eigener Discovery API-app und andere API-apps Discovery-Anwendung beim Start zu registrieren.
4. Füllen Sie bei der Bereitstellung app Standardeinstellungen aller API-apps (und Kunden) mit den Endpunkten von anderen API-apps. Dies ist in vorlagenbereitstellungen und API-Apps jetzt Geben Sie Kontrolle über die Url.

## <a name="using-api-apps-with-logic-apps"></a>Verwenden von API-Apps mit Logik Apps

Neue API-apps Modell funktioniert gut mit [Logik Apps Schemaversion 2015-08-01](../app-service-logic/app-service-logic-schema-2015-08-01.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel Sie im [API-Apps-Dokumentation](https://azure.microsoft.com/documentation/services/app-service/api/). Sie entsprechend das neue Modell für API-Apps aktualisiert. Darüber hinaus erreichen Sie in den Foren für Weitere Informationen oder Hilfe bei der Migration:

- [MSDN-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
- [Stack-Überlauf](http://stackoverflow.com/questions/tagged/azure-api-apps)
