<properties
    pageTitle="Aktivieren Sie offline-Synchronisierung für Ihre Azure Mobile (Android)"
    description="Erfahren Sie, wie App Service Mobile Apps Cache und Sync Offlinedaten Anwendungscode Android verwenden"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Offline-Synchronisierung für Ihre mobilen Android aktivieren

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Übersicht

Dieses Lernprogramm umfasst die offline Synchronisierungsfunktion Azure Mobile Apps für Android. Offline synchronisieren können Benutzer eine mobile Anwendung interagieren&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;selbst wenn keine Verbindung zum Netzwerk. Änderungen werden in einer lokalen Datenbank gespeichert. Sobald das Gerät wieder online ist, werden diese Änderungen mit remote-Backend synchronisiert.

Ist dies die erste Version von Azure Mobile Apps, sollten Sie das Lernprogramm [Erstellen Sie ein Android]abschließen. Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, müssen Sie Data Access Erweiterung Pakete zum Projekt hinzufügen. Weitere Informationen zu Server-Erweiterung Paketen finden Sie unter [mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Erfahren Sie mehr über die offline Synchronisierungsfunktion finden Sie [Offline Daten-Synchronisierung in Azure Mobile Apps].

## <a name="update-the-app-to-support-offline-sync"></a>Aktualisieren Sie die Anwendung offline Synchronisierung unterstützen

Mit offline synchronisieren zum Lesen und Schreiben von eine *Synchronisierungstabelle* (mithilfe der *IMobileServiceSyncTable* -Schnittstelle), gehört einer **SQLite** Datenbank auf dem Gerät.

Und zwischen dem Gerät und Azure Mobile Services ziehen, verwenden Sie einen *Synchronisierungskontext* (*MobileServiceClient.SyncContext*), die mit der lokalen Datenbank speichert Daten lokal initialisiert.

1. In `TodoActivity.java`, kommentieren Sie vorhandene Definition von `mToDoTable` und Ausführung der Synchronisierung:

        private MobileServiceSyncTable<ToDoItem> mToDoTable;

2. In der `onCreate` Methode, kommentieren Sie vorhandene Initialisierung des `mToDoTable` und diese Definition:

        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);

3. In `refreshItemsFromTable` kommentieren Sie die Definition des `results` und diese Definition:

        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();

4. Kommentieren Sie die Definition des `refreshItemsFromMobileServiceTable`.

5. Kommentieren Sie die Definition des `refreshItemsFromMobileServiceTableSyncTable`:

        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }

6. Kommentieren Sie die Definition des `sync`:

        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>Testen der Anwendung

In diesem Abschnitt Testen Sie das Verhalten mit WiFi auf und WiFi offline Szenario erstellen deaktivieren.

Wenn Sie Datenelemente hinzufügen, werden im lokalen SQLite Speicher frei, sondern mobile Service nicht synchronisiert, bis die Schaltfläche **Aktualisieren** . Andere apps können unterschiedliche Vorschriften, wenn Daten synchronisiert werden jedoch Demonstrationszwecken hat in diesem Lernprogramm der Benutzer explizit anfordern.

Beim Drücken der Schaltfläche wird eine neue Hintergrundaufgabe gestartet. Zunächst werden alle Änderungen auf den lokalen Speicher Synchronisierungskontext, dann zieht alle geändert Daten aus Azure in der lokalen Tabelle legt.

### <a name="offline-testing"></a>Offline-Tests

1. Setzen Sie das Gerät oder den Simulator im *Flugmodus*. Dies erstellt eine offline-Szenario.

2. *Einige Aufgaben* hinzufügen oder einige Elemente als abgeschlossen markieren. Beenden Sie das Gerät oder den Simulator (oder das Schließen der Anwendung) und neu starten. Überprüfen, ob Änderungen auf dem Gerät gespeichert wurden, weil sie statt in der lokalen SQLite Speicher.

3. Anzeigen des Inhalts der Azure *TodoItem* Tabelle mit einem SQL-wie *SQL Server Management Studio*oder einen anderen Client *Fiddler* oder *Briefträger Tool*. Überprüfen Sie, ob die neuen Elemente sind _nicht_ mit dem Server synchronisiert

    + Node.js-Backend gehen [Azure-Portal](https://portal.azure.com/)und Mobile-App Backend finden Sie **Einfache Tabellen** > **TodoItem** an den Inhalt der `TodoItem` Tabelle.
    + Zeigen Sie für .NET Backend Inhaltsverzeichnis ein SQL-Tool wie *SQL Server Management Studio*oder einen anderen Client *Fiddler* oder *Postbote an*

4. WiFi Gerät oder Simulator aktivieren. Drücken Sie dann die Schaltfläche **Aktualisieren** .

5. Die TodoItem Daten erneut in Azure-Portal anzeigen. Neue und geänderte TodoItems sollte nun angezeigt werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Offline-Daten synchronisieren in Azure mobiler Apps]

* [Bewölkung: Offline synchronisieren in Azure Mobile Dienste] \(Hinweis: Videos auf Mobile Services ist jedoch offline synchronisieren funktioniert ähnlich wie in Azure Mobile Apps\)


<!-- URLs. -->

[Offline-Daten synchronisieren in Azure mobiler Apps]: app-service-mobile-offline-data-sync.md

[Erstellen einer Android]: app-service-mobile-android-get-started.md

[Bewölkung: Offline synchronisieren in Azure Mobile Dienste]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

