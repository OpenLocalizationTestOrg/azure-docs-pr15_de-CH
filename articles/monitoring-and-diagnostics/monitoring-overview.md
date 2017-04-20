<properties
    pageTitle="Übersicht über Microsoft Azure überwachen | Microsoft Azure"
    description="Obere Überblick Überwachung und Diagnose in Microsoft Azure Alerts, Webhooks, skalieren und mehr."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"l
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="robb"/>

# <a name="overview-of-monitoring-in-microsoft-azure"></a>Übersicht über die Überwachung von Microsoft Azure

Dieser Artikel bietet eine grundlegende Übersicht über Azure Ressourcen überwachen. Es enthält Verweise auf Informationen auf bestimmte Ressourcen.  Allgemeine Informationen zur Überwachung die Anwendung aus Sicht nicht Azure finden Sie [Hinweise zur Überwachung und Diagnose](../best-practices-monitoring.md).

Cloudanwendungen sind viele bewegliche Teile komplex. Überwachung stellt Daten sicherzustellen, dass Ihre Anwendung einrichten und fehlerfrei ausgeführt. Es hilft Ihnen, Probleme abzuwehren oder über die Problembehandlung. Darüber hinaus können Sie Daten, Tiefe Einblicke über die Anwendung. Dieses Wissen kann Anwendungsperformance oder Wartungsfreundlichkeit verbessern oder Automatisierung von Vorgängen, bei denen eine manuelle Eingriffe.

Das folgende Diagramm zeigt eine grundlegende Übersicht der Azure-Überwachung, einschließlich der Protokolle, die Sie sammeln können und was Sie mit diesen Daten tun können.   

![Logisches Modell für die Überwachung und Diagnose nicht Compute-Ressourcen](./media/monitoring-overview/MonitoringAzureResources-non-compute_v3.png)

Abbildung 1: Modell zur Überwachung und Diagnose nicht Compute-Ressourcen

<br/>

![Logisches Modell für die Überwachung und Diagnose für Serverressourcen](./media/monitoring-overview/MonitoringAzureResources-compute_v3.png)

Abbildung 2: Modell zur Überwachung und Diagnose für Serverressourcen


## <a name="monitoring-sources"></a>Überwachung der Quellen
### <a name="activity-logs"></a>Aktivitätsprotokolle
Das Aktivitätsprotokoll (früher Betrieb oder Überwachungsprotokolle) suchen Sie Informationen über die Ressource von der Azure-Infrastruktur. Das Protokoll enthält Informationen wie bei Ressourcen erstellt oder zerstört werden.  

### <a name="host-vm"></a>Host VM
**Nur berechnen**


Einige Ressourcen wie Cloud Services virtuelle Computer berechnen und Service Fabric ist eine dedizierte Host VM mit. Host-VM entspricht der Stamm VM im Hyper-V-Hypervisor-Modell. In diesem Fall können Sie auf nur die VM Host neben dem Gastbetriebssystem Metriken sammeln.  

Für andere Azure Services gibt es nicht notwendigerweise eine 1:1-Zuordnung zwischen Ressource und einen bestimmten Host VM Host VM Metriken nicht verfügbar sind.


### <a name="resource---metrics-and-diagnostics-logs"></a>Ressource - Metriken und Diagnoseprotokolle
Sammlerstücke Metriken hängen von den Ressourcentyp. Z. B. virtuelle Computer Statistiken auf Disk i/o und Prozent CPU enthält. Aber diese Werte nicht für einen Service Bus-Warteschlange, die Metriken wie Warteschlange Größe und Durchsatz bietet.

Compute-Ressourcen erhalten Sie Metriken für das Gastbetriebssystem und Diagnose Module wie Azure-Diagnose. Azure Diagnostics hilft sammeln und Weiterleiten abzurufen Diagnosedaten an andere Positionen, einschließlich der Azure-Speicher.

Eine Liste der derzeit besaß Metriken steht unter [Metrik unterstützt](monitoring-supported-metrics.md).

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Anwendung – Diagnoseprotokolle Anwendungsprotokolle und Metriken
**Nur berechnen**

Programme können auf dem Gastbetriebssystem im Modell berechnen ausführen. Sie geben einen eigenen Satz von Protokollen und Metriken.

Arten von Metriken

- Leistungsindikatoren
- Anwendungsprotokolle
- Windows-Ereignisprotokolle
- .NET Quelle
- IIS-Protokolle
- Manifest-ETW-Basis
- Speicherabbilder
- Fehlerprotokolle des Kunden


## <a name="uses-for-monitoring-data"></a>Wird zur Überwachung von Daten

### <a name="visualize"></a>Visualisieren
Visualisierung der Überwachungsdaten in Grafiken und Diagrammen hilft Ihnen dabei, Trends viel schneller als die Daten durchsuchen  

Einige visualisierungsmethoden umfassen:

- Verwenden des Azure-Portals
- Routendaten Azure Anwendung Einblicke
- Weiterleitung von Daten an Microsoft PowerBI
- Das Werkzeug aus dem Archiv in Azure-Speicher gelesen oder ein Drittanbieter-Visualisierungstool verwenden live streaming Routendaten

### <a name="archive"></a>Archiv
Überwachungsdaten normalerweise in Azure-Speicher geschrieben und dort gehalten werden, bis Sie sie löschen.

Verwendungsmöglichkeiten von dieser Daten:

- Nach dem Schreiben, haben Sie andere Tools innerhalb oder außerhalb von Azure Lesen und verarbeiten.
- Sie laden die Daten lokal für die lokale Archivierung oder Ändern Ihrer Aufbewahrungsrichtlinie in die Cloud zu Daten für längere Zeit.  
- Sie lassen die Daten in Azure-Speicher, hätte Azure abhängig von der Datenmenge halten Sie bezahlen.
-

### <a name="query"></a>Abfrage
Azure Monitor REST API Plattform Befehlszeilenschnittstelle (CLI) Befehle, PowerShell-Cmdlets oder .NET SDK können Sie Zugriff auf die Daten im System oder Azure-Speicher

Beispiele:

-  Abrufen von Daten für eine benutzerdefinierte Überwachung Anwendung geschrieben haben
-  Benutzerdefinierte Abfragen erstellen und Senden von Daten an eine Fremdanbieter-Anwendung.

### <a name="route"></a>Route
Sie können Daten an andere Positionen in Echtzeit übertragen.

Beispiele:

- Anwendung Erkenntnisse senden Sie, damit dort die Visualisierungstools verwendet werden können.
- Senden Sie an Event Hubs, damit an Drittanbieter-Tools für die Analyse in Echtzeit weiterleiten kann.

### <a name="automate"></a>Automatisierung
Überwachungsdaten für Ereignisse verwenden oder sogar ganze Prozesse gehören:

- Skalierungsgröße Daten berechnen Instanzen nach oben oder unten Anwendung abhängig.
- Senden Sie e-Mails, wenn eine Metrik einen vorgegebenen Schwellenwert überschreitet.
- Rufen Sie eine URL (Webhook) zum Ausführen einer Aktion in einem System von Azure
- Starten Sie ein Runbook in Azure Automatisierung eine Vielzahl von Aufgaben

## <a name="methods-of-use"></a>Anwendung
Im Allgemeinen können Sie Daten überwachen, routing und Abruf mit einer der folgenden Methoden bearbeiten. Nicht alle Methoden sind für alle Aktionen oder Datentypen.

- [Azure-portal](https://portal.azure.com)
- [PowerShell](insights-powershell-samples.md)  
- [Plattformübergreifende Befehlszeilenschnittstelle (CLI)](insights-cli-samples.md)
- [REST-API](https://msdn.microsoft.com/library/dn931943.aspx)
- [.NET SDK](https://msdn.microsoft.com/library/dn802153.aspx)

## <a name="azures-monitoring-offerings"></a>Azure Überwachung Angebote
Azure hat zur Überwachung der bare-Metal-Infrastruktur zu anwendungstelemetrie. Die beste Strategie Überwachung kombiniert mit drei umfassenden und Einblick in den Zustand Ihrer Dienste.

- [Azure Monitor](http://aka.ms/azmondocs) – bietet Visualisierung, Abfrage, routing, Warnung, skalieren und Automatisierung auf Azure-Infrastruktur (Aktivitätsprotokoll) und jede einzelne Azure Ressource (Diagnoseprotokolle). Dieser Artikel ist Teil der Azure-Monitor-Dokumentation. Azure Monitorname erschien September 25 beim entzünden 2016.  Der vorherige Name wurde "Azure Einblicke."  
- [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) – bietet umfassende Erkennung und Diagnose Probleme auf der Anwendungsschicht des Dienstes, integrierte Daten von Azure überwachen. Es ist die Diagnose Standardplattform für App Service Web Apps.  Sie können Daten von anderen Diensten weiterleiten.  
- [Protokollanalyse](https://azure.microsoft.com/documentation/services/log-analytics/) Teil [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) – bietet eine ganzheitliche IT-Management-Lösung für lokalen und Drittanbieter-Cloud-basierte Infrastruktur (z. B. AWS) neben Azure-Ressourcen.  Daten von Azure Monitor können direkt an Protokollanalyse weitergeleitet werden Metriken und Protokolle für die gesamte Umgebung zentral zu sehen.     


## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über

- [Azure Monitor Video entzünden 2016](https://myignite.microsoft.com/videos/4977)
- [Erste Schritte mit Azure](monitoring-get-started.md)
- [Azure-Diagnose](../azure-diagnostics.md) Wenn Sie zur diagnose von Problemen in der Cloud-Dienst, virtuellen oder Service Fabric-Anwendung.
- [Anwendung Einblicke](https://azure.microsoft.com/documentation/services/application-insights/) Wenn Diagnose Probleme in Ihrer Anwendung App Service möchten.
- [Problembehandlung bei Azure Storage](../storage/storage-e2e-troubleshooting.md) bei Speicher-Blobs, Tabellen oder Warteschlangen verwenden
- [Protokollanalyse](https://azure.microsoft.com/documentation/services/log-analytics/) und [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite)
