<properties 
   pageTitle="Application Gateway Zugriff und Performance-Protokolle und Metriken überwachen | Microsoft Azure"
   description="Informationen Sie zum Aktivieren und verwalten Zugriff und Performance-Protokollen für Application Gateway"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="amitsriva" />

# <a name="diagnostics-logging-and-metrics-for-application-gateway"></a>Diagnose-Protokollierung und Metriken für Application Gateway

Azure bietet die Möglichkeit, mit Protokollierung und Metriken überwachen

[**Protokollierung**](#enable-logging-with-powershell) - Performance, Access und anderen Protokollen gespeichert oder aus einer Ressource für Überwachungszwecke genutzt werden kann.

[**Metrik**](#metrics) - Application Gateway derzeit hat eine Metrik. Diese Metrik misst den Durchsatz der Anwendungsgateway in Bytes pro Sekunde.

Können verschiedene Protokolle in Azure Verwaltung und Problembehandlung Anwendungsgateways. Einige dieser Protokolle kann erfolgen über das Portal und alle Protokolle aus einer Azure BLOB-Speicher extrahiert und in anderen Tools wie [Protokollanalyse](../log-analytics/log-analytics-azure-networking-analytics.md), Excel und PowerBI angezeigt. Erfahren Sie mehr über die verschiedenen Protokolle aus der folgenden Liste:

- **Überwachungsprotokolle:** [Überwachungsprotokolle Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (ehemals Betriebsprotokolle) können Sie alle Vorgänge übermittelt der Azure-Abonnements und deren Status anzuzeigen. Überwachungsprotokolle sind standardmäßig aktiviert und können im vorschauportal Azure angezeigt werden.
- **Zugriff auf Protokolle:** Verwenden Sie dieses Protokoll Application Gateway Zugriffsmuster anzeigen und Analysieren wichtiger Informationen des Anrufers IP-URL angefordert, Antwort Wartezeit, Bytes und Rückgabecode. Zugriffsprotokoll werden alle 300 Sekunden. Dieses Protokoll enthält einen Datensatz pro Anwendungsgateway. Die Anwendungsinstanz Gateway kann von "InstanceId"-Eigenschaft angegeben.
- **Performance-Protokolle:** Dieses Protokoll können Sie Performance-Gateways Instanzen anzeigen. Dieses Protokoll zeichnet Leistungsinformationen per Instanz insgesamt Anforderung bedient, einschließlich Durchsatz in Bytes, Anforderungsfehler Count fehlerfreie und fehlerhafte Backend-Instanzenzahl Anfragen insgesamt bereitgestellt. Performance-Protokolls werden alle 60 Sekunden.
- **Firewall-Protokolle:** Dieses Protokoll können Sie Anfragen anzeigen, die über ein Gateway Detection und Prevention Modus protokolliert werden, die mit Web Application Firewall konfiguriert.

>[AZURE.WARNING] Protokolle sind nur für Ressourcen in der Ressourcen-Manager-Bereitstellungsmodell. Protokolle können keine Ressourcen im klassischen Bereitstellungsmodell. Zum besseren Verständnis der beiden Modelle, verweisen auf Artikel [Verständnis Ressourcenmanager und klassischen Bereitstellung](../resource-manager-deployment-model.md) .

## <a name="enable-logging-with-powershell"></a>Protokollierung mit PowerShell

Audit-Protokollierung wird automatisch für jede Ressource Ressourcen-Manager aktiviert. Zugriff und Performance-Protokollierung, um die Daten über die Protokolle sammeln müssen. Zum Aktivieren der Protokollierung finden Sie die folgenden Schritte aus: 

1. Beachten Sie des Speicherkontos Ressourcen-ID, wo die Protokolldaten gespeichert ist. Wäre der Form: /subscriptions/\<SubscriptionId\>/resourceGroups/\<den Namen\>/providers/Microsoft.Storage/storageAccounts/\<speicherkontoname\>. Jedes Storage-Konto in Ihrem Abonnement kann verwendet werden. Vorschauportal können Sie diese Informationen.

    ![Vorschauportal - Application Gateway Diagnostics](./media/application-gateway-diagnostics/diagnostics1.png)

2. Hinweis Die Application Gateway-Ressourcen-ID für die Protokollierung aktiviert ist. Dies wäre der Form: /subscriptions/\<SubscriptionId\>/resourceGroups/\<Namen\>/providers/Microsoft.Network/applicationGateways/\<Anwendungsname Gateway\>. Vorschauportal können Sie diese Informationen.

    ![Vorschauportal - Application Gateway Diagnostics](./media/application-gateway-diagnostics/diagnostics2.png)

3. Diagnose-Protokollierung mit dem folgenden Powershell-Cmdlet:

        Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true  

>[AZURE.INFORMATION] Audit-Protokolle erfordern keine separaten Speicherkonto. Die Verwendung des Speichers für Zugriff und Performance-Protokollierung fallen Gebühren.

## <a name="enable-logging-with-azure-portal"></a>Protokollierung mit Azure-portal

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu der Ressource im Azure-Portal. Klicken Sie auf **Diagnoseprotokollen**. Das folgende Bild ist dies zum ersten Mal Diagnose sieht das Blade:

Application Gateway sind 3-Protokolle verfügbar.

- Zugang
- Leistungsprotokoll
- Firewall-Protokoll

Klicken Sie zum Starten der Datensammlung **Diagnose aktivieren** .

![Diagnose Einstellung blade][1]

### <a name="step-2"></a>Schritt 2

Auf dem Blatt **Diagnose Einstellung** werden die Einstellungen für die Diagnoseprotokolle festgelegt. In diesem Beispiel dient Protokollanalyse Protokolle speichern. Klicken Sie unter **Protokollanalyse** Arbeitsbereich konfigurieren **Konfigurieren** . Ereignis-Hubs und ein Speicherkonto lässt sich die Diagnoseprotokolle speichern.

![Diagnose-blade][2]

### <a name="step-3"></a>Schritt 3

Wählen Sie einen vorhandenen Arbeitsbereich OMS oder erstellen Sie eine neue. In diesem Beispiel wird eine vorhandene verwendet.

![OMS-Arbeitsbereiche][3]

### <a name="step-4"></a>Schritt 4

Nach Abschluss des Vorgangs bestätigen Sie Einstellungen, und klicken Sie auf **Speichern** , um die Einstellung zu speichern.

![Auswahl bestätigen][4]

## <a name="audit-log"></a>Prüfprotokoll

Standardmäßig wird dieses Protokoll (ehemals "Betriebsprotokoll") von Azure generiert.  Die Protokolle werden 90 Tage lang in Azure Ereignisprotokolle Speicher beibehalten. Erfahren Sie mehr über diese Protokolle lesen [Ereignisse anzeigen und Überwachungsprotokolle](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="access-log"></a>Zugang

Dieses Protokoll wird nur generiert, wenn Sie es aktiviert haben eine pro Application Gateway wie in den vorhergehenden Schritten. Die Daten werden im Speicherkonto gespeichert angegebenen, wenn die Protokollierung aktiviert. Jeder Access Application Gateway wird im JSON-Format protokolliert, wie im folgenden Beispiel dargestellt:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayAccess",
        "time": "2016-04-11T04:24:37Z",
        "category": "ApplicationGatewayAccessLog",
        "properties": {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIP":"37.186.113.170",
            "clientPort":"12345",
            "httpMethod":"HEAD",
            "requestUri":"/xyz/portal",
            "requestQuery":"",
            "userAgent":"-",
            "httpStatus":"200",
            "httpVersion":"HTTP/1.0",
            "receivedBytes":"27",
            "sentBytes":"202",
            "timeTaken":"359",
            "sslEnabled":"off"
        }
    }

## <a name="performance-log"></a>Leistungsprotokoll

Dieses Protokoll wird nur generiert, wenn Sie es aktiviert haben eine pro Application Gateway wie in den vorhergehenden Schritten. Die Daten werden im Speicherkonto gespeichert angegebenen, wenn die Protokollierung aktiviert. Die folgenden Daten protokolliert:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayPerformance",
        "time": "2016-04-09T00:00:00Z",
        "category": "ApplicationGatewayPerformanceLog",
        "properties": 
        {
            "instanceId":"ApplicationGatewayRole_IN_1",
            "healthyHostCount":"4",
            "unHealthyHostCount":"0",
            "requestCount":"185",
            "latency":"0",
            "failedRequestCount":"0",
            "throughput":"119427"
        }
    }


## <a name="firewall-log"></a>Firewall-Protokoll

Dieses Protokoll wird nur generiert, wenn Sie es aktiviert haben eine pro Anwendung Gateway wie in den vorhergehenden Schritten. Dieses Protokoll erfordert auch, dass Web Application Firewall auf ein Gateway konfiguriert werden. Die Daten werden im Speicherkonto gespeichert angegebenen, wenn die Protokollierung aktiviert. Die folgenden Daten protokolliert:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
        "operationName": "ApplicationGatewayFirewall",
        "time": "2016-09-20T00:40:04.9138513Z",
        "category": "ApplicationGatewayFirewallLog",
        "properties":     {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIp":"108.41.16.164",
            "clientPort":1815,
            "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
            "ruleId":"OWASP_973336",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "action":"Logged",
            "site":"Global",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
    }

## <a name="view-and-analyze-the-audit-log"></a>Anzeigen und Analysieren des Überwachungsprotokolls

Sie können anzeigen und Analysieren von Audit Log Daten mit einer der folgenden Methoden:

- **Azure Tools:** Abrufen von Informationen aus der Überwachungsprotokolle Azure PowerShell, Azure-Befehlszeilenschnittstelle (CLI), Azure REST API oder vorschauportal Azure.  Anleitung für jede Methode sind in Artikel [Überwachungsvorgänge mit Ressourcen-Manager](../resource-group-audit.md) aufgeführt.
- **Power BI:** Wenn Sie bereits ein [Power BI](https://powerbi.microsoft.com/pricing) -Konto haben, können Sie es kostenlos. Mit [Überwachungsprotokolle Azure content Pack für Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) können Sie Ihre Daten mit vorkonfigurierten Dashboards, mit denen Sie als analysieren-ist oder anpassen.

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>Anzeigen und Analysieren von Access, Leistung und Firewall-Protokoll

Azure [Protokollanalyse](../log-analytics/log-analytics-azure-networking-analytics.md) können den Leistungsindikator und das Speicherkonto BLOB-Dateien und umfasst Visualisierung und leistungsstarke Suchfunktionen die Protokolle zu analysieren.

Sie können auch das Speicherkonto verbinden und die JSON-Protokolleinträge für Zugriff und Performance-Protokolle. Downloaden Sie die JSON-Dateien, können Sie diese CSV und in Excel, PowerBI oder andere Daten-Visualisierungstool konvertieren.

>[AZURE.TIP] Wenn Sie mit Visual Studio und grundlegenden Konzepten ändern Werte für Konstanten und Variablen in C# vertraut sind, können Sie die [Protokoll-Konverter Tools](https://github.com/Azure-Samples/networking-dotnet-log-converter) von Github.

## <a name="metrics"></a>Metriken

Metriken ist für bestimmte Azure Ressourcen, Leistungsindikatoren im Portal angezeigt werden. Für Application Gateway ist eine Metrik zum Zeitpunkt der Abfassung dieses Artikels. Diese Metrik wird Durchsatz und im Portal angezeigt werden. Ein Gateway und Sie auf **Kriterien**.  Wählen Sie im Abschnitt **verfügbaren Metriken** anzeigen, den Durchsatz. In der folgenden Abbildung sehen Sie ein Beispiel mit den Filtern, die zum Anzeigen der Daten in anderen Bereichen verwendet werden können.

Eine Liste der aktuellen Metriken unterstützen, finden Sie auf [unterstützte Metriken mit Azure](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![metrische anzeigen][5]

## <a name="alert-rules"></a>Warnregeln

Warnregeln können anhand von Kriterien für eine Ressource gestartet werden. Dies bedeutet für Application Gateway eine Warnung rufen eine Webhook oder e-Mail-Administrator ist der Durchsatz des Anwendung-Gateways über, unter oder unter einen Schwellenwert für einen angegebenen Zeitraum.

Das folgende Beispiel führt Sie durch Erstellen einer Warnregel, die eine e-Mail an einen Administrator sendet, nachdem Durchsatz Schwellenwert verletzt wurde.

### <a name="step-1"></a>Schritt 1

Klicken Sie auf start **metrische Warnung hinzufügen** . Diese Blade aus dem Blade Metriken erreichbar.

![Warnregeln blade][6]

### <a name="step-2"></a>Schritt 2

Geben Sie Namen Bedingung Blatt **Regel hinzufügen** informieren Sie Abschnitte und klicken Sie auf **OK** , wenn Sie fertig.

Der Selektor **Condition** ermöglicht 4 Werte, **größer als**, **größer als oder gleich**, **kleiner als**oder **kleiner als oder gleich**.

Auswahl **Zeitraum** können für die Kommissionierung für einen Zeitraum von 5 bis 6 Stunden.

Indem **e-Mail-Besitzer, beteiligte Personen, und Leser** kann e-Mail dynamische basierend auf Benutzer, die Zugriff auf diese Ressource verfügen. Andernfalls kann eine durch Trennzeichen getrennte Liste der Benutzer in das Textfeld **zusätzliche Administrator Email(s)** bereitgestellt werden.

![Blatt hinzufügen][7]

Wenn der Schwellenwert überschritten wird, kommt eine e-Mail in der folgenden Abbildung ähnelt:

![Schwellenwert verletzt e-Mail][8]

Eine Liste der Warnungen zeigt metrische Warnung erstellt wurde und bietet eine Übersicht über alle Warnregeln.

![Warnungsregel anzeigen][9]

Weitere Informationen zu Benachrichtigungen finden Sie auf [Benachrichtigung empfangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Besuchen Sie zum Verständnis über Webhooks und werden mit Warnungen Verwendung [Konfigurieren Webhook Azure metrische Ausschreibung](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>Nächste Schritte

- Visualisieren Sie Zähler und Ereignisprotokolle mit [Protokollanalyse](../log-analytics/log-analytics-azure-networking-analytics.md) 
- Blogbeitrag [Azure Überwachungsprotokolle mit Power BI visualisieren](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Anzeigen und Analysieren von Azure Überwachungsprotokolle Power BI und](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) Blogbeitrag.

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png