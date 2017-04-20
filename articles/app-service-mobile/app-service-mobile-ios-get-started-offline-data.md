<properties
    pageTitle="Aktivieren Sie offline-Synchronisierung für Ihre Azure Mobile (iOS)"
    description="Informationen Sie zur Verwendung von App Service Mobile Apps auf Cache und Synchronisation offline in der iOS-Anwendung"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-ios-mobile-app"></a>Offline-Synchronisierung für mobile iOS-Anwendung aktivieren

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Übersicht

Dieses Lernprogramm umfasst die offline Synchronisierungsfunktion Azure Mobile Apps für iOS. Offline synchronisieren können Endbenutzer eine mobile Anwendung interagieren&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;selbst wenn keine Verbindung zum Netzwerk. Änderungen werden in einer lokalen Datenbank gespeichert. Sobald das Gerät wieder online ist, werden diese Änderungen mit remote-Backend synchronisiert.

Ist dies die erste Version von Azure Mobile Apps, sollten Sie das Lernprogramm [iOS-App erstellen]abschließen. Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, müssen Sie Data Access Erweiterung Pakete zum Projekt hinzufügen. Weitere Informationen zu Server-Erweiterung Paketen finden Sie unter [mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Erfahren Sie mehr über die offline Synchronisierungsfunktion finden Sie [Offline Daten-Synchronisierung in Azure Mobile Apps].

## <a name="review-sync"></a>Überprüfen Sie den Clientcode synchronisieren

[IOS-App erstellen] Tutorial bereits heruntergeladene Clientprojekt enthält Code für offline-Synchronisation mit einer lokalen Core basierende Datenbank. In diesem Abschnitt wird eine Übersicht darüber, was bereits im Code Lernprogramms enthalten ist. Eine grundlegende Übersicht über die Funktion finden Sie unter [Offline Daten synchronisieren in Azure Mobile Apps].

Die offline-Daten synchronisieren Synchronisierung Azure Mobile Apps ermöglicht Benutzer interagieren mit einer lokalen Datenbank, wenn das Netzwerk nicht zugegriffen werden. Zum Verwenden dieser Funktionen in Ihrer Anwendung initialisieren Synchronisierungskontext des `MSClient` und einen lokalen Speicher. Verweisen Sie die Tabelle durch die `MSSyncTable` Schnittstelle.

1. Beachten Sie den Typ des Members in **QSTodoService.m** (Objective-C) oder **ToDoTableViewController.swift** (Swift) `syncTable` ist `MSSyncTable`. Offline synchronisieren verwendet diese Schnittstelle Sync Tabelle anstelle von `MSTable`. Wird eine Synchronisierungstabelle zum lokalen Store und mit remote-Backend mit expliziten Push- und Pull-Vorgänge nur synchronisiert werden.

    Verwenden Sie die Methode zum Abrufen eines Verweises auf eine Synchronisierungstabelle `syncTableWithName` auf `MSClient`. Offline synchronisieren Funktionen entfernen `tableWithName` statt.

2. Bevor Tabellenvorgänge ausgeführt werden können, muss der lokale Speicher initialisiert. Hier ist der entsprechende Code. 
    
    **Objective-C**:
    
    In der `QSTodoService.init` Methode:
    
    
            MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
            self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
    
    
    **Swift**:
    
    In der `ToDoTableViewController.viewDidLoad` Methode:
    
    
            let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
            let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
            self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
            client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
    

    Dies erstellt einen lokalen Speicher über die Benutzeroberfläche `MSCoreDataStore`, die in Mobile Apps SDK bereitgestellt wird. Sie können stattdessen bieten einem anderen lokalen Speicher durch Implementieren der `MSSyncContextDataSource` Protokoll. 
    
    Auch der erste Parameter der `MSSyncContext` Konflikt Ereignishandler angegeben. Da wir übergeben `nil`, den Standardhandler Konflikt, die Konflikte nicht erhalten.
    
3. Nun, wir die eigentliche Synchronisierung durchführen, und Daten von remote-Backend.

    **Objective-C**:
    
    `syncData`zunächst legt neue ändert, dann `pullData` zum Abrufen von Daten aus remote-Backend. Wiederum die Methode `pullData` neue Daten, die eine Abfrage entspricht:
    
    
            -(void)syncData:(QSCompletionBlock)completion
            {
                // push all changes in the sync context, then pull new data
                [self.client.syncContext pushWithCompletion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
                    [self pullData:completion];
                }];
            }
    
            -(void)pullData:(QSCompletionBlock)completion
            {
                MSQuery *query = [self.syncTable query];
    
                // Pulls data from the remote server into the local table.
                // We're pulling all items and filtering in the view
                // query ID is used for incremental sync
                [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
    
                    // Let the caller know that we have finished
                    if (completion != nil) {
                        dispatch_async(dispatch_get_main_queue(), completion);
                    }
                }];
            }
        
        
      **Swift**:
        
        
        func onRefresh(sender: UIRefreshControl!) {
            UIApplication.sharedApplication().networkActivityIndicatorVisible = true
            
            self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
                (error) -> Void in
                
                UIApplication.sharedApplication().networkActivityIndicatorVisible = false
                
                if error != nil {
                    // A real application would handle various errors like network conditions,
                    // server conflicts, etc via the MSSyncContextDelegate
                    print("Error: \(error!.description)")
                    
                    // We will just discard our changes and keep the servers copy for simplicity
                    if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                        for opError in opErrors {
                            print("Attempted operation to item \(opError.itemId)")
                            if (opError.operation == .Insert || opError.operation == .Delete) {
                                print("Insert/Delete, failed discarding changes")
                                opError.cancelOperationAndDiscardItemWithCompletion(nil)
                            } else {
                                print("Update failed, reverting to server's copy")
                                opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                            }
                        }
                    }
                }
                self.refreshControl?.endRefreshing()
            }
        } 
    
    
    In der Version Objective-C in `syncData`, rufen wir zuerst `pushWithCompletion` auf den Synchronisierungskontext. Diese Methode ist ein Mitglied der `MSSyncContext` (die Synchronisierung Tabelle selbst) denn diese Änderung für alle Tabellen werden. Nur Datensätze, die in irgendeiner Weise lokal (über CRUD-Vorgänge) geändert wurden, werden an den Server gesendet. Dann das Hilfsprogramm `pullData` aufgerufen wird, werden die Aufrufe `MSSyncTable.pullWithQuery` entfernte Daten abgerufen und in der lokalen Datenbank speichern.
    
    In der Swift-Version ist kein Aufruf von `pushWithCompletion`. Ist der Push-Vorgang nicht erforderlich war. Gibt ausstehende Änderungen im Synchronisierungskontext für die Tabelle, die einen Pushvorgang durchführt, gibt ziehen immer einen Push zuerst. Allerdings haben Sie mehr als eine Synchronisierungstabelle ist es am besten Push um sicherzustellen, dass alles auf Verknüpfte Tabellen konsistent ist explizit aufrufen.
    
    In Objective-C und Swift, die Methode `pullWithQuery` können Sie eine Abfrage zum Filtern der Datensätze abrufen möchten. In diesem Beispiel ruft die Abfrage nur alle Datensätze in der entfernten `TodoItem` Tabelle.
    
    Der zweite Parameter `pullWithQuery` Abfrage-ID, die für *inkrementelle Synchronisierung*verwendet wird. Inkrementelle Synchronisierung ruft nur Datensätze geändert seit der letzten Synchronisierung mit des Datensatzes `UpdatedAt` Zeitstempel (namens `updatedAt` im lokalen Speicher.) Die Abfrage-ID sollte eine beschreibende Zeichenfolge, die eindeutig für jede logische Abfrage in Ihrer Anwendung ist. Um inkrementelle Synchronisierung abmelden, übergeben `nil` als Abfrage-ID. Beachten Sie, dass dies möglicherweise ineffizient, da alle Datensätze auf Pull-Vorgang abgerufen werden.

5. Objective-C app synchronisiert Wenn wir ändern oder Daten hinzufügen, ein Benutzer führt die Aktion aktualisieren und starten. Swift app synchronisiert wird, wenn ein Benutzer die Aktion aktualisieren und starten. 

Da die app synchronisiert, wenn Daten geändert (Objective-C) oder der Anwendung starten (Objective-C und Swift), geht die Anwendung der Benutzer online ist. In einem anderen Abschnitt aktualisieren wir die Anwendung Benutzer bearbeiten können, auch wenn sie offline sind.

## <a name="review-core-data"></a>Zentrale Daten ansehen

Verwendung der wichtigsten Daten Offlinespeicher müssen Sie bestimmte Tabellen und Felder im Datenmodell definieren. Die Beispiel-app enthält bereits ein Datenmodell mit dem richtigen Format. In diesem Abschnitt gehen wir durch diese Tabellen und ihre Verwendung.

- Öffnen Sie **QSDataModel.xcdatamodeld**. Gibt es vier Tabellen – drei das SDK verwendet werden und eine Tabelle für die Todo Elemente selbst:     *MS_TableOperations: für die Verfolgung von Artikeln, die mit dem Server synchronisiert    * MS_TableOperationErrors: zum Nachverfolgen von Fehlern, die während der Synchronisierung von Offlinedateien auftreten     *MS_TableConfig: zum Nachverfolgen des Letzteres Zeit der letzten Synchronisierungsvorgang für alle Pullvorgänge aktualisiert    * TodoItem : Zum Speichern von Todo-Elementen. System Spalten **CreatedAt**, **UpdatedAt**und **Version** sind optional Systemeigenschaften.

>[AZURE.NOTE] Azure Mobile Apps SDK reserviert Spaltennamen, wird mit "**``**". Verwenden Sie nicht dieses Präfix auf Systemspalten, andernfalls die Spaltennamen geändert wenn remote-Backend verwenden.

- Bei der Synchronisation offline verwenden, müssen Sie die Systemtabellen wie folgt definieren.

    ### <a name="system-tables"></a>Systemtabellen

    **MS_TableOperations**

    ![][defining-core-data-tableoperations-entity]

  	| Attribut  |    Typ     |
  	|----------- |   ------    |
  	| ID         | Ganzzahl 64  |
  	| itemId     | Zeichenfolge      |
  	| Eigenschaften | Binäre Daten |
  	| Tabelle      | Zeichenfolge      |
  	| tableKind  | Ganze Zahl 16  |

    <br>**MS_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

  	| Attribut  |    Typ     |
  	|----------- |   ------    |
  	| ID         | Zeichenfolge      |
  	| operationId | Ganzzahl 64 |
  	| Eigenschaften | Binäre Daten |
  	| tableKind  | Ganze Zahl 16  |

    <br>**MS_TableConfig**

    ![][defining-core-data-tableconfig-entity]

  	| Attribut  |    Typ     |
  	|----------- |   ------    |
  	| ID         | Zeichenfolge      |
  	| Schlüssel        | Zeichenfolge      |
  	| Schlüsseltyp    | Ganzzahl 64  |
  	| Tabelle      | Zeichenfolge      |
  	| Wert      | Zeichenfolge      |

    ### <a name="data-table"></a>Datentabelle

    **TodoItem**

  	| Attribut    |  Typ   | Hinweis                                                   |
  	|-----------   |  ------ | -------------------------------------------------------|
  	| ID           | Zeichenfolge, die als erforderlich gekennzeichnet  | Primärschlüssel in remote-Speicher                            |
  	| Abschließen     | Boolescher Wert | Feld TODO                                        |
  	| Text         | Zeichenfolge  | Feld TODO                                        |
  	| createdAt | Datum    | (optional) wird CreatedAt System-Eigenschaft         |
  	| updatedAt | Datum    | (optional) wird UpdatedAt System-Eigenschaft         |
  	| Version   | Zeichenfolge  | zu Konflikten, wird Version verwendet (optional) |


## <a name="setup-sync"></a>Synchronisierungsverhalten der Anwendung ändern

In diesem Abschnitt ändern Sie die Anwendung, damit app-Start oder beim Einfügen und Aktualisieren von Elementen nicht synchronisiert wird, aber nur bei Bewegung Aktualisierungsschaltfläche ausgeführt wird.

**Objective-C**:

1. In **QSTodoListViewController.m**ändern **Spiele** entfernen Sie den Aufruf von `[self refresh]` am Ende der Methode. Nun die Daten nicht mit dem Server app-Start synchronisiert jedoch stattdessen wird der Inhalt des lokalen Informationsspeichers.

2. Ändern Sie **QSTodoService.m**, die Definition von `addItem` , dass nach dem Einfügen des Elements nicht synchronisiert wird. Entfernen Sie die `self syncData` blockieren und durch folgenden Wortlaut ersetzt:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. Ändern Sie die Definition des `completeItem` wie oben beschrieben; Entfernen die Block für `self syncData` und durch folgenden Wortlaut ersetzt:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

**Swift**:

1. In `viewDidLoad` in **ToDoTableViewController.swift**, kommentieren Sie diese beiden Zeilen zu synchronisieren auf Anwendung starten. Zum Zeitpunkt dieses Artikels schreiben aktualisiert Swift Todo app nicht den Dienst, wenn hinzufügt oder ein Element nur für app-Start abgeschlossen ist.

        self.refreshControl?.beginRefreshing()
        self.onRefresh(self.refreshControl)


## <a name="test-app"></a>Testen der Anwendung

In diesem Abschnitt werden Sie offline Szenario simuliert eine ungültige URL verbunden. Beim Hinzufügen von Daten werden werden im lokalen Datenspeicher von Core statt, sondern mobile Backend nicht synchronisiert.

1. Ändern Sie die Mobile Anwendung URL **QSTodoService.m** auf eine ungültige URL, und führen Sie die Anwendung erneut aus:

    **Objective-C** in QSTodoService.m:
    
            self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
    
    **Swift** in ToDoTableViewController.swift:

        let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")

2. Einige Aufgaben hinzufügen. Beenden Sie den Simulator (oder das Schließen der Anwendung) und neu starten. Überprüfen Sie, ob die Änderung beibehalten wurde.

3. Anzeigen des Inhalts der Remotetabelle TodoItem:

    + Node.js-Backend gehen [Azure-Portal](https://portal.azure.com/)und Mobile-App Backend finden Sie **Einfache Tabellen** > **TodoItem** an den Inhalt der `TodoItem` Tabelle.
    + Zeigen Sie für .NET Backend Inhaltsverzeichnis ein SQL-Tool wie SQL Server Management Studio oder einen anderen Client Fiddler oder Postbote an

    Überprüfen Sie, ob die neuen Elemente *nicht* mit dem Server synchronisiert wurde:

4. Die URL, die auf in **QSTodoService.m** und führen Sie die Anwendung erneut aus. Dieser Bewegung aktualisieren die Liste der Elemente ziehen. Sie sehen ein Drehfeld ausgeführt.

5. Die TodoItem Daten erneut anzeigen. Neue und geänderte TodoItems sollte nun angezeigt werden.

## <a name="summary"></a>Zusammenfassung

Um offline Synchronisierungsfeature unterstützt, verwendet die `MSSyncTable` Schnittstelle und initialisiert `MSClient.syncContext` mit einem lokalen Speicher. In diesem Fall wurde der lokale Speicher Core basierende Datenbank.

Bei einen lokalen Core-Datenspeicher verwenden, müssen Sie mehrere Tabellen mit den [richtigen Eigenschaften](#review-core-data)definieren.

Normalen CRUD-Operationen für Azure Mobile Apps arbeiten, als ob die app immer noch verbunden, aber alle im lokalen Speicher Vorgänge.

Wenn wir den lokalen Speicher mit dem Server synchronisiert, verwendet die `MSSyncTable.pullWithQuery`Methode.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Offline-Daten synchronisieren in Azure mobiler Apps]

* [Bewölkung: Offline synchronisieren in Azure Mobile Dienste] \(Hinweis: Videos auf Mobile Services ist jedoch offline synchronisieren funktioniert ähnlich wie in Azure Mobile Apps\)

<!-- URLs. -->


[IOS-App erstellen]: app-service-mobile-ios-get-started.md
[Offline-Daten synchronisieren in Azure mobiler Apps]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Bewölkung: Offline synchronisieren in Azure Mobile Dienste]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
