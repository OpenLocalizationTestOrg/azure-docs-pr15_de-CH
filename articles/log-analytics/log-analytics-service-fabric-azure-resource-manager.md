<properties
    pageTitle="Optimieren Sie Ihre Umgebung mit der Lösung Service Fabric Protokollanalyse | Microsoft Azure"
    description="Service Fabric-Lösung können Risiken und Health Service Fabric Applikationen, Micro-Services, Knoten und Cluster."
    services="log-analytics"
    documentationCenter=""
    authors="niniikhena"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="nini"/>



# <a name="service-fabric-solution-in-log-analytics"></a>Service Fabric Lösung Protokollanalyse

> [AZURE.SELECTOR]
- [Ressourcen-Manager](log-analytics-service-fabric-azure-resource-manager.md)
- [PowerShell](log-analytics-service-fabric.md)

Dieser Artikel beschreibt, wie die Lösung Service Fabric in Protokollanalyse identifiziert und Fehlerbehebung in Service Fabric-Cluster.

Service Fabric-Lösung verwendet Azure-Diagnose Daten aus Ihrem Service Fabric VMs durch Erfassung von Daten aus Tabellen Azure Bündel. Anschließend liest Protokollanalyse Fabric Service Framework Ereignisse, einschließlich der **zuverlässigen Service**, **Akteur Ereignisse**, **Operationelle Ereignisse**, und **benutzerdefinierte ETW-Ereignisse**. Mit dem lösungsdashboard können Sie wichtige Probleme und entsprechende Ereignisse in Ihrer Umgebung Service Fabric anzeigen.

Zunächst mit der Lösung müssen Sie einen Arbeitsbereich Protokollanalyse Service Fabric-Cluster herstellen. Hier sind drei Szenarios zu berücksichtigen:

1. Wenn kein Cluster Service Fabric bereitgestellt haben, gehen Sie bei ***einem Service Fabric-Cluster bereitstellen Protokollanalyse Arbeitsbereich*** Bereitstellen eines neuen Clusters und Bericht Protokollanalyse konfiguriert.

2. Wenn Hosts mit anderen OMS Solutions wie im Cluster Service Fabric-Leistungsindikatoren erfassen möchten, gehen Sie in ***Bereitstellen eines Service Fabric-Clusters an einem OMS-Arbeitsbereich mit VM-Erweiterung installiert.***

3. Wenn Sie Ihren Service Fabric-Cluster und möchten für die Verbindung zum Protokollanalyse bereits bereitgestellt haben, führen Sie die Schritte ***Protokollanalyse ein Storage-Konto hinzugefügt.***


##<a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a>Bereitstellen einer Protokollanalyse Arbeitsbereich verbundenen Service Fabric-Cluster.
Diese Vorlage bewirkt Folgendes:


1. Stellt eine Azure Service Fabric-Cluster Protokollanalyse Arbeitsbereich bereits verbunden. Sie haben die Möglichkeit, einen neuen Arbeitsbereich erstellen und Bereitstellen der Vorlage oder den Namen eines bereits vorhandenen Protokollanalyse Workspace.
2. Arbeitsbereich Protokollanalyse hinzugefügt Diagnose Speicherkonto.
3. Ermöglicht Service Fabric-Lösung im Arbeitsbereich Protokollanalyse.

[![In Azure bereitstellen](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)


Nach Auswahl der Schaltfläche bereitstellen kommen der Azure-Portal mit Parametern zum Bearbeiten von. Eine neue Ressourcengruppe erstellen Sie einen neuen Arbeitsbereichsnamen Protokollanalyse Eingabe: ![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

Akzeptieren Sie die Vertragsbedingungen, und drücken Sie "Erstellen", um die Bereitstellung zu starten. Nachdem die Bereitstellung abgeschlossen ist, müsste der neue Arbeitsbereich und Cluster erstellt und die WADServiceFabric * Tabellen Ereignis, WADWindowsEventLogs und WADETWEvent hinzugefügt:

![Service Fabric](./media/log-analytics-service-fabric/4.png)

##<a name="deploy-a-service-fabric-cluster-connected-to-an-oms-workspace-with-vm-extension-installed"></a>Bereitstellen einer Service Fabric-Clusters verbunden mit OMS Arbeitsbereich mit VM-Erweiterung installiert.
Diese Vorlage bewirkt Folgendes:

1. Stellt eine Azure Service Fabric-Cluster Protokollanalyse Arbeitsbereich bereits verbunden. Sie können einen neuen Arbeitsbereich erstellen oder eine bestehende verwenden.
2. Arbeitsbereich Protokollanalyse hinzugefügt Diagnose Speicherkonten.
3. Ermöglicht Service Fabric-Lösung im Arbeitsbereich Protokollanalyse.
4. Jede VM Maßstab im Cluster Service Fabric installiert MMA-Agent-Erweiterung. Der MMA-Agent installiert können Sie Leistungsdaten über die Knoten anzeigen.


[![In Azure bereitstellen](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)


Die gleichen Schritte erforderlichen Eingabeparameter und eine Bereitstellung beginnen. Erneut sollten Sie den neuen Arbeitsbereich, Cluster und Bündel Tabellen alle sehen:

![Service Fabric](./media/log-analytics-service-fabric/5.png)

###<a name="viewing-performance-data"></a>Anzeigen von Leistungsdaten

Perf Data aus Knoten anzeigen:
</br>
- Starten des Protokollanalyse Arbeitsbereichs von Azure-Portal.

![Service Fabric](./media/log-analytics-service-fabric/6.png)

- Rufen Sie die Einstellungsseite im linken Bereich, und wählen Sie Daten >> Windows-Leistungsindikatoren >> "Ausgewählten Leistungsindikatoren hinzufügen": ![Service Fabric](./media/log-analytics-service-fabric/7.png)

- Im Protokoll mithilfe von folgenden Abfragen Kennzahlen über Knoten befassen:
</br>

    ein. Vergleichen Sie die durchschnittliche CPU-Auslastung auf allen Knoten in der letzten Stunde, welche Knoten Probleme und in welchen Zeitabständen hatte ein Knoten eine Spitze:

    ``` Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR. ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)


    b. Ähnliche Liniendiagramme für den verfügbaren Speicher auf jedem Knoten mit dieser Abfrage anzeigen:

    ```Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.```

    Verwenden Sie diese Abfrage, um eine Liste aller Knoten, mit den exakten Durchschnittswert für verfügbare MB für jeden Knoten anzuzeigen:

    ```Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)


    c. In dem Fall, der zu einem bestimmten Knoten Drilldown anhand der stündlichen Durchschnitt, Mindest-, Hoechst- und 75 prozentuale CPU-Auslastung können Sie mithilfe dieser Abfrage (Computer Feld Ersetzen):

    ```Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

    Informationen Sie weitere über Leistungsmetriken Protokollanalyse [Hier] (https://blogs.technet.microsoft.com/msoms/tag/metrics/)


##<a name="adding-an-existing-storage-account-to-log-analytics"></a>Protokollanalyse hinzufügen ein Storage-Konto

Diese Vorlage fügt einfach vorhandenen Speicherkonten einer neuen oder vorhandenen Protokollanalyse Arbeitsbereich hinzu.
</br>

[![In Azure bereitstellen](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

>[AZURE.NOTE] Wenn Sie mit einem bereits vorhandenen Protokollanalyse Arbeitsbereich arbeiten, wählen Sie bei einer Ressourcengruppe "Vorhandene verwenden" und suchen Sie die Ressourcengruppe, den OMS-Arbeitsbereich. Erstellen Sie eine neue ein, wenn andernfalls.
![Service Fabric](./media/log-analytics-service-fabric/8.png)

Nach dem Bereitstellen der Vorlage werden können Sie das Speicherkonto Protokollanalyse Arbeitsbereich verbunden. In diesem Fall hinzugefügt oben erstellten Exchange-Arbeitsbereich eine weitere Speicherkonto.
![Service Fabric](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Service Fabric-Ereignisse anzeigen

Nachdem die Bereitstellung abgeschlossen und Service Fabric-Lösung im Arbeitsbereich aktiviert wurde wählen Sie **Service Fabric** Kachel im Portal Protokollanalyse Service Fabric-Dashboard starten aus Das Schaltpult enthält die Spalten in der folgenden Tabelle. Jede Spalte listet die Top zehn Ereignisse nach Anzahl der Spalte Kriterien für den angegebenen Zeitraum. Sie können eine protokollsuche ausführen, die die gesamte Liste durch Klicken auf **Alle** unten rechts jeder Spalte, oder durch Klicken auf die Spaltenüberschrift.

| **Service Fabric-Ereignis** | **Beschreibung** |
| --- | --- |
| Wichtige Punkte | Eine Anzeige von Themen RunAsyncFailures RunAsynCancellations und Knoten ab. |
| Operationelle Ereignisse | Wichtige operationelle Ereignisse wie anwendungsaktualisierung und Bereitstellung. |
| Zuverlässiger Service-Ereignisse | Wichtige zuverlässig Ereignisse eine Runasyncinvocations. |
| Actor-Ereignisse | Die Micro-Dienstleistungen, wie Ausnahmen von einer Methode Akteur Akteur Aktivierungen und deaktivierungen, und wichtige Akteur-Ereignisse. |
| Application-Ereignisse | Alle benutzerdefinierten ETW-Ereignissen in der Anwendung. |

![Service Fabric-dashboard](./media/log-analytics-service-fabric/sf3.png)

![Service Fabric-dashboard](./media/log-analytics-service-fabric/sf4.png)


Tabelle von Datenerfassungsmethoden und andere Details wie der Daten für Service.

| Plattform | Direkte Agent | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Windows|![Nein](./media/log-analytics-malware/oms-bullet-red.png)|![Nein](./media/log-analytics-malware/oms-bullet-red.png)| ![Ja](./media/log-analytics-malware/oms-bullet-green.png)|            ![Nein](./media/log-analytics-malware/oms-bullet-red.png)|![Nein](./media/log-analytics-malware/oms-bullet-red.png)|10 Minuten |


>[AZURE.NOTE] Auf oben das Dashboard **basierend auf 7 Tagen** können Sie den Bereich dieser Ereignisse in Service Fabric-Lösung ändern. Sie können auch Ereignisse innerhalb der letzten 7 Tage, 1 Tag oder sechs Stunden anzeigen. Oder Sie können **benutzerdefinierte** benutzerdefinierten Datumsbereich an.


## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [Log durchsucht Protokollanalyse](log-analytics-log-searches.md) Service Fabric detaillierte Daten anzeigen.
