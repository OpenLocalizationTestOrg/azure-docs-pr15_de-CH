<properties
    pageTitle="Verwalten von Parallelität in Microsoft Azure-Speicher"
    description="Verwalten von Parallelität Blob, Warteschlange, Tabelle und Datei"
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jahogg"/>

# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Verwalten von Parallelität in Microsoft Azure-Speicher

## <a name="overview"></a>Übersicht

Moderne Internet-basierte Programme haben in der Regel mehrere Benutzer anzeigen und Aktualisieren von Daten gleichzeitig. Dies erfordert Entwickler genau überlegen, wie eine vorhersagbare Umgebung für die Endbenutzer besonders für Szenarien, in denen mehrere Benutzer dieselben Daten aktualisieren können. Es gibt drei primären Parallelität Strategien Entwickler normalerweise berücksichtigen:  


1.  Vollständige Parallelität – lesen Sie eine Anwendung ausführen, die ein Update als Teil der Aktualisierung prüft, wenn die Anwendung die Daten geändert hat letzten Daten. Beispielsweise stellen zwei Benutzer eine Wiki-Seite ein Update zur selben Seite müssen dann die Wiki-Plattform das zweite Update das erste Update – nicht überschrieben wird, dass beide Benutzer wissen, ob die Aktualisierung erfolgreich war. Diese Strategie wird häufig in ASP.NET-Webanwendungen verwendet.
2.  Eingeschränkte Parallelität – eine Anwendung auf Aktualisieren dauert eine Sperre für ein Objekt verhindert, dass andere Benutzer die Daten aktualisieren, bis die Sperre aufgehoben wird. Beispielsweise wird in einem Master/Slave Daten Replikation Szenario, in dem nur Updates ausführen, Master enthalten in der Regel eine exklusive Sperre für einen längeren Zeitraum Daten, um sicherzustellen, dass kein anderer Benutzer aktualisiert werden können.
3.  Letzte Writer Wins – ein Ansatz, alle Aktualisierungsoperationen ohne Überprüfung, ob eine andere Anwendung die Daten zuerst seit der Anwendung wurde die Daten lesen. Diese Strategie (oder eine formale Strategie) wird gewöhnlich verwendet, in denen Daten so partitioniert werden, dass keine Gefahr, dass mehrere Benutzer dieselben Daten zugreifen. Es kann auch nützlich sein, kurzlebige Daten verarbeitet werden.  

Dieser Artikel enthält einen Überblick darüber, wie die Azure-Speicherplattform Entwicklung vereinfacht durch erstklassigen Support für alle drei Strategien Parallelität.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure-Speicher – vereinfacht die Cloud-Entwicklung
Der Azure-Speicher-Dienst unterstützt alle drei Strategien zwar unverwechselbar bieten vollständige Unterstützung für vollständige und eingeschränkte Parallelität, da es entwickelt wurde, zu starke Konsistenzmodell, die garantiert, dass der Speicherdienst schreibt Daten einfügen oder Aktualisierungsvorgang alle weiteren Zugriffe auf Daten das neueste Update angezeigt. Speicherplattformen, mit denen ein Modell eventual Consistency haben eine Verzögerung zwischen, wenn ein Schreibvorgang durchgeführt wird, durch einen Benutzer und die aktualisierten Daten von anderen Benutzern so kompliziert Entwicklung von Clientanwendungen, um Inkonsistenzen verhindern Endbenutzer sehen.  

Neben der Auswahl einer geeigneten Parallelität Strategie beachten Entwickler auch wie eine Änderungen – insbesondere auf das gleiche Objekt Transaktionen isoliert. Azure-Speicher-Dienst verwendet die Snapshot-Isolation zu Lesevorgänge gleichzeitig Schreibvorgänge in einer Partition erfolgen. Im Gegensatz zu anderen Isolierungsebenen garantiert die Snapshot-Isolation alle liest einen konsistenten Snapshot der Daten angezeigt, obwohl Updates auftreten – im Wesentlichen durch Rückgabe der letzten Commit Werte ein Update Transaktion verarbeitet wird.  

## <a name="managing-concurrency-in-blob-storage"></a>Verwalten von Parallelität im BLOB-Speicher
Sie können um optimistisch oder pessimistisch Parallelitätsmodellen Blobs zu Containern im Blob-Dienst zu verwenden. Wenn Sie nicht explizit Strategie letzten schreibt Wins angeben ist die Standardeinstellung.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Vollständige Parallelität für Blobs und Container  

Der Speicherdienst jedes Objekt Bezeichner zugewiesen. Dieser Bezeichner wird jedes Mal aktualisiert ein Aktualisierungsvorgang für ein Objekt ausgeführt wird. Der Bezeichner wird zurück an den Client als Teil einer HTTP GET-Antwort mit der ETag (Entitätstag) Header, der das HTTP-Protokoll definiert ist Durchführen eines Updates für ein Objekt kann ursprünglichen ETag mit bedingten Header senden, um sicherzustellen, dass ein Update nur dann stattfindet, wenn eine bestimmte Bedingung erfüllt ist die Bedingung ist in diesem Fall eine "If-Match" Header erfordert Storage Service um sicherzustellen, dass der Wert in der Aktualisierungsanfrage angegeben ETag der Speicherdienst gespeicherten identisch ist.  

Die Gliederung dieses Prozesses lautet wie folgt:  

1.  Abrufen eines BLOBs aus der Speicherdienst, die Antwort enthält einen ETag-Header-Wert, der die aktuelle Version des Objekts in der Speicherdienst identifiziert.
2.  Beim Aktualisieren des BLOBs enthalten Sie den ETag-Wert erhalten Sie in Schritt 1 die bedingte **If-Match** -Header der Anforderung an den Dienst senden.
3.  Der Dienst vergleicht den ETag-Wert in der Anforderung mit dem aktuellen ETag-Wert des Blob.
4.  Ist der aktuelle ETag-Wert des Blob eine andere Version als das ETag bedingte **If-Match** -Header in der Anforderung, zurückgibt der Dienst 412 Fehler an den Client. Dies bedeutet, dass ein anderer Prozess Blob aktualisiert hat, da der Client abgerufen an den Client.
5.  Ist der aktuelle ETag-Wert des Blob dieselbe Version wie das ETag bedingte **If-Match** -Header in der Anforderung wird der Dienst führt die angeforderte Operation und aktualisiert den aktuellen ETag-Wert des BLOBs an, dass eine neue Version erstellt wurde.  

Der folgende C#-Codeausschnitt, (mithilfe der Speicher-Clientbibliothek 4.2.0) zeigt eine einfache aus den Eigenschaften eines Blob zugegriffen wird, die zuvor abgerufen oder eingefügt wurde ETag-Wert wird zum Erstellen einer **If-Match AccessCondition** basiert. Dann mithilfe des **AccessCondition** Objekts Wenn sie Blob aktualisieren: **AccessCondition** -Objekt der Anforderung **If-Match** -Header hinzugefügt. Wenn ein anderer Prozess das Blob aktualisiert, gibt der BLOB-Dienst eine Statusnachricht HTTP 412 (Vorbedingung Fehler). Das vollständige Beispiel herunterladen: [Managing Concurrency Azure-Speicher verwenden](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

    // Retrieve the ETag from the newly created blob
    // Etag is already populated as UploadText should cause a PUT Blob call
    // to storage blob service which returns the etag in response.
    string orignalETag = blockBlob.Properties.ETag;

    // This code simulates an update by a third party.
    string helloText = "Blob updated by a third party.";

    // No etag, provided so orignal blob is overwritten (thus generating a new etag)
    blockBlob.UploadText(helloText);
    Console.WriteLine("Blob updated. Updated ETag = {0}",
    blockBlob.Properties.ETag);

    // Now try to update the blob using the orignal ETag provided when the blob was created
    try
    {
        Console.WriteLine("Trying to update blob using orignal etag to generate if-match access condition");
        blockBlob.UploadText(helloText,accessCondition:
        AccessCondition.GenerateIfMatchCondition(orignalETag));
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        {
            Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
            // TODO: client can decide on how it wants to handle the 3rd party updated content.
        }
        else
            throw;
    }  

Der Speicherdienst bietet auch Unterstützung für zusätzliche bedingten Header wie **If-Modified-Since**, **If-Unmodified-Since** **If-None-Match** sowie Kombinationen daraus. Weitere Informationen finden Sie auf MSDN [Bedingten Header für BLOB-Dienst angeben](http://msdn.microsoft.com/library/azure/dd179371.aspx) .  

In der folgenden Tabelle zusammengefasst Container Vorgänge bedingten Header wie **If-Match** in der Anforderung akzeptieren und einen ETag-Wert in der Antwort zurückgeben.  

| Vorgang                | Container ETag zurück | Bedingte Header akzeptiert |
|:-------------------------|:-----------------------------|:----------------------------|
| Container erstellen         | Ja                          | Nein                          |
| Abrufen von Eigenschaften | Ja                          | Nein                          |
| Container-Metadaten erhalten   | Ja                          | Nein                          |
| Container-Metadaten   | Ja                          | Ja                         |
| Container-ACL abrufen        | Ja                          | Nein                          |
| Container-ACL festlegen        | Ja                          | Ja (*)                     |
| Container löschen         | Nein                           | Ja                         |
| Leasing-Container          | Ja                          | Ja                         |
| Liste Blobs               | Nein                           | Nein                          |

(*) SetContainerACL definierten Berechtigungen werden zwischengespeichert und Updates diese Berechtigungen werden 30 Sekunden übertragen der Zeitraum Updates nicht unbedingt übereinstimmen.  

Die folgenden Tabelle werden BLOB-Vorgänge bedingten Header wie **If-Match** in der Anforderung akzeptieren und einen ETag-Wert in der Antwort zurückgeben.

| Vorgang           | ETag zurück | Bedingte Header akzeptiert           |
|:--------------------|:-------------------|:--------------------------------------|
| BLOBs speichern            | Ja                | Ja                                   |
| Abrufen von BLOBs            | Ja                | Ja                                   |
| BLOB-Eigenschaften | Ja                | Ja                                   |
| BLOB-Eigenschaften festlegen | Ja                | Ja                                   |
| Abrufen von BLOB-Metadaten   | Ja                | Ja                                   |
| Blob-Metadaten   | Ja                | Ja                                   |
| Lease Blob (*)      | Ja                | Ja                                   |
| Snapshot-Blob       | Ja                | Ja                                   |
| Kopieren des BLOBs           | Ja                | Ja (für Quell- und Ziel-Blob) |
| Copy Blob Abbrechen     | Nein                 | Nein                                    |
| Blob löschen         | Nein                 | Ja                                   |
| Block einfügen           | Nein                 | Nein                                    |
| Blockieren Liste      | Ja                | Ja                                   |
| Sperrliste abrufen      | Ja                | Nein                                    |
| Seite            | Ja                | Ja                                   |
| Get Seitenbereiche     | Ja                | Ja                                   |


(*) Das ETag Blob Lease Blob nicht geändert.  

### <a name="pessimistic-concurrency-for-blobs"></a>Eingeschränkte Parallelität für blobs
Um ein Blob für exklusive Verwendung gesperrt werden, erwerben Sie eine [Lease](http://msdn.microsoft.com/library/azure/ee691972.aspx) auf. Beim Abrufen einer Leases Sie angeben, wie lange die Lease benötigen: Dies kann für 15 bis 60 Sekunden oder unendlich die exklusive Sperre beträgt. Eine begrenzte Lease verlängern erneuern, und Sie können jeder Lease freigeben, wenn Sie fertig sind. Der BLOB-Dienst freigegeben automatisch begrenzte Leases beim ablaufen.  

Leases können verschiedene Strategien unterstützt werden, einschließlich nur Schreiben / Lesen, exklusiven Schreibzugriff freigegeben und exklusive Lesen Schreiben freigegebenen exklusiv lesen. Existiert eine Lease erzwingt der Speicherdienst exklusive schreibt (platzieren, festlegen und löschen) Exklusivität für Lesevorgänge sicherstellen muss der Entwickler sicherstellen, dass alle Clientanwendungen eine Lease-ID und diese nur einen einzigen Client verwenden jedoch eine gültige Lease-ID hat. Lesevorgänge, die keine Lease-ID Ergebnis in freigegebenen liest.  

Der folgende C#-Codeausschnitt zeigt ein Beispiel der exklusive Lease für 30 Sekunden auf ein Blob abrufen, Aktualisierung des Inhalts des BLOBs und Freigabe der Lease. Besteht bereits eine gültige Lease für das Blob Wenn Sie versuchen, eine neue Lease erwerben, gibt der BLOB-Dienst eine "HTTP (409) Conflict" Status Ergebnis zurück. Der folgende Ausschnitt verwendet ein Objekt **AccessCondition** Leasing-Informationen kapseln, wenn das Blob in der Speicherdienst aktualisieren anfordert.  Das vollständige Beispiel herunterladen: [Managing Concurrency Azure-Speicher verwenden](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    // Acquire lease for 15 seconds
    string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
    Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

    // Update blob using lease. This operation will succeed
    const string helloText = "Blob updated";
    var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
    blockBlob.UploadText(helloText, accessCondition: accessCondition);
    Console.WriteLine("Blob updated using an exclusive lease");

    //Simulate third party update to blob without lease
    try
    {
        // Below operation will fail as no valid lease provided
        Console.WriteLine("Trying to update blob without valid lease");
        blockBlob.UploadText("Update without lease, will fail");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
            Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
        else
            throw;
    }  

Wenn Sie einen Schreibvorgang für eine Lease Blob versuchen, ohne die Lease-ID, schlägt die Anforderung mit einem 412 Fehler. Beachten Sie, dass wenn die Lease abläuft, bevor die **UploadText** -Methode übergeben Sie weiterhin die Lease-ID, die Anforderung auch **412** Fehler fehl. Weitere Informationen zum Ablauf der Leasedauer und Leasing-Ids verwalten Dokumentation der [Lease Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx) REST.  

Leases können Blob folgenden eingeschränkte Parallelität verwalten:  


-   BLOBs speichern
-   Abrufen von BLOBs
-   BLOB-Eigenschaften
-   BLOB-Eigenschaften festlegen
-   Abrufen von BLOB-Metadaten
-   Blob-Metadaten
-   Blob löschen
-   Block einfügen
-   Blockieren Liste
-   Sperrliste abrufen
-   Seite
-   Get Seitenbereiche
-   Snapshot Blob - Lease ID existiert eine Lease optional
-   Kopieren des BLOBs - Lease müssen ID existiert eine Lease auf dem Ziel-blob
-   Abbruch Copy Blob - Lease-ID erforderlich, existiert eine unendliche Lease auf dem Ziel-blob
-   Leasing-Blob  

### <a name="pessimistic-concurrency-for-containers"></a>Eingeschränkte Parallelität für Container
Leases für Container ermöglichen die Synchronisierung Strategien für Blobs unterstützt (exklusive Schreiben / Lesen, exklusiven Schreibzugriff freigegeben und exklusive Lesen Schreiben freigegebenen exklusiv lesen) jedoch im Gegensatz zu Blobs der Speicherdienst nur Exklusivität für Löschvorgänge erzwingt. Um einen Container mit aktive Leases löschen, muss ein Client die aktive Lease-ID mit der Delete-Anforderung enthalten. Andere Container Vorgänge erfolgreich geleasten Container ohne die Lease-ID in diesem Fall sind Vorgänge freigegeben. Wenn Exklusivität des (Put oder Set) oder Lesevorgänge erforderlich ist sollten Sie Entwickler sicherstellen, dass alle Clients nur jeweils einen Client eine gültige Lease-ID eine Lease-ID und die verwenden  

Die folgenden Container können Leases eingeschränkte Parallelität verwalten:  

-   Container löschen
-   Abrufen von Eigenschaften
-   Container-Metadaten erhalten
-   Container-Metadaten
-   Container-ACL abrufen
-   Container-ACL festlegen
-   Leasing-Container  

Weitere Informationen finden Sie unter:  

- [Bedingte Header angeben für Dienstvorgänge Blob](http://msdn.microsoft.com/library/azure/dd179371.aspx)
- [Leasing-Container](http://msdn.microsoft.com/library/azure/jj159103.aspx)
- [Leasing-Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>Verwalten von Parallelität in den Dienst
Der Dienst verwendet vollständige Parallelität als Standardverhalten überprüft, wenn Sie Entitäten im Gegensatz zu den BLOB-Dienst arbeiten explizit Erstellungsort müssen vollständige Parallelität überprüft werden soll. Der Unterschied zwischen der Tabelle und Blob ist nur das Parallelitätsverhalten von Entitäten verwalten während der BLOB-Dienst die Parallelität von Containern und Blobs verwalten können.  

Vollständigen Parallelität und überprüfen, ob ein anderer Prozess seit vom tabellenspeicherdienst abgerufen eine Entität geändert, können Sie den ETag-Wert erhalten, wenn der Dienst eine Entität zurückgibt. Die Gliederung dieses Prozesses lautet wie folgt:  

1.  Abrufen einer Entität aus der tabellenspeicherdienst, die Antwort enthält einen ETag-Wert, der den aktuellen Bezeichner dieser Entität im Speicherdienst identifiziert.
2.  Beim Aktualisieren der Entität enthalten Sie den ETag-Wert erhalten Sie in Schritt 1 den obligatorischen **If-Match** -Header der Anforderung an den Dienst senden.
3.  Der Dienst vergleicht den ETag-Wert in der Anforderung mit dem aktuellen ETag-Wert der Entität.
4.  Ist der aktuelle ETag-Wert der Entität anders als das ETag obligatorisch **If-Match** -Header in der Anforderung, zurückgibt der Dienst 412 Fehler an den Client. Dies bedeutet, dass ein anderer Prozess die Entität wurde, seit der Client Abrufen an den Client.
5.  Wenn aktuelle ETag-Wert der Entität das ETag obligatorisch **If-Match** -Header in der Anforderung entspricht oder der **If-Match** -Header enthält das Platzhalterzeichen (*), wird der Dienst führt die angeforderte Operation und aktualisiert den aktuellen ETag-Wert der Entität, die aktualisiert wurde.  

Beachten Sie, dass im Gegensatz zu BLOB-Dienst der Dienst Client hinzufügen einen **If-Match** -Header in Aktualisierungsanfragen erfordert. Es ist jedoch möglich, eine unbedingte Aktualisierungsstrategie (letzte Writer Wins) und Parallelität umgehen, setzt der Client den **If-Match** -Header dem Platzhalterzeichen (*) in der Anforderung.  

Der folgende C#-Codeausschnitt zeigt eine Kundenentität, die entweder bereits erstellten oder Abrufen ihrer e-Mail-Adresse aktualisiert. Die erste einfügen oder Vorgang speichert den ETag-Wert in das Customer-Objekt abrufen und da im Beispiel dieselbe Objektinstanz verwendet den Ersetzungsvorgang ausgeführt, es sendet automatisch den ETag-Wert an die Tabelle Service Aktivieren des Dienstes für Parallelität überprüfen. Wenn ein anderer Prozess die Entität in Tabellenspeicher aktualisiert, gibt der Dienst eine Statusnachricht HTTP 412 (Vorbedingung Fehler).  Das vollständige Beispiel herunterladen: [Managing Concurrency Azure-Speicher verwenden](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    try
    {
        customer.Email = "updatedEmail@contoso.org";
        TableOperation replaceCustomer = TableOperation.Replace(customer);
        customerTable.Execute(replaceCustomer);
        Console.WriteLine("Replace operation succeeded.");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == 412)
            Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
        else
            throw;
    }  

Deaktivieren explizit die Überprüfung auf Parallelität, legen Sie die **ETag** -Eigenschaft des Objekts **Mitarbeiter** "*" vor den Ersetzungsvorgang ausführen.  

    customer.ETag = "*";  

Die folgende Tabelle zeigt, wie die Tabelle Entität ETag-Werte verwenden:

| Vorgang                | ETag zurück | Anforderungsheader If-Match erfordert |
|:-------------------------|:-------------------|:---------------------------------|
| Abfrage Entitäten           | Ja                | Nein                               |
| Element einfügen            | Ja                | Nein                               |
| Entität aktualisieren            | Ja                | Ja                              |
| Entität zusammenführen             | Ja                | Ja                              |
| Entität löschen            | Nein                 | Ja                              |
| Einfügen oder ersetzen Entität | Ja                | Nein                               |
| Einfügen oder Entität zusammenführen   | Ja                | Nein                               |

Hinweis das **Einfügen oder ersetzen** und **Einfügen oder Zusammenführen** Operationen *nicht* Parallelität Kontrollen durchgeführt werden, da sie einen ETag-Wert nicht an den Dienst senden.  

Im Allgemeinen sollten Entwickler Tabellen Parallelität hängen bei skalierbaren Anwendung. Pessimistische Sperren benötigt wird, können Entwickler eine Möglichkeit nehmen wird Zugriff auf Tabellen ein angegebenen BLOBs für jede Tabelle und versuchen, eine Lease für das Blob vor der Tabelle. Dieser Ansatz erfordert die Anwendung alle Datenpfade Zugriff Lease vor auf die Tabelle zu erhalten. Beachten Sie auch ist die minimale Leasedauer 15 Sekunden erfordert sorgfältige skalierbar.  

Weitere Informationen finden Sie unter:  

- [Auf Einheiten](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Verwalten von Parallelität in der Warteschlangendienst
Die Parallelität ist ein Problem im Message Queueing-Dienst ist, in dem mehrere Clients Nachrichten aus einer Warteschlange abrufen. Wenn eine Nachricht aus der Warteschlange abgerufen wird, enthält die Antwort die Nachricht und einem pop Eingang-Wert erforderlich ist, um die Nachricht zu löschen. Die Nachricht wird nicht automatisch aus der Warteschlange gelöscht nach abgerufen wurde, es jedoch nicht für andere Clients sichtbar für den Visibilitytimeout-Parameter angegebene Zeitintervall. Der Client, der die Nachricht empfängt soll die Meldung verarbeitet wurde und vor der TimeNextVisible angegebenen Element der Antwort berechnet basierend auf dem Wert des Parameters Visibilitytimeout löschen. Der Wert der Visibilitytimeout wird der Zeit hinzugefügt an dem die Meldung abgerufen wird, um den Wert des TimeNextVisible bestimmen.  

Der Warteschlangendienst keinen Support für optimistisch oder pessimistisch Parallelität und für diesen Grund Clients, die Verarbeitung von Nachrichten aus einer Warteschlange abgerufen sollten Nachrichten Idempotent zu verarbeitet werden. Eine letzte Writer WINS-Strategie wird für Aktualisierungsvorgänge wie SetQueueServiceProperties, SetQueueMetaData, SetQueueACL und UpdateMessage verwendet.  

Weitere Informationen finden Sie unter:  

- [Warteschlange Service REST-API](http://msdn.microsoft.com/library/azure/dd179363.aspx)
- [Nachrichten abrufen](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>Verwalten von Parallelität in den Dienst
Der Dienst kann mit zwei verschiedenen Protokolle Endpunkte – SMB und REST zugegriffen werden. Der REST-Dienst keinen Support für Parallelität oder pessimistische Sperren und alle Updates Folgen einer letzten Writer WINS-Strategie. SMB-Clients, die Freigaben zu mounten profitieren Datei sperren Mechanismen zum Verwalten des Zugriffs auf freigegebene Dateien, einschließlich der Möglichkeit, führen Sie pessimistische Sperren. Wenn SMB-Client eine Datei öffnet, wird der Zugriff auf und die Freigabe Modus. Festlegen einer Option Dateizugriff "Write" oder "Schreibgeschützt" mit einem Dateifreigabe-Modus von "None" führt die Datei vom SMB-Client gesperrt werden, bis die Datei geschlossen ist. Wenn REST einer Datei Vorgang wird SMB-Client die Datei gesperrt hat wird der REST-Dienst Statuscode 409 (Konflikt) mit dem Fehlercode SharingViolation zurückgegeben.  

Beim SMB-Client einer Datei zum Löschen öffnen markiert die Datei als ausstehender Löschvorgang bis SMB-Client geöffnete Handles für die Datei geschlossen werden. Während eine ausstehende löschen markiert ist, wird jede Operation REST der Datei Statuscode 409 (Konflikt) mit dem Fehlercode SMBDeletePending zurück. Statuscode 404 (nicht gefunden) wird nicht zurückgegeben, da der SMB-Client ausstehende Löschkennzeichen vor dem Schließen der Datei entfernt werden. In anderen Worten Statuscode 404 (nicht gefunden) nur erwartet wird, wenn die Datei entfernt wurde. Beachten Sie, dass eine Datei in einem SMB ausstehen löschen ist sie nicht in den Ergebnissen Dateien eingeschlossen wird. Beachten Sie, dass der REST löschen Datei- und REST löschen atomar engagieren und löschen aussteht führen.  

Weitere Informationen finden Sie unter:  

- [Verwalten von Datei sperren](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Zusammenfassung und nächste Schritte
Microsoft Azure Storage-Diensts wurde entwickelt, um die Bedürfnisse der komplexesten online-Applikationen ohne Entwickler oder Schlüssel Designannahmen wie Parallelität und die Datenkonsistenz, die sie mittlerweile selbstverständlich überdenken.  

Für vollständigen Anwendung in diesem Blog:  

- [Verwalten von Parallelität mit Azure Storage - Beispiel](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Weitere Informationen zu Azure-Speicher finden Sie unter:  

- [Microsoft Azure Storage-Startseite](https://azure.microsoft.com/services/storage/)
- [Einführung in Azure-Speicher](storage-introduction.md)
- Erste Schritte für [Blob](storage-dotnet-how-to-use-blobs.md), [Tabelle](storage-dotnet-how-to-use-tables.md), [Warteschlangen](storage-dotnet-how-to-use-queues.md)und [Dateien](storage-dotnet-how-to-use-files.md) Speicher
- Storage-Architektur – [Azure-Speicher: eine hochverfügbare Cloud-Speicherservice mit starker Konsistenz](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
