<properties
    pageTitle="Azure Service Health Monitor-Aktivitätsprotokolle Azure mit überwachen | Microsoft Azure"
    description="Finden Sie heraus, bei Azure Leistung Abbau- oder Service-Unterbrechung aufgetreten ist. "
    authors="rboucher"
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
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="track-azure-service-health-using-azure-monitor-activity-logs"></a>Verfolgen Sie Azure Service Health Monitor-Aktivitätsprotokolle Azure mit

Azure veröffentlicht jedes Mal eine Service-Unterbrechung oder eine Leistungsminderung. Diese Ereignisse in Azure-Portal durchsuchen und Sie können auch die [REST-API](https://msdn.microsoft.com/library/azure/dn931927.aspx) oder [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) programmgesteuert auf den vollständigen Satz von Ereignissen.

## <a name="browse-the-service-health-logs-for-your-subscription"></a>Durchsuchen Sie Service Health Protokolle für Ihr Abonnement

1. [Azure-Portal](https://portal.azure.com/)anmelden.

2. Auf **Home** finden Sie eine Kachel **Dienststatus**aufgerufen. Klicken Sie auf.

    ![Startseite](./media/insights-service-health/Insights_Home.png)

3. Finden Sie eine Liste aller Bereiche in Azure. Klicken Sie auf eine Region zu Aktivitätsprotokoll Abfrage mit Service-Fälle, die Ihre Abonnements in den letzten 24 Stunden ausgewirkt haben.

    ![Aktivität protokollieren Abonnement-Dienststatus](./media/insights-service-health/AzureActivityLogServiceHealth3.png)

4. Durch Klicken auf das Ereignis in der Tabelle können Sie die Details eines einzelnen Ereignisses anzeigen.

5. Ändern Sie **Timespan** um einen längeren Zeitraum anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

* [Monitor-Verfügbarkeit und Reaktionsfähigkeit der Webseite](../application-insights/app-insights-monitor-web-app-availability.md) Anwendung zum heraus, ob Ihre Seite finden ist.
