<properties 
   pageTitle="Anzeigen von Diagnoseprotokollen für Azure Data Lake Analytics | Microsoft Azure" 
   description="Verstehen Sie, wie setup und Diagnoseprotokolle für Azure Data Lake Analytics zugreifen " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="Blackmist" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="08/11/2016"
   ms.author="larryfr"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Zugriff auf Diagnoseprotokolle für Azure Data Lake Analytics

Informationen Sie zum Aktivieren des Diagnoseprotokolls für Ihr Konto Datenanalyse See und zum Anzeigen der Protokolle, die für Ihr Konto erfasst.

Organisationen können das Diagnoseprotokoll für ihr Konto Azure Data Lake Analytics Data Access Prüfpfade sammeln. Diese Protokolle enthalten Informationen wie:

* Eine Liste der Benutzer, die Zugriff auf die Daten.
* Wie häufig Daten zugegriffen werden.
* Wie viele Daten im Konto gespeichert.

## <a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).
- Für Daten See Analytics Public Preview **Azure Abonnement aktivieren** . [Siehe.](data-lake-analytics-get-started-portal.md#signup)
- **Azure Data Lake Analytics-Konto**. Gehen Sie auf der [Einstieg in Azure Data Lake Analytics Azure-Portal verwenden](data-lake-analytics-get-started-portal.md).

## <a name="enable-logging"></a>Aktivieren der Protokollierung

1. Melden Sie sich auf das neue [Azure-Portal](https://portal.azure.com).

2. Öffnen Sie Ihr Konto See Datenanalyse und klicken Sie **Diagnose** **der Datenanalyse See Konto Blatt klicken**.

3. **Diagnostische** Blatt ändern Sie den Protokolliergrad konfigurieren.

    ![Aktivieren des Diagnoseprotokolls] (./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Aktivieren von Diagnoseprotokollen")

    * **Status** **auf** Diagnose-Protokollierung fest
    * Sie können Speicher/die auf zwei verschiedene Arten von Daten.
        * Wählen Sie Log Daten an einen Hub Azure Ereignis **Ereignis Hub exportieren** . Verwenden Sie diese Option, haben Sie eine Weiterverarbeitung Pipeline eingehenden Protokolle in Echtzeit zu analysieren. Bei dieser Option müssen Sie die Details für Azure Event Hub bereitstellen verwenden möchten.
        * Wählen Sie zum Speichern von Protokollen Azure Storage-Konto **Speicherkonto exportieren** . Verwenden Sie diese Option, wenn Sie Daten archivieren möchten. Bei dieser Option müssen Sie ein Konto Azure-Speicher zum Speichern der Protokolle bereitstellen.
    * Geben Sie an, ob Sie Überwachungsprotokolle und/oder Anforderungsprotokolle erhalten möchten.
    * Geben Sie die Anzahl der Tage, für die die Daten aufbewahrt werden müssen.
    * Klicken Sie auf **Speichern**.

Sobald Sie diagnostische aktiviert haben, sehen Sie die Protokolle auf der Registerkarte **Diagnoseprotokolle** .

## <a name="view-logs"></a>Protokolle anzeigen

Gibt es zwei Verfahren zum Anzeigen von Protokolldaten für Ihr Konto See Datenanalyse.

* Von Konteneinstellungen See Datenanalyse
* Der Daten Speicherort Azure Storage-Konto

### <a name="using-the-data-lake-analytics-settings-view"></a>Data Lake Analytics Settings Ansicht

1. Klicken Sie auf die See Datenanalyse Account **Settings** Blade **Diagnoseprotokolle**.

    ![Ansicht Diagnose Protokollierung] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Diagnoseprotokolle anzeigen") 

2. Blatt **Diagnoseprotokolle** sollte **Überwachungsprotokolle** und **Anforderungsprotokolle**kategorisiert Protokolle angezeigt werden.
    * Anforderungsprotokolle aufnehmen jeder API-Anforderung Akonto See Datenanalyse.
    * Überwachungsprotokolle ähneln Protokolle fordern aber weniger detaillierte Aufschlüsselung der die Operationen auf See Datenanalyse-Konto. Beispielsweise kann ein einzelne Upload-API-Aufruf in Anforderungsprotokolle mehrere "Append" Vorgänge in den Überwachungsprotokollen führen.

3. Klicken Sie auf **Download** -Link für einen Protokolleintrag Protokolle herunterladen.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Von Azure Storage-Konto enthält Daten

1. Öffnen Sie das Azure Storage-Konto Blade Data Lake Analytics für Protokollierung zugeordnet und dann auf Blobs. Der **BLOB-** Blatt enthält zwei Container.

    ![Ansicht Diagnose Protokollierung] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Diagnoseprotokolle anzeigen")

    * Container **Einblicke Protokolle Audit** enthält die Überwachungsprotokolle.
    * Container **Einblicke Protokolle Anfragen** enthält die Anforderungsprotokolle.

2. In diesen Containern werden die Protokolle unter folgende Struktur gespeichert.

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json
    
    > [AZURE.NOTE] Die `##` Einträge im Pfad enthalten, Jahr, Monat, Tag und Stunde, in der das Protokoll erstellt wurde. Datenanalyse See erstellt eine Datei stündlich, so `m=` enthält immer den Wert `00`.

    Beispielsweise könnte ein Überwachungsprotokoll der vollständige Pfad:
    
        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Ebenso kann der vollständige Pfad zu einer an:
    
        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Protokoll-Struktur

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
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
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
| operationName   | Zeichenfolge | Name des Vorgangs, der angemeldet ist. Beispielsweise GetAggregatedJobHistory.              |
| resultType      | Zeichenfolge | Der Status des Vorgangs, z. B. 200.                                 |
| callerIpAddress | Zeichenfolge | Die IP-Adresse des anfordernden Clients                                |
| correlationId   | Zeichenfolge | Die Id des Protokolls. Dieser Wert kann verwendet werden, gruppiert verwandte Einträge |
| Identität        | Objekt | Die Identität, die das Protokoll erstellt hat                                            |
| Eigenschaften      | JSON   | Finden Sie im nächsten Abschnitt (Anforderung Protokollschema Eigenschaften) |

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
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job", 
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
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
| operationName   | Zeichenfolge | Name des Vorgangs, der angemeldet ist. Beispielsweise JobSubmitted.              |
| resultType | Zeichenfolge | Ein untergeordneter Auftragsstatus (OperationName). |
| resultSignature | Zeichenfolge | Weitere Informationen über den Status (OperationName). |
| Identität      | Zeichenfolge | Der Benutzer, der die angeforderte Operation. Z. B. susan@contoso.com.                                 |
| Eigenschaften      | JSON   | Finden Sie im nächsten Abschnitt (Audit Log Eigenschaftsschema) |

> [AZURE.NOTE] __ResultType__ __ResultSignature__ Informationen über das Ergebnis einer Operation und nur einen Wert enthalten, wenn ein Vorgang abgeschlossen wurde. Beispielsweise enthalten sie __OperationName__ Wert __JobStarted__ oder __JobEnded__enthält einen Wert.

#### <a name="audit-log-properties-schema"></a>Audit Log Eigenschaftsschema

| Name       | Typ   | Beschreibung                              |
|------------|--------|------------------------------------------|
| Auftrags | Zeichenfolge | Die dem Einzelvorgang zugeordnete ID  |
| JobName | Zeichenfolge | Der Name, der für den Auftrag angegeben wurde |
| JobRunTime | Zeichenfolge | Die Laufzeit verwendet, um den Auftrag zu verarbeiten |
| SubmitTime | Zeichenfolge | Die Uhrzeit (in UTC) der Auftrag gesendet wurde. |
| Startzeit | Zeichenfolge | Der Startzeitpunkt des Auftrags nach Einreichung (UTC). |
| Endzeit | Zeichenfolge | Der Zeitpunkt der Beendigung des Einzelvorgangs. |
| Parallelität | Zeichenfolge | Die Anzahl der Datenanalyse See Einheiten für diesen Auftrag bei angefordert. |

> [AZURE.NOTE] __SubmitTime__, __Startzeit__, __Endzeit__ und __Parallelität__ Angaben für einen Arbeitsgang und nur einen Wert enthalten, wenn ein Vorgang gestartet oder abgeschlossen wurde. Beispielsweise enthält __SubmitTime__ einen Wert __OperationName__ __JobSubmitted__anzeigt.

## <a name="process-the-log-data"></a>Verarbeiten der Daten

Azure Data Lake Analytics enthält ein Beispiel zum Verarbeiten und die Daten analysieren. Im Beispiel finden Sie unter [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure Data Lake Analytics](data-lake-analytics-overview.md)


