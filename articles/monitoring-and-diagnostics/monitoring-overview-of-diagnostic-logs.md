<properties
    pageTitle="Übersicht über Azure Diagnoseprotokolle | Microsoft Azure"
    description="Enthält Informationen zu Azure Diagnoseprotokolle und deren Verwendung in Azure Ressource sollten."
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
    ms.date="10/12/2016"
    ms.author="johnkem; magoedte"/>

# <a name="overview-of-azure-diagnostic-logs"></a>Übersicht über Azure Diagnoseprotokolle
**Diagnoseprotokolle Azure** sind Protokolle ausgegeben, die eine Ressource, die umfangreiche, häufig Daten über den Vorgang der Ressource. Der Inhalt dieser Protokolle variiert je nach Typ (z. B. Windows System-Ereignisprotokoll ist ein Diagnoseprotokoll für VMs und Blob, Tabelle und Warteschlange Protokolle sind Kategorien Diagnoseprotokolle für Speicherkonten) und das [Aktivitätsprotokoll (ehemals Überwachungsprotokoll oder Betriebsprotokoll)](monitoring-overview-activity-logs.md), bietet Einblicke in die Vorgänge, die auf Ressourcen in Ihrem Abonnement ausgeführt wurden. Nicht alle Ressourcen unterstützen die neue Diagnoseprotokolle beschriebenen. Die folgenden unterstützten Dienste zeigt welche Ressourcentypen neue Diagnoseprotokolle unterstützen.

![Logische Anordnung von Diagnoseprotokollen](./media/monitoring-overview-of-diagnostic-logs/logical-placement-chart.png)

## <a name="what-you-can-do-with-diagnostic-logs"></a>Mit Diagnoseprotokolle möglich
Hier sind einige Dinge, die Sie mit Diagnoseprotokolle können:

- Speichern sie ein **Speicherkonto** für die Überwachung oder manuellen Überprüfung. Sie können die Retentionszeit (in Tagen) **Diagnose Settings**.
- Für die Aufnahme von einem Fremdanbieterdienst oder benutzerdefinierte Analytics Lösung PowerBI [stream sie **Ereignis-Hubs** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) .
- Mit [OMS-Protokollanalyse](../log-analytics/log-analytics-azure-storage-json.md) analysieren

## <a name="diagnostic-settings"></a>Diagnostische Settings
Diagnoseprotokolle für nicht-Serverressourcen werden diagnostische Standardeinstellungen konfiguriert. **Diagnostische Einstellungen** für eine Resource Control:

- Wo Diagnoseprotokolle (Speicherkonto, Ereignis-Hubs und OMS-Protokollanalyse) gesendet werden.
- Welche Kategorien protokollieren werden.
- Wie lange jeder Kategorie in ein Speicherkonto – Aufbewahrung Null Tage aufbewahrt werden bedeutet, dass ständig Protokolle. Andernfalls kann dieser Wert zwischen 1 und 2147483647. Speichern von Protokollen in ein Speicherkonto ist deaktiviert, wenn Aufbewahrungsrichtlinien festgelegt (z. B. nur Ereignis Hubs oder OMS Optionen), die Aufbewahrungsrichtlinien haben keine Auswirkung.

Dazu werden einfach über das Diagnose-Blade für eine Ressource in Azure-Portal über Azure PowerShell und CLI-Befehle oder über die [REST-API von Azure Monitor](https://msdn.microsoft.com/library/azure/dn931943.aspx)konfiguriert.

> [AZURE.WARNING] Diagnoseprotokolle und Metriken für Serverressourcen (z. B. VMs oder Service Fabric) verwenden [einen eigenen Mechanismus für die Konfiguration und Auswahl der Ausgaben](../azure-diagnostics.md).

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Aktivieren der Sammlung von Diagnoseprotokollen
Auflistung von Diagnoseprotokollen ermöglicht eine Ressource erstellen oder eine Ressource über die Ressource Blade im Portal erstellt. Sie können auch Diagnoseprotokolle jederzeit Azure PowerShell oder CLI-Befehle oder die REST-API von Azure überwachen.

> [AZURE.TIP] Diese Schritte gelten nicht direkt an jeder Ressource. Siehe Schema Links am Ende dieser Seite, um spezielle Schritte verstehen, die für bestimmte Ressourcen gelten.

[Dieser Artikel veranschaulicht die Verwendung einer Ressourcenvorlage auf Diagnose können beim Erstellen einer Ressource](./monitoring-enable-diagnostic-logs-using-template.md)

### <a name="enable-diagnostic-logs-in-the-portal"></a>Aktivieren von Diagnoseprotokollen im portal
Compute-Ressourcentypen erstellen ermöglicht Windows oder Linux Azure-Diagnose können Sie die Diagnoseprotokolle in Azure-Portal aktivieren:

1.  Rufen Sie **neu** , und wählen Sie die Ressource, der Sie interessieren.
2.  Nach dem Konfigurieren der grundlegenden und eine Größe im Blatt **Einstellungen** unter **Überwachung** **aktiviert** und wählen Sie ein Speicherkonto, in dem Sie die Diagnoseprotokolle speichern möchten. Normale Datenraten für Speicher und Transaktionen sind belastet werden, wenn ein Speicherkonto Diagnose schicken.

    ![Aktivieren von Diagnoseprotokollen beim Erstellen von Ressourcen](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3.  Klicken Sie auf **OK** , und erstellen Sie die Ressource zu.

Für nicht-Serverressourcen können Sie Diagnoseprotokolle in Azure-Portal aktivieren, nachdem eine Ressource erstellt wurde, wie folgt:

1.  Blade für die Ressource gehen und **Diagnose** Blade öffnen.
2.  Klicken Sie **auf** , und wählen Sie ein Speicherkonto bzw. Ereignis-Hub.

    ![Aktivieren von Diagnoseprotokollen nach Ressource erstellen](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3.  Wählen Sie unter **Protokolle**welche **Kategorien protokollieren** oder streamen möchten.
4.  Klicken Sie auf **Speichern**.

### <a name="enable-diagnostic-logs-via-powershell"></a>Diagnoseprotokolle über PowerShell aktivieren
Aktivieren Sie Diagnoseprotokolle über Azure PowerShell-Cmdlets Befehlen.

Verwenden Sie diesen Befehl, um Speicher Diagnoseprotokolle in einem Speicher-Konto zu aktivieren:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true

Die Storage-Konto-ID ist die Ressourcen-Id für das Speicherkonto, Sie die Protokolle möchten.

Verwenden Sie diesen Befehl zum streaming von Diagnoseprotokollen an einen Hub Ereignis aktivieren:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true

Service Bus Regel-ID ist eine Zeichenfolge mit diesem Format: `{service bus resource ID}/authorizationrules/{key name}`.

Verwenden Sie diesen Befehl zum Aktivieren von Diagnoseprotokollen Protokollanalyse Arbeitsbereich senden:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [log analytics workspace id] -Enabled $true

> [AZURE.NOTE] Der Parameter WorkspaceId ist nicht verfügbar in der Oktober-Ausgabe. In der November-Version verfügbar werden.

Der Protokollanalyse Workspace-ID im Azure-Portal erhalten.

Sie können diese Parameter mehrere Ausgabeoptionen können kombinieren.

### <a name="enable-diagnostic-logs-via-cli"></a>Aktivieren von Diagnoseprotokollen über CLI
Aktivieren Sie Diagnoseprotokolle über CLI Azure Befehlen:

Verwenden Sie diesen Befehl, um Speicher Diagnoseprotokolle in einem Speicher-Konto zu aktivieren:

    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true

Die Storage-Konto-ID ist die Ressourcen-Id für das Speicherkonto, Sie die Protokolle möchten.

Verwenden Sie diesen Befehl zum streaming von Diagnoseprotokollen an einen Hub Ereignis aktivieren:

    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true

Service Bus Regel-ID ist eine Zeichenfolge mit diesem Format: `{service bus resource ID}/authorizationrules/{key name}`.

Verwenden Sie diesen Befehl zum Aktivieren von Diagnoseprotokollen Protokollanalyse Arbeitsbereich senden:

    azure insights diagnostic set --resourceId <resourceId> --workspaceId <workspaceId> --enabled true

> [AZURE.NOTE] Der Parameter WorkspaceId ist nicht verfügbar in der Oktober-Ausgabe. In der November-Version verfügbar werden.

Der Protokollanalyse Workspace-ID im Azure-Portal erhalten.

Sie können diese Parameter mehrere Ausgabeoptionen können kombinieren.

### <a name="enable-diagnostic-logs-via-rest-api"></a>Diagnoseprotokolle über REST-API aktivieren
Ändern der Diagnoseeinstellungen Azure Monitor REST API finden Sie [Dieses Dokument](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-diagnostic-settings-in-the-portal"></a>Diagnostische Einstellungen im Portal verwalten

Um sicherzustellen, dass alle Ressourcen ordnungsgemäß mit Diagnose eingerichtet sind, können Sie an die **Überwachung** Blade im Portal navigieren und **Diagnoseprotokolle** Blade öffnen.

![Diagnostische Protokolle Blade im portal](./media/monitoring-overview-of-diagnostic-logs/manage-portal-nav.png)

Möglicherweise klicken Sie auf "Weitere Dienste" zu Blade überwachen.

Auf diesem Blatt können Sie anzeigen und Filtern alle Ressourcen, die unterstützen Diagnoseprotokolle, ob sie haben Diagnose aktiviert und welches Speicherkonto, Ereignis-Hub und Protokollanalyse Arbeitsbereich Protokolle übertragen werden.

![Protokolle Blade Diagnoseergebnisse im portal](./media/monitoring-overview-of-diagnostic-logs/manage-portal-blade.png)

Auf eine Ressource zeigt alle Protokolle, die im Speicherkonto gespeichert wurden und die Option Diagnose Einstellungen zu deaktivieren. Klicken Sie auf Download, um Protokolle für einen bestimmten Zeitraum herunterladen.

![Diagnostische Protokolle Blade eine Ressource](./media/monitoring-overview-of-diagnostic-logs/manage-portal-logs.png)

> [AZURE.NOTE] Diagnoseprotokolle nur in dieser Ansicht angezeigt und zum Download verfügbar sein, wenn Sie diagnostische Einstellungen speichern ein Speicherkonto konfiguriert haben.

Der Link für **Diagnostische** bringt Blatt Diagnoseeinstellungen, wo Sie aktivieren, deaktivieren oder Diagnose Einstellungen für die ausgewählte Ressource ändern.

## <a name="supported-services-and-schema-for-diagnostic-logs"></a>Unterstützte Dienste und Schema Diagnoseprotokolle
Das Schema für Diagnoseprotokolle variiert je nach Ressource und Protokolldateien. Nachfolgend werden die unterstützten Dienste und ihr Schema.

| Dienst                       | Schema & Dokumente                                                                                                   |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------|
|    Software zum Lastenausgleich     |    [Protokollanalyse Azure Lastenausgleich (Vorschau)](../load-balancer/load-balancer-monitor-log.md)             |
|    Netzwerk-Sicherheitsgruppen    |    [Protokollanalyse für Netzwerk-Sicherheitsgruppen (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)     |
|    Anwendungsgateways       |    [Diagnoseprotokolle für Application Gateway](../application-gateway/application-gateway-diagnostics.md)     |
|    Key Vault                  |    [Azure-Tresor Protokollierung](../key-vault/key-vault-logging.md)                                                 |
|    Azure Suche               |    [Aktivieren und Verwenden von Search Datenverkehr Analytics](../search/search-traffic-analytics.md)                         |
|    Datenspeicher See            |    [Diagnoseprotokolle für Azure See Datenspeicher zugreifen](../data-lake-store/data-lake-store-diagnostic-logs.md) |
|    Datenanalyse See        |    [Zugriff auf Diagnoseprotokolle für Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
|    Logik-Apps                 |    Kein Schema verfügbar.                                                                                         |
|    Azure Batch                |    [Azure Batch-Diagnoseprotokolls](../batch/batch-diagnostics.md)                                              |
|    Azure-Automatisierung           |    [Protokollanalyse Azure Automatisierung](../automation/automation-manage-send-joblogs-log-analytics.md)          |
|    Ereignis-Hub                  |    Kein Schema verfügbar.                                                                                         |
|    Servicebus                |    Kein Schema verfügbar.                                                                                         |
|    Stream Analytics           |    Kein Schema verfügbar.                                                                                         |

## <a name="supported-log-categories-per-resource-type"></a>Unterstützte Kategorien protokollieren pro Ressource

|Ressourcentyp|Kategorie|Anzeigename der Kategorie|
|---|---|---|
|Microsoft.Automation/automationAccounts|JobLogs|Aufträge|
|Microsoft.Automation/automationAccounts|JobStreams|Job-Streams|
|Microsoft.Batch/batchAccounts|ServiceLog|Dienst protokolliert|
|Microsoft.DataLakeAnalytics/accounts|Überwachung|Audit-Protokolle|
|Microsoft.DataLakeAnalytics/accounts|Anfragen|Anforderungsprotokolle|
|Microsoft.DataLakeStore/accounts|Überwachung|Audit-Protokolle|
|Microsoft.DataLakeStore/accounts|Anfragen|Anforderungsprotokolle|
|Microsoft.EventHub/namespaces|ArchiveLogs|Protokolle archivieren|
|Microsoft.EventHub/namespaces|OperationalLogs|Betriebsprotokolle|
|Microsoft.KeyVault/vaults|AuditEvent.|Audit-Protokolle|
|Microsoft.Logic/workflows|WorkflowRuntime|Workflow Runtime Ereignisse|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Netzwerksicherheitsgruppe-Ereignis|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Netzwerk-Sicherheitsgruppe Regel Zähler|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|Netzwerk-Sicherheitsgruppe Regel Flow-Ereignis|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Warnereignisse Balancer laden|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Load Balancer Prüfpunkt-Zustand|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Anwendungsprotokoll Gateway-Zugriff|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Application Gateway Performance Log|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Anwendungsprotokoll Gateway-Firewall|
|Microsoft.Search/searchServices|OperationLogs|Protokolle|
|Microsoft.ServerManagement/nodes|RequestLogs|Anforderungsprotokolle|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Betriebsprotokolle|
|Microsoft.StreamAnalytics/streamingjobs|Ausführung|Ausführung|
|Microsoft.StreamAnalytics/streamingjobs|Erstellen|Erstellen|

## <a name="next-steps"></a>Nächste Schritte
- [Stream Diagnoseprotokolle an **Ereignis Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Einstellungen Sie diagnostische mithilfe der Azure-Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931931.aspx)
- [Analyse der Protokolle mit OMS-Protokollanalyse](../log-analytics/log-analytics-azure-storage-json.md)
