<properties
    pageTitle="Mithilfe von PowerShell Alarme für Azure Services erstellen | Microsoft Azure"
    description="Verwenden Sie PowerShell Azure Alerts erstellen die Benachrichtigung oder Automatisierung auslösen können, wenn die angegebene Bedingung erfüllt sind."
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

# <a name="use-powershell-to-create-alerts-for-azure-services"></a>Mithilfe von PowerShell Alarme für Azure Services erstellen

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt, wie Azure mithilfe von PowerShell Alarme einrichten.  

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


Für Weitere Informationen geben Sie immer ```get-help``` und dann den PowerShell-Befehl auf Hilfe benötigen.

## <a name="create-alert-rules-in-powershell"></a>Erstellen von Warnregeln in PowerShell

1. Melden Sie sich bei Azure.   

    ```PowerShell
    Login-AzureRmAccount

    ```

2. Eine Liste der Abonnements Ihnen zur Verfügung stehen. Stellen Sie sicher, dass Sie das richtige Abonnement verwenden. Andernfalls legen Sie diese auf die richtige Ausgabe mit `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

3.  Zum Auflisten der vorhandenen Regeln auf einer Ressourcengruppe Befehl:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

4. Um eine Regel zu erstellen, müssen Sie einige wichtige Informationen zuerst haben. 
    - Die **Ressourcen-ID** für die Ressource eine Warnung festlegen möchten
    - Die **Metrik Definitionen** für diese Ressource

    Eine Möglichkeit, die Ressourcen-ID ist mit der Azure-Portal. Wenn die Ressource bereits erstellt wurde, wählen sie das Portal. Wählen Sie auf dem nächsten Blatt *Eigenschaften* im Abschnitt *Settings* . Die Ressourcen-ID ist ein Feld in das nächste Blatt. Eine andere Möglichkeit ist [Azure-Ressourcen-Explorer](https://resources.azure.com/)verwenden.

    Eine Beispiel-Ressourcen-Id für eine Webanwendung ist

    ```
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Sie können `Get-AzureRmMetricDefinition` Liste aller metrischen Definitionen für eine bestimmte Ressource anzeigen.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id>
    ```

    Im folgenden Beispiel wird eine Tabelle mit den Namen und die Einheit für die Metrik.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

    ```
    Eine vollständige Liste der verfügbaren Optionen für Get-AzureRmMetricDefinition wird mit Get-MetricDefinitions.


5. Im folgenden Beispiel wird eine Warnung für eine Websiteressource. Die Warnung Trigger bei jeder konsistent Datenverkehr für 5 Minuten und empfängt keinen Datenverkehr für 5 Minuten.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```

6. Webhook erstellen oder e-Mail senden, wenn eine Warnung ausgelöst wird, müssen Sie zunächst erstellen Sie e-Mail- oder Webhooks. Dann sofort erstellen Sie die Regel anschließend mit-Aktionen und wie im folgenden Beispiel gezeigt. Sie können nicht Webhook oder e-Mails mit zuordnen Regeln über PowerShell bereits erstellt.


    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```


7. Verwenden Sie zum Erstellen einer Warnung, die ausgelöst unter einer bestimmten Bedingung im Aktivitätsprotokoll Befehle der form

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmLogAlertRule -Name myLogAlertRule -Location "East US" -ResourceGroup myresourcegroup -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup resourcegroupbeingmonitored -Actions $actionEmail, $actionWebhook
    ```

    -OperationName entspricht ein Ereignis im Aktivitätsprotokoll. Beispiele sind *Microsoft.Compute/virtualMachines/delete* und *microsoft.insights/diagnosticSettings/write*.

    PowerShell-Befehl [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) können Sie eine Liste der möglichen OperationNames. Alternativ können Sie das Azure-Portal zu Aktivitätsprotokoll Abfragen bestimmte über Vorgänge, die Sie eine Benachrichtigung erstellen möchten. Die Vorgänge in der Grafik Protokollansicht die Anzeigenamen angezeigt. Suchen in JSON für den Eintrag, und ziehen Sie den OperationName Wert.   

8. Überprüfen Sie, ob Ihre Alerts ordnungsgemäß erstellt wurde, indem die einzelnen Regeln.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

9. Löschen Sie Ihre Alerts. Diese Befehle löschen zuvor in diesem Artikel erstellten Regeln.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Nächste Schritte

* [Überblick Azure überwachen](monitoring-overview.md) die Arten von Informationen Sie sammeln und überwachen.
* Erfahren Sie mehr über [Webhooks Warnungen konfigurieren](insights-webhooks-alerts.md).
* Erfahren Sie mehr über [Azure Automatisierung Runbooks](..\automation\automation-starting-a-runbook.md).
* Erhalten Sie einen [Überblick über das Sammeln von Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md) sammeln detaillierte Hochfrequenz-Metriken für den Dienst.
* Erhalten Sie eine [Übersicht über Metriken Auflistung](insights-how-to-customize-monitoring.md) um sicherzustellen, dass der Dienst verfügbar ist und reagiert wird.
