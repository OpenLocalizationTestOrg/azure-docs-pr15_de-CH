<properties
    pageTitle="Azure Networking Analytics Lösung Protokollanalyse | Microsoft Azure"
    description="Azure Networking Analytics Lösung können Protokollanalyse Azure Network Security Protokolle und Azure Application Gateway-Protokolle."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="richrund"/>

# <a name="azure-networking-analytics-preview-solution-in-log-analytics"></a>Azure Networking Analytics (Vorschau) Lösung Protokollanalyse

>[AZURE.NOTE] Dies ist eine [Vorschau Lösung](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Azure Networking Analytics Lösung können Protokollanalyse Azure Application Gateway-Protokolle und Protokolle von Azure Network Security.

Protokollierung von Azure Application Gateway Protokolle und Azure netzwerksicherheitsgruppen können. Diese Protokolle wurden BLOB-Speicher, wo sie dann von Protokollanalyse suchen und Analyse indiziert werden können.

Folgende Protokolle werden für Anwendungsgateways unterstützt:

+ ApplicationGatewayAccessLog
+ ApplicationGatewayPerformanceLog

Für Netzwerk-Sicherheitsgruppen werden folgende Protokolle unterstützt:

+ NetworkSecurityGroupEvent
+ NetworkSecurityGroupRuleCounter

## <a name="install-and-configure-the-solution"></a>Installieren und Konfigurieren der Lösung

Gehen Sie folgendermaßen vor, Installation und Konfiguration von Azure Networking Analytics-Lösung:

1.  Aktivieren des Diagnoseprotokolls für die zu überwachenden Ressourcen:
  + [Application Gateway](../application-gateway/application-gateway-diagnostics.md)
  + [Netzwerk-Sicherheitsgruppe](../virtual-network/virtual-network-nsg-manage-log.md)
2.  Konfigurieren Sie Protokollanalyse Protokolle mithilfe der Schritte in [JSON-Dateien im BLOB-Speicher](../log-analytics/log-analytics-azure-storage-json.md)aus dem BLOB-Speicher zu lesen.
3.  Aktivieren Sie Azure Networking Analytics-Lösung mithilfe der Schritte im [Lösungskatalog Lösungen Protokollanalyse hinzufügen](log-analytics-add-solutions.md).  

Wenn Sie das Diagnoseprotokoll für einen bestimmten Ressourcentyp nicht aktivieren, werden die Dashboard-Blades für diese Ressource leer sein.

## <a name="review-azure-networking-analytics-data-collection-details"></a>Überprüfen Sie Netzwerk Analytics Azure Einzelheiten zur Datensammlung

Azure Networking Analytics Lösung sammelt Diagnoseprotokolle Azure BLOB-Speicher für Azure-Anwendungsgateways und Netzwerk-Sicherheitsgruppen.
Es ist kein Agent für die Datensammlung.

Tabelle von Datenerfassungsmethoden und andere Details wie der Daten für Azure Netzwerk Analysen.

| Plattform | Direkte agent | System Center Operations Manager (SCOM) agent | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Azure|![Nein](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Nein](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Ja](./media/log-analytics-azure-networking/oms-bullet-green.png)|            ![Nein](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Nein](./media/log-analytics-azure-networking/oms-bullet-red.png)| 10 Minuten|

## <a name="use-azure-networking-analytics"></a>Verwenden von Azure Netzwerke Analytics

Nach der Installation der Lösung Zusammenfassung des Clients anzeigen und Serverfehlern für die überwachte Anwendungsgateways mit **Azure Networking Analytics** nebeneinander auf der Seite **Übersicht** Protokollanalyse.

![Bild der Azure-Netzwerk Analytics Fläche](./media/log-analytics-azure-networking/log-analytics-azurenetworking-tile.png)

Nach die Kachel **Übersicht anklicken** können Übersichten der Protokolle anzeigen und dann einen Drilldown ausführen für folgende Details:

+ Application Protokolle Gateway
  - Client und Server Fehler Application Gateway Zugriffsprotokolle
  - Anfragen pro Stunde für jeden Application Gateway
  - Fehler bei Anfragen pro Stunde für jeden Application Gateway
  - Fehler vom Benutzer-Agent-Anwendung-Gateways
+ Gateway Anwendungsperformance
  - Host-Zustand für Application Gateway
  - Maximale und 95. Prozentwert für Application Gateway konnte Anfragen
+ Netzwerk-Sicherheitsgruppe blockiert flows
  - Netzwerkregeln für Gruppe mit blockierten
  - MAC-Adressen mit blockierten
+ Netzwerk-Sicherheitsgruppe Datenflüsse zulässig
  - Netzwerkregeln für Gruppe mit zulässigen
  - MAC-Adressen mit zulässigen


![Bild von Azure Networking Analytics dashboard](./media/log-analytics-azure-networking/log-analytics-azurenetworking01.png)

![Bild von Azure Networking Analytics dashboard](./media/log-analytics-azure-networking/log-analytics-azurenetworking02.png)

![Bild von Azure Networking Analytics dashboard](./media/log-analytics-azure-networking/log-analytics-azurenetworking03.png)

![Bild von Azure Networking Analytics dashboard](./media/log-analytics-azure-networking/log-analytics-azurenetworking04.png)

### <a name="to-view-details-for-any-log-summary"></a>Details für jede Zusammenfassung anzeigen

1. Klicken Sie auf der Seite **Übersicht** **Azure Networking Analytics** .
2. Dashboard **Networking Analytics Azure** lesen Sie die Zusammenfassung in einem Blade und dann auf eine detaillierte Informationen auf der Suchseite Protokoll anzeigen.

    Auf Suchseiten Protokoll Ergebnisse Zeit detaillierte und Suchverlauf Protokoll angezeigt. Sie können auch Filtern Facetten um die Ergebnisse einzuschränken.

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [Log durchsucht Protokollanalyse](log-analytics-log-searches.md) Azure Networking Analytics Detaildaten anzeigen.
