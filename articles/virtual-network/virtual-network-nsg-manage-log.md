<properties
   pageTitle="NSGs Operationen, Ereignisse und Leistungsindikatoren überwachen | Microsoft Azure"
   description="Aktivieren Sie Zähler, Ereignisse und operative Protokollierung für NSGs"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/14/2016"
   ms.author="jdial" />

#<a name="log-analytics-for-network-security-groups-nsgs"></a>Protokollanalyse für Netzwerk-Sicherheitsgruppen (NSGs)

Sie können verschiedene Protokolle in Azure Verwaltung und Problembehandlung NSGs. Einige dieser Protokolle kann erfolgen über das Portal und alle Protokolle aus einer Azure BLOB-Speicher extrahiert und in verschiedene Tools [Protokollanalyse](../log-analytics/log-analytics-azure-networking-analytics.md)und Excel PowerBI angezeigt. Erfahren Sie mehr über die verschiedenen Protokolle aus der Liste.

- **Überwachungsprotokolle:** [Überwachungsprotokolle Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (ehemals Betriebsprotokolle) können Sie alle Vorgänge übermittelt der Azure-Abonnements und deren Status anzuzeigen. Überwachungsprotokolle sind standardmäßig aktiviert und können im vorschauportal Azure angezeigt werden.
- **Event Logs:** Dieses Protokoll können Sie anzeigen, welche Regeln NSG VMs und Instanz Rollen MAC-Adresse angewendet werden. Der Status dieser Regeln wird alle 60 Sekunden erfasst.
- **Leistungsindikatorenprotokolle:** Dieses Protokoll können wie oft anzeigen jedes NSG Regel zum Verweigern oder Zulassen von Datenverkehr angewendet wurde.

>[AZURE.WARNING] Protokolle sind nur für Ressourcen in der Ressourcen-Manager-Bereitstellungsmodell. Protokolle können keine Ressourcen im klassischen Bereitstellungsmodell. Zum besseren Verständnis der beiden Modelle, verweisen auf Artikel [Verständnis Ressourcenmanager und klassischen Bereitstellung](../resource-manager-deployment-model.md) .

##<a name="enable-logging"></a>Aktivieren der Protokollierung
Audit-Protokollierung wird automatisch am immer für jede Ressource Ressourcen-Manager aktiviert. Sie müssen Ereignis und Leistungsindikatoren sammeln Daten über diese Protokolle Protokollierung aktivieren. Gehen Sie folgendermaßen vor um die Protokollierung zu aktivieren.

1.  Anmelden bei [Azure-Portal](https://portal.azure.com). Haben Sie bereits eine vorhandene Netzwerksicherheitsgruppe [eine NSG erstellen](virtual-networks-create-nsg-arm-ps.md) , bevor Sie fortfahren.

2.  Klicken Sie im vorschauportal auf **Durchsuchen** >> **Netzwerk-Sicherheitsgruppen**.

    ![Vorschauportal - Netzwerk von Sicherheitsgruppen](./media/virtual-network-nsg-manage-log/portal-enable1.png)

3. Wählen Sie ein bestehendes Netzwerk Sicherheit.

    ![Vorschauportal - Einstellungen Network security](./media/virtual-network-nsg-manage-log/portal-enable2.png)

4. Blatt **Einstellungen** auf **Diagnose**und klicken Sie dann im Bereich **Diagnose** neben **Status** **auf**
5. Im Blatt **Einstellungen** auf **Speicherkonto**, und wählen Sie einen vorhandenen Speicher Konto oder eine neue erstellen.  

>[AZURE.INFORMATION] Audit-Protokolle erfordern keine separaten Speicherkonto. Die Verwendung des Speichers für Ereignis und Regel Protokollierung fallen Gebühren.

6. Wählen Sie in der Dropdown-Liste unter **Speicherkonto**aus, ob protokollieren Ereignisse und Leistungsindikatoren und dann auf **Speichern**.

    ![Vorschauportal - Diagnoseprotokolle](./media/virtual-network-nsg-manage-log/portal-enable3.png)

## <a name="audit-log"></a>Prüfprotokoll
Standardmäßig wird dieses Protokoll (ehemals "Betriebsprotokoll") von Azure generiert.  Die Protokolle werden 90 Tage lang in Azure Ereignisprotokolle Speicher beibehalten. Erfahren Sie mehr über diese Protokolle lesen [Ereignisse anzeigen und Überwachungsprotokolle](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="counter-log"></a>Leistungsindikatorenprotokoll
Dieses Protokoll wird nur generiert, wenn Sie jeweils pro NSG wie oben beschrieben aktiviert haben. Die Daten werden im Speicherkonto gespeichert angegebenen, wenn die Protokollierung aktiviert. Jede Regel, die auf Ressourcen angewendet werden wie folgt im JSON-Format protokolliert.

    {
        "time": "2015-09-11T23:14:22.6940000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupRuleCounter",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupCounters",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"DenyAllOutBound",
            "direction":"Out",
            "type":"block",
            "matchedConnections":0
            }
    }

## <a name="event-log"></a>Ereignisprotokoll
Dieses Protokoll wird nur generiert, wenn Sie jeweils pro NSG wie oben beschrieben aktiviert haben. Die Daten werden im Speicherkonto gespeichert angegebenen, wenn die Protokollierung aktiviert. Die folgenden Daten protokolliert:

    {
        "time": "2015-09-11T23:05:22.6860000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupEvent",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupEvents",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"AllowVnetOutBound",
            "direction":"Out",
            "priority":65000,
            "type":"allow",
            "conditions":{
                "destinationPortRange":"0-65535",
                "sourcePortRange":"0-65535",
                "destinationIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32",
                "sourceIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32"
            }
        }
    }

## <a name="view-and-analyze-the-audit-log"></a>Anzeigen und Analysieren des Überwachungsprotokolls
Sie können anzeigen und Analysieren von Audit Log Daten mit einer der folgenden Methoden:

- **Azure Tools:** Abrufen von Informationen aus der Überwachungsprotokolle Azure PowerShell, Azure-Befehlszeilenschnittstelle (CLI), Azure REST API oder vorschauportal Azure.  Anleitung für jede Methode sind in Artikel [Überwachungsvorgänge mit Ressourcen-Manager](../resource-group-audit.md) aufgeführt.
- **Power BI:** Wenn Sie bereits ein [Power BI](https://powerbi.microsoft.com/pricing) -Konto haben, können Sie es kostenlos. Mit [Überwachungsprotokolle Azure content Pack für Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/) können Sie Ihre Daten mit vorkonfigurierten Dashboards, mit denen Sie als analysieren-ist oder anpassen.

## <a name="view-and-analyze-the-counter-and-event-log"></a>Zähler und Ereignisprotokoll anzeigen und analysieren

Azure [Protokollanalyse](../log-analytics/log-analytics-azure-networking-analytics.md) können den Leistungsindikator und das Speicherkonto BLOB-Dateien und umfasst Visualisierung und leistungsstarke Suchfunktionen die Protokolle zu analysieren.

Sie können auch an das Speicherkonto und JSON-Protokolleinträge für Ereignis-und Leistungsindikatoren abrufen. Downloaden Sie die JSON-Dateien, können Sie diese CSV und in Excel, PowerBI oder andere Daten-Visualisierungstool konvertieren.

>[AZURE.TIP] Wenn Sie mit Visual Studio und grundlegenden Konzepten ändern Werte für Konstanten und Variablen in C# vertraut sind, können Sie die [Protokoll-Konverter Tools](https://github.com/Azure-Samples/networking-dotnet-log-converter) von Github.

## <a name="next-steps"></a>Nächste Schritte

- Visualisieren Sie Zähler und Ereignisprotokolle mit [Protokollanalyse](../log-analytics/log-analytics-azure-networking-analytics.md)
- Blogbeitrag [Azure Überwachungsprotokolle mit Power BI visualisieren](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Anzeigen und Analysieren von Azure Überwachungsprotokolle Power BI und](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) Blogbeitrag.
