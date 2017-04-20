<properties
    pageTitle="Übersicht über Azure Aktivitätsprotokoll | Microsoft Azure"
    description="Erfahren Sie, was der Azure-Aktivitätsprotokoll ist und wie Sie innerhalb Ihrer Azure-Abonnement sollten verwenden."
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
    ms.date="10/25/2016"
    ms.author="johnkem"/>

# <a name="overview-of-the-azure-activity-log"></a>Übersicht über Azure Aktivitätsprotokoll
Das **Aktivitätsprotokoll Azure** ist ein Protokoll, das einen Einblick in die Vorgänge enthält, die auf Ressourcen in Ihrem Abonnement ausgeführt wurden. Das Aktivitätsprotokoll wurde früher als "Audit Logs" oder "Betriebsprotokolle" da Steuerungsebene Ereignisse für Ihre Abonnements gibt. Das Aktivitätsprotokoll verwenden, Sie bestimmen die ', und ' für (PUT, POST, DELETE), die für die Ressourcen in Ihrem Abonnement Schreibvorgänge. Sie können auch den Status des Vorgangs und anderen relevanten Eigenschaften verstehen. Das Aktivitätsprotokoll enthält keine Lesevorgänge (GET).

Das Aktivitätsprotokoll unterscheidet sich von [Diagnoseprotokollen](./monitoring-overview-of-diagnostic-logs.md)sind alle Protokolle, die von einer Ressource ausgegeben. Diese Protokolle enthalten Informationen über den Vorgang der Ressource statt auf die Ressource.

Ereignisse aus der Azure-Portal CLI PowerShell-Cmdlets mit Aktivitätsprotokoll abrufen und Azure Monitor REST-API.

Diese- [video Einführung Aktivitätsprotokoll](https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz)anzeigen.  

## <a name="what-you-can-do-with-the-activity-log"></a>Aktivitäts-Log möglich
Hier sind einige Dinge, die Sie mit dem Aktivitätsprotokoll können:

- Fragen Sie ab und in **Azure-Portal**.
- Fragen Sie über REST-API, PowerShell-Cmdlets oder CLI ab.
- [Erstellen Sie eine e-Mail- oder Webhook-Warnung, die ein Aktivitätsprotokoll-Ereignis auslöst.](./insights-auditlog-to-webhook-email.md)
- [Ein **Speicherkonto** für die Archivierung oder manuelle Überprüfung speichern](./monitoring-archive-activity-log.md). Die Retentionszeit (in Tagen) **Profilen anmelden**können.
- PowerBI mit [**PowerBI Content Pack**](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/)analysieren.
- [Damit ein **Ereignis Hub** stream](./monitoring-stream-activity-logs-event-hubs.md) für die Aufnahme von einem Fremdanbieterdienst oder benutzerdefinierte Analytics Lösung PowerBI.

## <a name="export-the-activity-log-with-log-profiles"></a>Das Aktivitätsprotokoll mit Protokoll exportieren
**Protokoll Profil** steuert, wie die Aktivitätsprotokoll exportiert wird. Profil-Protokoll verwenden, können Sie Folgendes konfigurieren:

- Wo das Aktivitätsprotokoll (Speicherkonto oder Ereignis Hubs) gesendet werden soll
- Welche Kategorien (schreiben, löschen, Aktion) sollte gesendet werden
- Welche Bereiche (Speicherorte) exportiert werden sollen
- Wie lange das Aktivitätsprotokoll in ein Speicherkonto – Aufbewahrung Null Tage aufbewahrt werden bedeutet, ständig Protokolle. Andernfalls kann der Wert eine beliebige Anzahl von Tagen zwischen 1 und 2147483647 sein. Wenn Aufbewahrungsrichtlinien festgelegt, jedoch in ein Speicherkonto Protokolle speichern (z. B. wenn nur Ereignis Hubs oder OMS Optionen ausgewählt sind) deaktiviert ist, sind die Aufbewahrungsrichtlinien wirkungslos.

Diese Einstellung kann über die Option "Exportieren" in das Aktivitätsprotokoll Blade im Portal konfiguriert werden. Sie können auch konfigurierten programmgesteuert [mithilfe von Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShell-Cmdlets oder CLI sein. Ein Abonnement kann nur ein Protokoll Profil haben.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Konfigurieren Sie Protokoll Profile Azure-portal
Stream Aktivitätsprotokoll an Event Hub oder mithilfe der Option "Exportieren" im Azure-Portal in ein Speicherkonto speichern können.

1. Navigieren Sie zu dem **Aktivitätsprotokoll** Blade über das Menü auf der linken Seite des Portals.

    ![Navigieren Sie im Portal Aktivitätsprotokoll](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klicken Sie **Exportieren** oben das Blade.

    ![Schaltfläche Exportieren im portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Auf dem Blatt angezeigt wird, können Sie Folgendes auswählen:  
    - Bereiche, für die Sie Ereignisse exportieren möchten
    - das Speicherkonto, Ereignisse speichern möchten
    - die Anzahl der Tage, die diese Ereignisse im Speicher beibehalten werden soll. Eine Einstellung von 0 Tagen behält immer die Protokolle.
    - der Service Bus-Namespace in der einen Ereignis-Hub für streaming diese Ereignisse erstellt werden sollen.

    ![Aktivitätsprotokoll Blade exportieren](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klicken Sie auf **Speichern** , um diese Einstellung zu speichern. Die Standardeinstellungen werden sofort zu Ihrem Abonnement angewendet.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Konfigurieren Sie Protokoll Profile Azure PowerShell-Cmdlets
#### <a name="get-existing-log-profile"></a>Vorhandene Protokolldatei Profil
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Hinzufügen eines Profils anmelden
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Eigenschaft         | Erforderlich | Beschreibung   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name             | Ja      | Name des Profils anmelden.                                 |
| StorageAccountId | Nein       | Ressourcen-ID des Speicherkonto, das Aktivitätsprotokoll gespeichert werden soll.                         |
| serviceBusRuleId | Nein       | Service Bus Regel-ID für den Service Bus-Namespace möchten Sie Ereignis-Hubs in erstellt haben. Eine Zeichenfolge mit diesem Format: `{service bus resource ID}/authorizationrules/{key name}`. |
| Standorte        | Ja      | Durch Kommas getrennte Liste der Regionen für die Aktivitätsprotokoll Ereignisse erfassen möchten.              |
| RetentionInDays  | Ja      | Anzahl von Tagen zwischen 1 und 2147483647, welche Ereignisse beibehalten werden soll. Speichert der Wert NULL Protokolle unbegrenzt (unbegrenzt). |
| Kategorien       | Nein       | Durch Kommas getrennte Liste der Ereigniskategorien, die erfasst werden. Mögliche Werte sind schreiben, löschen und Aktion.                                 |

#### <a name="remove-a-log-profile"></a>Protokoll Profil entfernen
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Konfigurieren Sie Protokoll Profile CLI plattformübergreifende Azure
#### <a name="get-existing-log-profile"></a>Vorhandene Protokolldatei Profil
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
Die `name` -Eigenschaft sollte den Namen des Profils anmelden.

#### <a name="add-a-log-profile"></a>Hinzufügen eines Profils anmelden
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Eigenschaft         | Erforderlich | Beschreibung   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name             | Ja      | Name des Profils anmelden.                                 |
| storageId        | Nein       | Ressourcen-ID des Speicherkonto, das Aktivitätsprotokoll gespeichert werden soll.                         |
| serviceBusRuleId | Nein       | Service Bus Regel-ID für den Service Bus-Namespace möchten Sie Ereignis-Hubs in erstellt haben. Eine Zeichenfolge mit diesem Format: `{service bus resource ID}/authorizationrules/{key name}`. |
| Standorte        | Ja      | Durch Kommas getrennte Liste der Regionen für die Aktivitätsprotokoll Ereignisse erfassen möchten.              |
| retentionInDays  | Ja      | Anzahl von Tagen zwischen 1 und 2147483647, welche Ereignisse beibehalten werden soll. Speichert der Wert NULL Protokolle unbegrenzt (unbegrenzt).     |
| Kategorien       | Nein       | Durch Kommas getrennte Liste der Ereigniskategorien, die erfasst werden. Mögliche Werte sind schreiben, löschen und Aktion.                                 |

#### <a name="remove-a-log-profile"></a>Protokoll Profil entfernen
```
azure insights logprofile delete --name my_log_profile
```

## <a name="event-schema"></a>Ereignisschema
Jedes Ereignis im Aktivitätsprotokoll enthält einen JSON-Blob entspricht dem folgenden Beispiel:

```
{
  "value": [ {
    "authorization": {
      "action": "microsoft.support/supporttickets/write",
      "role": "Subscription Admin",
      "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
    },
    "caller": "admin@contoso.com",
    "channels": "Operation",
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
    },
    "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "description": "",
    "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
    "eventName": {
      "value": "EndRequest",
      "localizedValue": "End request"
    },
    "eventSource": {
      "value": "Microsoft.Resources",
      "localizedValue": "Microsoft Resources"
    },
    "httpRequest": {
      "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
      "clientIpAddress": "192.168.35.115",
      "method": "PUT"
    },
    "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
    "level": "Informational",
    "resourceGroupName": "MSSupportGroup",
    "resourceProviderName": {
      "value": "microsoft.support",
      "localizedValue": "microsoft.support"
    },
    "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
    "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "operationName": {
      "value": "microsoft.support/supporttickets/write",
      "localizedValue": "microsoft.support/supporttickets/write"
    },
    "properties": {
      "statusCode": "Created"
    },
    "status": {
      "value": "Succeeded",
      "localizedValue": "Succeeded"
    },
    "subStatus": {
      "value": "Created",
      "localizedValue": "Created (HTTP Status Code: 201)"
    },
    "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
    "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
    "subscriptionId": "s1"
  } ],
"nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```

| Elementnamen         | Beschreibung             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Autorisierung        | BLOB RBAC-Eigenschaften des Ereignisses. Normalerweise enthält die Eigenschaften "Action", "Role" und "Bereich".|
| Anrufer               | E-Mail-Adresse des Benutzers, der die Operation UPN-Anspruch oder SPN Anspruch auf Verfügbarkeit ausgeführt hat.|
| Kanäle             | Die folgenden Werte: "Admin", "Betrieb"|
| correlationId        | In der Regel eine GUID im Zeichenfolgenformat. Ereignisse, die eine CorrelationId Teilen gehören die gleiche Uber-Aktion.   |
| Beschreibung          | Statischer Text-Beschreibung eines Ereignisses.                              |
| eventDataId          | Eindeutiger Bezeichner des Ereignisses.    |
| Ereignisquelle          | Name der Azure Service oder Infrastruktur, die dieses Ereignis generiert hat.    |
| httpRequest          | BLOB, beschreibt die HTTP-Anforderung. Enthält in der Regel "ClientRequestId", "ClientIpAddress" und "Methode" (HTTP-Methode. Zum Beispiel setzen).                            |
| Ebene                | Ebene des Ereignisses. Die folgenden Werte: "Kritisch", "Fehler", "Warning", "Information" und "Verbose"  |
| resourceGroupName    | Name der Ressourcengruppe für die betroffenen Ressource.               |
| resourceProviderName | Namen des Ressourcenanbieters für betroffene Ressourcen             |
| resourceUri          | Ressourcen-Id der betroffenen Ressource.                               |
| operationId          | Eine GUID, die gemeinsam die Ereignisse, die einem Vorgang entsprechen.         |
| operationName        | Name des Vorgangs.  |
| Eigenschaften           | Festlegen von `<Key, Value>` Paar (ein Wörterbuch) beschreibt die Details des Ereignisses.                                |
| Status               | Eine Zeichenfolge, die den Status des Vorgangs. Einige allgemeine Werte: begonnen, In Bearbeitung, erfolgreich, Fehler aktiv gelöst.                                |
| Untergeordneter            | Normalerweise der HTTP-Statuscode der entsprechenden Rest aufrufen, aber auch andere Zeichenfolgen beschreiben ein untergeordneter wie diese gemeinsamen Werte: OK (HTTP-Statuscode: 200), erstellt (HTTP-Statuscode: 201) akzeptiert (HTTP-Statuscode: 202), wird kein Inhalt (HTTP-Statuscode: 204), ungültige Anforderung (HTTP-Statuscode: 400), wurde nicht gefunden (HTTP-Statuscode: 404), Konflikt (HTTP-Statuscode : 409), interner Serverfehler (HTTP-Statuscode: 500) Service nicht verfügbar (HTTP-Statuscode: 503), Gateway-Timeout (HTTP-Statuscode: 504). |
| eventTimestamp       | Timestamp, als das Ereignis vom Azure-Dienst verarbeitet die Anforderung entspricht das Ereignis generiert wurde.     |
| submissionTimestamp  | Timestamp, als das Ereignis für die Abfrage wurde.             |
| subscriptionId       | Azure-Abonnement-Id.  |
| nextLink             | Fortsetzungstoken Abrufen die nächste Gruppe von Ergebnissen, wenn sie in mehreren Antworten aufgeteilt werden. Normalerweise erforderlich, wenn mehr als 200 Datensätze vorhanden sind.     |

## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr über das Aktivitätsprotokoll (früher Überwachungsprotokolle)](../resource-group-audit.md)
- [Stream Azure Aktivitätsprotokoll zu Ereignis Hubs](./monitoring-stream-activity-logs-event-hubs.md)
