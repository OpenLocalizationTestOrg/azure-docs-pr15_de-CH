<properties 
    pageTitle="App Service API app Trigger | Microsoft Azure" 
    description="Implementieren von Triggern in API-App in Azure App Service" 
    services="logic-apps" 
    documentationCenter=".net" 
    authors="guangyang"
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="na" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="rachelap"/>

# <a name="azure-app-service-api-app-triggers"></a>Azure App Service API-app-Triggern

>[AZURE.NOTE] Diese Version des Artikels gilt für API-apps 2014-12-01-Vorschau-Schemaversion.


## <a name="overview"></a>Übersicht

Dieser Artikel erläutert die API app Trigger implementieren und nutzen sie eine Logik-app.

Alle Codeausschnitte in diesem Thema werden [FileWatcher API-App Codebeispiel](http://go.microsoft.com/fwlink/?LinkId=534802)kopiert. 

Hinweis müssen Sie downloaden das folgenden Nuget-Paket für den Code in diesem Artikel zum Erstellen und ausführen: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Was sind API-app Trigger?

Es ist häufig eine API-App ein Ereignis auszulösen, damit Clients die API-app die Reaktion auf das Ereignis einleiten können. Der REST-basierten API-Mechanismus, der dieses Szenario unterstützt wird einen API-app Trigger aufgerufen. 

Angenommen Ihr Clientcode [app Twitter Connector-API](../app-service-logic/app-service-logic-connector-twitter.md) und Code auf eine Aktion basierend auf neuen Tweets, die bestimmte Wörter enthalten. In diesem Fall können Sie Umfrage oder Push-Trigger einrichten, zu dieser Anforderung.

## <a name="poll-trigger-versus-push-trigger"></a>Umfrage Trigger und Push-Auslöser

Derzeit werden zwei Arten von Auslösern:

- Umfrage Trigger - Client fragt API-app für die Benachrichtigung über ein Ereignis, dass ausgelöst 
- Push-Trigger - Client wird von der API-app benachrichtigt, wenn ein Ereignis ausgelöst wird 

### <a name="poll-trigger"></a>Umfrage-trigger

Umfrage Trigger als reguläre REST-API implementiert und Clients (z. B. eine Anwendung Logik) Abfragen um Benachrichtigung erwartet. Während der Client erhalten kann, ist der Umfrage Trigger selbst statusfrei. 

Die folgende Informationen zu der Anforderung und Antwort-Pakete zeigen einige wichtige Aspekte des Vertrags Trigger Umfrage:

- Anforderung
    - HTTP-Methode: Abrufen
    - Parameter
        - TriggerState - dieser optionale Parameter kann Clients Zustand, damit der Umfrage Trigger korrekt auf Benachrichtigung oder nicht entscheiden kann, ob auf den angegebenen Status Basis festlegen.
        - API-Parameter
- Antwort
    - Status Code **200** - Anforderung gültig ist, und eine Benachrichtigung vom Trigger. Der Inhalt der Benachrichtigung werden Antworttext. Ein "Retry-After"-Header in der Antwort gibt an, dass zusätzliche Benachrichtigungsdaten mit einer nachfolgenden Anforderung abgerufen werden müssen.
    - Status Code **202** - Anforderung gültig ist, aber keine neue Benachrichtigung vom Trigger.
    - Status Code **4xx** - Anforderung ist ungültig. Der Client sollte die Anforderung nicht wiederholen.
    - Status Code **5xx** - Anfrage ein interner Serverfehler oder ein vorübergehendes Problem resultieren. Der Client sollte die Anforderung wiederholen.

Der folgende Codeausschnitt ist ein Beispiel zum Implementieren eines Umfrage Triggers.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

Zum Testen dieser Umfrage Trigger folgendermaßen Sie vor:

1. Bereitstellen Sie API-App mit einer **öffentlichen anonymen**Authentifizierung.
2. Rufen Sie den Vorgang **Tippen** , um eine Datei. Die folgende Abbildung zeigt eine Beispiel für eine Anforderung über Briefträger.
   ![Bedienung über Postbote aufrufen](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. Aufrufen des Umfrage Triggers mit der **TriggerState** -Parameter auf einen Zeitstempel vor Schritt2. Die folgende Abbildung zeigt Beispiel Anfrage Briefträger.
   ![Umfrage Trigger über Postbote aufrufen](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Push-Auslöser

Ein Push-Trigger wird als reguläre REST-API implementiert, die Benachrichtigungen an Clients legt benachrichtigt, wenn bestimmte Ereignisse registriert haben.

Die folgende Informationen zu der Anforderung und Antwort-Pakete zeigen einige wichtige Aspekte des Vertrags Trigger Push.

- Anforderung
    - HTTP-Methode: Einfügen
    - Parameter
        - TriggerId: erforderlich - nicht transparente Zeichenfolge (z. B. eine GUID), der die Registrierung eines Push-Triggers darstellt.
        - CallbackUrl: erforderlich - URL des Rückrufs aufgerufen wird, wenn das Ereignis ausgelöst wird. Der Aufruf ist ein einfacher HTTP-POST-Aufruf.
        - API-Parameter
- Antwort
    - Status Code **200** - Anforderung an den Client erfolgreich registriert.
    - Status Code **4xx** - Anforderung ist ungültig. Der Client sollte die Anforderung nicht wiederholen.
    - Status Code **5xx** - Anfrage ein interner Serverfehler oder ein vorübergehendes Problem resultieren. Der Client sollte die Anforderung wiederholen.
- Rückruf
    - HTTP-Methode: Buchen
    - Anforderungsinhalt: Benachrichtigungsinhalt.

Der folgende Codeausschnitt ist ein Beispiel zum Implementieren einer Push-Auslöser:

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

Zum Testen dieser Umfrage Trigger folgendermaßen Sie vor:

1. Bereitstellen Sie API-App mit einer **öffentlichen anonymen**Authentifizierung.
2. Wechseln Sie zu [http://requestb.in/](http://requestb.in/) dienen als Ihr Callback-URL wird ein RequestBin erstellt.
3. Rufen Sie den Push-Trigger mit einer GUID als **TriggerId** und die RequestBin-URL als **CallbackUrl**.
   ![Push-Auslöser über Postbote aufrufen](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Rufen Sie den Vorgang **Tippen** , um eine Datei. Die folgende Abbildung zeigt eine Beispiel für eine Anforderung über Briefträger.
   ![Bedienung über Postbote aufrufen](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Überprüfen der RequestBin Bestätigen der Push-Trigger Rückruf mit Eigenschaft aufgerufen wird.
   ![Umfrage Trigger über Postbote aufrufen](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Beschreiben von Triggern in API-definition

Trigger implementieren und Bereitstellen von API-app in Azure Blade **-API-Definition** im vorschauportal Azure navigieren und Sie sehen, dass Trigger in der Benutzeroberfläche definitionsgemäß Swagger 2.0 API API-app getrieben, automatisch erkannt werden.

![API-Definition Blade](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Klicken Sie **Swagger downloaden** und die JSON-Datei sehen Sie Ergebnisse, die der folgenden ähnelt:

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

Die Erweiterung **X-ms-Scheduler-Trigger** sind Trigger im API-Definition und wird automatisch durch das API-Gateway app Wenn API-Definition über das Gateway anfordern, wenn die Anforderung an eine der folgenden Kriterien. (Sie können auch diese Eigenschaft manuell hinzufügen.)

- Umfrage-trigger
    - Wenn **die HTTP-Methode ist.**
    - Wenn die **OperationId** -Eigenschaft Zeichenfolge **Trigger**enthält.
    - Enthält die **Parameters** -Eigenschaft einen Parameter mit der Eigenschaft **Name** auf **TriggerState**festgelegt.
- Push-Auslöser
    - Wenn die HTTP-Methode **setzen**.
    - Wenn die **OperationId** -Eigenschaft Zeichenfolge **Trigger**enthält.
    - Enthält die **Parameters** -Eigenschaft einen Parameter mit der Eigenschaft **Name** auf **TriggerId**festgelegt.

## <a name="use-api-app-triggers-in-logic-apps"></a>API-app-Triggern Logik Apps verwenden

### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a>Auflisten und Logik apps Designer API app Auslöser konfigurieren

Wenn Sie eine Anwendung Logik in derselben Ressourcengruppe wie die API-app erstellen, können die Designer Leinwand einfach durch Klicken hinzufügen Sie. Die folgenden Bilder verdeutlichen dies:

![Trigger Logik App-Designer](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Umfrage Trigger Logik App Designer konfigurieren](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Push-Auslöser Logik App Designer konfigurieren](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>API-app-Triggern Logik Apps optimieren

Nach einer API-app Trigger hinzufügen, sind ein paar Dinge verbessern zur bei API-app in einer Anwendung Logik.

Beispielsweise **TriggerState** Parameter für Abstimmung Trigger auf den folgenden Ausdruck in der Anwendung Logik fest. Dieser Ausdruck sollte letzten Aufruf des Triggers App Logik ausgewertet und dieser Wert zurück.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

Hinweis: Eine Erläuterung der Funktionen im obigen Ausdruck finden Sie der Dokumentation [Logik App Workflow Definition Language](https://msdn.microsoft.com/library/azure/dn948512.aspx).

Logik app Benutzer müssen zu der Ausdruck oben für den **TriggerState** -Parameter mit dem Trigger. Es ist möglich, diesen Wert Vorgabe Logik app Designer durch Erweiterung Eigenschaft **X-ms-Scheduler-Empfehlung**.  Die **X-ms -** Erweiterung Sichtbarkeitseigenschaft kann auf *interne* festgelegt werden, damit Parameter selbst nicht im Designer angezeigt wird.  Der folgende Codeausschnitt zeigt, dass.

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

Für Push-Auslöser muss der Parameter **TriggerId** Logik app eindeutig identifizieren. Empfohlen wird diese Eigenschaft auf den Namen des Workflows mit dem folgenden Ausdruck festlegen:

    @workflow().name

Mit Erweiterungseigenschaften **X-ms-Scheduler-Empfehlung** und **X-ms-Sichtbarkeit** in der API-Definition vermitteln API-app Logik app Designer dieser Ausdruck für den Benutzer automatisch festgelegt.

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>Erweiterungseigenschaften in API-Definition hinzufügen

Zusätzliche Metadaten - Erweiterung Eigenschaften **X-ms-Scheduler-Empfehlung** und **X-ms-Sichtbarkeit** - in API-Definition auf zwei Arten hinzugefügt werden: statisch oder dynamisch.

Statische Metadaten können Sie direkt bearbeiten Sie die Datei */metadata/apiDefinition.swagger.json* in das Projekt und die Eigenschaften manuell hinzufügen.

Dynamische Metadaten-API-apps können Sie die Datei SwaggerConfig.cs, um einen Vorgang Filter hinzufügen, der diese Erweiterungen bearbeiten.

    GlobalConfiguration.Configuration 
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


Folgendes ist ein Beispiel, wie diese Klasse implementiert werden kann, um dynamische Metadaten-Szenario zu erleichtern.

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
 
