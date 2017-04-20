<properties
    pageTitle="Aktivieren Sie offline-Synchronisierung für Ihre Azure Mobile (Xamarin Android)"
    description="Erfahren Sie, wie App Service Mobile App Cache und Synchronisation offline Daten in Ihrer Anwendung Xamarin Android verwenden"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Offline-Synchronisierung für Ihre mobilen Xamarin.Android aktivieren

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Übersicht

Dieses Lernprogramm führt die offline Synchronisierungsfunktion Azure Mobile Apps für Xamarin.Android. Offline Sync ermöglicht Endbenutzern, mit einer app - anzeigen, hinzufügen oder Ändern von Daten, selbst wenn keine Verbindung zum Netzwerk zu interagieren. Änderungen werden in einer lokalen Datenbank gespeichert.
Sobald das Gerät wieder online ist, werden diese Änderungen mit dem remote-Dienst synchronisiert.

In dieser Übung aktualisieren Sie das Clientprojekt aus Tutorial [Erstellen Xamarin Android app] offline Funktionen Azure Mobile Apps unterstützen. Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, müssen Sie Data Access Erweiterung Pakete zum Projekt hinzufügen. Weitere Informationen zu Server-Erweiterung Paketen finden Sie unter [mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Erfahren Sie mehr über die offline Synchronisierungsfunktion finden Sie [Offline Daten-Synchronisierung in Azure Mobile Apps].

## <a name="update-the-client-app-to-support-offline-features"></a>Aktualisieren Sie die Clientanwendung Unterstützung offline Funktionen

Azure Mobile-Anwendung offline-Funktionen können Sie mit einer lokalen Datenbank interagieren wird offline Szenario. Zum Verwenden dieser Funktionen in Ihrer Anwendung initialisieren Sie eine [SyncContext] auf einem lokalen Speicher. Verweisen Sie die Tabelle [IMobileServiceSyncTable] [IMobileServiceSyncTable] Benutzeroberfläche. SQLite wird der lokale Speicher auf dem Gerät.

1. Öffnen Sie in Visual Studio NuGet-Paket-Manager im Projekt [Erstellen Xamarin Android app] -Lernprogramms abgeschlossen.  Suchen und **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-Paket installieren.

2. Öffnen Sie die Datei ToDoActivity.cs, und kommentieren Sie den `#define OFFLINE_SYNC_ENABLED` Definition.

3. Drücken Sie in Visual Studio **F5** und führen Sie die Clientanwendung. Die Anwendung funktioniert wie zuvor offline synchronisieren aktiviert. Die lokale Datenbank ist jedoch jetzt mit Daten aufgefüllt, die in einem offline verwendet werden können.

## <a name="update-sync"></a>Aktualisieren Sie so trennen Sie die Back-End-Anwendung

In diesem Abschnitt unterbrechen Sie die Verbindung zum Backend Mobile-Anwendung offline Situation simuliert. Beim Hinzufügen von Daten von Ausnahmehandler informiert, dass die Anwendung im Offlinemodus befindet. In diesem Zustand neue Elemente im lokalen Speicher hinzugefügt und Ausführung ein Push verbunden mit mobilen Anwendung Backend synchronisiert.

1. Bearbeiten Sie ToDoActivity.cs in das freigegebene Projekt. Ändern der **ApplicationURL** auf eine ungültige URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Veranschaulichen Sie offline Verhalten, indem Sie Wifi und Mobilfunknetzen auf dem Gerät deaktivieren oder Flugmodus.

2. Drücken Sie **F5** zum Erstellen und Ausführen der Anwendung. Beachten Sie die Synchronisierung konnte nicht aktualisieren, wenn die app gestartet.

3. Neue Einträge, und beachten Sie, dass bei jedem Klicken auf **Speichern**Push Status [CancelledByNetworkError] fehlschlägt. Jedoch vorhanden neue Aufgaben im lokalen Speicher, bis sie mobile app Backend abgelegt werden können.  In einer app Produktion unterdrückt diese Ausnahmen verhält sich die Clientanwendung wie bei noch mit mobile app-Back-End verbunden ist.

4. Schließen Sie die Anwendung und neu starten Sie, um sicherzustellen, dass neue Elemente erstellten lokalen Speicher beibehalten werden.

5. (Optional) Öffnen Sie **Server-Explorer**in Visual Studio. Navigieren Sie zu der Datenbank in **Azure**->**SQL-Datenbanken**. Ihre Datenbank Maustaste und wählen **im SQL Server-Objekt-Explorer öffnen**. Jetzt können Sie Ihre SQL-Datenbanktabelle und dessen Inhalt durchsuchen. Stellen Sie sicher, dass die Daten in der Back-End-Datenbank nicht geändert wurde.

6. (Optional) Ein REST Fiddler oder Postbote verwenden, um Ihre mobilen Backend mit einer GET-Abfrage im Formular Abfrage `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Update app Backend Mobile App wiederherstellen

Schließen Sie in diesem Abschnitt die app mobile app-Back-End. Beim Ausführen der Anwendung die `OnCreate` Ereignishandleraufrufe `OnRefreshItemsSelected`. Diese Methode ruft `SyncAsync` auf dem lokalen Speicher mit Backend-Datenbank synchronisieren.

1. Öffnen Sie ToDoActivity.cs in das freigegebene Projekt, und die Änderung der Eigenschaft **ApplicationURL** zurückgesetzt.

2. Drücken Sie **F5 und führen Sie die Anwendung** . Die app synchronisiert Ihre lokalen Änderungen mit Push- und Pull-Vorgänge mit Azure Mobile App-Backend bei der `OnRefreshItemsSelected` -Methode ausgeführt wird.

3. (Optional) Anzeigen der aktualisierten Daten mit SQL Server-Objekt-Explorer oder einem anderen Tool wie Fiddler. Hinweis die Daten wurde zwischen Azure Mobile App Back-End-Datenbank und den lokalen Speicher synchronisiert.

4. Klicken Sie in der Anwendung auf das Kontrollkästchen neben einigen Elementen im lokalen Informationsspeicher abzuschließen.

  `CheckItem`Ruft `SyncAsync` Element mit Backend-Mobile-Anwendung Synchronisieren jedes abgeschlossen. `SyncAsync`Ruft Push und Pull. **Wenn Sie ein Pullabonnement für eine Tabelle ausführen, die der Client, geändert hat ein Push wird immer automatisch ausgeführt**. Dies gewährleistet, dass alle Tabellen im lokalen Speicher Beziehungen konsistent bleiben. Dieses Verhalten möglicherweise unerwartete Push. Weitere Informationen zu diesem Verhalten finden Sie unter [Offline Daten synchronisieren in Azure Mobile Apps].

## <a name="review-the-client-sync-code"></a>Überprüfen Sie den Clientcode synchronisieren

Xamarin-Clientprojekt, das heruntergeladen, wenn Sie das Lernprogramm [Xamarin Android app erstellen] bereits abgeschlossen enthält Code für offline mit einer lokalen SQLite Datenbank synchronisiert. Hier wird eine kurze Übersicht über bereits Tutorial C Ode Lieferumfang. Eine grundlegende Übersicht über die Funktion finden Sie unter [Offline Daten synchronisieren in Azure Mobile Apps].

* Bevor Tabellenvorgänge ausgeführt werden können, muss der lokale Speicher initialisiert. Die lokale Datenbank initialisiert beim `ToDoActivity.OnCreate()` führt `ToDoActivity.InitLocalStoreAsync()`. Diese Methode erstellt ein lokales SQLite Datenbank mit der `MobileServiceSQLiteStore` -Klasse Azure Mobile Apps Client SDK.

    Die `DefineTable` -Methode erstellt eine Tabelle in den lokalen Speicher, die die Felder in den bereitgestellten Typ entspricht `ToDoItem` bei. Der Typ muss alle Spalten enthalten, die in der entfernten Datenbank. Es ist möglich, nur eine Teilmenge der Spalten speichern.

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            // To use a different conflict handler, pass a parameter to InitializeAsync.
            // For more details, see http://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }


* Die `toDoTable` Mitglied `ToDoActivity` ist die `IMobileServiceSyncTable` anstelle des `IMobileServiceTable`. Die IMobileServiceSyncTable weist alle erstellen, lesen, aktualisieren und löschen (CRUD) Tabelle mit der lokalen Datenbank.

    Wenn Änderungen Backend Azure Mobile-Anwendung durch Aufrufen abgelegt werden soll `IMobileServiceSyncContext.PushAsync()`. Der Synchronisierungskontext hilft Beziehungen verfolgen und Änderungen in eine Clientanwendung, wenn geändert wurde alle Tabellen beibehalten `PushAsync` aufgerufen.

    Der bereitgestellte Code ruft `ToDoActivity.SyncAsync()` synchronisieren, wenn die Todoitem-Liste aktualisiert oder ein Todoitem hinzugefügt oder abgeschlossen. Nach jeder lokalen Code-synchronisiert.

    In den bereitgestellten Code alle Datensätze in der entfernten `TodoItem` Tabelle abgefragt, aber es kann auch zum Filtern von Datensätzen durch die Abfrage-Id übergeben und Abfrage `PushAsync`. Weitere Informationen finden Sie im Abschnitt *Inkrementelle Synchronisierung* [Offline Daten]synchron in Azure Mobile Apps.

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Offline-Daten synchronisieren in Azure mobiler Apps]
* [Azure mobiler Apps .NET SDK HOWTO][8]

<!-- URLs. -->
[Xamarin Android app erstellen]: ../app-service-mobile-xamarin-android-get-started.md
[Offline-Daten synchronisieren in Azure mobiler Apps]: ../app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Xamarin Android app erstellen]: app-service-mobile-xamarin-android-get-started.md
[Offline-Daten synchronisieren in Azure mobiler Apps]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: http://xamarin.com/download
[Xamarin extension]: http://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
