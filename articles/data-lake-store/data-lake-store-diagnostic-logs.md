<properties 
   pageTitle="Anzeigen von Diagnoseprotokollen Azure Datenspeicher See | Microsoft Azure" 
   description="Verstehen Sie, wie setup und Diagnoseprotokolle für Azure See Datenspeicher zugreifen " 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Diagnoseprotokolle für Azure See Datenspeicher zugreifen

Informationen Sie zum Aktivieren des Diagnoseprotokolls für Ihr Konto Datenspeicher See und zum Anzeigen der Protokolle, die für Ihr Konto erfasst.

Organisationen können das Diagnoseprotokoll für ihren Azure See Datenspeicher Konto sammeln Daten Zugriff auf Audit-Trails, Informationen der Liste der Benutzer Zugriff auf die Daten bietet, wie häufig die Daten zugegriffen werden, wie viele Daten im Konto usw..

## <a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

- **See Datenspeicher Azure-Konto**. Gehen Sie auf der [Einstieg in Azure See Datenspeicher Azure-Portal](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Aktivieren Sie das Diagnoseprotokoll für Ihr Konto See Datenspeicher

1. Melden Sie sich auf das neue [Azure-Portal](https://portal.azure.com).

2. Öffnen Sie Ihr Konto See Datenspeicher und klicken Sie **Diagnose** **der See Datenspeicher Konto Blatt klicken**.

3. **Diagnostische** Blatt ändern Sie den Protokolliergrad konfigurieren.

    ![Aktivieren des Diagnoseprotokolls] (./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Aktivieren von Diagnoseprotokollen")

    * **Status** **auf** Diagnose-Protokollierung fest
    * Sie können Speicher/die auf zwei verschiedene Arten von Daten.
        * Die Option exportieren **Event Hub** Protokoll Daten an einen Hub Azure-Ereignis. Wahrscheinlich werden Sie diese Option verwenden, haben Sie eine Weiterverarbeitung Pipeline eingehenden Protokolle in Echtzeit zu analysieren. Bei dieser Option müssen Sie die Details für Azure Event Hub bereitstellen verwenden möchten.
        * Die Option **Konto** exportieren Protokolle Azure Storage-Konto gespeichert. Sie verwenden diese Option, wenn Daten archivieren, die Batch-Verarbeitung zu einem späteren Zeitpunkt. Bei dieser Option müssen Sie ein Konto Azure-Speicher zum Speichern der Protokolle bereitstellen.
    * Geben Sie an, ob Sie Überwachungsprotokolle und/oder Anforderungsprotokolle erhalten möchten.
    * Geben Sie die Anzahl der Tage, für die die Daten aufbewahrt werden müssen.
    * Klicken Sie auf **Speichern**.

Sobald Sie diagnostische aktiviert haben, sehen Sie die Protokolle auf der Registerkarte **Diagnoseprotokolle** .

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>Diagnoseprotokolle anzeigen für Ihr Konto See Datenspeicher

Es gibt zwei Verfahren zum Anzeigen von Protokolldaten für Ihr Konto See Datenspeicher.

* Aus dem Datenspeicher See anzeigen Einstellungen
* Der Daten Speicherort Azure Storage-Konto

### <a name="using-the-data-lake-store-settings-view"></a>Data Lake Speicher Settings Ansicht

1. Klicken Sie auf die See Datenspeicher Account **Settings** Blade **Diagnoseprotokolle**.

    ![Ansicht Diagnose Protokollierung] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Diagnoseprotokolle anzeigen") 

2. Blatt **Diagnoseprotokolle** sollte **Überwachungsprotokolle** und **Anforderungsprotokolle**kategorisiert Protokolle angezeigt werden.
    * Anforderungsprotokolle aufnehmen jeder API-Anforderung Akonto See Datenspeicher.
    * Überwachungsprotokolle ähneln Protokolle fordern aber weniger detaillierte Aufschlüsselung der die Operationen auf See Datenspeicher-Konto. Beispielsweise kann ein einzelne Upload-API-Aufruf in Anforderungsprotokolle mehrere "Append" Vorgänge in den Überwachungsprotokollen führen.

3. Klicken Sie auf **Download** -Link für jeden Protokolleintrag Protokolle herunterladen.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Von Azure Storage-Konto enthält Daten

1. Öffnen des Azure-Speicher Konto Blades See Datenspeicher für Protokollierung zugeordnet und dann auf Blobs. Der **BLOB-** Blatt enthält zwei Container.

    ![Ansicht Diagnose Protokollierung] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Diagnoseprotokolle anzeigen")

    * Container **Einblicke Protokolle Audit** enthält die Überwachungsprotokolle.
    * Container **Einblicke Protokolle Anfragen** enthält die Anforderungsprotokolle.

2. In diesen Containern werden die Protokolle unter folgende Struktur gespeichert.

    ![Ansicht Diagnose Protokollierung] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Diagnoseprotokolle anzeigen")

    Beispielsweise könnte der vollständige Pfad Überwachungsprotokoll`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`

    Ähnlich, der vollständige Pfad in ein Anforderungslog möglich`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>Die Struktur der Daten verstehen

Die Prüfung und Anforderung Protokolle sind im JSON-Format. In diesem Abschnitt werden die Struktur von JSON für Anforderung und Überwachungsprotokolle.

### <a name="request-logs"></a>Anforderungsprotokolle

Hier ist ein Eintrag in das JSON-Format an. Jedes Blob hat ein Stammobjekt genannte **Datensätze** , die ein Array von Objekten Protokoll enthält.

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Protokollschema "Anforderung"

| Name            | Typ   | Beschreibung                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| Zeit            | Zeichenfolge | Der Zeitstempel (UTC) des Protokolls                                              |
| resourceId      | Zeichenfolge | Die ID der Ressource, die Vorgang dauerte Platzieren auf                            |
| Kategorie        | Zeichenfolge | Die Kategorie. Beispielsweise **fordert**.                                   |
| operationName   | Zeichenfolge | Name des Vorgangs, der angemeldet ist. Beispielsweise Getfilestatus.              |
| resultType      | Zeichenfolge | Der Status des Vorgangs, z. B. 200.                                 |
| callerIpAddress | Zeichenfolge | Die IP-Adresse des anfordernden Clients                                |
| correlationId   | Zeichenfolge | Mit der Id des Protokolls können mehrere verwandte Einträge zu gruppieren |
| Identität        | Objekt | Die Identität, die das Protokoll erstellt hat                                            |
| Eigenschaften      | JSON   | Einzelheiten finden Sie unten                                                          |

#### <a name="request-log-properties-schema"></a>Anforderung Protokollschema Eigenschaften

| Name                 | Typ   | Beschreibung                                               |
|----------------------|--------|-----------------------------------------------------------|
| HttpMethod           | Zeichenfolge | Die HTTP-Methode verwendet für die Operation. Beispielsweise erhalten. |
| Pfad                 | Zeichenfolge | Der Pfad der Vorgang wurde durchgeführt                   |
| RequestContentLength | int    | Die Inhaltslänge der HTTP-Anforderung                    |
| ClientRequestId      | Zeichenfolge | Die Id, die diese Anforderung eindeutig              |
| Startzeit            | Zeichenfolge | Die Zeit, mit der der Server die Anforderung empfangen         |
| Endzeit              | Zeichenfolge | Die Zeit, mit der der Server eine Antwort gesendet              |

### <a name="audit-logs"></a>Audit-Protokolle

Hier ein Beispieleintrag in JSON-formatierte Überwachungsprotokoll. Jedes Blob hat ein Stammobjekt genannte **Datensätze** , die ein Array von Objekten Protokoll enthält

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Audit-Protokoll-schema

| Name            | Typ   | Beschreibung                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| Zeit            | Zeichenfolge | Der Zeitstempel (UTC) des Protokolls                                              |
| resourceId      | Zeichenfolge | Die ID der Ressource, die Vorgang dauerte Platzieren auf                            |
| Kategorie        | Zeichenfolge | Die Kategorie. Z. B. **Überwachung**.                                      |
| operationName   | Zeichenfolge | Name des Vorgangs, der angemeldet ist. Beispielsweise Getfilestatus.              |
| resultType      | Zeichenfolge | Der Status des Vorgangs, z. B. 200.                                 |
| correlationId   | Zeichenfolge | Mit der Id des Protokolls können mehrere verwandte Einträge zu gruppieren |
| Identität        | Objekt | Die Identität, die das Protokoll erstellt hat                                            |
| Eigenschaften      | JSON   | Einzelheiten finden Sie unten                                                          |

#### <a name="audit-log-properties-schema"></a>Audit Log Eigenschaftsschema

| Name       | Typ   | Beschreibung                              |
|------------|--------|------------------------------------------|
| StreamName | Zeichenfolge | Der Pfad der Vorgang wurde durchgeführt  |


## <a name="samples-to-process-the-log-data"></a>Beispiele zum Verarbeiten der Daten

Azure See Datenspeicher enthält ein Beispiel zum Verarbeiten und die Daten analysieren. Im Beispiel finden Sie unter [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="see-also"></a>Siehe auch

- [Übersicht über Azure See Datenspeicher](data-lake-store-overview.md)
- [Sichere Daten im Datenspeicher See](data-lake-store-secure-data.md)

