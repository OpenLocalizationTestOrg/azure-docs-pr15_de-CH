<properties
    pageTitle="Erste Schritte mit Azure | Microsoft Azure"
    description="Erste Schritte mit Azure Monitor Einblick in die Verwendung Ihrer Ressourcen und eine Aktion von Daten."
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
    ms.date="10/19/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-azure-monitor"></a>Erste Schritte mit Azure

Azure Monitor ist der Plattform, die eine einzige Quelle für die Überwachung von Azure-Ressourcen bietet. Mit Azure können Sie darstellen, abzufragen weiterleiten, archivieren und Maßnahmen Kennzahlen und Protokolle von Ressourcen in Azure. Sie können diese Daten mit dem Monitor Portal, [Monitor-PowerShell-Cmdlets](./insights-powershell-samples.md), [Plattformübergreifende CLI](insights-cli-samples.md)oder [Azure Monitor REST APIs](https://msdn.microsoft.com/library/dn931943.aspx)arbeiten. In diesem Artikel gehen wir ein Paar der wichtigsten Komponenten der Azure-Monitor über das Portal Demo.

1. Im Portal **Weitere** Services navigieren Sie und suchen Sie die Option **Überwachen** . Klicken Sie auf den Stern, um diese Option zu Ihrer Favoritenliste hinzufügen, sodass es immer in der linken Navigationsleiste zugänglich ist.

    ![In der Dienstliste überwachen](./media/monitoring-get-started/monitor-more-services.png)

2. Klicken Sie auf **Monitor** **Überwachen** Blatt öffnen. Diese Blade vereint alle Überwachung Einstellungen und Daten in eine konsolidierte Ansicht. Ersten Abschnitt **Aktivitätsprotokoll** geöffnet.

    ![Monitor-Blade-navigation](./media/monitoring-get-started/monitor-blade-nav.png)

    > [AZURE.WARNING] **Service-Benachrichtigung** und **Benachrichtigungsgruppen** Optionen angezeigt erscheint nur mit denen, die Private Vorschau dieser Features beigetreten sind.

    Azure Monitor hat drei Hauptkategorien von Überwachungsdaten: **Aktivitätsprotokoll**, **Metriken**und **Diagnoseprotokolle**.

3. Klicken Sie auf **Aktivität protokollieren** , um sicherzustellen, dass Abschnitt Aktivität angezeigt wird.

    ![Aktivität protokollieren blade](./media/monitoring-get-started/monitor-act-log-blade.png)

    Alle Operationen auf Ressourcen in Ihrem Abonnement beschreibt das [**Aktivitätsprotokoll**](./monitoring-overview-activity-logs.md) . Mithilfe des Aktivitätsprotokolls, Sie bestimmen die ', und "alle erstellen aktualisieren oder Löschen von Ressourcen in Ihrem Abonnement. Aktivitätsprotokoll weist Sie beispielsweise beim Web app wurde und wer es. Aktivität protokollieren von Ereignissen sind in der Plattform gespeicherten Abfragen für 90 Tage.
   
    Sie können erstellen und Speichern von Abfragen für allgemeine Filter und die wichtigsten Abfragen Portal Dashboard anheften, damit Sie immer wissen, ob Ereignisse, die Ihren Kriterien entsprechen.

4. Filtern Sie die Ansicht einer bestimmten Ressourcengruppe in der letzten Woche und dann auf die Schaltfläche **Speichern** .

    ![Aktivität protokollieren Abfrage speichern](./media/monitoring-get-started/monitor-act-log-save.png)

5. Klicken Sie auf die Schaltfläche **Pin** .

    ![Klicken Sie auf Pin für das Aktivitätsprotokoll](./media/monitoring-get-started/monitor-act-log-pin.png)

    Die meisten Ansichten in dieser exemplarischen Vorgehensweise können einem Dashboard fixiert werden. So können Sie eine Informationsquelle für operative Daten auf Ihre Dienste erstellen. 

6. Zurück zu Ihrem Dashboard. Sie sehen jetzt, dass die Abfrage (Anzahl der Ergebnisse) im Dashboard angezeigt wird. Dies ist nützlich, wenn Sie schnell alle kritischen Aktionen sehen kürzlich Ihr Abonnement z.B.. eine neue Rolle zugewiesen wurde, oder ein virtuellen Computer wurde gelöscht.

    ![Aktivitätsprotokoll Dashboard fixiert](./media/monitoring-get-started/monitor-act-log-db.png)

7. Die Kachel **Überwachen** und auf Abschnitt **Metrics** . Sie müssen zunächst eine Ressource auswählen, Filtern und auswählen, mit der Dropdown-Optionen am Anfang des Blades.

    ![Ressource für Metriken filtern](./media/monitoring-get-started/monitor-met-filter.png)

    Alle Azure Ressourcen ausgeben [**Metriken**](./monitoring-overview-metrics.md). Diese Ansicht vereint alle zentralen Konsole leicht verstehen Leistung von Ressourcen.

8. Nach Auswahl eine Ressource werden alle verfügbare Metriken auf der linken Seite des Blades angezeigt. Sie können mehrere Messgrößen gleichzeitig durch Auswählen von Metriken Diagramm und Diagrammbereich und ändern. Sie können auch alle metrischen Alerts festlegen für diese Ressource anzeigen.

    ![Blatt Metrik](./media/monitoring-get-started/monitor-metric-blade.png)

    > [AZURE.NOTE] Einige Metriken werden nur die Ressource [Anwendung Einblicke](../application-insights/app-insights-overview.md) oder Windows oder Linux Azure-Diagnose aktivieren.

9. Wenn Sie Ihr Diagramm zufrieden sind, können die Schaltfläche **Pin** Sie Ihrem Dashboard anheften.

10. **Monitor** -Blade und auf **Diagnoseprotokollen**.

    ![Diagnoseprotokolle blade](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [**Diagnoseprotokolle**](monitoring-overview-of-diagnostic-logs.md) sind Protokolle ausgegeben *durch* eine Ressource, die Daten über den Vorgang mit dieser Ressource. Network Security Group Regel Indikatoren und Logik App Workflow Protokolle sind z. B. beide Arten von Diagnoseprotokollen. Diese Protokolle können in ein Speicherkonto gespeichert, an einen Hub Ereignis übertragen oder [Protokollanalyse](../log-analytics/log-analytics-overview.md)gesendet. Protokollanalyse ist Microsofts operative Intelligence für erweiterte Such- und Warnung.
   
    Im Portal können Sie anzeigen und Filtern einer Liste aller Ressourcen in Ihrem Abonnement identifizieren sie Diagnoseprotokolle aktiviert haben.

11. Klicken Sie auf eine Ressource in die Diagnoseprotokolle Blade. Diagnoseprotokolle in ein Speicherkonto gespeichert wird, sehen Sie eine Liste der stündlichen Protokolle, die Sie direkt herunterladen können.

    ![Diagnoseprotokolle für eine Ressource](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    Sie können auch **Diagnostische Einstellungen**einrichten oder Ändern der Standardeinstellungen für Archivierung ein Speicherkonto ermöglicht, streaming Event Hubs oder Protokollanalyse Arbeitsbereich senden klicken.

    ![Aktivieren von Diagnoseprotokollen](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    Diagnoseprotokolle Protokollanalyse eingerichtet, können Sie sie im Bereich **protokollsuche** des Portals suchen.

12. Navigieren Sie zum Abschnitt **Alerts** des Monitor-Blades.

    ![Alerts-Blade für Öffentliche](./media/monitoring-get-started/monitor-alerts-nopp.png)

    Hier können Sie alle [**Alerts**](./monitoring-overview-alerts.md) für Azure Ressourcen verwalten. Dazu gehören Benachrichtigungen Metriken, Aktivität protokollieren Ereignisse (in privaten Vorschau) Anwendung Einblicke Webtests (Speicherorte) und Application Insights proaktive Diagnose. Alarme auslösen per e-Mail gesendet werden oder HTTP POST Webhook URL.
   
13. Klicken Sie zum Erstellen einer Warnung **metrische Warnung hinzufügen** .

    ![metrische Benachrichtigung hinzufügen](./media/monitoring-get-started/monitor-alerts-add.png)

    Eine Warnung kann dann in Ihr Dashboard Zustand jederzeit sehen angeheftet werden.

14. Monitor-Abschnitt enthält auch Links zu [Anwendung Einblicke](../application-insights/app-insights-overview.md) und [Protokollanalyse](../log-analytics/log-analytics-overview.md) Management. Diese Microsoft-Serverprodukte haben umfassende Integration mit Azure.

15. Wenn Sie keine Anwendung Einblicke oder Protokollanalyse verwenden, sind wahrscheinlich Azure Monitor eine Partnerschaft mit aktuellen Überwachung, Protokollierung und Alarmierung Produkte. Finden Sie eine vollständige Liste unserer [Partnerseite](./monitoring-partners.md) und Hinweise zu integrieren.

Erstellen Sie folgendermaßen Fixieren aller relevante Kacheln ein Dashboard und umfassende Einblicke in Ihre Anwendung und Infrastruktur wie diese:

![Azure Monitor-dashboard](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie die [Übersicht über Azure Monitor](./monitoring-overview.md)
