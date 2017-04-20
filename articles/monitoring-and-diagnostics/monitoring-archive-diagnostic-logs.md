<properties
    pageTitle="Archivieren von Azure Diagnoseprotokolle | Microsoft Azure"
    description="So archivieren Sie Ihre Azure Diagnoseprotokolle für langfristige Archivierung ein Speicherkonto."
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
    ms.date="08/26/2016"
    ms.author="johnkem"/>

# <a name="archive-azure-diagnostic-logs"></a>Azure Diagnoseprotokolle archivieren
In diesem Artikel zeigen wir Azure-Portal PowerShell-Cmdlets, CLI oder REST API Verwendung der [Azure-Diagnoseprotokolle](monitoring-overview-of-diagnostic-logs.md) in ein Speicherkonto archivieren. Diese Option ist nützlich, wenn Sie die Diagnoseprotokolle mit optionalen Aufbewahrungsrichtlinie für Überwachung, statische Analyse oder Sicherung beibehalten möchten.

## <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie beginnen, müssen Sie [erstellen ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) , die Diagnoseprotokolle archivieren können. Wir empfehlen, dass Sie kein vorhandenes Speicherkonto verwenden, die andere nicht überwachen Daten darin gespeichert, sodass Sie Zugriff auf Überwachungsdaten besser steuern können. Jedoch wenn Sie Ihre Aktivitätsprotokoll und Diagnose Metriken ein Speicherkonto archiviert werden, kann er mithilfe dieses Speicherkonto für die Diagnoseprotokolle sowie alle Daten an einem zentralen Ort sinnvoll. Das verwendete Speicherkonto muss ein Speicherkonto allgemeine kein BLOB-Speicher-Konto.

## <a name="diagnostic-settings"></a>Diagnostische Settings
Die Diagnoseprotokolle mit einer der folgenden Methoden um archivieren, legen Sie eine **Diagnose-Einstellung** für eine bestimmte Ressource. Eine Diagnose Einstellung für eine Ressource die Kategorien definiert, die gespeichert oder übertragen wurden und die Ergebnisse protokolliert – Speicher Konto bzw. Ereignis-Hub. Außerdem wird die Aufbewahrungsrichtlinie (Anzahl von Tagen beibehalten) für jede Kategorie in ein Speicherkonto gespeicherten Ereignisse definiert. Wenn eine Aufbewahrungsrichtlinie auf NULL festgelegt ist, werden Ereignisse für diese Kategorie (das heißt, immer), unbegrenzt gespeichert. Eine Aufbewahrungsrichtlinie kann eine beliebige Anzahl von Tagen zwischen 1 und 2147483647 sonst. [Erfahren Sie mehr über Diagnose Einstellungen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).

## <a name="archive-diagnostic-logs-using-the-portal"></a>Über das Portal Archiv Diagnoseprotokolle

1. Klicken Sie im Portal in Ressource Blade für die Ressource auf der möchten aktivieren Archivierung von Diagnoseprotokollen.
2. Wählen Sie im Abschnitt **Überwachung** Einstellungsmenü Ressource **Diagnose**.

    ![Überwachung auf ressourcenmenü](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-sec.png)
3. Das Kontrollkästchen Sie **Konto exportieren**und anschließend ein Speicherkonto. Legen Sie gegebenenfalls eine Anzahl von Tagen auf diese Protokolle behalten die **Aufbewahrung (Tage)** Regler. Aufbewahrung von Null Tage speichert Protokolle unbegrenzt.

    ![Diagnostische Protokolle blade](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-blade.png)
4. Klicken Sie auf **Speichern**.

Diagnoseprotokolle sind diesem Storage-Konto archiviert, sobald neue Daten generiert werden.

## <a name="archive-diagnostic-logs-via-the-powershell-cmdlets"></a>Diagnoseprotokolle Archiv über PowerShell-Cmdlets

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Eigenschaft         | Erforderlich | Beschreibung                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| ResourceId       | Ja      | Ressourcen-ID der Ressource auf die Diagnose gesetzt werden soll.                            |
| StorageAccountId | Nein       | Ressourcen-ID des Speicherkontos, Diagnoseprotokolle gespeichert werden soll.                          |
| Kategorien       | Nein       | Durch Trennzeichen getrennte Liste von Kategorien protokollieren aktivieren.                                                     |
| Aktiviert          | Ja      | Booleschen Diagnose aktiviert oder deaktiviert dieser Ressource.                  |
| RetentionEnabled | Nein       | Boolescher Wert, wenn eine Aufbewahrungsrichtlinie auf diese Ressource aktiviert sind.                            |
| RetentionInDays  | Nein       | Anzahl der Tage für die Ereignisse zwischen 1 und 2147483647 beibehalten werden soll. Ein Wert von NULL speichert Protokolle unbegrenzt. |

## <a name="archive-the-activity-log-via-the-cross-platform-cli"></a>Archivieren Sie das Aktivitätsprotokoll über CLI plattformübergreifende

```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Eigenschaft         | Erforderlich | Beschreibung                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| resourceId       | Ja      | Ressourcen-ID der Ressource auf die Diagnose gesetzt werden soll.                            |
| storageId        | Nein       | Ressourcen-ID des Speicherkontos, Diagnoseprotokolle gespeichert werden soll.                          |
| Kategorien       | Nein       | Durch Trennzeichen getrennte Liste von Kategorien protokollieren aktivieren.                                                     |
| aktiviert          | Ja      | Booleschen Diagnose aktiviert oder deaktiviert dieser Ressource.                  |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Diagnoseprotokolle Archiv über die REST-API
Informationen zum Festlegen einer Diagnose Einstellung mit der Azure-Monitor REST-API [finden Sie in diesem Dokument](https://msdn.microsoft.com/library/azure/dn931931.aspx) .

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Schema der Diagnoseprotokolle im Speicherkonto
Nachdem Archivierung eingerichtet haben, wird ein Behälter im Speicherkonto erstellt als eine Kategorien protokollieren eines Ereignisses aktiviert haben. Die Blobs im Container gilt das gleiche Format Diagnoseprotokolle und Aktivitätsprotokoll. Die Struktur dieser Blobs ist:

> Einblicke - Protokolle-{Protokollname Kategorie} / = ResourceId/ABONNEMENTS / {Abonnement-ID} /RESOURCEGROUPS/ {Ressourcengruppennamen} /PROVIDERS/ {Ressourcenname Anbieter} / {Resource type} / {Name Ressource} / y = {numerischen vierstellige} / m = {numerischen Monat} / d = {zweistelligen numerischen Tag} / h = {zweistellige 24-Stunden-hour}/m=00/PT1H.json

Oder einfach

> Einblicke - Protokolle-{Protokollname Kategorie} / ResourceId = / {Ressource Id} / y = {numerischen vierstellige} / m = {numerischen Monat} / d = {zweistelligen numerischen Tag} / h = {zweistellige 24-Stunden-hour}/m=00/PT1H.json

Ein Blob kann z. B. sein:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json

Jede PT1H.json Blob enthält einen JSON-Blob Ereignisse innerhalb einer Stunde im BLOB-URL angegeben (z. B. h = 12). Während der Stunde werden Ereignisse bei deren Auftreten der PT1H.json-Datei angehängt. Der Wert für die Minuten (m = 00) immer 00, da einzelne Blobs pro Stunde Diagnoseprotokoll Ereignisse unterteilt werden.

Jedes Ereignis wird in der Datei PT1H.json im folgenden Format "Datensätze" Array gespeichert:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Elementnamen  | Beschreibung                                                                                                 |
|---------------|-------------------------------------------------------------------------------------------------------------|
| Zeit          | Timestamp, als das Ereignis vom Azure-Dienst verarbeitet die Anforderung entspricht das Ereignis generiert wurde. |
| resourceId    | Ressourcen-ID der betroffenen Ressource.                                                                       |
| operationName | Name des Vorgangs.                                                                                      |
| Kategorie      | Kategorie des Ereignisses.                                                                                  |
| Eigenschaften    | Festlegen von `<Key, Value>` Paare (d. h. Wörterbuch) beschreibt die Details des Ereignisses.                            |

> [AZURE.NOTE] Die Eigenschaften und die Verwendung dieser Eigenschaften variieren je nach der Ressource.

## <a name="next-steps"></a>Nächste Schritte
- [Herunterladen von Blobs für die Analyse](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Stream Diagnoseprotokolle an Ereignis Hubs](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Weitere Informationen über Diagnoseprotokolle](monitoring-overview-of-diagnostic-logs.md)
