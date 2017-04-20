<properties
    pageTitle="Vertikale Skalierung von Azure Virtual Machine Maßstab legt | Microsoft Azure"
    description="Eine virtuelle Maschine auf Überwachungswarnmeldungen mit Azure Automation vertikal skalieren"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="madhana"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="guybo"/>

# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Vertikal skalieren mit virtuellen skalieren

Dieser Artikel beschreibt die vertikale Skalierung von Azure [Virtual Machine Maßstab legt](https://azure.microsoft.com/services/virtual-machine-scale-sets/) mit oder ohne erneute. Vertikale Skalierung des VMs sind nicht im Maßstab legt finden Sie [vertikal skalieren Azure VM mit Azure Automation](../virtual-machines/virtual-machines-windows-vertical-scaling-automation.md).

Vertikale Skalierung, auch _Vergrößern_ und _Verkleinern_, bedeutet erhöhen oder Verringern der Größe einer Arbeitslast auf Virtual Machine (VM). Vergleichen Sie dies mit [horizontale Skalierung](./virtual-machine-scale-sets-autoscale-overview.md), so genannte _Skalieren_ und _Dezimalstellen_, die Anzahl der VMs je nach Arbeitslast geändert wird.

Nun ist eine vorhandene VM entfernen und durch eine neue ersetzen. Beim Vergrößern oder Verkleinern des VMs in einer VM skalieren soll in einigen Fällen Größe vorhandene virtuelle Computer und Ihre Daten beibehalten, während in anderen Fällen Sie Bereitstellen neuer VMs die neue Größe müssen. Dieses Dokument beschreibt beide Fälle.

Vertikale Skalierung kann hilfreich sein:

- Ein Dienst auf virtuellen Computern erstellt ist (z. B. am Wochenende) ausgelastet. VM zu verkleinern kann monatliche Kosten reduzieren.
- VM vergrößern, um größere Nachfrage ohne zusätzliche VMs.

Sie können einrichten vertikale Skalierung, ausgelöste auf metrische Grundlage Alerts von VM Skalierung festlegen. Aktivierung die Warnung wird ausgelöst ein Webhook dieser Trigger ein Runbook die Waage umfassen kann nach oben oder unten festgelegt. Vertikale Skalierung kann folgendermaßen konfiguriert werden:

1. Erstellen Sie ein Konto Azure Automatisierung mit Ausführen als.
2. Importieren Sie vertikale Skalierung von Azure Automatisierung Runbooks VM Maßstab legt Ihr Abonnement.
3. Ihr Runbook eine Webhook hinzufügen.
4. Die VM Skalierung festlegen Webhook Benachrichtigung mit eine Warnung hinzugefügt.

> [AZURE.NOTE] Vertikale Skalierung nur möglich innerhalb bestimmter Bereiche VM Größe. Vergleich der Spezifikationen jeder Größe vor beliebig skalieren (höherer Zahl gibt nicht die immer größeren VM). Sie können zwischen den folgenden Größen skalieren:

>| VM-Größen skalieren Paar |   |
|---|---|
|  Standard_A0 | Standard_A11 |
|  Standard_D1 |  Standard_D14 |
|  Standard_DS1 |  Standard_DS14 |
|  Standard_D1v2 |  Standard_D15v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Registrieren Sie Azure Automatisierung mit Ausführen als

Zunächst müssen Sie ist ein Azure Automation-Konto erstellen, die Instanzen VM festlegen skalieren verwendet Runbooks gehostet wird. Kürzlich eingeführte [Azure Automation](https://azure.microsoft.com/services/automation/) das Feature "Ausführen als Konto" die Einstellung von Dienstprinzipalnamen für die automatische Ausführung der Runbooks im Auftrag des Benutzers sehr einfach macht. Weitere Informationen finden Sie in folgenden Artikel:

* [Runbooks mit Azure ausführen als Konto authentifizieren](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Ihr Abonnement importieren Sie Azure Automatisierung vertikale Skalierung runbooks

Benötigt für die vertikale Skalierung der VM Maßstab legt Runbooks werden bereits in Azure Automation Runbook Galerie veröffentlicht. Importieren Schritte sie Ihr Abonnement die in diesem Artikel:

* [Runbook und Modul Kataloge für Azure Automation](../automation/automation-runbook-gallery.md)

Wählen Sie die Option Galerie durchsuchen Runbooks im aus:

![Runbooks importiert werden][runbooks]

Runbooks, die importiert werden, werden angezeigt. Wählen Sie Runbook basierend auf vertikale Skalierung mit oder ohne erneute soll:

![Runbooks Galerie][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Ihr Runbook ein Webhook hinzufügen

Wenn Sie importierte hinzufügen Runbooks, müssen eine Webhook Runbooks ausgelöst in eine Warnung von VM Skalierung festlegen können. Erstellen einer Webhook für Ihr Runbook Details werden in diesem Artikel beschrieben:

* [Azure Automatisierung webhooks](../automation/automation-webhooks.md)

> [AZURE.NOTE] Stellen Sie sicher, dass Webhook URI vor dem Webhook Dialogfeld schließen, wie Sie dies im nächsten Abschnitt werden kopiert.

## <a name="add-an-alert-to-your-vm-scale-set"></a>Hinzufügen einer Benachrichtigung der VM Skalierung festlegen

Es folgt ein PowerShell-Skript zeigt wie VM Skalierung festlegen eine Warnung hinzugefügt. Die folgenden Artikel zu den Namen der Metrik für die Warnung ausgelöst: [Azure Monitor Skalierung allgemeinen Bewertungsgrundlagen](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [AZURE.NOTE] Es wird empfohlen, einen angemessenen Zeitfenster für die Warnung konfigurieren, um vermeiden vertikale Skalierung und alle zugehörigen Unterbrechung, oft. Sollten Sie ein mindestens 20 bis 30 Minuten. Betrachten Sie horizontale Skalierung benötigen Sie eine Unterbrechung zu vermeiden.

Weitere Informationen zum Erstellen von Warnungen finden Sie in folgenden Artikeln:

* [Azure Monitor PowerShell Schnellstart-Beispiele](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Azure Monitor plattformübergreifende CLI Schnellstart-Beispiele](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Zusammenfassung

Dieser Artikel zeigt Beispiele für vertikale Skalierung. Mit diesen - automatisierungskonto Runbooks, Webhooks, Alarme können Sie eine Vielzahl von Ereignissen mit angepassten Aktionen verbinden.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
