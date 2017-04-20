<properties
   pageTitle="Lastenausgleich Vorgänge, Ereignisse und Leistungsindikatoren überwachen | Microsoft Azure"
   description="Erfahren Sie, wie Warnereignisse und Prüfpunkt Health-Protokollierung für Azure-Lastenausgleich"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="log-analytics-for-azure-load-balancer-preview"></a>Protokollanalyse Azure Lastenausgleich (Vorschau)

Sie können verschiedene Protokolle in Azure Verwaltung und Problembehandlung von Lastenausgleich. Einige dieser Protokolle können über das Portal zugegriffen werden. Alle Protokolle können aus einem Azure blobspeicher extrahiert und in anderen Tools wie Excel und PowerBI angezeigt werden. Erfahren Sie mehr über die verschiedenen Protokolle aus der Liste.

- **Überwachungsprotokolle:** [Überwachungsprotokolle Azure](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) (ehemals Betriebsprotokolle) können Sie alle Vorgänge übermittelt der Azure-Abonnements und deren Status anzuzeigen. Überwachungsprotokolle sind standardmäßig aktiviert und können im Azure-Portal angezeigt werden.
- **Warnung Ereignisprotokolle:** Dieses Protokoll können Sie anzeigen, welche Warnungen für Lastenausgleich ausgelöst werden. Der Status des Lastenausgleichs werden alle fünf Minuten. Dieses Protokoll wird nur geschrieben, wenn eine Load Balancer-Warnung ausgelöst wird.
- **Health Prüfpunkt Protokolle:** Dieses Protokoll können Sie prüfen, Prüfpunkt Kontrollkästchen Zustand, wie viele Instanzen in Load Balancer Backend und Prozentsatz der virtuellen Maschinen Netzwerkverkehr vom Lastenausgleicher empfangen werden. Dieses Protokoll wird auf Änderung des Prüfpunkts Ereignis geschrieben.

>[AZURE.IMPORTANT] Protokollieren von Analytics arbeitet nur mit Verbindung zum Internet zum Lastenausgleich. Protokolle sind nur für Ressourcen in der Ressourcen-Manager-Bereitstellungsmodell. Protokolle können keine Ressourcen im klassischen Bereitstellungsmodell. Weitere Informationen über die Bereitstellungsmodelle finden Sie unter [Understanding Ressourcenmanager und klassischen Bereitstellung](../../articles/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Aktivieren der Protokollierung

Audit-Protokollierung wird automatisch für jede Ressource Ressourcen-Manager aktiviert. Sie müssen Ereignis und Gesundheit Prüfpunkt Protokollierung zum Sammeln der Daten über die Protokolle aktivieren. Gehen Sie folgendermaßen vor, um Protokollierung zu aktivieren.

Anmelden bei [Azure-Portal](http://portal.azure.com). Haben Sie bereits ein Lastenausgleich [ein Lastenausgleich erstellen](load-balancer-get-started-internet-arm-ps.md) , bevor Sie fortfahren.

1. Klicken Sie im Portal auf **Durchsuchen**.
2. Wählen Sie **zum Lastenausgleich**.

    ![Portal - Lastenausgleich](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Wählen Sie eine vorhandene Lastenausgleich >> **Alle Einstellungen**.
4. Auf der rechten Seite des Dialogfelds unter dem Namen Lastenausgleich einen Bildlauf zu **Überwachen**, klicken Sie auf **Diagnose**.

    ![Portal - Load-Balancer-settings](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. Wählen Sie im Bereich **Diagnostics** unter **Status** **auf**.
6. Klicken Sie auf **Speichern**.
7. Unter **Protokolle**ein Storage-Konto auswählen oder einen neuen erstellen. Verwenden Sie den Schieberegler, um zu bestimmen, wie viele Tage Ereignis Dat in den Ereignisprotokollen bleibt. 8. Klicken Sie auf **Speichern**.

    ![Portal - Diagnoseprotokolle](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

>[AZURE.INFORMATION] Audit-Protokolle erfordern keine separaten Speicherkonto. Die Verwendung Speicher für Ereignis und Gesundheit Prüfpunkt Protokollierung Gebühren anfallen.

## <a name="audit-log"></a>Prüfprotokoll

Das Überwachungsprotokoll wird standardmäßig generiert. Die Protokolle werden 90 Tage lang in Azure Ereignisprotokolle Speicher beibehalten. Erfahren Sie mehr über diese Protokolle lesen [Ereignisse anzeigen und Überwachungsprotokolle](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="alert-event-log"></a>Warnung Ereignisprotokoll

Dieses Protokoll wird nur generiert, wenn Sie es aktiviert haben eine pro Load Balancer. Die Ereignisse werden im JSON-Format protokolliert und gespeichert im Speicherkonto angegebenen, wenn die Protokollierung aktiviert. Folgendes ist ein Beispiel für ein Ereignis.

    {
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }

Die JSON-Ausgabe zeigt die *Eventname* -Eigenschaft die beschreiben den Grund für den Lastenausgleich erstellt eine Warnung. In diesem Fall wurde die Warnung generiert durch TCP-Port Erschöpfung Source IP NAT Grenzwerte (SNAT) zurückzuführen.

## <a name="health-probe-log"></a>Gesundheit Prüfpunkt Protokoll

Dieses Protokoll wird nur generiert, wenn Sie es aktiviert haben eine pro Load Balancer wie oben. Die Daten werden im Speicherkonto gespeichert angegebenen, wenn die Protokollierung aktiviert. Ein Container mit dem Namen "Einblicke Protokolle Loadbalancerprobehealthstatus" erstellt und die folgenden Daten protokolliert:

    {
        "records":
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 1,
                "healthPercentage": 50.000000
            }
        },
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 0,
                "healthPercentage": 100.000000
            }
        }
    }

Die JSON-Ausgabe zeigt die grundlegende Informationen für den Prüfpunkt Status im Feld Eigenschaften. *DipDownCount* -Eigenschaft zeigt die Gesamtzahl der Instanzen auf Back-End der Netzwerkverkehr durch fehlerhaften Prüfpunkt Antworten nicht erhalten.

## <a name="view-and-analyze-the-audit-log"></a>Anzeigen und Analysieren des Überwachungsprotokolls

Sie können anzeigen und Analysieren von Audit Log Daten mit einer der folgenden Methoden:

- **Azure Tools:** Abrufen von Informationen aus der Überwachungsprotokolle Azure PowerShell, Azure-Befehlszeilenschnittstelle (CLI), Azure REST API oder vorschauportal Azure. Anleitung für jede Methode sind in Artikel [Überwachungsvorgänge mit Ressourcen-Manager](../../articles/resource-group-audit.md) aufgeführt.
- **Power BI:** Wenn Sie nicht bereits ein [Power BI](https://powerbi.microsoft.com/pricing) -Konto verfügen, können Sie es kostenlos. [Überwachungsprotokolle Azure content Pack für Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs)verwenden, Ihre Daten mit vorkonfigurierten Dashboards analysieren oder Ansichten entsprechend Ihren Erfordernissen anpassen.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Integritätstest und Ereignisprotokoll anzeigen und analysieren

Sie müssen das Speicherkonto und Protokolleinträge JSON-Ereignis und Gesundheit Prüfpunkt Protokolle abrufen. Downloaden Sie die JSON-Dateien, können Sie diese CSV und in Excel, PowerBI oder andere Daten-Visualisierungstool konvertieren.

>[AZURE.TIP] Wenn Sie mit Visual Studio und grundlegenden Konzepten ändern Werte für Konstanten und Variablen in C# vertraut sind, können Sie die [Protokoll-Konverter Tools](https://github.com/Azure-Samples/networking-dotnet-log-converter) von Github.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- Blogbeitrag [Azure Überwachungsprotokolle mit Power BI visualisieren](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Anzeigen und Analysieren von Azure Überwachungsprotokolle Power BI und](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) Blogbeitrag.

## <a name="next-steps"></a>Nächste Schritte

[Load Balancer Prüfpunkte verstehen](load-balancer-custom-probe-overview.md)
