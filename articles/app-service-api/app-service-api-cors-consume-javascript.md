<properties
    pageTitle="CORS-Unterstützung in App Service | Microsoft Azure"
    description="Informationen Sie zum CORS-Unterstützung in Azure Azure App Service verwenden."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/27/2016"
    ms.author="rachelap"/>

# <a name="consume-an-api-app-from-javascript-using-cors"></a>Verwenden einer API-app von JavaScript mit CORS

App Service bietet integrierte Unterstützung für [Cross Ursprung Ressource freigeben (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), wodurch Clients JavaScript API-apps zu domänenübergreifende Aufrufe von APIs befinden. App Service können Sie CORS-Zugriff auf die API ohne Code in Ihre API konfigurieren.

Dieser Artikel enthält zwei Abschnitte:

* Die [CORS konfigurieren](#corsconfig) erläutert im Allgemeinen CORS für API-app, WebApp oder mobile Anwendung konfigurieren. Dies gilt für alle Frameworks, die vom App, einschließlich .NET, Node.js und Java unterstützt. 

* Beginnend mit dem Abschnitt [fortfahren .NET Einsteiger - Tutorials](#tutorialstart) , ist Artikel eine Anleitung, die CORS unterstützen, auf was Sie im [ersten API-Apps, die erste Schritte haben](app-service-api-dotnet-get-started.md)zeigt. 

## <a id="corsconfig"></a>CORS in Azure App Service konfigurieren

Sie können CORS in Azure-Portal oder [Ressourcenmanager Azure](../azure-resource-manager/resource-group-overview.md) Tools konfigurieren.

#### <a name="configure-cors-in-the-azure-portal"></a>Konfigurieren von CORS in Azure-portal

8. In einem Browser zum [Azure-Portal](https://portal.azure.com/).

2. **Anwendungsdienste**auf und klicken Sie dann auf den Namen der API-app.

    ![Wählen Sie im Portal API-app](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. Neben Blade **API-app** öffnet **Einstellungen** Blatt **API** Abschnitt und dann auf **CORS**.

    ![Wählen Sie im Blatt Einstellungen CORS](./media/app-service-api-cors-consume-javascript/clicksettings.png)

11. Text Feld Geben Sie die URL oder URLs Aufrufe sollen aus JavaScript.


    Wenn Ihre Anwendung JavaScript Web app mit dem Namen Todolistangular bereitgestellt wird, geben Sie "https://todolistangular.azurewebsites.net" ein. Alternativ geben Sie ein Sternchen (*) gibt an, dass alle Ursprungsdomänen akzeptiert werden.


13. Klicken Sie auf **Speichern**.

    ![Klicken Sie auf Speichern](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Nach dem Klicken auf **Speichern**akzeptiert der API-app JavaScript-Aufrufe aus dem angegebenen URLs.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>Konfigurieren von CORS mithilfe von Azure-Ressourcen-Manager-tools

Sie können CORS für API-app konfigurieren, mithilfe von [Azure-Ressourcen-Manager Vorlagen](../resource-group-authoring-templates.md) in Befehlszeilentools wie [Azure PowerShell](../powershell-install-configure.md) und [Azure-CLI](../xplat-cli-install.md). 

Öffnen Sie beispielsweise einer Azure-Ressourcen-Manager-Vorlage, die CORS-Eigenschaft festgelegt, [azuredeploy.json-Datei für dieses Lernprogramm Anwendung im Repository](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Suchen Sie den Abschnitt der Vorlage, die wie folgt aussieht:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>Weiterhin .NET erste-Schritte-Lernprogramm

Wenn Sie die Node.js oder Java-Einsteiger-Serie für API-apps sind, haben Sie immer gestartet Serie. Fahren Sie mit Abschnitt " [Nächste Schritte](#next-steps) " finden Sie Vorschläge für Weitere Informationen zu API-Apps.

Der Rest dieses Artikels ist .NET Einsteiger-Serie und geht davon aus, dass [das erste Lernprogramm](app-service-api-dotnet-get-started.md)erfolgreich.

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>Bereitstellen Sie ToDoListAngular auf eine neue Webanwendung

[Das erste Lernprogramm](app-service-api-dotnet-get-started.md)ist eine Zwischenebene API-app und API-app ein Daten-Ebene erstellt. In diesem Lernprogramm erstellen Sie eine einseitige Anwendung (SPA) Web app Zwischenebene-API aufruft, app. App der mittleren Ebene API CORS aktivieren SPA arbeiten müssen. 

In der [Aufgabenliste Anwendung](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list)ist das Projekt ToDoListAngular einen einfachen AngularJS-Client, der mittleren Ebene ToDoListAPI Web API Projekt aufruft. JavaScript-Code in die Datei *app/scripts/todoListSvc.js* mit dem AngularJS HTTP-API aufgerufen. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 
        
            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>Erstellen einer neuen Web-Applikation für das ToDoListAngular-Projekt

So neue App Service Web app erstellen und Bereitstellen eines Projekts, ähnelt der zum [Erstellen und Bereitstellen einer API-app in das erste Lernprogramm in dieser Serie](app-service-api-dotnet-get-started.md#createapiapp)gesehen haben. Der einzige Unterschied ist der Anwendungstyp **Web App** **API-**App.  Screenshots der Dialogfelder finden Sie unter 

1. Im **Projektmappen-Explorer**mit der rechten Maustaste des ToDoListAngular Projekts und dann auf **Veröffentlichen**.

3.  Klicken Sie auf der Registerkarte **Profil** des **Veröffentlichen** -Assistenten auf **Microsoft Azure App Service**.

5. Klicken Sie im Dialogfeld **App-Dienst** **neu**.

3. **Hosting** -Registerkarte im Dialogfeld **Create App Service** Geben Sie einen **Web App Name** , der in der **.azurewebsites.NET* -Domäne eindeutig ist. 

5. Wählen Sie den Azure- **Abonnement** zu arbeiten.

6. Wählen Sie in der Dropdownliste **Ressourcengruppe** derselben Ressourcengruppe, die Sie zuvor erstellt haben.

4. Wählen Sie in der Dropdownliste **App Service-Plan** zuvor erstellten Planes. 

7. Klicken Sie auf **Erstellen**.

    Visual Studio erstellt Web app, ein Veröffentlichungsprofil verschafft und zeigt die **Verbindung** des **Veröffentlichen** -Assistenten.

    Klicken Sie **Veröffentlichen** noch nicht. Im folgenden Abschnitt Konfigurieren Sie die neuen Web app app der mittleren Ebene API aufrufen, die in App Service ausgeführt wird. 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>Festlegen der mittleren Ebene URL im Web app settings

1. Zum [Azure-Portal](https://portal.azure.com/), und navigieren Sie anschließend zu **Web App** Blade für Web-app, die Sie zum Hosten des TodoListAngular (front End)-Projekts erstellt.

2. Klicken Sie auf **Settings > Einstellungen**.

3. Fügen Sie in **App** -Einstellungen den folgenden Schlüssel und Wert:

  	|Schlüssel|Wert|Beispiel
  	|---|---|---|
  	|toDoListAPIURL|https://{Your mittlere Ebene API Anw.-Name} .azurewebsites .net|https://todolistapi0121.azurewebsites.NET|

4. Klicken Sie auf **Speichern**.

    Wenn der Code in Azure ausgeführt wird, überschreibt dieser Wert Localhost-URL, die in der Datei *Web.config* . 

    Der Code, der die Einstellung Wert ist in *index.cshtml*:

        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>

    Der Code in *todoListSvc.js* verwendet die Einstellung:

        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>Bereitstellen des Webprojekts ToDoListAngular neue Web App

*  Klicken Sie in Visual Studio im Schritt **Verbindung** des **Veröffentlichen** -Assistenten **Veröffentlichen**.

    Visual Studio stellt das ToDoListAngular Projekt neue Web App und öffnet einen Browser den URL des Web app. 

### <a name="test-the-application-without-cors-enabled"></a>Testen Sie die Anwendung ohne CORS aktiviert 

2. Öffnen Sie in Ihrem Browser Entwicklertools Konsolenfenster.

3. Klicken Sie im Browserfenster, das AngularJS UI anzeigt auf **Liste** .

    Der JavaScript-Code versucht app der mittleren Ebene API aufrufen, aber der Aufruf schlägt fehl, da das front-End in einer anderen Domäne als Back-End ausgeführt wird. Browserfenster Developer Toolskonsole zeigt eine Fehlermeldung Cross-Ursprung.

    ![Ursprungsübergreifende-Fehlermeldung](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>Konfigurieren von CORS für die mittlere Ebene API-app

In diesem Abschnitt Konfigurieren Sie die Einstellung CORS in Azure für die mittlere Ebene ToDoListAPI API-app. Diese Einstellung ermöglicht die Zwischenebene API-app JavaScript aus Web app Anrufe, die für das ToDoListAngular-Projekt erstellt.

8. In einem Browser zum [Azure-Portal](https://portal.azure.com/).

2. **Anwendungsdienste**auf und klicken Sie dann auf ToDoListAPI (mittlere Ebene) API-app.

    ![Wählen Sie im Portal API-app](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. Neben Blade **API-app** öffnet **Einstellungen** Blatt **API** Abschnitt und dann auf **CORS**.

    ![Wählen Sie im Portal CORS](./media/app-service-api-cors-consume-javascript/clicksettings.png)

12. Geben Sie im Textfeld die URL für die ToDoListAngular (front End) Web app. Z. B. Wenn Sie ToDoListAngular Project Web app mit dem Namen todolistangular0121 bereitgestellt, können Aufrufe von der URL `https://todolistangular0121.azurewebsites.net`.

    Alternativ geben Sie ein Sternchen (*) gibt an, dass alle Ursprungsdomänen akzeptiert werden.

13. Klicken Sie auf **Speichern**.

    ![Klicken Sie auf Speichern](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Nach dem Klicken auf **Speichern**akzeptiert der API-app JavaScript-Aufrufe aus der angegebenen URL. In diesem Screenshot akzeptiert die ToDoListAPI0223 API-app JavaScript-Client Aufrufen aus ToDoListAngular Web app.

### <a name="test-the-application-with-cors-enabled"></a>Testen Sie die Anwendung mit CORS aktiviert

* Öffnen Sie auf der Web-app HTTPS-URL an. 

    Dieser Zeitpunkt kann die Anwendung anzeigen, hinzufügen, bearbeiten und Löschen von Aufgaben. 

    ![Beispiel-app auf Aufgabenliste](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>App Service CORS im Vergleich zu Web-API CORS

In einem Projekt Web API installieren Sie [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet-Paket um im Code anzugeben, welche Domänen Ihre API akzeptiert aus JavaScript aufgerufen.
 
Web API CORS Unterstützung ist flexibler als App Service CORS Unterstützung. Im Code können Sie beispielsweise verschiedener akzeptierten Herkunft für andere Aktionsmethoden angeben, während für App-Dienst CORS Sie eine akzeptierte Ursprung aller API-app Methoden angeben.

> [AZURE.NOTE] Versuchen Sie nicht, Web API CORS und App-Dienst CORS in eine API-app verwenden. App Service CORS Vorrang und Web API CORS haben keine Auswirkung. Z. B. Wenn Sie eine Ursprungsdomäne App Service aktivieren und alle Ursprungsdomänen im Web API Code ermöglichen, akzeptiert app Azure-API Aufrufe aus der Domäne nur in Azure angegebenen.

### <a name="how-to-enable-cors-in-web-api-code"></a>CORS im Web API Code aktivieren

Die folgenden Schritte zusammengefasst Verfahren zum Aktivieren der Web API CORS Unterstützung. Weitere Informationen finden Sie unter [ASP.NET Web API 2 Cross-Ursprung Anfragen aktivieren](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. Installieren Sie in einem Projekt Web-API das [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet-Paket.

1. Enthalten ein `config.EnableCors()` Codezeile im **Register** -Methode der **WebApiConfig** -Klasse, wie im folgenden Beispiel. 

        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
                
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
    
                // Web API routes
                config.MapHttpAttributeRoutes();
    
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }

1. Web API-Controller hinzufügen ein `using` -Anweisung für die `System.Web.Http.Cors` -Namespace und die `EnableCors` Attribut Controllerklasse oder einzelner Aktionsmethoden. Im folgenden Beispiel gilt CORS-Unterstützung für den gesamten Controller.

        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController
 
## <a name="using-azure-api-management-with-api-apps"></a>Verwenden von Azure API Management mit API-Apps

Bei Verwendung von Azure API Management mit einer API-app konfigurieren Sie CORS API-Verwaltung in der API-app. Weitere Informationen finden Sie in folgenden Ressourcen:

* [Überblick über Azure-API (video: CORS beginnt am 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [API-Management cross-Domain-Richtlinien](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)
 
## <a name="troubleshooting"></a>Problembehandlung

Falls Sie ein Problem beim Durchlaufen dieses Lernprogramm ausführen, sind hier einige Vorschläge zur Problembehandlung.

* Stellen Sie sicher, dass Sie die neueste Version von [Azure SDK für .NET Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003)verwenden.

* Stellen Sie sicher, dass die eingegebene `https` in der CORS-Einstellung und stellen Sie sicher, dass Sie verwenden `https` der Front-End-Web App.

* Stellen Sie sicher, dass eingegebene Einstellung CORS in API-app der mittleren Ebene und nicht in der Front-End-Anwendung.

* Konfigurieren von CORS im Anwendungscode und Azure App Service Beachten Sie, dass die App Service CORS Einstellung überschreiben was Sie im Anwendungscode tun. 

Weitere Informationen zu Visual Studio finden Sie unter Features vereinfachen Problembehandlung [Problembehandlung Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Nächste Schritte 

In diesem Artikel wurde gezeigt, wie App Service CORS-Unterstützung aktivieren, dass Client-JavaScript-Code in einer anderen Domäne eine API aufrufen kann. Erfahren Sie mehr über API-apps, lesen Sie die [Einführung in die Authentifizierung in App Service](../app-service/app-service-authentication-overview.md)und fahren mit der [Benutzerauthentifizierung für API-apps](app-service-api-dotnet-user-principal-auth.md) -Lernprogramm.
