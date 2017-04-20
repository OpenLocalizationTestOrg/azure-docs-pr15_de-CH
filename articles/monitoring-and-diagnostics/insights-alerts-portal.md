<properties
    pageTitle="Mithilfe von Azure Portal Alarme für Azure Services erstellen | Microsoft Azure"
    description="Verwenden des Azure-Portals Azure Alerts erstellen die Benachrichtigung oder Automatisierung auslösen können, wenn die angegebene Bedingung erfüllt sind."
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
    ms.date="09/23/2016"
    ms.author="robb"/>

# <a name="use-azure-portal-to-create-alerts-for-azure-services"></a>Mithilfe von Azure Portal Alarme für Azure Services erstellen

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt, wie Azure mithilfe von Azure Portal Alarme einrichten.   

Basierend auf den Kennzahlen zur Überwachung oder Ereignisse in Azure Services erhalten.

- **Metrikwerte** - Warnung ausgelöst, wenn der Wert einer angegebenen Metrik einen Schwellenwert überschreitet, in Richtung zuweisen. D. h. löst sowohl wenn die Bedingung erfüllt ist und dann anschließend als Bedingung ist nicht erfüllt.    
- **Aktivität protokollieren von Ereignissen** – für *jedes* Ereignis und nur bei eine bestimmten Anzahl von Ereignissen eine Benachrichtigung auslösen.


Sie können eine Warnung Folgendes bei:

- e-Mail-Benachrichtigung von Dienstadministratoren und Co-Administratoren
- e-Mail an zusätzliche e-Mails, die Sie angeben.
- Rufen Sie eine webhook
- Starten Sie die Ausführung von Azure Runbook (nur von Azure-Portal)

Konfigurieren und Informationen über Warnregeln

- [Azure-portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [Befehlszeilenschnittstelle (CLI)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)


## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Erstellen einer Warnregel für eine Metrik mit Azure-portal

1. Suchen Sie im [Portal](https://portal.azure.com/)die Ressource sind zu überwachen und wählen sie.

2. **Alarme** und **Warnregeln** Abschnitt Überwachung auswählen Text und Symbol können für verschiedene Ressourcen variieren.  

    ![Überwachung](./media/insights-alerts-portal/AlertRulesButton.png)


3. Wählen Sie **quot** und die Felder ausfüllen.

    ![Benachrichtigung hinzufügen](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Name** der Warnung Regel, und wählen Sie eine **Beschreibung**, die in e-Mail-Benachrichtigungen zeigt.
5. Wählen Sie die **Metrik** zu überwachen, und wählen einen **Bedingung** und **Schwellenwert** -Wert für die Metrik. Auch der **Zeitraum** , der metrische Regel muss, bevor die Warnung Trigger erfüllt werden ausgewählt haben. Wenn Sie Periode "PT5M" und die Warnung über 80 % CPU sucht, löst die Warnung so die CPU ständig über 80 % 5 Minuten wurde Nach Abschluss der erste Trigger, wird erneut die CPU unter 80 % 5 Minuten bleibt. Die CPU-Messung tritt jede Minute.   

6. Aktivieren Sie **... Besitzer e-Mail-** Administratoren und Co-Administratoren gesendet werden, wenn die Warnung ausgelöst wird.

7. Wenn zusätzliche e-Mails eine Benachrichtigung erhalten, wenn die Warnung ausgelöst werden soll, fügen sie im Feld **zusätzliche Administrator Email(s)** . Trennen Sie mehrere e-Mails durch Semikolons-*email@contoso.com;email2@contoso.com*

8. Platzieren Sie in einem gültigen URI im Feld **Webhook** soll aufgerufen werden, wenn die Warnung ausgelöst wird.

9. Bei Verwendung von Azure Automation können Sie auswählen, ein Runbook ausgeführt werden, wenn die Warnung ausgelöst wird.

10. Wählen Sie **OK** nach Abschluss der Warnung.   

Innerhalb weniger Minuten die Warnung ist aktiv und wird wie zuvor beschrieben.

## <a name="managing-your-alerts"></a>Ihre Alerts verwalten

Nachdem Sie eine Benachrichtigung erstellt haben, können Sie es und:

- Hier ein Diagramm Metrikschwellenwert und den tatsächlichen Werten vom Vortag.
- Bearbeiten oder löschen.
- **Deaktivieren** oder **Aktivieren** möchten Sie vorübergehend anhalten oder fortsetzen Benachrichtigungen für diese Warnung.



## <a name="next-steps"></a>Nächste Schritte

* [Überblick Azure überwachen](monitoring-overview.md) die Arten von Informationen Sie sammeln und überwachen.
* Erfahren Sie mehr über [Webhooks Warnungen konfigurieren](insights-webhooks-alerts.md).
* Erfahren Sie mehr über [Azure Automatisierung Runbooks](..\automation\automation-starting-a-runbook.md).
* Eine [Übersicht über die Diagnoseprotokolle](monitoring-overview-of-diagnostic-logs.md) und Messwerte detaillierte Hochfrequenz-für den Dienst.
* Erhalten Sie eine [Übersicht über Metriken Auflistung](insights-how-to-customize-monitoring.md) um sicherzustellen, dass der Dienst verfügbar ist und reagiert wird.
