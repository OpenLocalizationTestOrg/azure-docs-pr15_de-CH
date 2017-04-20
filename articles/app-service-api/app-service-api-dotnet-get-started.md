<properties
    pageTitle="Erste Schritte mit API-Apps und ASP.NET in App Service | Microsoft Azure"
    description="Informationen Sie zum Erstellen, bereitstellen und Nutzen einer ASP.NET API-app in Azure App Service mit Visual Studio 2015."
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
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="rachelap"/>

# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>Erste Schritte mit API-Apps, ASP.NET und stolz in Azure App Service

[AZURE.INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Dies ist die erste in einer Reihe von Lernprogrammen, die zeigen, wie Features von Azure App Service, die für die Entwicklung und hosting-Rest APIs hilfreich sind.  Dieses Lernprogramm umfasst Unterstützung für Metadaten-API-stolz Format.

Sie erfahren:

* Das Erstellen und Bereitstellen von [API-apps](app-service-api-apps-why-best-platform.md) in Azure App Service mithilfe von Tools in Visual Studio 2015.
* Wie API Discovery automatisieren mit Swashbuckle NuGet-Paket Swagger API Metadaten dynamisch generiert.
* Wie Swagger API Metadaten Clientcode für API-app automatisch generiert.

## <a name="sample-application-overview"></a>Beispiel-Anwendungsübersicht

In diesem Lernprogramm arbeiten Sie mit einer einfachen Aufgabe Liste beispielanwendung. Die Anwendung hat eine einseitige Anwendung (SPA)-front-End, eine Zwischenebene ASP.NET Web API und eine Datenebene ASP.NET Web API.

![API-Apps Anwendung Beispieldiagramm](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Hier ist ein Screenshot des [AngularJS](https://angularjs.org/) -front-Ends.

![API-Apps-Beispiel Aufgabenliste](./media/app-service-api-dotnet-get-started/todospa.png)

Die Visual Studio-Projektmappe enthält drei Projekte:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** - front-End: ein AngularJS SPA, die mittlere Ebene aufruft.

* **ToDoListAPI** - der mittleren Schicht: ein ASP.NET Web API-Projekt, das die Datenebene um CRUD-Operationen auf Aufgaben aufgerufen.

* **ToDoListDataAPI** - der Datenebene: ein ASP.NET Web API-Projekt, das CRUD-auf Aufgaben Operationen.

Dreistufige Architektur ist eine der vielen Architekturen, die Sie implementieren können, indem API-Apps und werden hier nur zu Demonstrationszwecken verwendet. Der Code in jeder Schicht ist möglichst API-Apps Funktionen veranschaulichen. Beispielsweise verwendet die Datenebene Serverspeicher anstelle einer Datenbank als seine Dauerhaftigkeit.

In diesem Lernprogramm haben Sie zwei Web API-Projekte, und in der Cloud in App Service API-apps.

Nächste Lernprogramm der Serie bereit SPA front-End in der cloud

## <a name="prerequisites"></a>Erforderliche Komponenten

* ASP.NET Web API - angenommen Tutorial Anleitung, Sie grundlegende Kenntnisse zum Arbeiten mit ASP.NET [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) in Visual Studio.

* Ein Azure-Konto - können Sie [kostenlos ein Azure-Konto öffnen](/pricing/free-trial/?WT.mc_id=A261C142F) oder [Abonnementvorteile Visual Studio aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

    Wenn Sie mit Azure App Service beginnen, bevor Sie für ein Azure-Konto, gehen Sie [versuchen App](http://go.microsoft.com/fwlink/?LinkId=523751)Service. Dort sofort können eine kurzlebige Starter-app in App Service – **keine Kreditkarte erforderlich**und keine Zusagen.

* Visual Studio 2015 [Azure SDK für .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) – das SDK installiert Visual Studio 2015 automatisch, wenn es noch nicht.

    * Klicken Sie in Visual Studio auf Hilfe -> über Microsoft Visual Studio und sicherstellen, dass Sie "Azure App Service-Tools v2.9.1" oder höher installiert sein.

    ![Azure App Tools Version](./media/app-service-api-dotnet-get-started/apiversion.png)

    >[AZURE.NOTE] Wie viele Abhängigkeiten SDK auf Ihrem Computer bereits konnte das SDK installieren lange mehrere Minuten bis eine halbe Stunde dauern.

## <a name="download-the-sample-application"></a>Beispielanwendung herunterladen

1. [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) Repository herunterladen

    Sie **Downloaden ZIP** klicken oder das Repository auf dem lokalen Computer klonen.

2. Öffnen Sie die Aufgabenliste Projektmappe in Visual Studio 2015 oder 2013.
   1. Sie müssen jede vertrauen.
        ![Sicherheitshinweis](./media/app-service-api-dotnet-get-started/securitywarning.png)

3. Erstellen Sie die Projektmappe (STRG + UMSCHALT + B) NuGet-Pakete wiederherstellen.

    Vorgang die Anwendung vor der Bereitstellung angezeigt werden soll, können Sie es lokal ausführen. Sicherstellen, dass ToDoListDataAPI Startprojekt ist die Projektmappe und führen. Sie sollten einen HTTP-Fehler 403 im Browser angezeigt.

## <a name="use-swagger-api-metadata-and-ui"></a>Swagger API-Metadaten und Benutzeroberfläche verwenden

Unterstützung für [Swagger](http://swagger.io/) 2.0 API Metadaten ist in Azure App Service integriert. Jede API-app können ein URL-Endpunkts, das Metadaten-API in Swagger JSON-Format zurückgibt. Von diesem Endpunkt zurückgegebene Metadaten kann zum Generieren von Clientcode verwendet werden.

Ein ASP.NET Web API-Projekt kann stolz Metadaten mit [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet-Paket dynamisch generieren. Swashbuckle NuGet-Paket ist bereits in der ToDoListDataAPI und ToDoListAPI installiert, die Sie heruntergeladen haben.

In diesem Abschnitt des Lernprogramms betrachten Sie generierten Swagger 2.0-Metadaten und wiederholen testen UI stolz Metadaten basiert.

1. Das ToDoListDataAPI-Projekt (**nicht** das Projekt ToDoListAPI) als Startprojekt festgelegt.

    ![ToDoDataAPI als Startprojekt festlegen](./media/app-service-api-dotnet-get-started/startupproject.png)

2. Drücken Sie F5, oder klicken Sie auf **Debuggen > Debuggen** das Projekt im Debugmodus ausführen.

    Der Browser wird geöffnet und zeigt die Fehlerseite HTTP 403.

3. Fügen Sie in der Adressleiste des Browsers `swagger/docs/v1` am Ende der Zeile und drücken Sie dann die EINGABETASTE. (Die URL ist `http://localhost:45914/swagger/docs/v1`.)

    Dies ist die Standard-URL zur Swashbuckle Swagger 2.0 JSON-Metadaten für die API zurückgegeben.

    Wenn Sie Internet Explorer verwenden, fordert der Browser Sie eine *v1.json* -Datei.

    ![Metadaten Sie JSON in Internet Explorer](./media/app-service-api-dotnet-get-started/iev1json.png)

    Wenn Sie Chrome, Firefox oder Kante verwenden, zeigt der Browser die JSON im Browserfenster. Unterschiedliche Browser behandeln JSON unterschiedlich und Browserfenster möglicherweise nicht immer genau das Beispiel.

    ![JSON-Metadaten in Chrom](./media/app-service-api-dotnet-get-started/chromev1json.png)

    Das folgende Beispiel zeigt das erste stolz Metadaten für die API mit der Definition für die Get-Methode. Diese Metadaten ist die Swagger-Benutzeroberfläche, mit denen Sie in den folgenden Schritten, und Sie können sie in einem späteren Abschnitt des Lernprogramms Clientcode automatisch generiert.

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },

4. Schließen Sie den Browser und beenden Sie Visual Studio debuggen.

5. Öffnen Sie die Datei *App_Start\SwaggerConfig.cs* im ToDoListDataAPI-Projekt im **Projektmappen-Explorer**Zeile 174 Scrollen Sie und kommentieren Sie den folgenden Code.

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    *SwaggerConfig.cs* Datei wird beim Installieren des Pakets Swashbuckle in einem Projekt erstellt. Die Datei enthält verschiedene Arten Swashbuckle konfigurieren.

    Der Code, den Sie nicht kommentiert haben kann die Swagger-Benutzeroberfläche, die Sie in den folgenden Schritten verwenden. Beim Erstellen eines Web-API mit der API-app-Projektvorlage wird dieser Code standardmäßig aus Sicherheitsgründen auskommentiert.

6. Führen Sie das Projekt erneut aus.

7. Fügen Sie in der Adressleiste des Browsers `swagger` am Ende der Zeile und drücken Sie dann die EINGABETASTE. (Die URL ist `http://localhost:45914/swagger`.)

8. Seite Swagger-Benutzeroberfläche angezeigt wird, klicken Sie auf **Aufgabenliste** , um die verfügbaren Methoden anzuzeigen.

    ![Stolz Benutzeroberfläche verfügbaren Methoden](./media/app-service-api-dotnet-get-started/methods.png)

9. Klicken Sie auf der ersten **Get** -Schaltfläche in der Liste.

10. Geben Sie im Abschnitt **Parameter** ein Sternchen als Wert der `owner` Parameter klicken **sie versuchen**.

    Beim Hinzufügen von Authentifizierung in nachfolgenden Lernprogramme bieten die mittlere Ebene die aktuelle Benutzer-ID mit der Datenebene. Jetzt haben alle Sternchen als deren Besitzer-ID, während die Anwendung ohne Authentifizierung aktiviert.

    ![Stolz Benutzeroberfläche testen](./media/app-service-api-dotnet-get-started/gettryitout1.png)

    Die Swagger-Benutzeroberfläche ruft Aufgabenliste Get-Methode und zeigt den Antwortcode und JSON.

    ![Stolz UI ausprobieren Ergebnisse](./media/app-service-api-dotnet-get-started/gettryitout.png)

11. Klicken Sie auf **Buchen**, und klicken Sie im Feld unter die **Modellschema**.

    Auf die Modellschema prefills Feld, in dem Sie den Parameterwert für die Post-Methode angeben können. (Wenn in Internet Explorer funktioniert, verwenden Sie einen anderen Browser oder geben Sie Parameterwert im nächsten Schritt ein.)  

    ![Stolz UI ausprobieren buchen](./media/app-service-api-dotnet-get-started/post.png)

12. Ändern von JSON in der `todo` Eingabeparameter ein, wie im folgenden Beispiel aussieht, oder Beschreibungstext ersetzen:

        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }

13. Klicken Sie auf **ausprobieren**.

    Aufgabenliste-API gibt einen HTTP-204-Antwortcode, der anzeigt.

14. Klicken Sie auf die erste Schaltfläche **Abrufen** , und klicken Sie dann in diesem Abschnitt der Seite **ausprobieren** .

    Die Antwort der Get-Methode enthält jetzt das neue Element.

15. Optional: Versuchen auch einsetzen, löschen und Abrufen von ID-Methoden.

16. Schließen Sie den Browser und beenden Sie Visual Studio debuggen.

Swashbuckle funktioniert mit jeder ASP.NET Web API-Projekt. Wenn Sie stolz Metadaten generieren, ein vorhandenes Projekt hinzufügen möchten, installieren Sie Paket Swashbuckle.

>[AZURE.NOTE] Swagger-Metadaten enthält eine eindeutige ID für jede API. Standardmäßig kann Swashbuckle doppelte stolz Operation IDs für die Web-API Controllermethoden generieren. Dies geschieht, wenn der Controller wie HTTP-Methoden überladen hat `Get()` und `Get(id)`. Informationen zur Überladung Behandlung finden Sie unter [Anpassen Swashbuckle generierte API-Definitionen](app-service-api-dotnet-swashbuckle-customize.md). Wenn Sie ein Web API-Projekt in Visual Studio mithilfe der Azure-API-App-Vorlage erstellen, ist Vorgang eindeutigen IDs generierten Code die *SwaggerConfig.cs* -Datei automatisch hinzugefügt.  

## <a id="createapiapp"></a>API-app in Azure erstellen und Bereitstellen von Code

In diesem Abschnitt verwenden Sie Azure Tools in Visual Studio **Web veröffentlichen** -Assistenten erstellen Sie eine neue API-app in Azure integriert sind. Dann ToDoListDataAPI an die neue API-Anwendung bereitstellen und rufen Sie die API mit Swagger-Benutzeroberfläche.

1. Im **Projektmappen-Explorer**mit der rechten Maustaste des ToDoListDataAPI Projekts und dann auf **Veröffentlichen**.

    ![Klicken Sie auf Veröffentlichen in Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)

2.  Klicken Sie im Schritt **Profil** des **Veröffentlichen** -Assistenten auf **Microsoft Azure App Service**.

    ![Klicken Sie auf Azure App Service im Web veröffentlichen](./media/app-service-api-dotnet-get-started/selectappservice.png)

3. Azure-Konto melden Sie an, wenn Sie nicht bereits geschehen, oder aktualisieren Sie Ihre Anmeldeinformationen, wenn sie abgelaufen sind.

4. Wählen Sie im Dialogfeld App Service Azure **Abonnement** verwenden möchten, und klicken Sie auf **neu**.

    ![Klicken Sie im Dialogfeld App Service auf neu](./media/app-service-api-dotnet-get-started/clicknew.png)

    **Hosting** -Registerkarte im Dialogfeld **App Service erstellen** wird angezeigt.

    Da Sie ein Web API-Projekt, die Swashbuckle installiert hat bereitstellen, nimmt Visual Studio eine API-App erstellen möchten. Dies ist nach **API-App Name** Titel und die Tatsache, dass die Dropdownliste **Änderungstyp** **API-**App.

    ![Anwendungstyp App Service-Dialogfeld](./media/app-service-api-dotnet-get-started/apptype.png)

5. Geben Sie eine **API-App Name** , der in der **.azurewebsites.NET* -Domäne eindeutig ist. Sie können den Standardnamen übernehmen, den Visual Studio schlägt.

    Wenn Sie einen Namen, den einem anderen Benutzer bereits verwendet wurde eingeben, wird rechts ein rotes Ausrufezeichen angezeigt.

    Die URL der API-Anwendung `{API app name}.azurewebsites.net`.

6. In der Dropdownliste **Ressourcengruppe** auf **neu**und dann geben Sie ein "ToDoListGroup" oder einen anderen Namen Sie.

    Eine Ressourcengruppe ist eine Sammlung von Azure Ressourcen wie API-apps, Datenbanken, VMs und So weiter. Für dieses Lernprogramm empfiehlt es sich, eine neue Ressourcengruppe erstellen, da können sie leicht in einem Schritt alle Azure-Ressourcen löschen, die Sie für das Lernprogramm erstellen.

    Diese können Sie vorhandene [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) auswählen oder einen neuen erstellen einen Namen von jeder Ressourcengruppe in Ihrem Abonnement eingeben.

7. Klicken Sie auf **neu** , neben der Dropdownliste **App Service-Plan** .

    Der Screenshot zeigt Beispielwerte für **API-App Name**, **Abonnement-**und **Ressourcengruppe** Werte unterscheiden.

    ![App Service Dialogfeld erstellen](./media/app-service-api-dotnet-get-started/createas.png)

    In den folgenden Schritten erstellen Sie einen App Service-Plan für die neue Ressourcengruppe. Ein App Service-Plan gibt die API-app läuft auf Serverressourcen. Beispielsweise wählt die freie Ebene während der Ausführung API-app auf freigegebenen VMs für einige bezahlte Tiers auf dedizierten VMs Ausführung. Informationen zu App Service-Pläne Übersicht [App Service-Pläne](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

8. Klicken Sie im Dialogfeld **Konfigurieren App Service-Plan** Geben Sie ein "ToDoListPlan" oder einen anderen Namen Sie.

9. Wählen Sie in der Dropdown-Liste **Speicherort** den Speicherort, die Sie.

    Diese Einstellung gibt die Azure-Rechenzentrum Ihre Anwendung ausgeführt wird. Wählen Sie [Wartezeit](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)minimieren.

10. Klicken Sie in der Dropdownliste **Größe** **frei**.

    Für dieses Lernprogramm bietet kostenlose Tarif ausreichende Leistung.

11. Klicken Sie im Dialogfeld **Konfigurieren App Service-Plan** auf **OK**.

    ![Klicken Sie auf Konfigurieren OK in App Service-Plan](./media/app-service-api-dotnet-get-started/configasp.png)

12. Klicken Sie im Dialogfeld **Create App Service** **Erstellen**.

    ![Klicken Sie im Dialogfeld Create App Service auf Erstellen](./media/app-service-api-dotnet-get-started/clickcreate.png)

    Visual Studio erstellt die API-app und ein Veröffentlichungsprofil, das alle erforderlichen API-App. Anschließend wird im **Veröffentlichen** -Assistenten verwenden das Projekt geöffnet.

    Der **Veröffentlichen** -Assistent wird geöffnet, auf der Registerkarte **Verbindung** (siehe unten).

    Zeigen auf der Registerkarte **Verbindung** **Server-** und **Sitename** Einstellungen für Ihre API-Anwendung. Der **Benutzername** und das **Kennwort** sind Azure erstellt Deployment-Anmeldeinformationen. Nach der Bereitstellung öffnet Visual Studio einen Browser an den **Ziel-URL** (der einzige Zweck der **Ziel-URL**ist).  

13. Klicken Sie auf **Weiter**.

    ![Klicken Sie auf der Registerkarte Veröffentlichen Verbindung weiter](./media/app-service-api-dotnet-get-started/connnext.png)

    Die nächste Registerkarte ist **die Registerkarte (siehe unten)** . Hier können Sie die Registerkarte Konfiguration Build um ein Debugbuild [Remotedebuggen](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)bereitstellen ändern. Die Registerkarte enthält außerdem **Optionen für veröffentlichen**:

    * Entfernen Sie zusätzliche Dateien am Ziel
    * Vorkompilieren während der Veröffentlichung
    * Ausschließen von Dateien aus dem Ordner App_Data

    In diesem Lernprogramm benötigen Sie keiner. Eine ausführliche Beschreibung der Funktionsweise finden Sie unter [wie: Bereitstellen einer Web Project verwenden per Mausklick Veröffentlichen in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).

14. Klicken Sie auf **Weiter**.

    ![Klicken Sie auf Weiter im Registerkarte Web veröffentlichen](./media/app-service-api-dotnet-get-started/settingsnext.png)

    Anschließend wird die Registerkarte **Vorschau** (siehe unten), wodurch Sie die Möglichkeit, Dateien aus dem Projekt in der API-app kopiert werden soll. Wenn Sie ein Projekt an eine API-Anwendung, die Sie zum zuvor bereits bereitstellen, werden nur geänderte Dateien kopiert. Wenn finden eine Übersicht darüber, was kopiert werden soll, können Sie die **Vorschau starten** klicken.

15. Klicken Sie auf **Veröffentlichen**.

    ![Klicken Sie auf der Registerkarte Vorschau veröffentlichen veröffentlichen](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Visual Studio wird das Projekt ToDoListDataAPI neue API-App bereitgestellt. **Das Ausgabefenster** protokolliert erfolgreiche Bereitstellung und in einem Browserfenster geöffnet, um die URL der API-app wird eine Seite "erstellt" angezeigt.

    ![Ausgabe Fenster erfolgreiche Bereitstellung](./media/app-service-api-dotnet-get-started/deploymentoutput.png)

    ![Neue API erstellt Anwendungsseite](./media/app-service-api-dotnet-get-started/appcreated.png)

16. URL in die Adressleiste des Browsers hinzufügen "swagger" und dann die EINGABETASTE. (Die URL ist `http://{apiappname}.azurewebsites.net/swagger`.)

    Der Browser zeigt dieselbe Swagger-Benutzeroberfläche, die Sie zuvor gesehen haben, aber jetzt in der Cloud ausgeführt. Probieren Sie die Get-Methode und Sie sehen, dass Sie auf 2 Standard-Aufgaben. Die zuvor vorgenommenen Änderungen wurden im Speicher des lokalen Computers gespeichert.

17. [Azure-Portal](https://portal.azure.com/)zu öffnen.

    Azure-Portal ist eine Weboberfläche zur Verwaltung von Azure Ressourcen wie API-apps.

18. Klicken Sie auf **Weitere Dienste > Anwendungsdienste**.

    ![Anwendungsdienste durchsuchen](./media/app-service-api-dotnet-get-started/browseas.png)

19. In **App-Services** -Blade und klicken Sie auf die neue API-app. (In Azure-Portal Fenster rechts *Blades*heißen.)

    ![App-Services-blade](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

    Zwei Blades öffnen. Eine-Blade hat eine Übersicht über die API-app und hat eine lange Liste von Eigenschaften, die Sie anzeigen und ändern können.

20. Blatt **Einstellungen** **API** Abschnitt und dann auf **API-Definition**.

    ![API-Definition in Einstellungen-Blades](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    Blade **-API-Definition** können Sie die URL angeben, der Swagger 2.0 Metadaten im JSON-Format zurückgibt. Wenn Visual Studio die API-Anwendung erstellt, wird die URL der API-Definition auf den Standardwert für Swashbuckle generiert Metadaten, die Sie zuvor gesehen haben die API-app Basis für den URL plus `/swagger/docs/v1`.

    ![URL der API-definition](./media/app-service-api-dotnet-get-started/apidefurl.png)

    Beim Auswählen einer API-app Clientcode generieren Visual Studio die Metadaten abgerufen von diesem URL.

## <a id="codegen"></a>Generieren von Clientcode für die Datenebene

Einer der Vorteile der Integration von stolz in Azure API-apps ist automatische Generierung. Generierte Client-Klassen vereinfachen Code schreiben, eine API-Anwendung aufruft.

ToDoListAPI Projekt bereits erstellten Clientcode, aber in den folgenden Schritten Sie löschen und regenerieren, wie die Code Generation.

1. Löschen Sie im Visual Studio- **Projektmappen-Explorer**im Projekt ToDoListAPI *ToDoListDataAPI* . **Achtung: Löschen Sie nur den Ordner nicht das ToDoListDataAPI Projekt**

    ![Generierter Clientcode löschen](./media/app-service-api-dotnet-get-started/deletecodegen.png)

    Dieser Ordner wurde erstellt, mit der Code-Generierung, die zu durchlaufen.

2. ToDoListAPI Projekt klicken und dann auf **Hinzufügen > REST API-Client**.

    ![REST-API in Visual Studio hinzufügen](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3. Klicken Sie im Dialogfeld **REST API-Client hinzufügen** klicken Sie auf **URL Swagger**und klicken Sie auf **Azure Anlage auswählen**.

    ![Wählen Sie Azure Anlage](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

4. Im Dialogfeld **App Service** erweitern Sie die Ressourcengruppe für dieses Lernprogramm verwenden und Ihre API-Anwendung auswählen, und klicken Sie auf **OK**.

    ![API-app für Code-Erzeugung auswählen](./media/app-service-api-dotnet-get-started/codegenselect.png)

    Beachten Sie, dass wenn wieder die **REST API-Client hinzufügen** Dialogfeld Feld mit der API-Definition URL-Wert ausgefüllt wurde, die in das Portal sah.

    ![URL der API-Definition](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    >[AZURE.TIP] Eine alternative Möglichkeit zum Abrufen von Metadaten für Code-Erzeugung ist die URL direkt anstatt im Dialogfeld eingeben. Oder wenn Clientcode vor der Bereitstellung in Azure generiert werden soll, konnte Web API Projekt lokal ausführen, gehen Sie zu der URL, Swagger JSON-Datei bereitstellt, speichern und verwenden Sie die Option **vorhandene stolz Metadatendatei auswählen** .

5. Klicken Sie im Dialogfeld **REST API-Client hinzufügen** auf **OK**.

    Visual Studio erstellt einen Ordner namens nach API-Anwendung und -Client-Klassen generiert.

    ![Dateien für generierte client](./media/app-service-api-dotnet-get-started/codegenfiles.png)

6. Öffnen Sie im Projekt ToDoListAPI *controllers\todolistcontroller* um den Code in Zeile 40 anzuzeigen, die die API mit generierten Client aufruft.

    Der folgende Codeausschnitt zeigt, wie der Code des Clientobjekts instanziiert und die Get-Methode aufruft.

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }

        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }

    Konstruktorparameter Ruft die Endpunkt-URL von der `toDoListDataAPIURL` app festlegen. In der Datei Web.config wird dieser Wert zum URL lokalen IIS Express des Projekts API festgelegt, damit die Anwendung lokal ausgeführt werden kann. Wenn den Konstruktorparameter auslassen, ist der Standardendpunkt URL generiert Code aus.

7. Die Clientklasse wird unter einem anderen Namen basierend auf API-Namen generiert; Ändern Sie den Code in *controllers\todolistcontroller* , damit der Name entspricht im Projekt erstellten. Wenn die API App ToDoListDataAPI071316 benannt, würden Sie diesen Code ändern:

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

dazu:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-to-host-the-middle-tier"></a>Erstellen Sie eine API zum Hosten der mittleren Ebene

Sie [die Ebenen-API-Anwendung erstellt und bereitgestellt, Code](#createapiapp).  Jetzt gehen Sie für die mittlere Ebene API-app.

1. Klicken Sie im **Projektmappen-Explorer**die Zwischenebene ToDoListAPI Projekt (nicht die Datenebene ToDoListDataAPI), und klicken Sie auf **Veröffentlichen**.

    ![Klicken Sie auf Veröffentlichen in Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)

2.  Klicken Sie auf der Registerkarte **Profil** des **Veröffentlichen** -Assistenten auf **Microsoft Azure App Service**.

3. Klicken Sie im Dialogfeld **App-Dienst** **neu**.

4. **Hosting** -Registerkarte im Dialogfeld **Create App Service** übernehmen Sie **API App** Standardnamen, oder geben Sie einen eindeutigen Namen in der Domäne **.azurewebsites.NET* .

5. Wählen Sie Verwendung Azure- **Abonnement** .

6. Wählen Sie in der Dropdownliste **Ressourcengruppe** derselben Ressourcengruppe, die Sie zuvor erstellt haben.

7. Wählen Sie in der Dropdownliste **App Service-Plan** zuvor erstellten Planes. Standardmäßig wird dieser Wert verwendet.

8. Klicken Sie auf **Erstellen**.

    Visual Studio erstellt die API-app, ein Veröffentlichungsprofil verschafft und zeigt die **Verbindung** des **Veröffentlichen** -Assistenten.

9.  Klicken Sie im Schritt **Verbindung** des **Veröffentlichen** -Assistenten **Veröffentlichen**.

    Visual Studio stellt das ToDoListAPI Projekt neue API-App und öffnet einen Browser die URL der API-app. "Erfolgreich erstellt" angezeigt.

## <a name="configure-the-middle-tier-to-call-the-data-tier"></a>Konfigurieren der mittleren Schicht die Datenebene aufrufen

Wenn die Zwischenebene API-Anwendung jetzt aufgerufen wird, würde es versuchen Aufrufen die Datenebene mit dem Localhost-URL, die noch in der Datei Web.config. In diesem Abschnitt geben Sie die Tier-API app URL zu einer Umgebung in der mittleren Ebene API-app. Wenn der Code in der mittleren Ebene API-app Daten Tier URL Einstellung abruft, wird die umgebungseinstellung überschrieben Neuigkeiten in der Datei Web.config.

1. Zum [Azure-Portal](https://portal.azure.com/), und navigieren Sie an die **API-App** -Blade für API-Anwendung, die Sie zum Hosten des Projekts TodoListAPI (mittlere Ebene) erstellt.

2. Klicken Sie in der API-App **Einstellungen** Blade auf **Anwendung**.

3. API-App **Einstellungen** Blatt **App** Einstellungen Scrollen Sie und fügen Sie den folgenden Schlüssel und Wert hinzu. Der Wert ist der URL der ersten API-App in diesem Lernprogramm veröffentlichte.

  	| **Schlüssel** | toDoListDataAPIURL |
  	|---|---|
  	| **Wert** | https://{Your API app der Name der Datenebene} .azurewebsites .net |
  	| **Beispiel** | https://todolistdataapi.azurewebsites.NET |

4. Klicken Sie auf **Speichern**.

    ![Speichern Sie für App-Einstellungen](./media/app-service-api-dotnet-get-started/asinportal.png)

    Beim Ausführen des Codes in Azure überschreibt dieser Wert nun die Localhost-URL, die in der Datei Web.config.

## <a name="test"></a>Test

1. Wechseln Sie in einem Browserfenster den URL der neuen Zwischenebene API-app, die nur für ToDoListAPI erstellt. Sie erhalten es durch Klicken auf die URL der API-app main Blade im Portal.

2. URL in die Adressleiste des Browsers hinzufügen "swagger" und dann die EINGABETASTE. (Die URL ist `http://{apiappname}.azurewebsites.net/swagger`.)

    Der Browser zeigt dieselbe Swagger-Benutzeroberfläche, die für ToDoListDataAPI, aber jetzt vorhin `owner` ist kein erforderliches Feld für den Ladevorgang Zwischenebene API-app für Sie Wert auf der Ebene API-Anwendung senden. (Lernprogramme Authentifizierung dagegen senden die mittlere Ebene tatsächliche Benutzer-IDs für die `owner` Parameter; für jetzt es Sternchen programmieren wird.)

3. Probieren Sie die Get-Methode und Methoden zum Überprüfen Zwischenebene API-app erfolgreich die Ebenen-API-Anwendung aufrufen.

    ![Stolz UI Get-Methode](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Problembehandlung

Für den Fall, dass Sie auf ein Problem gestoßen, während Sie dieses Lernprogramm durchgehen werden einige Vorschläge zur Problembehandlung:

* Stellen Sie sicher, dass Sie die neueste Version von [Azure SDK für .NET](http://go.microsoft.com/fwlink/?linkid=518003)verwenden.

* Die Projektnamen sind ähnliche (ToDoListAPI, ToDoListDataAPI). Wenn Dinge schauen, wie in der Anleitung beschrieben, wenn Sie mit einem Projekt arbeiten, stellen Sie sicher, das richtige Projekt geöffnet haben.

* Wenn Sie in einem Unternehmensnetzwerk und Azure App Service über eine Firewall bereitstellen möchten, sicherstellen Sie, dass die Ports 443 und 8172 für Web Deploy geöffnet sind. Wenn Sie diese Ports öffnen können, können Sie andere Bereitstellungsmethoden.  Finden Sie [Ihre app Azure App Service bereitstellen](../app-service-web/web-sites-deploy.md).

* "Route muss eindeutig sein" Fehler - könnten Sie diese, wenn versehentlich falsche Projekt App API bereitstellen und später die richtige darauf bereitstellen. Zum Beheben dieses bereitstellen das richtige Projekt API-app und auf **die Registerkarte des **Veröffentlichen** -Assistenten** wählen Sie **zusätzliche Dateien am Ziel entfernen**.

Nachdem Sie Ihre ASP.NET API-Anwendung in Azure App Service ausgeführt haben, sollten Sie weitere Informationen zu Visual Studio-Funktionen, die Problembehandlung zu vereinfachen. Informationen zur Protokollierung Remotedebuggen und mehr finden Sie unter [Problembehandlung bei Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Nächste Schritte

Sie haben gesehen, wie Projekte Web API für API-apps bereitstellen, Generieren von Clientcode für API-apps und API-apps von .NET Clients verwenden. Zeigt das nächste Lernprogramm dieser Serie Verwendung [CORS um API-apps von JavaScript-Clients verwenden](app-service-api-cors-consume-javascript.md).

Weitere Informationen zum Generieren von Code finden Sie unter Repository [Azure-AutoRest](https://github.com/azure/autorest) auf GitHub.com. Öffnen Sie Hilfe zu Problemen mit dem generierten Client ein [Problem im Repository AutoRest](https://github.com/azure/autorest/issues).

Neue API-app-Projekte erstellen, verwenden Sie die Vorlage **Azure API-App** .

![API-App-Vorlage in Visual Studio](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

**Azure API-App** -Projektvorlage entspricht die **leere** ASP.NET 4.5.2 Vorlage auf das Kontrollkästchen Web-API unterstützen und Swashbuckle NuGet-Paket installieren. Die Vorlage fügt außerdem Swashbuckle Konfigurationscode verhindert die Erstellung von doppelten stolz Vorgang IDs. Nach dem Erstellen einer API-App-Projekt können Sie Bereitstellen einer API-App in diesem Lernprogramm sah genauso.
