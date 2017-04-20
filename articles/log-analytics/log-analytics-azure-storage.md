<properties
    pageTitle="Datensammlung Azure-Speicher Protokollanalyse Übersicht | Microsoft Azure"
    description="Azure-Ressourcen können Protokolle und Metriken Azure Storage-Konto häufig schreiben mithilfe der Azure-Diagnose. Protokollanalyse können diese Indexdaten und durchsuchbar zu machen."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="collecting-azure-storage-data-in-log-analytics-overview"></a>Datensammlung Azure-Speicher Protokollanalyse-Übersicht

Viele Azure Ressourcen können Protokolle und Metriken in Azure Speicherkonto schreiben. Protokollanalyse können diese Daten und vereinfachen Ihre Azure Ressourcen überwachen.

Azure-Speicher schreiben kann eine Ressource Azure Diagnostics oder eigene Möglichkeit, Daten zu schreiben. Diese Daten können in verschiedenen Formaten an folgenden Speicherorten geschrieben werden:

+ Azure-Tabelle
+ Azure blob
+ EventHub

Protokollanalyse unterstützt Azure Services, die Daten mithilfe von [Azure Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)geschrieben. Protokollanalyse unterstützt darüber hinaus andere Dienste, die Protokolle und Metriken in verschiedenen Formaten und ausgegeben.  

>[AZURE.NOTE] Sie sind normale Azure Datenraten für Speicher und Transaktionen, wenn Sie ein Speicherkonto Diagnose senden und wenn Protokollanalyse Daten aus Ihrem Speicherkonto liest berechnet.

![Diagramm der Azure-Speicher](media/log-analytics-azure-storage/azure-storage-diagram.png)

## <a name="supported-azure-resources"></a>Unterstützt Azure-Ressourcen

Protokollanalyse können Datensammlung für Azure Folgendes:

| Ressourcentyp | Protokolle (Diagnose Kategorien) | Protokoll-Analyse-Lösung |
| --------------------------------------- | -------------------------------- | --------------- |
| Anwendung Einblicke | Verfügbarkeit <br> Benutzerdefinierte Ereignisse <br> Ausnahmen <br> Anfragen <br> | Anwendung Einblicke (Vorschau) |
| API-Management | | *keine* (Vorschau) |
| Automatisierung <br> Microsoft.Automation/AutomationAccounts | JobLogs <br> JobStreams          | AzureAutomation (Vorschau) |
| Key Vault <br> Microsoft.KeyVault/Vaults               | AuditEvent.                       | Schlüsseltresor (Vorschau) |
| Application Gateway <br> Microsoft.Network/ApplicationGateways   | ApplicationGatewayAccessLog <br> ApplicationGatewayPerformanceLog | AzureNetworking (Vorschau) |
| Netzwerk-Sicherheitsgruppe <br> Microsoft.Network/NetworkSecurityGroups | NetworkSecurityGroupEvent <br> NetworkSecurityGroupRuleCounter | AzureNetworking (Vorschau) |
| Service Fabric                          | ETWEvent <br> Operationelles Ereignis <br> Zuverlässige Akteur-Ereignis <br> Zuverlässiger Service-Ereignis| ServiceFabric (Vorschau) |
| Virtuelle Computer | Linux-Syslog <br> Windows-Ereignis <br> IIS-Protokoll <br> Windows ETWEvent | *keine* |
| Webrollen <br> Worker-Rollen | Linux-Syslog <br> Windows-Ereignis <br> IIS-Protokoll <br> Windows ETWEvent | *keine* |

>[AZURE.NOTE] Überwachen von Azure virtuelle Computer (Linux und Windows), empfehlen wir [Log Analytics VM-Erweiterung](log-analytics-azure-vm-extension.md)installieren. Der Agent ermöglicht tiefere Einblicke in Ihre virtuellen Computer als bei der Diagnose in den Speicher geschrieben.

Sie können zusätzliche Protokolle für OMS stimmen die [Feedback-Seite](http://feedback.azure.com/forums/267889-azure-log-analytics/category/88086-log-management-and-log-collection-policy)analysieren Prioritäten helfen.


- Siehe [Analysieren Azure Diagnoseprotokolle Protokollanalyse mit](log-analytics-azure-storage-json.md) erfahren wie Protokollanalyse Protokolle von Azure Services lesen kann, die [Azure Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)unterstützen:
  - Azure-Tresor
  - Azure-Automatisierung
  - Application Gateway
  - Netzwerk-Sicherheitsgruppen
- [Mit BLOB-Speicher für IIS und Tabellenspeicher für Ereignisse](log-analytics-azure-storage-iis-table.md) zu erfahren, wie Protokollanalyse Protokolle lesen können, Azure, Schreiben Diagnose Tabelle oder IIS-Protokolle geschrieben BLOB-Speicher, Services einschließlich:
  - Service Fabric
  - Webrollen
  - Worker-Rollen
  - Virtuelle Computer


Anwendung Informationen ist in privaten Vorschau und kontinuierlichen Export auf BLOB-Speicher verwendet. Verbinden die private Vorschau wenden Sie sich an Ihrem Microsoft Account Team oder sehen Sie die [Feedback-Site](https://feedback.azure.com/forums/267889-log-analytics/suggestions/6519248-integration-with-app-insights).

## <a name="next-steps"></a>Nächste Schritte

- [Analysieren von Azure Diagnoseprotokolle Protokollanalyse mit](log-analytics-azure-storage-json.md) Azure Protokolle gelesen Dienste, Schreiben Diagnose BLOB-Speicher im JSON-Format.
- [Mit BLOB-Speicher für IIS und Tabellenspeicher für Ereignisse](log-analytics-azure-storage-iis-table.md) die Protokolle für Azure services, Schreiben Diagnose Tabelle oder IIS-Protokolle auf BLOB-Speicher geschrieben.
- [Projektmappen können](log-analytics-add-solutions.md) Einblick in die Daten.
- [Verwenden von Suchabfragen](log-analytics-log-searches.md) zum Analysieren der Daten.
