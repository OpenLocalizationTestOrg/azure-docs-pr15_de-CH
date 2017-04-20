<properties
    pageTitle="Benachrichtigung für Azure Services erhalten | Microsoft Azure"
    description="Benachrichtigt werden, wenn Warnregeln Bedingung erfüllt sind."
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
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="receive-alert-notifications"></a>Benachrichtigung erhalten

Basierend auf den Kennzahlen zur Überwachung oder Ereignisse in Azure Services erhalten.

Warnregeln auf einen Metrikwert überquert der Wert einer angegebenen Metrik einen Schwellenwert zugeordnet, die Warnregel aktiv und eine Benachrichtigung senden. Für eine Warnregel für Ereignisse eine Regel eine Benachrichtigung senden auf *jedes* Ereignis und nur beim Auftreten einer bestimmten Anzahl von Ereignissen.

Wenn Sie eine Warnregel erstellen, können Sie Optionen Dienstadministratoren und Co-Administratoren oder ein anderer Administrator angeben können eine e-Mail-Benachrichtigung senden auswählen. Eine Benachrichtigung wird gesendet, wenn die Regel aktiv ist und eine Warnung aufgelöst wird.

Sie können die [REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx) oder [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) konfigurieren und Informationen zu Warnregeln programmgesteuert.

## <a name="create-an-alert-rule"></a>Erstellen einer Warnregel

1. Klicken Sie im [Portal](https://portal.azure.com/)auf **Durchsuchen**und dann auf eine Ressource, die Sie überwachen möchten.

2. Klicken Sie auf **Warnregeln** Kachel **Operationen** objektiv.

3. Klicken Sie **Warnung hinzufügen** .

    ![Benachrichtigung hinzufügen](./media/insights-receive-alert-notifications/Insights_AddAlert.png)

4. Sie können die Warnregel nennen, und eine Beschreibung, die in der e-Mail angezeigt wird.

5. Wenn Sie **Kennzahlen** auswählen wird eine Bedingung und einen Schwellenwert Wert für die Metrik auswählen. Dies ist der Zeitraum, der Monitor und der Zeichnungsfläche Warnung Aktivität Azure verwendet.

    ![Bedingung und Schwellenwert](./media/insights-receive-alert-notifications/Insights_ConditionAndThreshold.png)

6. Sie können **Ereignisse**auch benachrichtigt, wenn ein bestimmtes Ereignis geschieht.

    ![Ereignisse](./media/insights-receive-alert-notifications/Insights_Events.png)

7. Schließlich können Sie per e-Mail benachrichtigt Administratoren zuständig.

Nach dem Klicken auf **Speichern**werden innerhalb weniger Minuten Sie informiert, wenn die gewählte Metrik den Schwellenwert überschreitet.

## <a name="managing-your-alert-rules"></a>Verwalten von Warnregeln

Nachdem Sie eine Warnregel erstellt haben, können eine Vorschau der Warnung Schwellenwert verglichen Metrik vom Vortag.

![Ereignisse](./media/insights-receive-alert-notifications/Insights_EditAlert.png)


Natürlich können Sie diese Regel, und **Deaktivieren** oder **Aktivieren** möchten Sie vorübergehend beenden Benachrichtigungen zu bearbeiten.

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren Webhooks auf Ihre Alerts](insights-webhooks-alerts.md) Route Benachrichtigungen zu verschiedenen Kanälen
* [Dienstmetrik überwachen](insights-how-to-customize-monitoring.md) um sicherzustellen, dass der Dienst verfügbar ist und reagiert wird.
* Sammeln Sie detaillierte Hochfrequenz-Metriken für den Dienst [Überwachung und Diagnose](insights-how-to-use-diagnostics.md) .
* [Monitor-Verfügbarkeit und Reaktionsfähigkeit der Webseite](../application-insights/app-insights-monitor-web-app-availability.md) Anwendung zum heraus, ob Ihre Seite finden ist.
* [Leistung der Anwendung überwachen](../application-insights/app-insights-azure-web-apps.md) möchten Sie wissen genau, wie Code in der Cloud ausgeführt wird.
* [Anzeigen von Ereignissen und Überwachungsprotokolle](insights-debugging-with-events.md) zu, die in Ihrem Dienst geschehen.
* [Track-Dienststatus](insights-service-health.md) wann Azure Leistung Abbau- oder Service-Unterbrechung aufgetreten ist.
