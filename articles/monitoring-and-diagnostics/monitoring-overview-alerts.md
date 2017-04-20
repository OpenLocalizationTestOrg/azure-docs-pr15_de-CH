<properties
    pageTitle="Überblick in Microsoft Azure | Microsoft Azure"
    description="Alarme können Azure ressourcenmetriken, Ereignisse und Protokolle überwachen und benachrichtigt werden, wenn eine angegebene Bedingung erfüllt ist."
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
    ms.date="09/24/2016"
    ms.author="robb"/>

# <a name="overview-of-alerts-in-microsoft-azure"></a>Überblick in Microsoft Azure


Dieser Artikel beschreibt, welche Alerts nutzen sind und wie mit ihnen.  

## <a name="what-are-alerts"></a>Was sind Alerts?
Alerts sind eine Methode der Überwachung Azure ressourcenmetriken, Ereignisse oder Protokolle und dann benachrichtigt wird, wenn eine Bedingung erfüllt ist angeben.

Erhalten Sie Alerts für:

- **Metrikwerte**: Warnung, wenn der Wert einer angegebenen Metrik einen Schwellenwert überschreitet, die in Richtung zuweisen. D. h. löst sowohl wenn die Bedingung erfüllt ist und dann später als Bedingung ist nicht erfüllt.
- **Aktivität protokollieren von Ereignissen**: Diese Warnung auslösen jedes Ereignisses oder nur bei eine bestimmten Anzahl von Ereignissen.

## <a name="alerts-in-different-azure-services"></a>Alarme in verschiedenen Azure services

Alarme werden über verschiedene Dienste, einschließlich:

- **Anwendung Einblicke**: ermöglicht web Test und Metrik Alerts. Finden Sie unter [Alerts Anwendung Erkenntnisse](../application-insights/app-insights-alerts.md) und [Monitor Verfügbarkeit und Reaktionsfähigkeit einer Website](../application-insights/app-insights-monitor-web-app-availability.md).
- **Protokollanalyse (Operations Management Suite)**: ermöglicht das routing von Diagnoseprotokollen Protokollanalyse. Operations Management Suite ermöglicht Metrik, Protokoll und sonstige Benachrichtigungstypen. Weitere Informationen finden Sie unter [Alerts in Protokollanalyse](../log-analytics/log-analytics-alerts.md).  
- **Azure-Monitor**: Alerts für metrische Werte und Aktivität protokollieren von Ereignissen ermöglicht. Azure Monitor enthält [Azure Monitor REST-API](https://msdn.microsoft.com/library/dn931943.aspx).  Weitere Informationen finden Sie unter [Using Azure-Portal, PowerShell oder die Befehlszeilenschnittstelle Alerts erstellen](insights-alerts-portal.md).

## <a name="alert-actions"></a>Warnaktionen
Sie können eine Warnung Folgendes konfigurieren:

- Senden Sie e-Mail-Benachrichtigungen, Dienstadministrator, Co-Administratoren oder zusätzliche e-Mail-Adressen.
- Rufen Sie eine Webhook, wodurch Sie zusätzliche Automatisierung Aktionen.

 ![Alerts erläutert](./media/monitoring-overview-alerts/AlertsOverviewResource3.png)


## <a name="next-steps"></a>Nächste Schritte

Ruft Informationen zu Warnregeln und mit konfigurieren:

- [Azure-portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [Befehlszeilenschnittstelle (CLI)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
