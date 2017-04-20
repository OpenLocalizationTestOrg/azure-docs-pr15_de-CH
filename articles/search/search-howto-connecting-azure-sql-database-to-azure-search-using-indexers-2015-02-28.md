<properties 
    pageTitle="Azure Search Indexer mit SQL Azure-Datenbank mit | Microsoft Azure | Indexer" 
    description="Erfahren Sie, wie Daten von Azure SQL-Datenbank einen Indexer mit Azure Suchindex abrufen." 
    services="search" 
    documentationCenter="" 
    authors="chaosrealm" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="05/26/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Azure Search Indexer mit Azure SQL-Datenbank herstellen

Azure-Suchdienst ist eine gehostete Cloud-Suchdienst, der bieten einer hervorragende Umgebung erleichtert. Bevor Sie suchen können, müssen Sie Azure Search Index mit Daten zu füllen. Wenn die Daten in einer SQL Azure-Datenbank befindet, automatisieren neue **Azure Search Indexer für Azure SQL-Datenbank** (oder kurz **SQL Azure Indexer** ) in Azure Suche der Indizierung zu. Dies bedeutet, dass Sie weniger Code schreiben und weniger Infrastruktur zu kümmern.

Indexer arbeiten nur mit Azure SQL Azure VMs und [Azure DocumentDB](../documentdb/documentdb-search-indexer.md)SQL Server. In diesem Artikel konzentrieren wir uns auf Indexer, der Azure SQL-Datenbank arbeiten. Möchten Sie finden Unterstützung für zusätzliche Datenquellen, geben Sie Ihr Feedback [Bewertungsportal Azure suchen](https://feedback.azure.com/forums/263029-azure-search/).

In diesem Artikel behandelt Indexer verwenden, aber wir müssen auch Drilldown Features und Verhaltensweisen, die nur mit SQL-Datenbanken (z. B. integrierte Versionsvergleich) verfügbar sind.

## <a name="indexers-and-data-sources"></a>Indexer und Datenquellen

Einrichten und Konfigurieren eines Azure SQL-Indexers können Sie [Azure Suche REST-API](http://go.microsoft.com/fwlink/p/?LinkID=528173) zum Erstellen und Verwalten von **Indexer** und **Datenquellen**aufrufen. 

Sie können auch der [Indexer-Klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.indexer.aspx) in [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)oder Datenimport-Assistenten in [Azure-Portal](https://portal.azure.com) erstellen und Planen eines Indexers.

Eine **Datenquelle** gibt die Daten für den index, Anmeldeinformationen Zugriff auf Daten und Richtlinien, die Azure Suche ändert Daten (neue geändert oder gelöschte Zeilen) effizient zu aktivieren. Es ist als unabhängige Ressource definiert, sodass sie durch mehrere Indexer verwendet werden kann.

Ein **Indexer** ist eine Ressource, mit dem Ziel Suchindizes Datenquellen verbindet. Indexer wird folgendermaßen verwendet:
 
- Führen Sie eine einmalige Kopie der Daten zum Auffüllen des Indexes.
- Aktualisieren eines Indexes mit der Datenquelle auf einem Zeitplan.
- Führen Sie bei Bedarf einen Index nach Bedarf aktualisieren. 

## <a name="when-to-use-azure-sql-indexer"></a>SQL Azure Indexer verwenden

Je nach Faktoren für Ihre Daten mit SQL Azure Indexer kann oder nicht. Wenn Ihre Daten folgende passt, können Sie SQL Azure Indexer: 

- Die Daten stammen aus einer einzelnen Tabelle oder Ansicht
    - Wenn die Daten über mehrere Tabellen verteilt sind, können Sie eine Ansicht erstellen und verwenden Sie diese Ansicht mit der Indexer. Bedenken Sie jedoch, wenn Sie eine Ansicht verwenden Sie SQL Server integrierte Ermittlung verwenden können. In diesem Abschnitt Weitere Details anzeigen 
- Indexer in der Datenquelle verwendeten Datentypen unterstützt. Die meisten aber nicht von den SQL unterstützt werden. Details finden Sie unter [Zuordnen von Datentypen in Azure suchen](http://go.microsoft.com/fwlink/p/?LinkID=528105). 
- Nicht müssen Sie Echtzeit-Updates auf den Index, wenn eine Zeile ändert nahe. 
    - Der Indexer kann die Tabelle höchstens 5 Minuten erneut indizieren. Sollten Ihre Daten häufig ändern und die Änderung in den Index innerhalb von Sekunden oder Minuten einzelne berücksichtigt, empfehlen wir [Azure Suche Index API](https://msdn.microsoft.com/library/azure/dn798930.aspx) direkt verwenden. 
- Wenn Sie eine große Datenmenge und Ausführen der Indexer Zeitplan Schema ermöglicht effizient identifizieren geändert (und ggf. gelöscht) Zeilen. Weitere Informationen finden Sie unter "Aufnahme geändert und gelöschte Zeilen" unten. 
- Die Größe der indizierten Felder in einer Zeile überschreiten nicht die maximale Größe der Azure Suche indizieren Anforderung 16 MB. 

## <a name="create-and-use-an-azure-sql-indexer"></a>Erstellen und Verwenden eines Azure SQL-Indexers

Erstellen Sie zuerst die Datenquelle: 

    POST https://myservice.search.windows.net/datasources?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }


Die Verbindungszeichenfolge erhalten der [Azure-Verwaltungsportal](https://portal.azure.com); Verwenden Sie die `ADO.NET connection string` Option.

Erstellen Sie, Ziel Azure Search Index, wenn Sie nicht bereits eine haben. Sie können über das [Portal UI](https://portal.azure.com) -oder [Index-API erstellen](https://msdn.microsoft.com/library/azure/dn798941.aspx)dazu.  Wird das Schema der Zielindex kompatibel mit dem Schema der Quelltabelle - [Zuordnung zwischen SQL und Azure Suche Datentypen](#TypeMapping) für Details sehen.

Schließlich erstellen Sie Indexer einen Namen und Daten Quelle und Ziel Index verweisen:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Auf diese Weise erstellter Indexer keinen Zeitplan. Es automatisch wird einmal ausgeführt, sobald es erstellt wird. Sie können jederzeit einen **Indexer ausführen** -Anforderung ausgeführt werden:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

Sie können verschiedene Aspekte der Indexerverhalten Batchgröße und wie viele Dokumente übersprungen werden können, bevor eine indexerausführung fehlgeschlagen ist. Weitere Informationen finden Sie unter [Indexer-API erstellen](https://msdn.microsoft.com/library/azure/dn946899.aspx).
 
Sie müssen Azure Dienste zur Datenbank herzustellen. Eine Anleitung dazu finden Sie unter [Verbinden von Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) .

Um den Indizierungsstatus und Ausführung überwachen (Anzahl der indizierten Elemente, Fehler usw.) verwenden eine **Indexer** -Anforderung: 

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2015-02-28 
    api-key: admin-key

Die Antwort sollte wie folgt aussehen: 

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null 
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null 
            },
            ... earlier history items 
        ]
    }

Ausführungsverlauf enthält bis zu 50 zuletzt abgeschlossenen Befehlen reverse chronologisch sortiert werden (sodass die neueste Ausführung in der Antwort zuerst eintritt). Weitere Informationen zur Antwort finden Sie im [Indexer Status Abfragen](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Indexer werden nach einem Zeitplan ausgeführt.

Sie können auch den Indexer regelmäßig nach einem Zeitplan ausführen anordnen. Dazu füge **Zeitplan** -Eigenschaft beim Erstellen oder Aktualisieren des Indexers. Das folgende Beispiel zeigt eine PUT-Anforderung Indexer zu aktualisieren:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

**Interval** -Parameter ist erforderlich. Das Intervall bezieht sich auf die Zeit zwischen zwei aufeinander folgenden Indexer Ausführung. Das kleinste zulässige Intervall ist 5 Minuten. die längste beträgt einen Tag. Es muss als XSD-Wert "DayTimeDuration" (eine eingeschränkte Teilmenge der eine Dauer von [ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ) formatiert. Dieses Muster ist: `P(nD)(T(nH)(nM))`. Beispiele: `PT15M` alle 15 Minuten `PT2H` für alle 2 Stunden.

Optionale **StartTime** gibt bei der geplanten Ausführung beginnen. Wenn es weggelassen wird, wird die aktuelle UTC-Zeit verwendet. Diese Zeit kann in dem Fall die erste Ausführung geplant ist, seit der Startzeit der Indexer ausgeführt wurde in der Vergangenheit.  

Nur eine Ausführung der angegebenen Indexer kann gleichzeitig ausführen. Wenn ein Indexer bereits, beim nächsten starten soll ausgeführt wird, die Ausführung übersprungen und setzt beim nächsten Intervall, sofern keine anderen Indexer Aufträge ausgeführt.

Betrachten wir ein Beispiel zu konkreter. Nehmen Sie wir den folgenden stündlichen Zeitplan konfiguriert: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Hier geschieht Folgendes: 

1. Der erste Index Ausführungsbeginn im oder am 1. März 2015 12:00 Uhr UTC.
1. Angenommen Sie, diese Ausführung dauert 20 Minuten (oder wenn weniger als 1 Stunde).
1. Die zweite Ausführung beginnt im oder am 1. März 2015 1:00 Uhr 
1. Nehmen wir nun an, dass diese Ausführung länger als eine Stunde dauert (hierzu muss eine Vielzahl von Dokumenten dazu tatsächlich jedoch ist eine nützliche Abbildung) – z. B. 70 Minuten – damit um 10 Uhr abgeschlossen ist
1. Es ist jetzt 2:00 Uhr für die dritte Ausführung gestartet. Aber da die zweite Ausführung von 1: 00 Uhr wird weiterhin ausgeführt, die dritte Ausführung übersprungen. Die dritte Ausführung startet um 3 Uhr morgens

Hinzufügen, ändern oder löschen ein Zeitplans für einen vorhandenen Indexer eine **Indexer PUT** -Anforderung. 

## <a name="capturing-new-changed-and-deleted-rows"></a>Neue, geändert und gelöschten Zeilen erfassen

Wenn Sie einen Zeitplan und Ihre Tabelle eine nicht-triviale Zeilenanzahl enthält, verwenden Sie Änderung Erkennung Datenrichtlinien, damit der Indexer nur neuen oder geänderten Zeilen aktualisiert werden kann, ohne die gesamte Tabelle neu indizieren.

### <a name="sql-integrated-change-tracking-policy"></a>Integrierte SQL Überarbeitung der Richtlinie

Wenn die SQL-Datenbank [Versionsvergleich](https://msdn.microsoft.com/library/bb933875.aspx)unterstützt, empfehlen wir **SQL integrierten ändern Tracking Policy**. Diese Richtlinie ermöglicht die effizienteste ändern und es auch Azure Search zu gelöschten Zeilen ohne eine explizite "weichen"Löschspalte Ihrer Tabelle hinzuzufügen.

Integrierte Änderungsprotokoll wird mit SQL Server-Datenbankversionen unterstützt:
 
- SQL Server 2008 R2 oder höher, verwenden Sie SQL Server auf Azure VMs. 
- Azure SQL-Datenbank V12 verwenden Sie Azure SQL-Datenbank.

Wenn mit SQL integriert Versionsvergleich Richtlinie Geben Sie separate löschen Erkennung Datenrichtlinien - diese Richtlinie verfügt über integrierte Unterstützung zum Identifizieren von Zeilen gelöscht.

Diese Richtlinie kann nur mit Tabellen verwendet werden. Es kann mit Ansichten verwendet werden. Sie müssen aktivieren Änderungsprotokoll für die Tabelle, die Sie verwenden, bevor Sie diese Richtlinie verwenden können. Eine Anleitung finden Sie unter [Aktivieren und Deaktivieren von Überarbeitungen](https://msdn.microsoft.com/library/bb964713.aspx) . 

Verwenden Sie diese Richtlinie, erstellen oder Aktualisieren der Datenquelle wie folgt:
 
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
      }
    }

### <a name="high-water-mark-change-detection-policy"></a>Hochwassermarken Ermittlung Richtlinie

Während SQL integrierten Change Tracking Policy empfohlen wird, wird nicht Sie möglicherweise verwenden, wenn die Daten in einer Ansicht oder eine ältere Version von Azure SQL-Datenbank verwenden. Verwenden Sie in diesem Fall die Hochwassermarken Richtlinie. Diese Richtlinie kann verwendet werden, wenn die Tabelle eine Spalte enthält, die folgenden Kriterien erfüllen:

- Alle fügt Geben Sie einen Wert für die Spalte. 
- Alle Updates zu einem Element auch ändern den Wert der Spalte. 
- Der Wert dieser Spalte wird mit jeder Änderung erhöht.
- Abfragen, in denen ein `WHERE` Klausel wie `WHERE [High Water Mark Column] > [Current High Water Mark Value]` effizient ausgeführt werden kann.

Eine indizierte **Rowversion** -Spalte ist z.B. ein idealer Kandidat für die Hochwassermarken Spalte. Verwenden Sie diese Richtlinie, erstellen oder Aktualisieren der Datenquelle wie folgt: 

    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
      }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Weiche löschen Spalte löschen Erkennung Richtlinie

Wenn Zeilen aus der Tabelle gelöscht werden, wahrscheinlich die Zeilen aus dem Suchindex sowie löschen möchten. Verwenden Sie die integrierte SQL ändern Richtlinie ist dies für Sie durchgeführt. Jedoch helfen nicht die Hochwassermarken ändern Richtlinie mit gelöschten Zeilen. Was ist zu tun? 

Die Zeilen physisch aus der Tabelle entfernt, Sie Pech – es gibt keine Möglichkeit das Vorhandensein Datensätze abgeleitet, die nicht mehr vorhanden.  Die Technik "Soft löschen" können Sie jedoch um logisch gelöschte Zeilen zu markieren, ohne sie aus der Tabelle eine Spalte hinzufügen und Markieren von Zeilen mit einem markerwert in der Spalte löschen.

Der weiche löschen Technik können Sie angeben, weichen Richtlinie löschen wie folgt beim Erstellen oder Aktualisieren der Datenquelle: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

Beachten, dass **SoftDeleteMarkerValue** eine Zeichenfolge verwenden, die eine Darstellung des tatsächlichen Wert. Haben Sie eine Ganzzahlspalte, gelöschte Zeilen mit dem Wert 1 gekennzeichnet sind, z. B. `"1"`; haben Sie eine BIT-Spalte, gelöschte Zeilen werden mit den booleschen Wert true, verwenden `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Zuordnung zwischen SQL-Datentypen und Datentypen Azure suchen

|SQL-Datentyp | Zulässige Zielindex Feldtypen |Notizen 
|------|-----|----|
|Bit|Edm.Boolean Edm.String| |
|Int, Smallint, tinyint |Edm.Int32, Int64, Edm.String| |
| bigint | Int64, Edm.String | |
| Real, float |Edm.Double Edm.String | |
| Smallmoney Money dezimale numerische | Edm.String| Azure Suche unterstützt keine Edm.Double Dezimaltypen umwandeln, da diese Genauigkeit verlieren |
| Char, Nchar, Varchar, nvarchar | Edm.String<br/>Collection(EDM.String)|Collection(Edm.String) eine Zeichenfolgenspalte verwandeln erfordert eine Vorschau API-Version 2015-02-28-Vorschau. Informationen finden Sie [in diesem Artikel](search-api-indexers-2015-02-28-preview.md#CreateIndexer)| 
|Smalldatetime Datetime datetime2, Datum, datetimeoffset |Edm.DateTimeOffset Edm.String| |
|uniqueidentifier | Edm.String | |
|Geografie | Edm.GeographyPoint | Nur Geographie Instanzen mit SRID 4326 (der Standard) werden unterstützt. | | 
|RowVersion| N/A |Zeilenversion Spalten können nicht im Suchindex gespeichert werden, aber für Überarbeitung verwendet werden | |
| Zeit Timespan binäre Varbinary Bild Xml Geometrie, CLR-Typen | N/A |Nicht unterstützt |


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Q:** Kann SQL Azure Indexer auf IaaS VMs in Azure SQL-Datenbanken werden verwendet?

A: Ja. Sie müssen jedoch Suchdienst Verbindung mit der Datenbank die folgenden zwei Dinge zu ermöglichen. Finden Sie Artikel [Konfigurieren einer Verbindung von Azure Search Indexer zu SQL Server auf eine Azure-VM](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md) .

1. Sie müssen die Datenbank mit einem vertrauenswürdigen Zertifikat so konfigurieren, dass der Suchdienst SSL-Verbindungen zur Datenbank öffnen kann.
2. Konfigurieren Sie den Zugriff auf die IP-Adresse der Suchdienst Firewall.

**Q:** Kann ich mit SQL-Datenbanken mit lokalen SQL Azure Indexer verwenden? 

A: Wir empfehlen oder nicht unterstützen, damit Sie die Datenbanken für den Internetverkehr öffnen müssten. 

**Q:** Kann SQL Azure Indexer als in IaaS auf Windows Azure ausgeführte SQL Server-Datenbanken werden verwendet? 

A: Wir unterstützen dieses Szenario nicht da wir Indexer mit Datenbanken als SQL Server getestet.  

**Q:** Kann ich mehrere Indexer auf einen Zeitplan erstellen? 

A: Ja. Nur ein Indexer kann jedoch gleichzeitig auf einem Knoten ausgeführt werden. Benötigen Sie mehrere Indexer ausgeführt, sollten Sie die Skalierung Suchdienst zu mehr als einer Suche. 

**Q:** Beeinflusst Ausführen eines Indexers Abfrage-Systemlast? 

A: Ja. Indexer auf einem Knoten in der Search-Dienst ausgeführt wird und dieser Knoten Ressourcen von mehreren Indizierung und Datenverkehr und andere API-Anfragen. Sollten Sie intensive Arbeitslasten für Indizierung und Abfrage ausführen und eine hohe 503 Fehler oder zunehmende Antwortzeiten auftreten Suchdienst skalieren.
