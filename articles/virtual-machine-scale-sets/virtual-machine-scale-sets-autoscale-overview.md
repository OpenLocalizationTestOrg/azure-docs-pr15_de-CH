<properties
    pageTitle="Automatische Skalierung und Virtual Machine skalieren legt | Microsoft Azure"
    description="Lernen Sie mit Diagnose Autoscale Ressourcen automatisch Skalieren von virtuellen Maschinen in einer Skala."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="automatic-scaling-and-virtual-machine-scale-sets"></a>Automatische Skalierung und virtuellen Gruppen skalieren

Automatische Skalierung von virtuellen Computern in einer Skala ist das Erstellen bzw. Löschen von Computern in der Gruppe Bedarf Anforderungen übereinstimmen. Zunehmender Arbeitsaufwand erfordern eine Anwendung zusätzliche Ressourcen zu Aufgaben effektiv ausführen.

Automatische Skalierung ist automatisiert, erleichtert die Verwaltung. Der Verwaltungsaufwand reduziert wird, müssen Sie nicht ständig Überwachen der Systemleistung oder entscheiden, wie Sie Ressourcen verwalten. Skalierung ist ein elastische. Mehr Ressourcen als Last hinzugefügt werden, aber als Bedarf Ressourcen verringert Kosten und Leistung verwalten entfernt werden können.

Automatische Skalierung auf einer Skala mithilfe einer Vorlage Azure Ressourcenmanager, Azure PowerShell, Azure-CLI oder Azure-Portal einrichten.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Mit Ressourcen-Manager Vorlagen Skalierung einrichten

Anstatt bereitstellen und Verwalten von jeder Ressource der Anwendung, eine Vorlage, die alle Ressourcen in einer einzigen koordinierten Operation bereitgestellt. In der Vorlage Anwendungsressourcen definiert und Bereitstellungsparameter für verschiedene umge bungen angegeben. Die Vorlage umfasst JSON und Ausdrücke mit Werten für die Bereitstellung erstellen. Betrachten Sie weitere [Vorlagen Azure-Ressourcen-Manager erstellen](../resource-group-authoring-templates.md).

In der Vorlage Geben Sie das Element Kapazität:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 3
    },

Kapazität gibt die Anzahl der virtuellen Computer in der Gruppe. Sie können die Kapazität manuell Bereitstellen einer Vorlage mit einem anderen Wert ändern. Wenn Sie eine Vorlage ändern, dass nur die Kapazität bereitstellen, können Sie nur das SKU-Element mit aktualisierten einschließen.

Die Kapazität der Maßstab mit der Kombination der Ressource AutoscaleSettings und diagnoseerweiterung nicht automatisch geändert.

### <a name="configure-the-azure-diagnostics-extension"></a>Konfigurieren der Azure-Diagnose-Erweiterung

Automatische Skalierung ist nur möglich, wenn Metriken Auflistung auf jedem virtuellen Computer in der Skalierungsgruppe. Azure Diagnostics Erweiterung bietet die Überwachung und Diagnose-Funktionen, die die Metriken Auflistung Autoscale Ressource Bedürfnisse. Sie können die Erweiterung als Teil der Ressourcen-Manager-Vorlage.

Dieses Beispiel zeigt die verwendeten Variablen zu diagnoseerweiterung zu konfigurieren:

    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

Parameter werden bereitgestellt, wenn die Vorlage bereitgestellt wird. In diesem Beispiel werden der Name des Speicherkontos Daten und den Namen der eingestellten Skalierung aus der Daten bereitgestellt. In diesem Beispiel Windows Server werden auch der Thread Count-Leistungsindikator gesammelt. Alle verfügbaren Leistungsindikatoren in Windows oder Linux verwendet werden, um Diagnoseinformationen sammeln und Erweiterung Konfiguration enthalten sein.

Dieses Beispiel zeigt die Definition der Erweiterung in der Vorlage:

    "extensionProfile": {
      "extensions": [
        {
          "name": "Microsoft.Insights.VMDiagnosticsSettings",
          "properties": {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.5",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
              "storageAccount": "[variables('diagnosticsStorageAccountName')]"
            },
            "protectedSettings": {
              "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
              "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
              "storageAccountEndPoint": "https://core.windows.net"
            }
          }
        }
      ]
    }

Beim Ausführen die diagnoseerweiterung werden die Daten in einer Tabelle, die im Speicherkonto befindet, die Sie angeben. In der WADPerformanceCounters-Tabelle finden Sie die gesammelten Daten:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a>Konfigurieren Sie die AutoScaleSettings-Ressource

AutoscaleSettings Ressource verwendet die Informationen aus der diagnoseerweiterung erhöhen oder verringern Sie die Anzahl von virtuellen Computern in der Skalierungsgruppe entscheiden.

Dieses Beispiel zeigt die Konfiguration der Ressource in der Vorlage:

    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 650
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 550
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
      }
    }

Im Beispiel oben werden zwei Regeln erstellt, für die automatischen Skalierung Aktionen definieren. Die erste Regel definiert die Skalierung Aktion und die zweite Regel Skalierung in Aktion. Diese Werte werden in den Regeln bereitgestellt:

- **MetricName** - entspricht dieser Wert der Leistungsindikator in der Variablen Wadperfcounter für die diagnoseerweiterung definiert. Im obigen Beispiel wird der Thread Count-Leistungsindikator verwendet.  
- **MetricResourceUri** - Wert ist der Ressourcenbezeichner des virtuellen Computers Skalierung festlegen. Diese Kennung enthält den Namen der Ressource, den Namen des Ressourcenanbieters und den Namen der Maßstab skalieren.
- **TimeGrain** – ist dieser Wert die Granularität der Metriken gesammelt werden. Im vorhergehenden Beispiel werden Daten in Intervallen von einer Minute. Dieser Wert wird mit bestimmten verwendet.
- **Statistik** – dieser Wert bestimmt, wie die Metriken kombiniert werden, um die automatische Skalierung Aktion aufzunehmen. Mögliche Werte sind: Mittelwert, Min, Max.
- **Zeitfenster** – dieser Wert ist die Zeitspanne, in der Instanz Daten. Es muss zwischen 5 und 12 Stunden.
- **TimeAggregation** – dieser Wert bestimmt, wie die gesammelten Daten mit der Zeit kombiniert werden. Der Standardwert ist Durchschnitt. Mögliche Werte sind: Durchschnitt mindestens höchstens, letzte, Summe, Count.
- **Operator** -ist dieser Wert der Operator, mit dem die Daten und den Schwellenwert verglichen. Mögliche Werte sind: NotEquals, GreaterThan GreaterThanOrEqual, LessThan, LessThanOrEqual entspricht.
- **Schwellenwert** – dieser Wert ist der Wert, der die Aktion ausgelöst. Müssen Sie ausreichend Unterschied zwischen den Schwellenwert für die Aktion Dezentrales Skalieren und den Schwellenwert für die Skalierung in Aktion bereitstellen. Setzt die Werte gleich erwartet das System Wandel der Umsetzung eines Skalierung verhindert. Beispielsweise funktioniert nicht auf 600 Threads im vorangehenden Beispiel festlegen.
- **Richtung** – dieser Wert bestimmt die Aktion, die durchgeführt wird, wenn der Schwellenwert erreicht ist. Mögliche Werte sind erhöhen oder verringern.
- **Typ** – dieser Wert ist der Typ, erfolgen und muss auf ChangeCount festgelegt.
- **Wert** ist dieser Wert die Anzahl virtueller Computer, die hinzugefügt oder aus den Maßstab entfernt. Dieser Wert muss mindestens 1 sein.
- **Abklingzeit** – ist dieser Wert die Zeitspanne seit der letzten Skalierung Aktion warten muss, bevor die nächste Aktion auftritt. Dieser Wert muss zwischen 1 Minute und einer Woche.

Je nach verwendetem Leistungsindikator sind einige der Elemente in der Konfiguration unterschiedlich verwendet. Im vorherigen Beispiel ist der Thread Count Schwellenwert ist 650 Scale-Out-Aktion und Schwellenwert ist 550 für die Skalierung in Aktion. Verwenden einen Indikator wie Prozessorzeit (%) ist der Schwellenwert, den Prozentsatz der CPU-Auslastung festgelegt werden, Skalierung Aktion bestimmt.

Erstellung eine Last auf den virtuellen Computern, die eine Skalierung Aktion auslöst, wird die Kapazität der Gruppe basierend auf dem Wert in der Vorlage erhöht. Beispielsweise ist in einer Skala, die Kapazität auf 3 und die Aktion Skalierungswert festgelegt ist, auf 1 festgelegt:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Wenn der Auslastungstest erstellt wird, bei dem die durchschnittliche Threadanzahl zu Schwellenwert 650:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

Dezentrales Skalieren-Aktion ausgelöst, die die Kapazität der Gruppe um 1 erhöht wird:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 4
    },

Und ein virtuellen Computer auf die Skalierung hinzugefügt:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Nach einer Abklingzeit von fünf Minuten bleibt die durchschnittliche Anzahl der Threads auf den Computern über 600 wird einem anderen Computer der Gruppe hinzugefügt. Bleibt die durchschnittliche Threadanzahl unter 550, die Kapazität der eingestellten Skalierung um eins reduziert und ein Computer aus der Gruppe entfernt.

## <a name="set-up-scaling-using-azure-powershell"></a>Skalieren mit Azure PowerShell einrichten

Sehen Sie Beispiele für die Verwendung von PowerShell Skalierung einrichten bei [Azure Monitor PowerShell quick start Beispiele](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Skalieren mit Azure CLI einrichten

Sehen Sie Beispiele für die Verwendung von Azure CLI Skalierung einrichten bei [Azure Monitor plattformübergreifende CLI quick start Beispiele](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-the-azure-portal"></a>Skalieren mit der Azure-Portal einrichten

Sehen Sie ein Beispiel zur Verwendung von Azure-Portal Skalierung einrichten [erstellen eine Virtual Machine Skalierung festlegen](virtual-machine-scale-sets-portal-create.md)mithilfe des Azure-Portals.

## <a name="investigate-scaling-actions"></a>Skalierung Aktionen überprüfen

- [Azure-Portal]() – Sie erhalten derzeit eine begrenzte Informationen über das Portal.
- [Azure-Ressourcen-Explorer]() - dieses Tool ist am besten zu den aktuellen Zustand der Skalierung festlegen. Folgen Sie diesem Pfad und Sie sollten die Instanzansicht festgelegten Maßstab erstellen: Abonnements > {Subscription} > ResourceGroups > {der Ressourcengruppe} > Anbieter > Microsoft.Compute > VirtualMachineScaleSets > {Skalierungsgruppe} > VirtualMachines
- Azure PowerShell – mit diesem Befehl Informationen:

        Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
        Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput

- Schließen Sie Jumpbox virtuellen Computer würde andere Rechner und können dann die virtuellen Computer in der einzelnen Prozesse überwachen Remote zugreifen.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich ein Beispiel zum Erstellen eines Maßstab mit der automatischen Skalierung konfiguriert [automatisch skalieren Computer in eine Virtual Machine Skalierung festlegen](virtual-machine-scale-sets-windows-autoscale.md) .
- Beispiele von Azure Monitor Überwachungsfunktionen in [Azure Monitor PowerShell quick start Beispiele](../monitoring-and-diagnostics/insights-powershell-samples.md)
- Erfahren Sie mehr über Benachrichtigungsfunktionen [Autoscale Aktionen zum Senden von e-Mail- und Webhook Benachrichtigungen in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)verwenden.
- Weitere Informationen Sie über das [Überwachungsprotokolle zum Senden von e-Mail- und Webhook Benachrichtigungen in Azure Monitor verwenden](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
- Lernen Sie [Erweiterte Autoscale Szenarios](./virtual-machine-scale-sets-advanced-autoscale.md).
