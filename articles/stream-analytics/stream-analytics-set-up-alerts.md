<properties
    pageTitle="Einrichten von Alerts für Abfragen in Stream Analytics | Microsoft Azure"
    description="Grundlegendes zu Stream Analytics warnen"
    keywords="Einrichten von alerts"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Alarme für Azure Stream Analytics-Aufträge einrichten

## <a name="introduction-monitor-page"></a>: Einführungsseite Monitor

Sie können Alarme einrichten, um eine Warnung auslöst, wenn eine Metrik einen Zustand erreicht, die Sie angeben.

Beispielsweise "Ausgabeereignisse der letzten 15 Minuten ist < 100 e-Mail-Benachrichtigung senden an e-Mail-Id: xyz@company.com”.

Regeln können Kennzahlen über das Portal einrichten oder konfigurierten [programmgesteuert](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) Protokolle Daten werden können.

## <a name="set-up-alerts-through-the-azure-classic-portal"></a>Einrichten von Alerts über Azure-Verwaltungsportal

Gibt es zwei Arten Alarme in Azure-Verwaltungsportal einrichten:  

1.  Die Registerkarte **Monitor** des Projekts Stream Analytics  
2.  Das Protokoll in das Management-services  

## <a name="set-up-alert-through-the-monitor-tab-of-the-job-in-the-portal"></a>Warnung über die Registerkarte Monitor des Auftrags im Portal einrichten

1.  Wählen Sie auf der Registerkarte überwachen die Metrik und setup-Regeln klicken Sie auf **Regel hinzufügen** , unten im Dashboard.  

    ![Dashboard](./media/stream-analytics-set-up-alerts/01-stream-analytics-set-up-alerts.png)  

2.  Definieren Sie den Namen und die Beschreibung der Warnung  

    ![Regel erstellen](./media/stream-analytics-set-up-alerts/02-stream-analytics-set-up-alerts.png)  

3.  Geben Sie die Schwellenwerte, alert Bewertung Fenster und Aktionen für die Warnung  

    ![Startbedingungen definieren](./media/stream-analytics-set-up-alerts/03-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-through-the-operations-logs"></a>Einrichten von Alerts über das Log

1.  Wechseln Sie zur Registerkarte **Alerts** in Management Services in [Azure-Verwaltungsportal](https://manage.windowsazure.com).  
2.  Klicken Sie auf **Regel hinzufügen**  

    ![Kriterien](./media/stream-analytics-set-up-alerts/04-stream-analytics-set-up-alerts.png)  

3.  Definieren Sie den Namen und die Beschreibung der Warnung. Wählen Sie 'Stream Analytics Diensttyp und Auftragsname Dienstnamen ein.  

    ![Warnung definieren](./media/stream-analytics-set-up-alerts/05-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-in-the-azure-portal"></a>Alarme in Azure-Portal einrichten ##

Azure-Portal Stream Analytics-Projekt, das Sie interessiert, auf Durchsuchen Sie, und klicken Sie im Abschnitt **Überwachung** .  Öffnet **Metrik** Blatt klicken Sie **Warnung hinzufügen** .

  ![Azure Portal Einrichtung](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

Sie können die Warnregel nennen, und eine Beschreibung, die in der e-Mail angezeigt wird.

Wenn Sie Kennzahlen auswählen wird eine Bedingung und einen Schwellenwert Wert für die Metrik auswählen.

  ![Azure Portal wählen Metrik](./media/stream-analytics-set-up-alerts/07-stream-analytics-set-up-alerts.png)  

Weitere Informationen zum Konfigurieren von Warnungen in Azure-Portal finden Sie unter [Benachrichtigung empfangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  

## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
