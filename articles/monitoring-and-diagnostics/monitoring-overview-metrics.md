<properties
    pageTitle="Übersicht der Metrik in Microsoft Azure | Microsoft Azure"
    description="Übersicht über Metriken und ihre Verwendung in Microsoft Azure"
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="ashwink"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Übersicht über Microsoft Azure Kennzahlen 

Dieser Artikel beschreibt, welche Metriken in Microsoft Azure sind ihre Vorteile und zu verwenden.  

## <a name="what-are-metrics"></a>Was sind Kriterien?

Azure-Monitor können Sie Telemetrie Einblick in die Leistung und der Zustand der Arbeitslasten auf Azure zu nutzen. Die wichtigsten Azure Telemetriedaten sind Metriken (auch Indikatoren genannt) am Azure Ressourcen ausgegeben. Azure Monitor bietet verschiedene Methoden zum Konfigurieren und verwenden diese Metriken zur Überwachung und Problembehandlung.


## <a name="what-can-you-do-with-metrics"></a>Was können Sie mit Metriken?

Metriken sind wertvolle Telemetrie und ermöglichen Ihnen Folgendes:

- **Nachverfolgen der Leistung** der Ressource (wie VM, Website oder Anwendung Logik) von Metriken Portal Diagramm zeichnen und festhalten, Diagramm einem Dashboard.
- **Problem benachrichtigt** die Leistung der Ressource beeinträchtigen, wenn eine Metrik einen bestimmten Schwellenwert überschreitet.
- **Automatisierte Aktionen konfigurieren**oder Skalierung eine Ressource ein Runbook auslösen, wenn eine Metrik einen bestimmten Schwellenwert überschreitet.
- **Erweiterte Analysen durchführen** oder Berichte zur Leistung oder Nutzung Trends der Ressourcen.
- **Archiv** der Leistung oder Gesundheit Historie Ihre Ressource **für Compliance Prüfung** Zwecke.

##  <a name="metric-characteristics"></a>Metrische Merkmale
Metriken haben folgende Eigenschaften:

- Alle Vorgaben wurden **1-Minuten-Intervall**. Sie erhalten einen Metrikwert jede Minute aus der Ressource in Echtzeit Einblick in den Zustand und Status der Ressource.
- Metriken sind **verfügbar - der-vordefinierten ohne anmelden** oder zusätzliche Diagnose einrichten.
- Sie können **30 Tage** für jede Metrik zugreifen. Sie können schnell aktuelle und monatlichen Trends oder die Gesundheit der Ressource anzeigen.

Sie können:

- Erkunden, Zugriff und **alle Metriken** über Azure-Portal, wenn Sie eine Ressource und in einem Diagramm darstellen. 
- Konfigurieren Sie eine Metrik **Warnregel, die eine Benachrichtigung oder automatisierte Aktion,** , wenn die Metrik den Schwellenwert überschreitet, den Sie eingerichtet haben. Skalieren ist eine besondere automatisierte Aktion, mit der Sie Ressource entsprechen der Anfragen auf Ihrer Website laden oder Datenverarbeitungsressourcen skalieren. Basierend auf einer Schwelle Metrik können Sie eine Regel skalieren Einstellung Out in Skalieren konfigurieren.
- **Archiv** -Metriken für mehr oder offline reporting. Sie leiten die Metriken BLOB-Speicher, wenn Sie Diagnose für die Ressource zu konfigurieren.
- **Stream** -Metriken mit einem Ereignis-Hub ermöglicht das anschließend Azure Stream Analytics oder benutzerdefinierte apps Echtzeitanalyse weiterleiten. Sie können dazu die Diagnose Settings.
- **Route** alle Metriken OMS-Protokollanalyse instant Analytics suchen und benutzerdefinierte Warnung Metriken Daten Ihrer Ressourcen zu entsperren.
- **Verbrauchen** die Metriken über neue Azure Monitor REST-APIs.
- **Abfrage** -Metriken mit PowerShell-Cmdlets oder plattformübergreifende REST-API.

 ![Routing von Metriken in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview0.png)

## <a name="access-metrics-via-portal"></a>Metriken Portal Zugriff
Hier ist eine kurze Anleitung metrische Diagramm Azure-Portal erstellen

### <a name="view-metrics-after-creating-a-resource"></a>Anzeigen von Metriken nach dem Erstellen einer Ressource
1. Azure-Portal öffnen
2. Erstellen Sie eine neue App-Service - Website.
3. Nach dem Erstellen einer Websites werden Sie des Blades Übersicht der Website.
4. Sie können neue Kennzahlen als Kachel "Überwachung" anzeigen. Bearbeiten die Kachel und wählen Sie weitere

 ![Metriken für eine Ressource in Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview1.png)    

### <a name="access-all-metrics-in-a-single-place"></a>Zugriff auf alle Metriken an einem einzigen Ort
1. Azure-Portal öffnen 
2. Navigieren Sie zu der neuen Registerkarte Monitor und wählen Sie die Option Kennzahlen 
3. Sie können Ihr Abonnement, Ressourcengruppe und den Namen der Ressource aus der Dropdownliste auswählen. 
4. Sie können jetzt die Liste der verfügbaren Metriken anzeigen. 
5. Wählen Sie die Metrik interessieren und planen. 
6. Sie können es zum Dashboard Pin auf der oberen rechten Ecke auf fixieren.

 ![Alle Kennzahlen in einer einzigen Stelle Azure-Monitor zugreifen](./media/monitoring-overview-metrics/MetricsOverview2.png) 


>[AZURE.NOTE] Ohne jede weitere Diagnose können Sie Host-Ebene Metriken VMs (Azure Ressourcen-Manager basiert) und VM Maßstab legt zugreifen. Diese neuen Hostebene Metriken sind für Windows- und Linux-Instanzen verfügbar. Diese Metriken sind nicht zu verwechseln mit dem Gastbetriebssystem Metriken auf Anwendungsebene, die Sie auf Azure-Diagnose auf dem virtuellen Computer oder VMSS einschalten. Weitere Informationen zur Konfiguration von Azure-Diagnose finden Sie unter [Was ist Microsoft Azure-Diagnose](../azure-diagnostics.md).

## <a name="access-metrics-via-rest-api"></a>Access-Metriken über REST API
Azure Metriken Azure Monitor APIs erreichbar. Gibt es zwei APIs, mit denen Sie ermitteln und Metriken zugreifen. Verwenden der: 

- [Azure Monitor Metrik Definitionen REST-API](https://msdn.microsoft.com/library/mt743621.aspx) auf der Liste der Metriken für einen Service verfügbar.
- [Azure Monitor Metriken REST API](https://msdn.microsoft.com/library/mt743622.aspx) für den tatsächlichen Metriken Datenzugriff

>[AZURE.NOTE] Dieser Artikel behandelt die Metriken über die [neue API für Metriken](https://msdn.microsoft.com/library/dn931930.aspx) für Azure-Ressourcen. API für die neuen Metrik Definitionen API ist 2016-03-01 und für Metriken API ist 2016-09-01. Ältere metrische Definitionen und Metriken mit 2014-04-01 API-Version möglich.

Eine ausführliche exemplarische Vorgehensweise Azure Monitor REST APIs finden Sie unter [Exemplarische Vorgehensweise Azure Monitor REST API](monitoring-rest-api-walkthrough.md).

## <a name="export-options-for-metrics"></a>Exportoptionen für Metriken
Sie können zur Diagnose Protokolle Blade Registerkarte Monitor und die Exportoptionen für Metriken anzeigen. Metriken und Diagnoseprotokolle BLOB-Speicher Ereignis Hubs oder OMS-Protokollanalyse weitergeleitet, wählen Sie für die in diesem Artikel erwähnten Anwendungsfälle. 

 ![Exportoptionen für Metriken in Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview3.png)   

Sie können dies über [PowerShell](insights-powershell-samples.md), [CLI](insights-cli-samples.md) oder [Anderen APIs](https://msdn.microsoft.com/library/dn931943.aspx)Ressourcenmanager Vorlagen konfigurieren. 

## <a name="take-action-on-metrics"></a>Maßnahmen Kennzahlen
Sie können Benachrichtigungen oder automatisierte Aktionen auf Daten. Sie können Warnregeln bzw. skalieren möchten.

### <a name="alert-rules"></a>Warnregeln
Warnregeln können Kennzahlen. Diese Warnungsregeln überprüfen, ob eine Metrik einen bestimmten Schwellenwert überschritten wurde per e-Mail benachrichtigt und Auslösen einer Webhook verwendet werden kann, um jedes benutzerdefinierte Skript auszuführen. Die Webhook können Sie 3. Produktintegrationen konfigurieren.

 ![Metriken und Warnregeln in Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale"></a>Automatische Skalierung
Azure Ressourcen unterstützen mehrere Instanzen oder Skalierung Ihre Arbeitslasten zu behandeln. Skalierungsgröße gilt für Anwendungsdienste (webapps), Virtual Machine Maßstab legt (VMSS) und klassischen Clouddienste. Sie können Regeln skalieren oder skalieren, wenn eine bestimmte Metrik beeinträchtigen Ihre Arbeitslast einen Schwellenwert überschreitet, den Sie angeben. Weitere Informationen finden Sie unter [Übersicht über automatische Skalierung](monitoring-overview-autoscale.md).

 ![Metriken und skalieren in Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="supported-services-and-metrics"></a>Unterstützte Dienste und Metriken
Azure Monitor ist eine neue Metrik Infrastruktur. Es bietet Unterstützung für die folgenden Azure Dienste in Azure-Portal und die neue Version der Azure-Monitor-API:

- VMs (Azure Ressourcen-Manager basiert)
- Die Skalierung legt VM
- Stapelverarbeitung
- Ereignis-Hub namespace 
- Service Bus-Namespace (nur Premium-SKU)
- SQL (Version 12)
- Elastische SQL-Pool
- Websites
- Webserver-Farmen
- Logik-Apps
- IoT Hubs
- Redis Cache
- Netzwerk: Anwendungsgateways
- Suche

Können Sie eine detaillierte Liste der unterstützten Dienste und deren Kriterien auf [Azure Monitor Metriken - unterstützten Metriken pro Ressourcentyp](monitoring-supported-metrics.md). 


## <a name="next-steps"></a>Nächste Schritte

Verweisen Sie auf die Links in diesem Artikel. Darüber hinaus erfahren Sie:  

- über [gemeinsame Kriterien für die automatische Skalierung](insights-autoscale-common-metrics.md)
- Erstellen von [Warnregeln](insights-alerts-portal.md)




