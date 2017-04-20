<properties
    pageTitle="Offline-Synchronisierung für Ihre universelle Windows-Plattform (UWP) Mobile Apps aktivieren | Azure App Service"
    description="Erfahren Sie, wie Ihre app universelle Windows-Plattform (UWP) mit einer Azure Mobile App Cache und Synchronisation offline-Daten."
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-windows-app"></a>Offline-Synchronisierung für Ihre Windows aktivieren

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Übersicht

Dieses Lernprogramm veranschaulicht eine universelle Windows-Plattform (UWP)-app eine Azure Mobile App Back-End offline-Unterstützung hinzu. Offline Sync ermöglicht Endbenutzern, mit einer app - anzeigen, hinzufügen oder Ändern von Daten – selbst wenn keine Verbindung zum Netzwerk zu interagieren. Änderungen werden in einer lokalen Datenbank gespeichert. Sobald das Gerät wieder online ist, werden diese Änderungen mit remote-Backend synchronisiert.

In dieser Übung aktualisieren Sie UWP-app-Projekt von Tutorial [Erstellen einer Windows-Anwendung] offline Funktionen Azure Mobile Apps unterstützen. Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, müssen Sie Data Access Erweiterung Pakete zum Projekt hinzufügen. Weitere Informationen zu Server-Erweiterung Paketen finden Sie unter [mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Erfahren Sie mehr über die offline Synchronisierungsfunktion finden Sie [Offline Daten-Synchronisierung in Azure Mobile Apps].

## <a name="requirements"></a>Vorschriften

Dieses Lernprogramm erfordert die folgenden erforderlichen Komponenten:

* Visual Studio 2013 unter Windows 8.1 oder höher.
* Abschluss der [Erstellen einer Windows-Anwendung][Erstellen Sie eine Windows-Anwendung].
* [Azure Mobile Services SQLite Speicher][sqlite store nuget]
* [SQLite für universelle Windows-Plattform](http://www.sqlite.org/downloads)

## <a name="update-the-client-app-to-support-offline-features"></a>Aktualisieren Sie die Clientanwendung Unterstützung offline Funktionen

Azure Mobile-Anwendung offline-Funktionen können Sie mit einer lokalen Datenbank interagieren wird offline Szenario. Zum Verwenden dieser Funktionen in Ihrer Anwendung initialisieren [SyncContext] [ synccontext] zu einem lokalen Speicher. Verweisen Sie die Tabelle über die Schnittstelle [IMobileServiceSyncTable][IMobileServiceSyncTable] . SQLite wird der lokale Speicher auf dem Gerät.

1. Installieren der [SQLite Runtime für universelle Windows-Plattform](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).

2. Öffnen Sie in Visual Studio den NuGet-Paket-Manager für UWP-app-Projekt, das im Lernprogramm [Erstellen einer Windows-Anwendung] ausgeführt.
    Suchen und **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-Paket installieren.

4. Klicken Sie im Projektmappen-Explorer **Verweise** > **Verweis hinzufügen**  >  **Universal Windows** > **Extensions** **SQLite für universelle Windows-Plattform** und **Visual C++ 2015 Runtime für universelle Windows-Plattform apps**aktivieren.

    ![SQLite UWP Verweis hinzufügen][1]

5. Öffnen Sie die Datei MainPage.xaml.cs und Kommentieren der `#define OFFLINE_SYNC_ENABLED` Definition.

6. Drücken Sie in Visual Studio **F5** und führen Sie die Clientanwendung. Die Anwendung funktioniert wie zuvor offline synchronisieren aktiviert. Die lokale Datenbank ist jedoch jetzt mit Daten aufgefüllt, die in einem offline verwendet werden können.

## <a name="update-sync"></a>Aktualisieren Sie so trennen Sie die Back-End-Anwendung

In diesem Abschnitt unterbrechen Sie die Verbindung zum Backend Mobile-Anwendung offline Situation simuliert. Beim Hinzufügen von Daten von Ausnahmehandler informiert, dass die Anwendung im Offlinemodus befindet. In diesem Zustand neue Elemente im lokalen Speicher hinzugefügt und nächsten Ausführung Push verbunden Backend mobile Anwendung synchronisiert.

1. Bearbeiten Sie App.xaml.cs in das freigegebene Projekt. Kommentieren Sie die Initialisierung von **MobileServiceClient** und fügen Sie folgende Zeile eine Ungültiger mobile app-URL verwendet:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Sie können auch deaktivieren Wifi und Mobilfunknetzen auf dem Gerät offline Verhalten zeigen oder Flugmodus.

2. Drücken Sie **F5** zum Erstellen und Ausführen der Anwendung. Beachten Sie die Synchronisierung konnte nicht aktualisieren, wenn die app gestartet.

3. Neue Einträge, und beachten Sie Push jedem Status [CancelledByNetworkError] nicht **Speichern**klicken. Jedoch vorhanden neue Aufgaben im lokalen Speicher, bis sie mobile app Backend abgelegt werden können.  In einer app Produktion unterdrückt diese Ausnahmen verhält sich die Clientanwendung wie bei noch mit mobile app-Back-End verbunden ist.

4. Schließen Sie die Anwendung und neu starten Sie, um sicherzustellen, dass neue Elemente erstellten lokalen Speicher beibehalten werden.

5. (Optional) Öffnen Sie **Server-Explorer**in Visual Studio. Navigieren Sie zu der Datenbank in **Azure**->**SQL-Datenbanken**. Ihre Datenbank Maustaste und wählen **im SQL Server-Objekt-Explorer öffnen**. Jetzt können Sie Ihre SQL-Datenbanktabelle und dessen Inhalt durchsuchen. Stellen Sie sicher, dass die Daten in der Back-End-Datenbank nicht geändert wurde.

6. (Optional) Ein REST Fiddler oder Postbote verwenden, um Ihre mobilen Backend mit einer GET-Abfrage im Formular Abfrage `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Update app Backend Mobile App wiederherstellen

In diesem Abschnitt schließen Sie die app mobile app-Back-End. Dieser Netzwerk Verbindung der Anwendung simulieren.

Beim Ausführen der Anwendung die `OnNavigatedTo` Ereignishandleraufrufe `InitLocalStoreAsync`. Diese Methode ruft `SyncAsync` auf dem lokalen Speicher mit Backend-Datenbank synchronisieren. Die Anwendung versucht, beim Start synchronisieren.

1. Öffnen Sie App.xaml.cs in das freigegebene Projekt, und kommentieren Sie Ihre vorherige Initialisierung der `MobileServiceClient` den richtigen mobile-app-URL verwenden.

2. Drücken Sie **F5 und führen Sie die Anwendung** . Die Anwendung synchronisiert die lokalen Änderungen Azure Mobile App Backend mit Push- und Pull-Vorgänge bei der `OnNavigatedTo` -Ereignishandler ausgeführt wird.

3. (Optional) Anzeigen der aktualisierten Daten mit SQL Server-Objekt-Explorer oder einem anderen Tool wie Fiddler. Hinweis die Daten wurde zwischen Azure Mobile App Back-End-Datenbank und den lokalen Speicher synchronisiert.

4. Klicken Sie in der Anwendung auf das Kontrollkästchen neben einigen Elementen im lokalen Informationsspeicher abzuschließen.

  `UpdateCheckedTodoItem`Ruft `SyncAsync` Element mit Backend-Mobile-Anwendung Synchronisieren jedes abgeschlossen. `SyncAsync`Ruft Push und Pull. Aber **Wenn Sie ein Pullabonnement für eine Tabelle ausführen, die der Client, geändert hat ein Push wird immer automatisch ausgeführt**. Dadurch wird sichergestellt, dass alle Tabellen im lokalen Speicher Beziehungen konsistent bleiben. Dieses Verhalten möglicherweise unerwartete Push.  Weitere Informationen zu diesem Verhalten finden Sie unter [Offline Daten synchronisieren in Azure Mobile Apps].


##<a name="api-summary"></a>API-Zusammenfassung

Um offline Funktionen von mobile Dienste unterstützen, wir [IMobileServiceSyncTable] Schnittstelle und initialisiert [MobileServiceClient.SyncContext] [ synccontext] mit einer lokalen SQLite Datenbank. Wenn Sie offline arbeiten normalen CRUD-Operationen für Mobile Apps wie app noch angeschlossen ist, während die Vorgänge im lokalen Speicher. Die folgenden Methoden dienen den lokalen Speicher mit dem Server synchronisieren:

*  **[PushAsync]** Da diese Methode [IMobileServicesSyncContext]angehört, sind Änderungen für alle Tabellen Backend abgelegt. Nur Datensätze mit lokalen Änderungen werden an den Server gesendet.

* **[PullAsync] ** 
   ein [IMobileServiceSyncTable]angefangen. Gibt Überarbeitungen in der Tabelle, wird ein impliziter Push ausgeführt, um sicherzustellen, dass alle Tabellen im lokalen Speicher Beziehungen konsistent bleiben. Der Parameter *PushOtherTables* steuert, ob andere Tabellen im Kontext in einer impliziten Push abgelegt werden. Der *Abfrage* -Parameter hat einen [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] 
   oder OData-Abfrage zum Filtern der zurückgegebenen Daten. *QueryId* -Parameter wird verwendet, um inkrementelle Synchronisierung zu definieren. Weitere Informationen finden Sie unter  [Offline Daten synchronisieren in Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works).

* **[PurgeAsync]** Ihre app sollte regelmäßig dieser Methodenaufruf um veraltete Daten aus dem lokalen Speicher zu löschen. Verwenden Sie den Parameter *erzwingen* , müssen Sie Änderungen löschen, die noch nicht synchronisiert wurden.

Weitere Informationen zu diesen Konzepten finden Sie unter [Offline Daten synchronisieren in Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Weitere Informationen

In den folgenden Themen bieten Hintergrundinformationen offline synchronisieren Funktion Mobile Apps:

* [Offline-Daten synchronisieren in Azure mobiler Apps]
* [Azure mobiler Apps .NET SDK HOWTO][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Offline-Daten synchronisieren in Azure mobiler Apps]: app-service-mobile-offline-data-sync.md
[Erstellen einer Windows-Anwendung]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
