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
    ms.date="10/27/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Azure Search Indexer mit Azure SQL-Datenbank herstellen

Azure-Suchdienst ist eine gehostete Cloud-Suchdienst, der bieten einer hervorragende Umgebung erleichtert. Bevor Sie suchen können, müssen Sie Azure Search Index mit Daten zu füllen. Wenn die Daten in einer SQL Azure-Datenbank befindet, automatisieren neue **Azure Search Indexer für Azure SQL-Datenbank** (oder kurz **SQL Azure Indexer** ) der Indizierung zu. Dies bedeutet, dass Sie weniger Code schreiben und weniger Infrastruktur zu kümmern.

Dieser Artikel behandelt das Verwenden von Indexern, aber auch beschreibt die Funktionen, die nur mit SQL Azure-Datenbanken (z. B. integrierte Versionsvergleich) verfügbar sind. Azure unterstützt auch andere Datenquellen DocumentDB Azure BLOB-Speicher und Tabellenspeicher. Möchten Sie finden Unterstützung für zusätzliche Datenquellen, Feedback zum [Bewertungsportal Azure suchen](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Indexer und Datenquellen

Sie können einrichten und Konfigurieren einer Azure SQL-Indexer mit: 

- Datenimport-Assistent in der [Azure-portal](https://portal.azure.com)
- Azure Suche [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)
- Azure Suche [REST-API](http://go.microsoft.com/fwlink/p/?LinkID=528173)

In diesem Artikel verwenden wir die REST-API zum Erstellen und Verwalten von **Indexern** und **Datenquellen**angezeigt. 

Eine **Datenquelle** gibt die Daten für den index, Anmeldeinformationen Zugriff auf Daten und Richtlinien, die Änderungen in den Daten (neue, geänderte oder gelöschte Zeilen) effizient identifizieren. Es ist als unabhängige Ressource definiert, sodass sie durch mehrere Indexer verwendet werden kann.

Ein **Indexer** ist eine Ressource, mit dem Ziel Suchindizes Datenquellen verbindet. Indexer wird folgendermaßen verwendet:
 
- Führen Sie eine einmalige Kopie der Daten zum Auffüllen des Indexes.
- Aktualisieren eines Indexes mit der Datenquelle auf einem Zeitplan.
- Führen Sie bei Bedarf einen Index nach Bedarf aktualisieren. 

## <a name="when-to-use-azure-sql-indexer"></a>SQL Azure Indexer verwenden

Je nach Faktoren für Ihre Daten mit SQL Azure Indexer kann oder nicht. Wenn Ihre Daten folgende passt, können Sie SQL Azure Indexer: 

- Die Daten stammen aus einer einzelnen Tabelle oder Ansicht
    - Wenn die Daten über mehrere Tabellen verteilt sind, können Sie eine Ansicht erstellen und verwenden Sie diese Ansicht mit der Indexer. Wenn Sie eine Ansicht verwenden, wird nicht Sie können SQL Server integrierte Ermittlung jedoch. Weitere Informationen finden Sie in [diesem Abschnitt](#CaptureChangedRows). 
- Indexer in der Datenquelle verwendeten Datentypen unterstützt. Den meisten SQL Typen werden unterstützt. Details finden Sie unter [Zuordnen von Datentypen in Azure suchen](http://go.microsoft.com/fwlink/p/?LinkID=528105). 
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

Erstellen Sie, Ziel Azure Search Index, wenn Sie nicht bereits eine haben. Erstellen Sie einen Index mit dem [Portal UI](https://portal.azure.com) oder [Index-API erstellen](https://msdn.microsoft.com/library/azure/dn798941.aspx). Wird das Schema der Zielindex kompatibel mit dem Schema der Tabelle - [Zuordnung zwischen SQL und Azure Suche Datentypen](#TypeMapping)angezeigt.

Schließlich erstellen Sie Indexer einen Namen und Daten Quelle und Ziel Index verweisen:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Auf diese Weise erstellter Indexer keinen Zeitplan. Es wird automatisch ausgeführt, sobald es erstellt wird. Sie können jederzeit einen **Indexer ausführen** -Anforderung ausgeführt werden:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

Sie können verschiedene Aspekte der Indexerverhalten Batchgröße und wie viele Dokumente vor indexerausführung übersprungen werden können. Weitere Informationen finden Sie unter [Indexer-API erstellen](https://msdn.microsoft.com/library/azure/dn946899.aspx).
 
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

Sie können auch den Indexer regelmäßig nach einem Zeitplan ausführen anordnen. Zu diesem Zweck fügen Sie die **Zeitplan** -Eigenschaft beim Erstellen oder Aktualisieren des Indexers hinzu Das folgende Beispiel zeigt eine PUT-Anforderung Indexer zu aktualisieren:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

**Interval** -Parameter ist erforderlich. Das Intervall bezieht sich auf die Zeit zwischen zwei aufeinander folgenden Indexer Ausführung. Das kleinste zulässige Intervall ist 5 Minuten. die längste beträgt einen Tag. Es muss als XSD-Wert "DayTimeDuration" (eine eingeschränkte Teilmenge der eine Dauer von [ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ) formatiert. Dieses Muster ist: `P(nD)(T(nH)(nM))`. Beispiele: `PT15M` alle 15 Minuten `PT2H` für alle 2 Stunden.

Optionale **StartTime** gibt an, wann geplante Ausführung beginnen. Wenn es weggelassen wird, wird die aktuelle UTC-Zeit verwendet. Diese Zeit kann in dem Fall die erste Ausführung geplant ist, seit der Startzeit der Indexer ausgeführt wurde in der Vergangenheit.  

Nur eine Ausführung eines Indexers kann ausgeführt werden. Wenn Indexer ausgeführt wird, wenn seine Ausführung geplant ist, wird die Ausführung bis zum nächsten Zeitpunkt geplanten verschoben.

Betrachten wir ein Beispiel zu konkreter. Nehmen Sie wir den folgenden stündlichen Zeitplan konfiguriert: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Hier geschieht Folgendes: 

1. Der erste Index Ausführungsbeginn im oder am 1. März 2015 12:00 Uhr UTC.
1. Angenommen Sie, diese Ausführung dauert 20 Minuten (oder wenn weniger als 1 Stunde).
1. Die zweite Ausführung beginnt im oder am 1. März 2015 1:00 Uhr 
1. Nehmen wir nun an, dass diese Ausführung Stunde – z. B. 70 Minuten dauert, um 10 Uhr abgeschlossen ist
1. Es ist jetzt 2:00 Uhr für die dritte Ausführung gestartet. Aber da die zweite Ausführung von 1: 00 Uhr wird weiterhin ausgeführt, die dritte Ausführung übersprungen. Die dritte Ausführung startet um 3 Uhr morgens

Hinzufügen, ändern oder löschen ein Zeitplans für einen vorhandenen Indexer eine **Indexer PUT** -Anforderung. 

<a name="CaptureChangedRows">, / a >
## <a name="capturing-new-changed-and-deleted-rows"></a>Erfassen von neuen geändert und gelöschten Zeilen

Wenn Ihre Tabelle viele Zeilen enthält, verwenden Sie Datenrichtlinien Änderung erkennen. Ermittlung ermöglicht eine effiziente Abrufen nur neue oder geänderte Zeilen ohne die gesamte Tabelle neu zu indizieren.

### <a name="sql-integrated-change-tracking-policy"></a>Integrierte SQL Überarbeitung der Richtlinie

Wenn die SQL-Datenbank [Versionsvergleich](https://msdn.microsoft.com/library/bb933875.aspx)unterstützt, empfehlen wir **SQL integrierten ändern Tracking Policy**. Dies ist die effizienteste Richtlinie. Darüber hinaus können Azure Suche zu gelöschten Zeilen ohne eine explizite "weichen"Löschspalte Ihrer Tabelle hinzuzufügen.

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

<a name="HighWaterMarkPolicy"></a>
### <a name="high-water-mark-change-detection-policy"></a>Hochwassermarken Ermittlung Richtlinie

Während SQL integrierten Change Tracking Policy empfohlen wird, können sie nur mit Tabellen, Ansichten nicht verwendet werden. Wenn Sie eine Ansicht verwenden, sollten Sie Hochwassermarken Richtlinie. Diese Richtlinie kann verwendet werden, wenn die Tabelle oder Sicht eine Spalte enthält, die folgenden Kriterien erfüllen:

- Alle fügt Geben Sie einen Wert für die Spalte. 
- Alle Updates zu einem Element auch ändern den Wert der Spalte. 
- Der Wert dieser Spalte wird mit jeder Änderung erhöht.
- Abfragen mit folgenden, und ORDER BY-Klauseln effizient ausgeführt werden können: `WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`.

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

> [AZURE.WARNING] Wenn die Tabelle einen Spalte Hochwassermarken keinen Index, können Abfragen, die von der SQL-Indexer Timeout. Insbesondere der `ORDER BY [High Water Mark Column]` -Klausel erfordert einen Index effizient ausgeführt, wenn die Tabelle viele Zeilen enthält. 

Wenn Timeoutfehler auftreten, können Sie die `queryTimeout` Einstellung Indexer das Abfragetimeout auf einen höheren Wert als das Standardtimeout von 5 Minuten festgelegt. Um das Zeitlimit auf 10 Minuten festgelegt, z. B. erstellen oder Aktualisieren des Indexers mit der folgenden Konfiguration: 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

Sie können auch deaktivieren der `ORDER BY [High Water Mark Column]` Klausel. Allerdings wird nicht empfohlen, da die indexerausführung durch einen Fehler unterbrochen wird, Indexer hat alle Zeilen erneut verarbeitet läuft es später - auch wenn der Indexer bereits fast alle Zeilen Zeitpunkt verarbeitet hat, unterbrochen wurde. Deaktivieren der `ORDER BY` -Klausel, mit der `disableOrderByHighWaterMarkColumn` in der Definition der Indexer festlegen:  

    {
     ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Weiche löschen Spalte löschen Erkennung Richtlinie

Wenn Zeilen aus der Tabelle gelöscht werden, wahrscheinlich die Zeilen aus dem Suchindex sowie löschen möchten. Verwenden Sie die integrierte SQL ändern Richtlinie ist dies für Sie durchgeführt. Jedoch helfen nicht die Hochwassermarken ändern Richtlinie mit gelöschten Zeilen. Was ist zu tun? 

Wenn Zeilen aus der Tabelle physisch entfernt werden, kann Azure Search nicht Vorhandensein Datensätze abgeleitet, die nicht mehr vorhanden.  Die Technik "Soft löschen" können Sie jedoch logisch Zeilen löschen, ohne sie aus der Tabelle. Hinzufügen einer Spalte zu Tabelle oder Ansicht und markieren Zeilen gelöscht, Spalte.

Der weiche löschen Technik können Sie angeben, weichen Richtlinie löschen wie folgt beim Erstellen oder Aktualisieren der Datenquelle: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

Der **SoftDeleteMarkerValue** muss eine Zeichenfolge verwenden, die eine Darstellung des tatsächlichen Wert. Haben Sie eine Ganzzahlspalte, gelöschte Zeilen mit dem Wert 1 gekennzeichnet sind, z. B. `"1"`. Haben Sie eine BIT-Spalte, gelöschte Zeilen werden mit den booleschen Wert true, verwenden `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Zuordnung zwischen SQL-Datentypen und Datentypen Azure suchen

|SQL-Datentyp | Zulässige Zielindex Feldtypen |Notizen 
|------|-----|----|
|Bit|Edm.Boolean Edm.String| |
|Int, Smallint, tinyint |Edm.Int32, Int64, Edm.String| |
| bigint | Int64, Edm.String | |
| Real, float |Edm.Double Edm.String | |
| Smallmoney Money dezimale numerische | Edm.String| Azure Suche unterstützt keine Edm.Double Dezimaltypen umwandeln, da diese Genauigkeit verlieren |
| Char, Nchar, Varchar, nvarchar | Edm.String<br/>Collection(EDM.String)|Eine SQL-Zeichenfolge kann verwendet werden, ein Collection(Edm.String) Feld aufgefüllt, wenn die Zeichenfolge ein JSON-Array von Zeichenfolgen ist:`["red", "white", "blue"]` | 
|Smalldatetime Datetime datetime2, Datum, datetimeoffset |Edm.DateTimeOffset Edm.String| |
|uniqueidentifier | Edm.String | |
|Geografie | Edm.GeographyPoint | Nur Geographie Instanzen mit SRID 4326 (der Standard) werden unterstützt. | | 
|RowVersion| N/A |Zeilenversion Spalten können nicht im Suchindex gespeichert werden, aber für Überarbeitung verwendet werden | |
| Zeit Timespan binäre Varbinary Bild Xml Geometrie, CLR-Typen | N/A |Nicht unterstützt |

## <a name="configuration-settings"></a>Konfigurationen

SQL-Indexer macht mehrere Konfigurationen verfügbar: 

|Einstellung |Datentyp | Zweck | Standardwert |
|--------|----------|---------|---------------|
| queryTimeout | Zeichenfolge | Legt das Timeout für die Ausführung von SQL-Abfragen | 5 Minuten ("00: 05:00") |
| disableOrderByHighWaterMarkColumn | bool | Führt SQL Query zur Richtlinie Hochwassermarken die ORDER BY-Klausel weglassen. Siehe [Richtlinie Hochwassermarken](#HighWaterMarkPolicy) | falsch | 

Diese Einstellung wird der `parameters.configuration` Objekt in der Definition der Indexer. Um das Abfragetimeout 10 Minuten festzulegen, z. B. erstellen oder Aktualisieren des Indexers mit der folgenden Konfiguration: 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Q:** Kann SQL Azure Indexer auf IaaS VMs in Azure SQL-Datenbanken werden verwendet?

A: Ja. Sie müssen jedoch Suchdienst zu Ihrer Datenbank herstellen können. Weitere Informationen finden Sie unter [Konfigurieren einer Verbindung von Azure Search Indexer zu SQL Server auf eine Azure-VM](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).


**Q:** Kann ich mit SQL-Datenbanken mit lokalen SQL Azure Indexer verwenden? 

A: Wir empfehlen oder nicht unterstützen, damit Sie die Datenbanken für den Internetverkehr öffnen müssten. 

**Q:** Kann SQL Azure Indexer als in IaaS auf Windows Azure ausgeführte SQL Server-Datenbanken werden verwendet? 

A: Wir unterstützen dieses Szenario nicht da wir Indexer mit Datenbanken als SQL Server getestet.  

**Q:** Kann ich mehrere Indexer auf einen Zeitplan erstellen? 

A: Ja. Nur ein Indexer kann jedoch gleichzeitig auf einem Knoten ausgeführt werden. Benötigen Sie mehrere Indexer ausgeführt, sollten Sie die Skalierung Suchdienst zu mehr als einer Suche. 

**Q:** Beeinflusst Ausführen eines Indexers Abfrage-Systemlast? 

A: Ja. Indexer auf einem Knoten in der Search-Dienst ausgeführt wird und dieser Knoten Ressourcen von mehreren Indizierung und Datenverkehr und andere API-Anfragen. Sollten Sie intensive Arbeitslasten für Indizierung und Abfrage ausführen und eine hohe 503 Fehler oder zunehmende Antwortzeiten auftreten Suchdienst skalieren.
