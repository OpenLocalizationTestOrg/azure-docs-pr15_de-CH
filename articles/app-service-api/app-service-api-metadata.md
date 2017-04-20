<properties
    pageTitle="App Service API-Apps Metadaten für API-Erkennung und Code | Microsoft Azure"
    description="Hier erfahren Sie, wie Apps in Azure App Service API stolz Metadaten API Discovery und Code Generation zu erleichtern."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>App Service API-Apps Metadaten für API-Erkennung und code 

[Swagger 2.0](http://swagger.io/) API Metadaten ist in App Service API-Apps unterstützt. Habe mit stolz, aber wenn Sie es verwenden, profitieren Sie API-Apps-Funktionen, die Ermittlung und Verbrauch erleichtern.   

## <a name="swagger-endpoint"></a>Stolz Endpunkt

Sie können einen Endpunkt, der Swagger 2.0 JSON-Metadaten für eine API-app in einer API-app-Eigenschaft bereitstellt. Der Endpunkt kann relativ zum Basis-URL des API-app oder eine absolute URL sein. Absolute URLs können außerhalb der API-app zeigen. 

Viele downstream-Clients (z. B. Visual Studio Code Generation und PowerApps "Add API" Fluss), die URL muss öffentlich (nicht durch Benutzer oder Authentifizierung geschützt). Dies bedeutet, dass wenn App-Authentifizierung verwenden und die API-Definition innerhalb der app selbst verfügbar machen möchten, müssen Sie Authentifizierung verwenden anonymen Datenverkehr zu Ihrer API. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung für App Service API-Apps](app-service-api-authentication.md).

### <a name="portal-blade"></a>Portal-blade

[Azure-Portal](https://portal.azure.com/) kann die Endpunkt-URL angezeigt und auf Blade **-API-Definition** geändert werden.

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Azure-Ressourcen-Manager-Eigenschaft

API-Definition URL einer API-App können mit [Resource Explorer](https://resources.azure.com/) oder [Azure Resource Manager Vorlagen](../resource-group-authoring-templates.md) in Befehlszeilentools wie [Azure PowerShell](../powershell-install-configure.md) und [Azure-CLI](../xplat-cli-install.md). 

Wechseln Sie im **Ressourcen-Explorer**zur **Abonnements > {Subscription} > ResourceGroups > {der Ressourcengruppe} > Anbieter > Microsoft.Web > Websites > {Website} > Config > Web**, sehen Sie die `apiDefinition` Eigenschaft:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Ein Beispiel einer Azure-Ressourcen-Manager-Vorlage legt die `apiDefinition` -Eigenschaft öffnen Sie die [Datei azuredeploy.json in diesem Beispiel Aufgabenliste](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Suchen Sie den Abschnitt der Vorlage im JSON-Beispiel oben aussieht.

### <a name="default-value"></a>Standardwert

Wenn Sie Visual Studio API-app erstellen, API-Definition Endpunkt automatisch soll die Basis-URL der API-app plus `/swagger/docs/v1`. Dies ist die Standard-URL [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet-Paket zu dynamisch generierten stolz Metadaten für ein Projekt ASP.NET Web API verwendet. 

## <a name="code-generation"></a>Generieren von Code

Einer der Vorteile der Integration von stolz in Azure API-apps ist automatische Generierung. Generierte Client-Klassen vereinfachen Code schreiben, eine API-Anwendung aufruft.

Sie können Clientcode für API-Anwendung mithilfe von Visual Studio oder der Befehlszeile. Informationen zu Client-Klassen in Visual Studio ein Projekt ASP.NET Web API finden Sie unter [Erste Schritte mit API-Apps und ASP.NET](app-service-api-dotnet-get-started.md#codegen). Informationen über die Befehlszeile für alle Sprachen finden Sie unter der Readme-Datei des Repositorys [Azure-Autorest](https://github.com/azure/autorest) auf GitHub.com.
 
## <a name="next-steps"></a>Nächste Schritte

Ein schrittweises Lernprogramm, das hilft Ihnen beim Erstellen, bereitstellen und Nutzen einer Anwendung API finden Sie unter [Erste Schritte mit API-Apps in Azure App Service](app-service-api-dotnet-get-started.md).

Bei Verwendung von Azure API Management mit API-Apps können stolz Metadaten Sie die API in API importieren. Weitere Informationen finden Sie in [der Definition einer API mit Azure API Management importieren](../api-management/api-management-howto-import-api.md). 
