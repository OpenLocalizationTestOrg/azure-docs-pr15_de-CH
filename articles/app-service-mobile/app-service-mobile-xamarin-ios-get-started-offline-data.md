<properties
    pageTitle="Aktivieren Sie offline-Synchronisierung für Ihre Azure Mobile (Xamarin iOS)"
    description="Erfahren Sie, wie App Service Mobile App Cache und Synchronisation offline Daten in der Xamarin iOS-Anwendung"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Offline-Synchronisierung für Ihre mobilen Xamarin.iOS aktivieren

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Übersicht

Dieses Lernprogramm führt die offline Synchronisierungsfunktion Azure Mobile Apps für Xamarin.iOS. Offline Sync ermöglicht Endbenutzern, mit einer app - anzeigen, hinzufügen oder Ändern von Daten, selbst wenn keine Verbindung zum Netzwerk zu interagieren. Änderungen werden in einer lokalen Datenbank gespeichert. Sobald das Gerät wieder online ist, werden diese Änderungen mit dem remote-Dienst synchronisiert.

Aktualisieren Sie in diesem Lernprogramm Xamarin.iOS-app-Projekt vom [Erstellen einer Xamarin iOS-Anwendung] offline Funktionen Azure Mobile Apps unterstützen. Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, müssen Sie Data Access Erweiterung Pakete zum Projekt hinzufügen. Weitere Informationen zu Server-Erweiterung Paketen finden Sie unter [mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Erfahren Sie mehr über die offline Synchronisierungsfunktion finden Sie [Offline Daten-Synchronisierung in Azure Mobile Apps].

## <a name="update-the-client-app-to-support-offline-features"></a>Aktualisieren Sie die Clientanwendung Unterstützung offline Funktionen

Azure Mobile-Anwendung offline-Funktionen können Sie mit einer lokalen Datenbank interagieren wird offline Szenario. Zum Verwenden dieser Funktionen in Ihrer Anwendung initialisieren Sie eine [SyncContext] auf einem lokalen Speicher. Verweisen auf die Tabelle über die Schnittstelle [IMobileServiceSyncTable]. SQLite wird der lokale Speicher auf dem Gerät.

1. Öffnen Sie des NuGet-Paket-Managers im Projekt im Lernprogramm [erstellen eine Xamarin iOS-app] abgeschlossen und suchen installieren Sie **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-Paket.

2. Öffnen Sie die Datei QSTodoService.cs, und kommentieren Sie den `#define OFFLINE_SYNC_ENABLED` Definition.

3. Erstellen Sie und führen Sie die Clientanwendung. Die Anwendung funktioniert wie zuvor offline synchronisieren aktiviert. Die lokale Datenbank ist jedoch jetzt mit Daten aufgefüllt, die in einem offline verwendet werden können.

## <a name="update-sync"></a>Aktualisieren Sie so trennen Sie die Back-End-Anwendung

In diesem Abschnitt unterbrechen Sie die Verbindung zum Backend Mobile-Anwendung offline Situation simuliert. Beim Hinzufügen von Daten von Ausnahmehandler informiert, dass die Anwendung im Offlinemodus befindet. In diesem Zustand neue Elemente im lokalen Speicher hinzugefügt und nächsten Ausführung Push verbunden Backend mobile Anwendung synchronisiert.

1. Bearbeiten Sie QSToDoService.cs in das freigegebene Projekt. Ändern der **ApplicationURL** auf eine ungültige URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Veranschaulichen Sie offline Verhalten, indem Sie Wifi und Mobilfunknetzen auf dem Gerät deaktivieren oder Flugmodus.

2. Erstellen und Ausführen der Anwendung. Beachten Sie die Synchronisierung konnte nicht aktualisieren, wenn die app gestartet.

3. Neue Einträge, und beachten Sie, dass bei jedem Klicken auf **Speichern**Push Status [CancelledByNetworkError] fehlschlägt. Jedoch vorhanden neue Aufgaben im lokalen Speicher, bis sie mobile app Backend abgelegt werden können.  In einer app Produktion unterdrückt diese Ausnahmen verhält sich die Clientanwendung wie bei noch mit mobile app-Back-End verbunden ist.

4. Schließen Sie die Anwendung und neu starten Sie, um sicherzustellen, dass neue Elemente erstellten lokalen Speicher beibehalten werden.

5. (Optional) Wenn Sie Visual Studio auf einem PC installiert haben, öffnen Sie **Server-Explorer**. Navigieren Sie zu der Datenbank in **Azure**-> **SQL-Datenbanken**. Ihre Datenbank Maustaste und wählen **im SQL Server-Objekt-Explorer öffnen**. Jetzt können Sie Ihre SQL-Datenbanktabelle und dessen Inhalt durchsuchen. Stellen Sie sicher, dass die Daten in der Back-End-Datenbank nicht geändert wurde.

6. (Optional) Ein REST Fiddler oder Postbote verwenden, um Ihre mobilen Backend mit einer GET-Abfrage im Formular Abfrage `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Update app Backend Mobile App wiederherstellen

Schließen Sie in diesem Abschnitt die app mobile app-Back-End. Verschieben von Offlinestatus Onlinestatus mit mobilen Anwendung Back-End-Anwendung simuliert.   Wenn Sie Netzwerk-Bruch Netzwerkkonnektivität ausschalten simuliert, werden keine Änderungen.
Aktivieren Sie das Netzwerk erneut ein.  Beim Ausführen der Anwendung der `RefreshDataAsync` wird aufgerufen. Dies wiederum `SyncAsync` auf dem lokalen Speicher mit Backend-Datenbank synchronisieren.

1. Öffnen Sie QSToDoService.cs in das freigegebene Projekt, und die Änderung der Eigenschaft **ApplicationURL** zurückgesetzt.

2. Erstellen Sie und führen Sie die Anwendung. Die app synchronisiert Ihre lokalen Änderungen mit Push- und Pull-Vorgänge mit Azure Mobile App-Backend bei der `OnRefreshItemsSelected` -Methode ausgeführt wird.

3. (Optional) Anzeigen der aktualisierten Daten mit SQL Server-Objekt-Explorer oder einem anderen Tool wie Fiddler. Hinweis die Daten wurde zwischen Azure Mobile App Back-End-Datenbank und den lokalen Speicher synchronisiert.

4. Klicken Sie in der Anwendung auf das Kontrollkästchen neben einigen Elementen im lokalen Informationsspeicher abzuschließen.

  `CompleteItemAsync`Ruft `SyncAsync` Element mit Backend-Mobile-Anwendung Synchronisieren jedes abgeschlossen. `SyncAsync`Ruft Push und Pull.
  **Wenn Sie ein Pullabonnement für eine Tabelle ausführen, die der Client, geändert hat ein Push auf Sync Clientkontext immer zuerst ausgeführt automatisch**. Implizite Push stellt sicher, dass alle Tabellen im lokalen Speicher Beziehungen konsistent bleiben. Weitere Informationen zu diesem Verhalten finden Sie unter [Offline Daten synchronisieren in Azure Mobile Apps].

## <a name="review-the-client-sync-code"></a>Überprüfen Sie den Clientcode synchronisieren

Xamarin-Clientprojekt, das Sie nach Abschluss des Lernprogramms [erstellen eine Xamarin iOS-app] bereits heruntergeladen enthält Code für offline mit einer lokalen SQLite Datenbank synchronisiert. Hier wird eine kurze Übersicht darüber, was bereits im Code Lernprogramms enthalten ist. Eine grundlegende Übersicht über die Funktion finden Sie unter [Offline Daten synchronisieren in Azure Mobile Apps].

* Bevor Tabellenvorgänge ausgeführt werden können, muss der lokale Speicher initialisiert. Die lokale Datenbank initialisiert beim `QSTodoListViewController.ViewDidLoad()` führt `QSTodoService.InitializeStoreAsync()`. Diese Methode erstellt ein neues lokales SQLite Datenbank mit der `MobileServiceSQLiteStore` -Klasse Azure Mobile App Client SDK.

    Die `DefineTable` -Methode erstellt eine Tabelle in den lokalen Speicher, die die Felder in den bereitgestellten Typ entspricht `ToDoItem` bei. Der Typ muss alle Spalten enthalten, die in der entfernten Datenbank. Es ist möglich, nur eine Teilmenge der Spalten speichern.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }


* Die `todoTable` Mitglied `QSTodoService` ist die `IMobileServiceSyncTable` anstelle des `IMobileServiceTable`. Die IMobileServiceSyncTable weist alle erstellen, lesen, aktualisieren und löschen (CRUD) Tabelle mit der lokalen Datenbank.

    Wenn diese Änderungen Backend Azure Mobile-Anwendung durch Aufrufen von abgelegt werden soll `IMobileServiceSyncContext.PushAsync()`. Der Synchronisierungskontext hilft Beziehungen verfolgen und Änderungen in eine Clientanwendung, wenn geändert wurde alle Tabellen beibehalten `PushAsync` aufgerufen.

    Der bereitgestellte Code ruft `QSTodoService.SyncAsync()` synchronisieren, wenn die Todoitem-Liste aktualisiert oder ein Todoitem hinzugefügt oder abgeschlossen. Nach jeder lokalen app-synchronisiert. Ein Pullabonnement für eine ausstehende lokale Updates nachverfolgt wurde Tabelle vom Kontext ausgeführt wird, werden diese Pull-Vorgang automatisch Kontext Push zuerst ausgelöst.

    In den bereitgestellten Code alle Datensätze in der entfernten `TodoItem` Tabelle abgefragt, aber es kann auch zum Filtern von Datensätzen durch die Abfrage-Id übergeben und Abfrage `PushAsync`. Weitere Informationen finden Sie im Abschnitt *Inkrementelle Synchronisierung* [Offline Daten]synchron in Azure Mobile Apps.

        // QSTodoService.cs
        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Offline-Daten synchronisieren in Azure mobiler Apps]
* [Azure mobiler Apps .NET SDK HOWTO][8]

<!-- Images -->

<!-- URLs. -->
[Erstellen einer Xamarin iOS-app]: app-service-mobile-xamarin-ios-get-started.md
[Offline-Daten synchronisieren in Azure mobiler Apps]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md