<properties
    pageTitle="Überwachung und Diagnose mit Azure Ressourcenmanager Vorlage Windows Virtual Machine erstellen | Microsoft Azure"
    description="Verwenden einer Vorlage Azure Ressource-Manager einen neuen virtueller Computer für Windows Azure-Diagnose Erweiterung erstellen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sbtron"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>

# <a name="create-a-windows-virtual-machine-with-monitoring-and-diagnostics-using-azure-resource-manager-template"></a>Überwachung und Diagnose mit Azure Ressourcenmanager Vorlage erstellen Sie Windows Virtual machine

Der Azure-Diagnose-Erweiterung ermöglicht die Überwachung und Diagnosefunktionen auf Windows Azure Virtual Machine basiert. Sie können diese Funktionen auf dem virtuellen Computer einschließlich der Erweiterung als Teil der Vorlage Azure Ressource-Manager aktivieren. Weitere Informationen einschließlich Erweiterung als Teil einer Vorlage für virtuelle Computer finden Sie unter [Erstellen Azure Ressourcenmanager Vorlagen mit VM](virtual-machines-windows-extensions-authoring-templates.md) . Dieser Artikel beschreibt, wie Sie eine Vorlage für virtuelle Computer Windows Azure-Diagnose Erweiterung hinzufügen können.  
  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>Hinzufügen der Erweiterung Azure-Diagnose für die Definition der VM-Ressource 

Aktivieren die diagnoseerweiterung auf einem Windows-Computer müssen Sie die Erweiterung als eine VM-Ressource in der Ressourcen-Manager-Vorlage hinzufügen.

Für eine einfache Ressourcenmanager Basis Virtual Machine hinzufügen Erweiterung Konfiguration Array *Ressourcen* für den virtuellen Computer: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Eine andere allgemeine Konvention hinzufügen die Erweiterung Konfiguration Ressourcen Stammknoten der Vorlage statt unter dem Knoten des virtuellen Computers Ressourcen definieren. Bei diesem Ansatz müssen Sie explizit eine hierarchische Beziehung zwischen der Erweiterung und den virtuellen Computer mit den *Namen* und *Typ* angeben. Zum Beispiel: 
  
    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

Die Erweiterung ist immer mit dem virtuellen Computer, entweder direkt direkt unter der virtuellen Maschine Ressourcenknoten definieren oder auf Basisebene definieren und verwenden die hierarchische Benennungskonvention der virtuellen Maschine zugeordnet.

Für Virtual Machine Skalierung wird die Erweiterungskonfiguration in der *ExtensionProfile* -Eigenschaft des *VirtualMachineProfile*angegeben.
   
Die *Publisher* -Eigenschaft mit dem Wert des **Microsoft.Azure.Diagnostics** und die *Type* -Eigenschaft mit dem Wert **IaaSDiagnostics** eindeutig Azure-Diagnose-Erweiterung.

Der Wert der *Name* -Eigenschaft kann auf die Erweiterung in der Ressourcengruppe verwendet werden. Speziell auf **Microsoft.Insights.VMDiagnosticsSettings** aktivieren sie leicht identifiziert werden, indem die Azure klassischen Portal Portal sicherstellen, die dass der Überwachung Diagramme korrekt im klassischen Azure-Portal angezeigt.

*TypeHandlerVersion* gibt die Version der Erweiterung verwenden möchten. Festlegen von *AutoUpgradeMinorVersion* sorgt Nebenversion auf **true** man die letzte Nebenversion der Erweiterung, die verfügbar ist. Es empfiehlt sich, immer *AutoUpgradeMinorVersion* immer **true** ist, so dass Sie immer die neue Features und Fehlerbehebungen verfügbaren diagnoseerweiterung mit festzulegen. 

*Settings* -Element enthält Konfigurationseigenschaften für die Erweiterung, die festgelegt und von der Erweiterung (auch als öffentliche Konfiguration bezeichnet) gelesen werden kann. *Xmlcfg* -Eigenschaft enthält XML-basierte Konfiguration für die Diagnoseprotokolle Leistungsindikatoren usw. von Diagnose-Agent erfasst werden. Weitere Informationen zum XML-Schema finden Sie unter [Diagnose Konfigurationsschema](https://msdn.microsoft.com/library/azure/dn782207.aspx) . Häufig die eigentlichen XML-Konfiguration als Variable in der Azure-Ressourcen-Manager-Vorlage speichern und verketten und base64 codiert den Wert *Xmlcfg*gesetzt. Abschnitt [Diagnose Konfigurationsvariablen](#diagnostics-configuration-variables) mehr über Xml in Variablen speichern. Die *StorageAccount* -Eigenschaft gibt den Namen des Speicherkontos, Diagnosedaten übertragen werden. 
 
Eigenschaften in *ProtectedSettings* (auch als private Konfiguration bezeichnet) kann jedoch nicht nach festzulegenden gelesen werden. Nur-Schreiben-Art des *ProtectedSettings* erleichtert Geheimnisse wie Speicherschlüssel Konto gespeichert, in dem die Diagnosedaten geschrieben werden.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Diagnose Speicherkonto als Parameter angeben 

Die Diagnose Erweiterung Json obigen Codeausschnitt zwei Parameter *ExistingdiagnosticsStorageAccountName* und *ExistingdiagnosticsStorageResourceGroup* an das Speicherkonto für die Diagnose, Diagnosedaten Speicherort. Das Speicherkonto für die Diagnose angeben, Parameter wird problemlos Speicherkonto Diagnose in unterschiedlichen Umgebungen geändert z.B. möchten ein Speicherkonto verschiedene Diagnosedaten für Tests und eine andere für die Bereitstellung in der Produktion verwenden.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "The name of an existing storage account to which diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
            }
        }

Es ist empfehlenswert, in einer anderen Ressourcengruppe als Ressourcengruppe für den virtuellen Computer ein Speicherkonto für die Diagnose angeben. Eine Ressourcengruppe kann als eine Bereitstellungseinheit eigene Lebensdauer sein, ein virtuellen Computer können bereitgestellt und bereitgestellt als neue Konfigurationen Updates, versucht sind jedoch weiterhin die Diagnosedaten in demselben Speicherkonto über die Bereitstellung von virtuellen Computer speichern möchten. Das Speicherkonto ermöglicht in einer anderen Ressource das Speicherkonto Daten aus verschiedenen virtuellen Bereitstellung vereinfachen Problembehandlung in verschiedenen Versionen.

>[AZURE.NOTE] Wenn Sie eine Vorlage für virtuelle Computer Windows von Visual Studio erstellen möglicherweise Speicher Standardkonto gleichen Speicherkonto verwenden, den virtuellen Computer VHD hochgeladen festgelegt werden. Dies ist Erstinstallation der VM vereinfachen. Sie sollten gestalten Sie die Vorlage, um andere Speicherkonto verwenden, die als Parameter übergeben werden kann. 

## <a name="diagnostics-configuration-variables"></a>Diagnose-Konfigurationsvariablen
 
Diagnose-Erweiterung Json Codeausschnitt oben definiert eine Variable *Accountid* zur Vereinfachung der Storage-Konto Schlüssel für Diagnose-Speicher:   
    
    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


*Xmlcfg* -Eigenschaft für die diagnoseerweiterung ist definiert mit mehreren Variablen, die miteinander verkettet sind. Die Werte dieser Variablen werden in Xml müssen sie ordnungsgemäß mit Escapezeichen versehen werden beim Json-Variablen festlegen.

Folgt eine Beschreibung die Diagnose Konfigurations-Xml, die standard-Ebene Leistungsindikatoren Windows-Ereignisprotokolle und Diagnoseprotokolle Infrastruktur erfasst. Mit Escapezeichen versehen wurde und korrekt formatiert werden, damit die Konfiguration direkt in den Variablen Teil der Vorlage eingefügt werden kann. [Konfigurationsschema Diagnose](https://msdn.microsoft.com/library/azure/dn782207.aspx) für mehr Menschen lesbar wird der Konfigurations-Xml anzeigen
    
        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

Metriken Definition XML-Knoten in der Konfiguration oben ist ein wichtiger Konfigurationselement definiert werden, wie in Xml in *PerformanceCounter* -Knoten definierten Leistungsindikatoren gesammelt und gespeichert werden. 

> [AZURE.IMPORTANT] Diese Metriken Laufwerk Überwachung Diagramme und Alarme in Azure-Portal.  **Metriken** Knoten mit *ResourceID* **MetricAggregation** beizufügen Diagnose Konfiguration für VM VM Überwachungsdaten in Azure-Portal angezeigt werden soll. 

Folgendes ist ein Beispiel der XML-Metriken Definitionen: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

Das *ResourceID* -Attribut identifiziert eindeutig den virtuellen Computer in Ihrem Abonnement. Vergewissern Sie sich, die Funktionen Subscription()"und resourceGroup() verwenden, damit die Vorlage aktualisiert diese Werte basierend auf dem Abonnement und die Ressourcengruppe, die Sie bereitstellen.

Wenn Sie mehrere virtuelle Computer in einer Schleife erstellen müssen Sie Auffüllen *ResourceID* -Wertes mit einer copyIndex() Funktion jedes einzelnen VM richtig unterscheiden. Der Wert *XmlCfg* kann dazu folgendermaßen aktualisiert werden:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

Der Wert MetricAggregation *PT1H* und *PT1M* bedeuten eine Aggregation über eine Minute und eine Aggregation über eine Stunde.

## <a name="wadmetrics-tables-in-storage"></a>WADMetrics Tabellen im Speicher

Konfiguration der Metriken generiert Tabellen in das Speicherkonto Diagnose den folgenden Benennungskonventionen:

- **WADMetrics** : Standardpräfix für alle WADMetrics-Tabellen
- **PT1H** oder **PT1M** : Gibt an, dass die Tabelle Daten über 1 Stunde und 1 Minute enthält
- **P10D** : Gibt die Tabelle enthalten Daten für 10 Tage nach Beginn die Tabelle Datenerfassung
- **V2S** : Zeichenfolge
- **Yyyymmdd** : Datum die Tabelle Starts Datenerfassung

Beispiel: *WADMetricsPT1HP10DV2S20151108* wird Metriken Stunde für 10 Tage ab 11 Nov 2015 aggregiert Daten    

Jeder WADMetrics-Tabelle enthält folgenden Spalten:

- **PartitionKey**: die Partitionkey basierend auf den *ResourceID* Wert zur eindeutigen Identifizierung die VM-Ressource erstellt. für z. B.: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
- **RowKey** : entspricht dem Format <Descending time tick>:<Performance Counter Name>. Die absteigende Tick Berechnung ist max Zeit Ticks Zeit am Anfang der Periode Aggregation. Z.b. Die Abtastperiode gestartet am 10. November 2015 und 00:00Hrs UTC und der Berechnung: DateTime.MaxValue.Ticks - (neue DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Ticks). Speicher Verfügbare Bytes-Leistungsindikator den Zeilenschlüssel aussieht wie: 2519551871999999999__:005CMemory:005CAvailable:0020 Bytes
- **CounterName** : der Name des Leistungsindikators. Dies entspricht dem *CounterSpecifier* in der XML-Konfigurationsdatei definiert.
- **Maximale** : der maximale Wert des Leistungsindikators Zeitraum Aggregation.
- **Minimum** : der minimale Wert des Leistungsindikators Zeitraum Aggregation.
- **Summe** : die Summe aller Werte des Leistungsindikators Zeitraum Aggregation gemeldet.
- **Anzahl** : die Gesamtzahl der Werte für den Leistungsindikator gemeldet.
- **Durchschnitt** : (Gesamtanzahl) Durchschnittswert des Leistungsindikators Zeitraum Aggregation.


## <a name="next-steps"></a>Nächste Schritte

- Einen vollständigen Vorlage von einem Windows-Computer mit diagnoseerweiterung finden Sie unter [201-Vm-monitoring-Diagnose-Erweiterung](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
- Bereitstellen Sie die Ressource-Manager-Vorlage mit [Azure PowerShell](virtual-machines-windows-ps-manage.md) oder [Befehlszeile Azure](virtual-machines-linux-cli-deploy-templates.md)
- Erfahren Sie mehr über [authoring Azure-Ressourcen-Manager-Vorlagen](../resource-group-authoring-templates.md)







