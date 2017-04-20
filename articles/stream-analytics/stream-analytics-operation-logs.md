<properties 
    pageTitle="Debuggen mit und Dienstprotokolle in Stream Analytics | Microsoft Azure" 
    description="Gewusst-wie-Verwendung Stream Analytics Protokolle" 
    keywords="Service-Protokolle"
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

# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Debuggen Sie und dem Betrieb Protokolle Stream Analytics-Aufträge

Alle Azure Services liefern Betrieb Protokollierungsnachrichten Benutzern erfassen Informationen zu Management-Operationen. In Azure Stream Analytics dazu dienen für debugging-Zwecke wie Anzeigen des Jobstatus Job-Status und Fehlermeldungen aus den Status eines Auftrags im Laufe der Zeit verfolgen starten Verarbeitung ausgegeben.

## <a name="find-operation-logs-in-the-azure-management-portal"></a>Betrieb im Azure-Verwaltungsportal Protokolle suchen

Protokolle können auf zwei Arten zugegriffen werden:  

- Dashboard Stream Analytics-Auftrags  
- Management-Services in Azure-Verwaltungsportal  

## <a name="dashboard-of-the-stream-analytics-job"></a>Dashboard Stream Analytics-Auftrags

Eine Verknüpfung mit der entsprechenden Protokolle einen Stream Analytics-Auftrag wird auf der Dashboard Registerkarte angezeigt. Wenn Sie auf diesen Link klicken, wird es der so Filter zeigt aktuelle Protokolle für diesen Auftrag.

  ![Wählen Sie Management Services-Protokollen](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Management Services

Manuell die Protokolle für Stream Analytics und andere Dienste in Azure-Verwaltungsportal navigieren Sie zu:

1.  Klicken Sie auf **Management-Services** in [Azure-Verwaltungsportal](https://manage.windowsazure.com).
2.  Wählen Sie **Stream Analytics** für **Typ** und den Namen des Auftrags **Dienstnamens**.  

  ![Wählen Sie Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Überwachungsprotokolle in Azure-Portal suchen ##

Betriebsprotokolle für den Stream Analytics Job im Azure-Portal klicken Sie auf **Durchsuchen** und wählen Sie **Überwachungsprotokolle**.

  ![Azure-Portal wählen Sie Stream Analytics](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Anzeigen der Ereignisse der letzten 7 Tage für alle Ressourcen in Ihrem Abonnement eine-Blade wird geöffnet.  Sie können zum Ereignisse eines Typs angeben oder Zeitrahmen auf **Filter** filtern.

  ![Azure-Portal wählen Sie Stream Analytics](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Details zum Abrufen

Sie können nach Zeitraum und zum Anzeigen der Protokolle für den Job Status filtern.

Azure-Verwaltungsportal klicken Sie auf die Schaltfläche **Details** unten im Fenster Details zum ausgewählten Ereignis anzuzeigen. 

  ![Wählen Sie Details](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Klicken Sie auf einen Protokolleintrag, der detaillierte Ereignisse innerhalb in Azure-Portal.

  ![Azure-Portal Details auswählen](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Dort können Sie das Blade **Details** auf das Ereignis öffnen.

  ![Azure-Portal Details auswählen](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Debuggen eines Auftragsfehlers

Azure-Verwaltungsportal klicken Sie auf das Suchsymbol und Typ 'Fehler'. Dies ergibt alle Protokolle mit Fehlern. 

  ![Debuggen eines Auftragsfehlers](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

In Azure-Portal können Sie nach Maß an **kritischen** Ereignisse anzeigen filtern.

  ![Azure Portal Debuggen](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Sie können wählen Sie eine der Fehler und klicken Sie auf **Details** für Weitere Informationen zu diesem Fehler.  Einige Fehlermeldungen enthalten auch Informationen zum Beheben des Problems. 

  ![Vorgangsdetails](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

Falls Sie müssen [Support](https://azure.microsoft.com/support/options/) oder Informationen an das Team über das [MSDN-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), beachten Sie die Vorgangsdetails speziell die **Korrelations-ID**. 

## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
