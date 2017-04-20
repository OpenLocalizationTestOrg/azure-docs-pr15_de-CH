<properties
    pageTitle="Archivieren von Azure Aktivitätsprotokoll | Microsoft Azure"
    description="So archivieren Sie Ihre Azure-Aktivitätsprotokoll zur langfristigen Aufbewahrung in ein Speicherkonto."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="johnkem"/>

# <a name="archive-the-azure-activity-log"></a>Archiv Azure Aktivitätsprotokoll
In diesem Artikel zeigen wir Azure-Portal PowerShell-Cmdlets oder CLI plattformübergreifende Verwendung archivieren Ihre [**Azure-Aktivitätsprotokoll**](monitoring-overview-activity-logs.md) ein Speicherkonto. Diese Option ist nützlich, wenn Sie länger als 90 Tage (über Vollzugriff auf die Aufbewahrungsrichtlinie) für Überwachung, statische Analyse oder Sicherung Aktivitätenprotokoll beibehalten möchten. Wenn nur die Ereignisse für 90 Tage beibehalten müssen oder weniger Sie ein Speicherkonto Archivierung einrichten brauchen, da Aktivitätsprotokoll der Azure-Plattform 90 Tage lang aufbewahrt wird ohne Archivierung aktivieren.

## <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie beginnen, müssen Sie [erstellen ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) , Activity Log archivieren können. Wir empfehlen, dass Sie kein vorhandenes Speicherkonto verwenden, die andere nicht überwachen Daten darin gespeichert, sodass Sie Zugriff auf Überwachungsdaten besser steuern können. Jedoch wenn Sie Diagnoseprotokolle und Messverfahren ein Speicherkonto archiviert werden, können sie Speicher für Ihre Activity Log verwenden, bleiben alle Daten an einem zentralen Ort sinnvoll. Das verwendete Speicherkonto muss ein Speicherkonto allgemeine kein BLOB-Speicher-Konto.

## <a name="log-profile"></a>Protokoll-Profil
Das Aktivitätsprotokoll mit einer der folgenden Methoden um zu archivieren, legen Sie das **Protokoll Profil** für ein Abonnement. Profil-Protokoll definiert den Typ der Ereignisse, die gespeichert oder übertragen werden, und die Ausgaben – Speicher Konto bzw. Ereignis-Hub. Auch wird die Aufbewahrungsrichtlinie (Anzahl von Tagen beibehalten) in ein Speicherkonto gespeicherten Ereignisse definiert. Die Aufbewahrungsrichtlinie auf NULL festgelegt ist, werden Ereignisse auf unbestimmte Zeit gespeichert. Andernfalls kann dies auf einen Wert zwischen 1 und 2147483647 festgelegt werden. [Erfahren Sie mehr über Protokoll Profile hier](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles).

## <a name="archive-the-activity-log-using-the-portal"></a>Archivieren Sie das Aktivitätsprotokoll über das portal
1. Klicken Sie im Portal auf der linken Navigationsleiste **Aktivitätsprotokoll** . Wenn Sie eine Verknüpfung für die Protokolldatei nicht angezeigt wird, klicken Sie **Mehr Dienste** zuerst.

    ![Navigieren Sie zum Aktivitätsprotokoll blade](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. Klicken Sie am oberen Rand der Blade auf **Exportieren**.

    ![Klicken Sie auf die Schaltfläche Exportieren](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. Erscheint Blatt das Kontrollkästchen Sie **Exportieren in ein Speicherkonto** , und wählen Sie ein Speicherkonto.

    ![Ein Speicherkonto festlegen](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Definieren Sie mithilfe der Schieberegler oder Textfeld eine Anzahl von Tagen für die Aktivität protokollieren von Ereignissen in das Speicherkonto abgelegt werden soll. Wenn Sie möchten Ihre Daten beibehalten im Speicherkonto unbegrenzt, diese Anzahl auf NULL gesetzt.
5. Klicken Sie auf **Speichern**.

## <a name="archive-the-activity-log-via-powershell"></a>Das Aktivitätsprotokoll über PowerShell archivieren
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Eigenschaft         | Erforderlich | Beschreibung                                                                                                                                                                                                                                                                                       |
|------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StorageAccountId | Nein       | Ressourcen-ID des Speicherkontos, Aktivitätsprotokolle gespeichert werden soll.                                                                                                                                                                                                                        |
| Standorte        | Ja      | Durch Kommas getrennte Liste der Regionen für die Aktivitätsprotokoll Ereignisse erfassen möchten. Sie können eine Liste aller Bereiche [dieser Seite](https://azure.microsoft.com/en-us/regions) oder [Der Azure-REST-API](https://msdn.microsoft.com/library/azure/gg441293.aspx)anzeigen. |
| RetentionInDays  | Ja      | Anzahl von Tagen zwischen 1 und 2147483647, welche Ereignisse beibehalten werden soll. Speichert der Wert NULL Protokolle unbegrenzt (unbegrenzt).                                                                                                                                                                                             |
| Kategorien       | Ja      | Durch Kommas getrennte Liste der Ereigniskategorien, die erfasst werden. Mögliche Werte sind schreiben, löschen und Aktion.                                                                                                                                                                                 |
## <a name="archive-the-activity-log-via-cli"></a>Archivieren Sie das Aktivitätsprotokoll über CLI
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Eigenschaft        | Erforderlich | Beschreibung                                                                                                                                                                                                                                                                                       |
|-----------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name            | Ja      | Name des Profils anmelden.                                                                                                                                                                                                                                                                         |
| storageId       | Nein       | Ressourcen-ID des Speicherkontos, Aktivitätsprotokolle gespeichert werden soll.                                                                                                                                                                                                                        |
| Standorte       | Ja      | Durch Kommas getrennte Liste der Regionen für die Aktivitätsprotokoll Ereignisse erfassen möchten. Sie können eine Liste aller Bereiche [dieser Seite](https://azure.microsoft.com/en-us/regions) oder [Der Azure-REST-API](https://msdn.microsoft.com/library/azure/gg441293.aspx)anzeigen. |
| retentionInDays | Ja      | Anzahl von Tagen zwischen 1 und 2147483647, welche Ereignisse beibehalten werden soll. Der Wert NULL Speichern der Protokolle unbegrenzt (unbegrenzt).                                                                                                                                                                                             |
| Kategorien      | Ja      | Durch Kommas getrennte Liste der Ereigniskategorien, die erfasst werden. Mögliche Werte sind schreiben, löschen und Aktion.                                                                                                                                                                                 |

## <a name="storage-schema-of-the-activity-log"></a>Speicherschema des Aktivitätsprotokolls
Sobald Sie Archivierung eingerichtet haben, wird ein Behälter als ein Aktivitätsprotokoll Ereignis im Speicherkonto erstellt. Die Blobs im Container gilt das gleiche Format Aktivitätsprotokoll und Diagnoseprotokolle. Die Struktur dieser Blobs ist:

> Einblicke-Betrieb-Protokolle/Name = Standard-ResourceId = / ABONNEMENTS / {Abonnement-ID} / y = {numerischen vierstellige} / m = {numerischen Monat} / d = {zweistelligen numerischen Tag} / h = {zweistellige 24-Stunden-hour}/m=00/PT1H.json

Ein Blob kann z. B. sein:

> Insights-Operational-Logs/Name=default/ResourceID=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON

Jede PT1H.json Blob enthält einen JSON-Blob Ereignisse innerhalb einer Stunde im BLOB-URL angegeben (z. B. h = 12). Während der Stunde werden Ereignisse bei deren Auftreten der PT1H.json-Datei angehängt. Der Wert für die Minuten (m = 00) immer 00, da einzelne Blobs pro Stunde Aktivitätsprotokoll Ereignisse unterteilt werden.

Jedes Ereignis wird in der Datei PT1H.json im folgenden Format "Datensätze" Array gespeichert:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| Elementnamen    | Beschreibung                                                                                                    |
|-----------------|----------------------------------------------------------------------------------------------------------------|
| Zeit            | Timestamp, als das Ereignis vom Azure-Dienst verarbeitet die Anforderung entspricht das Ereignis generiert wurde.    |
| resourceId      | Ressourcen-ID der betroffenen Ressource.                                                                          |
| operationName   | Name des Vorgangs.                                                                                         |
| Kategorie        | Kategorie der Aktivität, z.B.. Schreiben, lesen, Aktion.                                                               |
| resultType      | Der Typ des Ergebnisses, z. b. Erfolg, Fehler, starten                                                            |
| resultSignature | Hängt den Ressourcentyp.                                                                                  |
| durationMs      | Dauer des Vorgangs in Millisekunden                                                                      |
| callerIpAddress | IP-Adresse des Benutzers, der die Operation UPN-Anspruch oder SPN Anspruch auf Verfügbarkeit ausgeführt hat.         |
| correlationId   | In der Regel eine GUID im Zeichenfolgenformat. Ereignisse, die eine CorrelationId Teilen gehören die gleiche Uber-Aktion.         |
| Identität        | Beschreibt die Autorisierung und Ansprüche JSON-Blob.                                                             |
| Autorisierung   | BLOB RBAC-Eigenschaften des Ereignisses. Normalerweise enthält die Eigenschaften "Action", "Role" und "Bereich".            |
| Ebene           | Ebene des Ereignisses. Die folgenden Werte: "Kritisch", "Fehler", "Warning", "Information" und "Verbose" |
| Speicherort        | Region, in dem der Speicherort aufgetreten ist (oder global).                                                             |
| Eigenschaften      | Festlegen von `<Key, Value>` Paare (d. h. Wörterbuch) beschreibt die Details des Ereignisses.                             |

> [AZURE.NOTE] Die Eigenschaften und die Verwendung dieser Eigenschaften variieren je nach der Ressource.

## <a name="next-steps"></a>Nächste Schritte
- [Herunterladen von Blobs für die Analyse](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Stream das Aktivitätsprotokoll auf Ereignis Hubs](monitoring-stream-activity-logs-event-hubs.md)
- [Weitere Informationen über das Aktivitätsprotokoll](monitoring-overview-activity-logs.md)
