<properties
    pageTitle="Plattformübergreifende Befehlszeilenschnittstelle (CLI) verwenden, um Warnungen für Azure Services erstellen | Microsoft Azure"
    description="Verwenden Sie die Befehlszeilenschnittstelle Azure Alerts erstellen die Benachrichtigung oder Automatisierung auslösen können, wenn die angegebene Bedingung erfüllt sind."
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
    ms.date="10/24/2016"
    ms.author="robb"/>

# <a name="use-the-cross-platform-command-line-interface-cli-to-create-alerts-for-azure-services"></a>Verwenden Sie plattformübergreifende Befehlszeilenschnittstelle (CLI), um Warnungen für Azure Services erstellen

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt, wie Azure mithilfe der Befehlszeilenschnittstelle (CLI) Alarme einrichten.

>[AZURE.NOTE] Azure Monitor ist der neue Name für die sogenannten "Azure Einblicke" bis zum 25. September 2016. Namespaces und somit die folgenden Befehle enthalten jedoch weiterhin die "Erkenntnisse".

Basierend auf den Kennzahlen zur Überwachung oder Ereignisse in Azure Services erhalten.

- **Metrikwerte** - Warnung ausgelöst, wenn der Wert einer angegebenen Metrik einen Schwellenwert überschreitet, in Richtung zuweisen. D. h. löst sowohl wenn die Bedingung erfüllt ist und dann anschließend als Bedingung ist nicht erfüllt.    
- **Aktivität protokollieren von Ereignissen** – für *jedes* Ereignis und nur bei eine bestimmten Anzahl von Ereignissen eine Benachrichtigung auslösen.

Sie können eine Warnung Folgendes bei:

- e-Mail-Benachrichtigung von Dienstadministratoren und Co-Administratoren
- e-Mail an zusätzliche e-Mails, die Sie angeben.
- Rufen Sie eine webhook
- Starten Sie die Ausführung von Azure Runbook (nur von Azure-Portal zu diesem Zeitpunkt)

Konfigurieren und Informationen über Warnregeln

- [Azure-portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [Befehlszeilenschnittstelle (CLI)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Immer erhalten Sie Hilfe zu Befehlen durch Eingabe von Befehlen und - Ende helfen. Zum Beispiel:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a>Erstellen von Warnregeln mithilfe der CLI

1. Führen Sie die erforderlichen Komponenten und bei Azure. [Azure Monitor CLI-Beispiele](insights-cli-samples.md)anzeigen Kurz gesagt, installieren Sie die CLI und führen Sie dieser Befehle aus. Sie erhalten Sie eingeloggt, welches Abonnement Sie verwenden und Sie Azure Monitor Befehle anzeigen.


    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2.  Um vorhandene Regeln für eine Ressourcengruppe aufzulisten, verwenden folgende Form **Azure Einblicke Alarmen Liste Regel** *[Optionen] &lt;ResourceGroup&gt; *

    ```console
    azure insights alerts rule list myresourcegroupname

    ```
3. Um eine Regel zu erstellen, müssen Sie einige wichtige Informationen zuerst haben.
    - Die **Ressourcen-ID** für die Ressource eine Warnung festlegen möchten
    - Die **Metrik Definitionen** für diese Ressource

    Eine Möglichkeit, die Ressourcen-ID ist mit der Azure-Portal. Wenn die Ressource bereits erstellt wurde, wählen sie das Portal. Wählen Sie auf dem nächsten Blatt *Eigenschaften* im Abschnitt *Settings* . Die *Ressourcen-ID* ist ein Feld in das nächste Blatt. Eine andere Möglichkeit ist [Azure-Ressourcen-Explorer](https://resources.azure.com/)verwenden.

    Eine Beispiel-Ressourcen-Id für eine Webanwendung ist

    ```console
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Um eine Liste der verfügbaren Metriken und Einheiten für diese Metriken für das vorherige Beispiel Ressource abzurufen, verwenden Sie folgenden CLI-Befehl:  

    ```console
    azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
    ```

    _PT1M_ ist die Granularität der verfügbaren Messung (1-Minuten-Intervallen). Mit unterschiedlichen Granularitäten bietet Ihnen verschiedene metrische Optionen.


4. Um eine Metrik basierende Warnregel erstellen, verwenden Sie einen Befehl im folgenden Format:

    **Azure Einblicke Alarmen Regelmetrik festlegen** *[Optionen] &lt;RuleName&gt; &lt;Ort&gt; &lt;ResourceGroup&gt; &lt;windowSize&gt; &lt;Operator&gt; &lt;Schwellenwert&gt; &lt;TargetResourceId&gt; &lt;MetricName&gt; &lt;TimeAggregationOperator&gt; *

    Im folgenden Beispiel wird eine Warnung für eine Websiteressource. Die Warnung Trigger bei jeder konsistent Datenverkehr für 5 Minuten und empfängt keinen Datenverkehr für 5 Minuten.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```

5. Webhook erstellen oder e-Mail senden, wenn eine Warnung ausgelöst wird, müssen Sie zunächst erstellen Sie e-Mail- oder Webhooks. Erstellen Sie die Regel sofort danach. Sie können nicht Webhook oder e-Mails mit zuordnen bereits Regeln über die CLI erstellt.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```


6. Verwenden Sie zum Erstellen einer Warnung, die ausgelöst wird unter einer bestimmten Bedingung im Aktivitätsprotokoll Form:

    **Einblicke Alarmen Regel Anmelden festlegen** *[Optionen] &lt;RuleName&gt; &lt;Ort&gt; &lt;ResourceGroup&gt; &lt;OperationName&gt; *

    Zum Beispiel

    ```console
    azure insights alerts rule log set myActivityLogRule eastus myresourceGroupName Microsoft.Storage/storageAccounts/listKeys/action
    ```

    Die OperationName entspricht ein Typ für einen Eintrag im Aktivitätsprotokoll. Beispiele sind *Microsoft.Compute/virtualMachines/delete* und *microsoft.insights/diagnosticSettings/write*.

    PowerShell-Befehl [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) können Sie eine Liste der möglichen OperationNames. Alternativ können Sie das Azure-Portal zu Aktivitätsprotokoll Abfragen bestimmte über Vorgänge, die Sie eine Benachrichtigung erstellen möchten. Die Vorgänge in der Grafik Protokollansicht die Anzeigenamen angezeigt. Suchen in JSON für den Eintrag, und ziehen Sie den OperationName Wert.   

7. Um zu überprüfen, ob Ihre Alerts ordnungsgemäß erstellt wurde, indem Sie eine einzelne Regel aus.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```

8. Verwenden Sie einen Befehl des Formulars, um Regeln zu löschen:

    **Einblicke Alarmen Regel löschen** [Optionen] &lt;ResourceGroup&gt; &lt;Regelname&gt;

    Diese Befehle löschen zuvor in diesem Artikel erstellten Regeln.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```



## <a name="next-steps"></a>Nächste Schritte

* [Überblick Azure überwachen](monitoring-overview.md) die Arten von Informationen Sie sammeln und überwachen.
* Erfahren Sie mehr über [Webhooks Warnungen konfigurieren](insights-webhooks-alerts.md).
* Erfahren Sie mehr über [Azure Automatisierung Runbooks](..\automation\automation-starting-a-runbook.md).
* Erhalten Sie einen [Überblick über das Sammeln von Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md) sammeln detaillierte Hochfrequenz-Metriken für den Dienst.
* Erhalten Sie eine [Übersicht über Metriken Auflistung](insights-how-to-customize-monitoring.md) um sicherzustellen, dass der Dienst verfügbar ist und reagiert wird.
