<properties
   pageTitle="Mit mehreren Regionen DocumentDB | Microsoft Azure"
   description="Erfahren Sie, wie der Zugriff auf Daten in mehreren Regionen von Azure DocumentDB vollständig verwaltete NoSQL-Datenbankdienst."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="kipandya"/>
   
# <a name="developing-with-multi-region-documentdb-accounts"></a>Entwickeln mit mehreren DocumentDB Konten

> [AZURE.NOTE] Globales DocumentDB Datenbanken ist allgemein verfügbar und für alle neu erstellten DocumentDB-Konten automatisch aktiviert. Wir arbeiten daran, globale Verteilerlisten für alle vorhandenen Konten aktivieren, aber in der Zwischenzeit sollten globale Verteilerlisten für Ihr Konto aktiviert [wenden](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , und wir werden aktivieren sie Sie jetzt.

Um [globale Verteilerlisten](documentdb-distribute-data-globally.md)nutzen können Clientanwendungen geordnete Einstellung Liste der Regionen Dokument Operationen verwendet werden. Dies erfolgt durch die Verbindungsrichtlinie festlegen. Basierend auf Kontokonfiguration Azure DocumentDB, regionaler Verfügbarkeit und die Liste der bevorzugten angegeben werden optimale Endpunkt ausgewählt vom SDK schreiben und Lesevorgänge. 

Diese Liste wird beim Initialisieren einer Verbindungs mit dem DocumentDB-Client SDKs angegeben. Die SDKs akzeptieren einen optionalen Parameter "PreferredLocations", eine geordnete Liste von Azure-Regionen.

Das SDK senden automatisch alle Schreibvorgänge auf den aktuellen Bereich schreiben. 

Alle Lesevorgänge werden an den ersten verfügbaren Bereich in der Liste PreferredLocations gesendet. Wenn die Anforderung fehlschlägt, wird Client nach unten zum nächsten Bereich fehl, usw. 

Der SDKs nur auf versucht Client Lesen im PreferredLocations aus. So werden z. B. das Konto verfügbar in drei Bereiche jedoch der Client gibt zwei nicht schreiben nur für PreferredLocations, dann keine Lesevorgänge aus der Region Schreiben bei Failover serviert.

Die Anwendung überprüfen den aktuellen Endpunkt schreiben und Lesen Endpunkt vom SDK von zwei Eigenschaften WriteEndpoint und ReadEndpoint verfügbar in SDK Version 1.8 Überprüfen ausgewählt. 

PreferredLocations-Eigenschaft nicht festgelegt ist, werden alle Anfragen aus dem aktuellen schreiben zugestellt. 


## <a name="net-sdk"></a>.NET SDK
Das SDK kann ohne Code verwendet werden. In diesem Fall, das SDK automatisch weist beide liest und schreibt in den aktuellen Bereich schreiben. 

Version 1.8 und .NET SDK wurde der ConnectionPolicy-Parameter für den Konstruktor "documentclient" eine Eigenschaft namens Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Diese Eigenschaft ist vom Typ Auflistung `<string>` und eine Liste der Namen der Region enthalten. Die Werte werden pro Spalte Bereichsname [Azure Regionen] formatiert [ regions] Seite ohne Leerzeichen vor oder nach dem ersten und letzten Zeichens bzw..

Aktuelle schreiben und Lesen Sie Endpunkte werden in DocumentClient.WriteEndpoint und DocumentClient.ReadEndpoint.

> [AZURE.NOTE] Die URLs für die Endpunkte sollten nicht als langlebige Konstanten betrachtet. Der Dienst kann diese jederzeit aktualisieren. Das SDK behandelt diese Änderung automatisch.

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

    //Setting read region selection preference 
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB 
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript und Python-SDKs
Das SDK kann ohne Code verwendet werden. In diesem Fall direkt das SDK Lese- und Schreibvorgänge auf den aktuellen Bereich schreiben. 

In Version 1.8 und jedes SDK später aufgerufen ConnectionPolicy Parameter für den Konstruktor "documentclient" eine neue Eigenschaft DocumentClient.ConnectionPolicy.PreferredLocations. Dieser Parameter ist ein Zeichenfolgenarray, das eine von Bereichsnamen Liste. Die Namen werden pro Spalte Bereichsname in [Azure Regionen] formatiert [ regions] Seite. Sie können auch vordefinierten Konstanten im Komfort-Objekt AzureDocuments.Regions

Aktuelle schreiben und Lesen Sie Endpunkte werden in DocumentClient.getWriteEndpoint und DocumentClient.getReadEndpoint.

> [AZURE.NOTE] Die URLs für die Endpunkte sollten nicht als langlebige Konstanten betrachtet. Der Dienst kann diese jederzeit aktualisieren. Das SDK wird diese Änderung automatisch behandelt.

Es folgt ein Codebeispiel für NodeJS-Javascript. Python und Java folgen das gleiche Muster.

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();
    
    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];
    
    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>REST 
Nachdem ein Konto in mehreren Regionen zur Verfügung gestellt wurde, können Clients Durchführen einer GET-Anforderung auf der folgenden URI seine Verfügbarkeit Abfragen.

    https://{databaseaccount}.documents.azure.com/

Der Service liefert eine Liste der Regionen und ihre entsprechenden DocumentDB Endpunkt URIs für Replikate. Der aktuelle Bereich schreiben wird in der Antwort angezeigt. Der Client kann den entsprechenden Endpunkt für alle weiteren Anfragen REST API wie folgt wählen.

Beispielantwort

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


-   Alle PUT, POST und DELETE-Anfragen müssen den angegebenen URI schreiben wechseln
-   Ruft alle und sonstige Anfragen schreibgeschützt (z. B. Abfragen) können jeder Endpunkt des Clients Wahl gehen

Schreiben Sie Anfragen an schreibgeschützte Bereiche mit HTTP-Fehler 403 ("verboten") fehl.

Ändert der Bereich schreiben nach der Client-Erkennungsphase fehl nachfolgenden Schreibvorgang in den vorherigen Bereich Schreiben mit HTTP-Fehler 403 ("verboten"). Der Client sollte die Liste der Regionen wieder zu Bereich aktualisierte schreiben dann.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über das Verteilen von Daten mit DocumentDB in den folgenden Artikeln:

- [Daten Sie mit DocumentDB](documentdb-distribute-data-globally.md)
- [Konsistenzebenen](documentdb-consistency-levels.md)
- [Funktionsweise der Durchsatz mit mehreren Bereichen](documentdb-manage.md#how-throughput-works-with-multiple-regions)
- [Fügen Sie Bereiche mit Azure-Portal hinzu](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/ 
