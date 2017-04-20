<properties
    pageTitle="Stream Azure Aktivitätsprotokoll zu Ereignis Hubs | Microsoft Azure"
    description="Enthält Informationen zum Streamen von Azure-Aktivitätsprotokoll zu Ereignis-Hubs."
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
    ms.date="10/03/2016"
    ms.author="johnkem"/>

# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Stream Azure Aktivitätsprotokoll zu Ereignis Hubs
[**Azure-Aktivitätsprotokoll**](./monitoring-overview-activity-logs.md) können in Echtzeit auf jede Anwendung, die integrierte Option "Exportieren" im Portal oder aktivieren Sie die Bus Regel-Id in einem Protokoll Profil Azure PowerShell-Cmdlets oder Azure CLI übertragen.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Mit dem Aktivitätsprotokoll Ereignis Hubs möglich
Hier sind ein paar Methoden Streaming-Funktionen für das Aktivitätsprotokoll verwenden können:

- **Stream Protokollierung und Telemetrie Systeme** – werden im Laufe der Zeit Ereignis Hubs streaming Mechanismus pipe Activity Log in Drittanbieter-SIEMs und analyselösungen.
- **Erstellen einer benutzerdefinierten Telemetrie und Protokollierung Plattform** – Wenn Sie bereits eine benutzerdefinierte Telemetrie Plattform oder zu eine hochgradig skalierbare Veröffentlichen-Abonnieren Art Event Hubs können Sie flexibel Aktivitätsprotokoll aufnehmen. [Siehe die Dan Rosanova Ereignis Hubs in einem weltweit Telemetrie Plattform verwenden.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Aktivieren des Aktivitätsprotokolls streaming
Sie können entweder programmgesteuert oder über das Portal des Aktivitätsprotokolls streaming. In beiden Fällen wählen einen Service Bus-Namespace und eine gemeinsame Richtlinien für diesen Namespace und Event-Hub wird das erste neue Aktivitätsprotokoll-Ereignis tritt in diesem Namespace erstellt. Wenn Sie einen Service Bus-Namespace nicht haben, müssen Sie zuerst erstellen. Aktivitätsprotokoll Ereignisse dieser Service Bus-Namespace bereits übertragen wurden, werden Ereignis-Hub, der zuvor erstellten wiederverwendet. Die freigegebene Richtlinie definiert die Berechtigungen der Streaming-Mechanismus. Heute muss auf ein Ereignis Hubs Berechtigungen **Verwalten**, **Lesen**und **Senden** . Erstellen oder ändern Service Bus-Namespace freigegebene Richtlinien im Verwaltungsportal Registerkarte "Konfigurieren" für Ihr Service Bus-Namespace. Aktualisieren Sie das Aktivitätsprotokoll Protokoll Profil umfassen streaming müssen der Änderung ListKey Berechtigung auf diesem Service Bus Autorisierungsregel.

### <a name="via-azure-portal"></a>Azure-Portal 
1. Navigieren Sie zu dem **Aktivitätsprotokoll** Blade über das Menü auf der linken Seite des Portals.

    ![Navigieren Sie im Portal Aktivitätsprotokoll](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klicken Sie **Exportieren** oben das Blade.

    ![Schaltfläche Exportieren im portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Erscheint Blatt können Sie Regionen auswählen, für die möchten Stream Ereignisse und den Service Bus-Namespace in der einen Ereignis-Hub für streaming diese Ereignisse erstellt werden sollen.

    ![Aktivitätsprotokoll Blade exportieren](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klicken Sie auf **Speichern** , um diese Einstellung zu speichern. Die Standardeinstellungen werden sofort zu Ihrem Abonnement angewendet.


### <a name="via-powershell-cmdlets"></a>Über PowerShell-Cmdlets
Wenn ein Protokoll Profil bereits vorhanden ist, müssen Sie dieses Profil entfernen.

1. Mit `Get-AzureRmLogProfile` um festzustellen, ob ein Protokoll Profil vorhanden ist
2. Verwenden Sie ggf. `Remove-AzureRmLogProfile` entfernen.
3. Mit `Set-AzureRmLogProfile` Sie ein Profil erstellen:

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

Service Bus Regel-ID ist eine Zeichenfolge mit diesem Format: {service Bus-Ressourcen-ID} /authorizationrules/ {Key Name} beispielsweise 

### <a name="via-azure-cli"></a>Über Azure-CLI
Wenn ein Protokoll Profil bereits vorhanden ist, müssen Sie dieses Profil entfernen.

1. Mit `azure insights logprofile list` um festzustellen, ob ein Protokoll Profil vorhanden ist
2. Verwenden Sie ggf. `azure insights logprofile delete` entfernen.
3. Mit `azure insights logprofile add` Sie ein Profil erstellen:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

Service Bus Regel-ID ist eine Zeichenfolge mit diesem Format: `{service bus resource ID}/authorizationrules/{key name}`.
 
## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Wie wird verwenden die Protokolldaten von Ereignis?
[Das Schema für das Aktivitätsprotokoll ist hier verfügbar](./monitoring-overview-activity-logs.md). Jedes Ereignis wird in einem Array von JSON-Blobs namens "Datensätze".

## <a name="next-steps"></a>Nächste Schritte
- [Das Aktivitätsprotokoll auf ein Speicherkonto archivieren](./monitoring-archive-activity-log.md)
- [Lesen Sie die Übersicht der Azure-Aktivitätsprotokoll](./monitoring-overview-activity-logs.md)
- [Basierend auf einem Ereignis Aktivitätsprotokoll einrichten](./insights-auditlog-to-webhook-email.md)
