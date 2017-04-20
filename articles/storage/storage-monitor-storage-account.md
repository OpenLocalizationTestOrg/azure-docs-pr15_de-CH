<properties
    pageTitle="Ein Speicherkonto überwachen | Microsoft Azure"
    description="Erfahren Sie, wie ein Speicherkonto in Azure mithilfe von Azure-Portal überwachen."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Ein Speicherkonto in Azure-Portal überwachen

## <a name="overview"></a>Übersicht

Sie können das Speicherkonto aus dem [Azure-Portal](https://portal.azure.com)überwachen. Beim Konfigurieren Ihres Speicherkontos Überwachungskonto für das Portal verwendet Azure Storage [Speicheranalyse](http://msdn.microsoft.com/library/azure/hh343270.aspx) Metriken für Ihr Konto und Daten protokolliert.

> [AZURE.NOTE]Zusätzliche Kosten sind verknüpft mit Daten in [Azure-Portal](https://portal.azure.com). Weitere Informationen finden Sie unter <a href="http://msdn.microsoft.com/library/azure/hh360997.aspx">Speicheranalyse und Abrechnung</a>. <br />

> Azure Dateispeicher derzeit Speicheranalyse Metriken unterstützt jedoch unterstützt noch keine Protokollierung. Sie können Messgrößen für Azure Dateispeicher [Azure-Portal](https://portal.azure.com).

> Speicherkonten mit Replikation Zone redundanten Speicher (ZRS) müssen nicht Metriken oder Protokollfunktion zurzeit aktiviert. 

> Ein detailliertes Handbuch mit Speicheranalyse und andere Tools identifizieren, diagnostizieren und beheben Probleme mit der Azure-Speicher finden Sie unter [Überwachung, diagnose und Problembehandlung von Microsoft Azure-Speicher](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="how-to-configure-monitoring-for-a-storage-account"></a>Gewusst wie: Konfigurieren der Überwachung für ein Speicherkonto

1. In [Azure-Portal](https://portal.azure.com)auf **Speicher**und klicken Sie Speicher-Kontonamen Dashboard öffnen.

2. Klicken Sie auf **Konfigurieren**, und die Einstellung **Überwachung** für Blob, Tabelle und Warteschlangendienste scrollen.

    ![MonitoringOptions](./media/storage-monitor-storage-account/Storage_MonitoringOptions.png)

3. Legen Sie die Überwachung und die Datenaufbewahrungsrichtlinie für jeden Dienst **Überwachen**:

    -  Um die Überwachungsstufe festzulegen, wählen Sie eine der folgenden:

      **Minimal** - sammelt Metriken wie Ingress-Ausgang, Verfügbarkeit, Latenz und Erfolg Prozentsätze für Blob, Tabelle und Warteschlangendienste zusammengefasst werden.

      **Verbose** - sammelt dieselben Metriken für jeden Speichervorgang in Azure Storage Service API neben minimalen Metriken. Ausführliche Kriterien ermöglichen näher Analyse der Probleme bei Anwendung.

      **Ausschalten** - Schaltet Überwachung. Vorhandene Überwachungsdaten wird bis zum Ende der Aufbewahrungsdauer beibehalten.

- Um die Datenaufbewahrungsrichtlinie Aufbewahrung **(in Tagen)**festzulegen, geben Sie die Anzahl der Tage Daten von 1 bis 365 Tage beibehalten. Wenn Sie keine Aufbewahrungsrichtlinie festlegen möchten, geben Sie NULL. Gibt es ist keine Aufbewahrungsrichtlinie, Sie die Daten löschen. Wir empfehlen eine Aufbewahrungsrichtlinie basierend auf den gewünschten Speicher Analysedaten für Ihr Konto beibehalten, damit alte und ungenutzte Analysedaten System nicht gelöscht werden.

4. Die Überwachungskonfiguration abgeschlossen haben, klicken Sie auf **Speichern**.

Starten Sie Überwachungsdaten im Dashboard und die Seite **Monitor** nach einer Stunde angezeigt.

Bis Konfigurieren der Überwachung für ein Speicherkonto keine Daten gesammelt und Metriken Diagramme auf dem Dashboard und **Seite** leer sind.

Nach dem Festlegen der Überwachungsebene und Aufbewahrungsrichtlinien können verfügbaren Metriken in [Azure-Portal](https://portal.azure.com)überwachen und die Metriken auf Metriken Diagramme verwendet. Eine Reihe von Metriken wird jede Überwachung Ebene angezeigt. **Metriken hinzufügen** können Sie hinzufügen oder entfernen Metriken Metriken.

Metriken werden in das Speicherkonto in vier Tabellen mit dem Namen $MetricsTransactionsBlob, $MetricsTransactionsTable, $MetricsTransactionsQueue und $MetricsCapacityBlob gespeichert. Weitere Informationen finden Sie unter [Über Storage Analytics-Metriken](http://msdn.microsoft.com/library/azure/hh343258.aspx).


## <a name="how-to-customize-the-dashboard-for-monitoring"></a>Gewusst wie: Anpassen des Dashboards für die Überwachung

Das Dashboard können Sie bis zu sechs Metriken auf Diagramm Metriken aus neun verfügbaren Metriken verwendet. Für jeden Dienst (Blob, Tabelle und Warteschlange) stehen die Metriken Verfügbarkeit, Erfolg Prozentsatz und Anfragen insgesamt. Im Schaltpult Metriken sind für minimale oder ausführliche Überwachung.

1. In [Azure-Portal](https://portal.azure.com)auf **Speicher**und klicken Sie dann auf den Namen des Speicherkontos Dashboard öffnen.

2. Ändern die Metriken, die im Diagramm dargestellt werden, führen Sie eine der folgenden Aktionen:

    - Um eine neue Metrik zum Diagramm hinzufügen möchten, klicken Sie auf den farbigen Kontrollkästchen metrische Header in der Tabelle unter dem Diagramm.

    - Ausblenden eine Metrik, die im Diagramm dargestellt, löschen Sie das farbige Kontrollkästchen metrische Kopfzeile

        ![Monitoring_nmore](./media/storage-monitor-storage-account/storage_Monitoring_nmore.png)

3. Standardmäßig zeigt das Diagramm Trends nur der aktuelle Wert der jede Metrik ( **Relative** Option oben im Diagramm). Wählen Sie zum Anzeigen einer y-Achse Absolute Werte sehen **absolut**.

4. Ändern des Zeitraums auswählen Metriken Diagramm zeigt 6 Stunden, 24 Stunden oder 7 Tage am oberen Rand des Diagramms


## <a name="how-to-customize-the-monitor-page"></a>Gewusst wie: Anpassen der Seite "Monitor"

Auf der Seite **Überwachen** den vollständigen Satz von Metriken für das Speicherkonto angezeigt.

- Hat das Speicherkonto minimale Überwachung konfiguriert werden wie Ingress-Ausgang, Verfügbarkeit, Latenz und Erfolg Prozentsätze Blob, Tabelle und Warteschlangendienste zusammengefasst.

- Hat das Speicherkonto ausführliche Überwachung konfiguriert stehen die Metriken auf eine feinere Granularität einzelne Speichervorgänge neben Service Level Aggregate.

Gehen Sie wählen, welche speichermetriken in die Metriken Diagramme und anzeigen, die auf **der Seite** angezeigt werden. Diese Einstellungen wirken sich nicht auf Sammlung, Aggregation und Speicherung von Daten im Speicherkonto Überwachung aus.

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Gewusst wie: metriktabelle Kennzahlen hinzufügen


1. In [Azure-Portal](https://portal.azure.com)auf **Speicher**und klicken Sie dann auf den Namen des Speicherkontos Dashboard öffnen.

2. Klicken Sie auf **Systemmonitor**.

    Die Seite " **Monitor** " wird geöffnet. Standardmäßig zeigt die Tabelle Kennzahlen eine Teilmenge von Metriken, die für die Überwachung verfügbar sind. Die Abbildung zeigt die Standardanzeige Monitor für ein Speicherkonto mit ausführlichen Überwachung für alle drei Dienste konfiguriert. Verwenden Sie **Metriken hinzufügen** die Metriken auswählen aus allen verfügbaren Metriken überwachen möchten.

    ![Monitoring_VerboseDisplay](./media/storage-monitor-storage-account/Storage_Monitoring_VerboseDisplay.png)

    > [AZURE.NOTE] Beim Auswählen der Metriken berücksichtigen Sie Kosten. Gibt Transaktion und Egress Kosten aktualisieren Überwachung angezeigt. Weitere Informationen finden Sie unter [Speicheranalyse und Abrechnung](http://msdn.microsoft.com/library/azure/hh360997.aspx).

3. Klicken Sie auf **Kriterien**.

    Aggregierten Metriken, die minimale Überwachung verfügbaren werden am Anfang der Liste. Bei aktiviertem Kontrollkästchen wird die Metrik in der Liste Kriterien angezeigt.

    ![AddMetricsInitialDisplay](./media/storage-monitor-storage-account/Storage_AddMetrics_InitialDisplay.png)

4. Zeigen Sie auf der rechten Seite des Dialogfelds eine Bildlaufleiste angezeigt, die Sie ziehen können, um zusätzliche Statistiken Bildlauf.

    ![AddMetricsScrollbar](./media/storage-monitor-storage-account/Storage_AddMetrics_Scrollbar.png)


5. Klicken Sie auf den Pfeil nach unten zu die Metrik begrenzt auf eine Liste der Vorgänge erweitern. Wählen Sie jede Operation, die in der metriktabelle [Azure-Portal](https://portal.azure.com)anzeigen möchten.

    In der folgenden Abbildung wurde die Autorisierung FEHLERPROZENTSATZ Metrik erweitert.

    ![ExpandCollapse](./media/storage-monitor-storage-account/Storage_AddMetrics_ExpandCollapse.png)


6. Metriken für alle Dienste auszuwählen, klicken Sie auf OK (Häkchen) die Überwachungskonfiguration aktualisieren. Die ausgewählten Kriterien werden metriktabelle hinzugefügt.

7. Um eine Metrik aus der Tabelle löschen, auf Metrik, um es auszuwählen, und dann auf **Löschen Metrik**.

    ![DeleteMetric](./media/storage-monitor-storage-account/Storage_DeleteMetric.png)

## <a name="how-to-customize-the-metrics-chart-on-the-monitor-page"></a>Gewusst wie: Metriken Diagramm auf der Seite anpassen

1. Wählen Sie auf **der Seite für das Speicherkonto in der metriktabelle** bis zu 6 Metriken Metriken Diagramm geplottet. Um eine Metrik auszuwählen, klicken Sie auf das Kontrollkästchen auf der linken Seite. Deaktivieren Sie das Kontrollkästchen, um eine Metrik aus dem Diagramm zu entfernen.

2. Wechseln zwischen relativen (nur Endwert) und absoluten Werten (y-Achse angezeigt) Diagramm wählen Sie **Relative** oder **Absolute** am oberen Rand des Diagramms aus

3.  Ändern der liegen Metriken Diagramm angezeigt, wählen Sie **6 Stunden**, **24 Stunden**oder **7 Tage** am oberen Rand des Diagramms.



## <a name="how-to-configure-logging"></a>Gewusst wie: Konfigurieren der Protokollierung

Für jede von Speicher-Services mit Ihrem Speicherkonto (Blob, Tabelle und Warteschlange) sparen Diagnoseprotokolle für Leseanfragen, Anfragen schreiben und Delete-Anfragen und lassen die Datenaufbewahrungsrichtlinie für die Dienste.

1. In [Azure-Portal](https://portal.azure.com)auf **Speicher**und klicken Sie dann auf den Namen des Speicherkontos Dashboard öffnen.

2. Klicken Sie auf **Konfigurieren**, und mit der nach-unten-Taste auf der Tastatur **Protokollierung**Bildlauf.

    ![Storagelogging](./media/storage-monitor-storage-account/Storage_LoggingOptions.png)


3. Konfigurieren Sie für jeden Dienst (Blob, Tabelle und Warteschlange) Folgendes ein:

    - Welche Anforderung protokollieren: Leseanfragen Anfragen schreiben und Delete-Anfragen.

    - Die Anzahl der Tage zum Aufbewahren der protokollierten Daten. Geben Sie 0 (null) ist, wenn keine Aufbewahrungsrichtlinie festgelegt werden soll. Eine Aufbewahrungsrichtlinie nicht festlegen, ist Sie die Protokolle löschen.

4. Klicken Sie auf **Speichern**.

Die Diagnoseprotokolle werden in einem BLOB-Container mit dem Namen $logs in das Speicherkonto gespeichert. Informationen zum Zugreifen auf die $logs Container finden Sie unter [Über Storage Analytics Protokollierung](http://msdn.microsoft.com/library/azure/hh343262.aspx).
