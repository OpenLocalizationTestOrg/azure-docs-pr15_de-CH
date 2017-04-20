<properties
   pageTitle="Sammeln von Protokollen mithilfe von Azure Diagnostics | Microsoft Azure"
   description="Dieser Artikel beschreibt das Einrichten Azure-Diagnose zum Sammeln von Protokollen aus einem Service Fabric-Cluster in Azure ausgeführt."
   services="service-fabric"
   documentationCenter=".net"
   authors="ms-toddabel"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="toddabel"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Sammeln von Protokollen mithilfe von Azure-Diagnose

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Einen Azure Service Fabric-Cluster ausführen, wird eine gute Idee, die Protokolle von allen Knoten an einem zentralen Ort sammeln. Die Protokolle hilft auf einem zentralen Sie analysieren und beheben Sie Probleme im Cluster oder in Programme und Dienste in diesem Cluster ausgeführt.

Eine Möglichkeit zum Hochladen und Sammeln von Protokollen ist mit Erweiterung Azure-Diagnose, die Protokolle Azure-Speicher hochgeladen. Die Protokolle sind nicht direkt im Speicher nützlich. Jedoch können Sie die Ereignisse aus dem Speicher gelesen und in einem Produkt wie [Protokollanalyse](../log-analytics/log-analytics-service-fabric.md), [Elastische](service-fabric-diagnostic-how-to-use-elasticsearch.md)oder einen anderen Protokoll-Analyse-Lösung einen externen Prozess.

## <a name="prerequisites"></a>Erforderliche Komponenten
Diese Tools verwenden zum Ausführen einiger Vorgänge in diesem Dokument:

* [Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics.md) (Bezug auf Azure Cloud Services hat aber Informationen und Beispiele)
* [Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](../powershell-install-configure.md)
* [Azure-Ressourcen-Manager-client](https://github.com/projectkudu/ARMClient)
* [Azure-Ressourcen-Manager-Vorlage](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)


## <a name="log-sources-that-you-might-want-to-collect"></a>Protokollquellen, die Sie sammeln möchten
- **Service Fabric Protokolle**: von der Plattform und standard Event Tracing for Windows (ETW) und Quelle ausgegeben. Verschiedene möglich Protokolle:
  - Operationelle Ereignisse: Protokolle für Service Fabric-Plattform führt Operationen. Erstellung der Dienste, Knoten ändert, und Aktualisierungsinformationen gehören.
  - [Zuverlässige Akteure programming Model-Ereignisse](service-fabric-reliable-actors-diagnostics.md)
  - [Zuverlässige Dienste programming Model-Ereignisse](service-fabric-reliable-services-diagnostics.md)
- **Anwendungsereignisse**: Ereignisse von Ihrem Service-Code ausgegeben und mit der Ereignisquelle Hilfsklasse gemäß der Visual Studio-Vorlagen geschrieben. Weitere Informationen zum Schreiben von Protokollen aus Ihrer Anwendung finden Sie [Überwachen und Services in einer lokalen Entwicklung Installation](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).


## <a name="deploy-the-diagnostics-extension"></a>Bereitstellen der Diagnose-Erweiterung
Der erste Schritt beim Sammeln von Protokollen ist Diagnose-Erweiterung auf die VMs im Service Fabric-Cluster bereitstellen. Diagnose-Erweiterung sammelt Protokolle auf jede VM und lädt sie das Speicherkonto, das Sie angeben. Die Schritte variieren etwas, ob Azure-Portal oder Azure Ressourcen-Manager verwenden. Die Schritte hängen auch, ob die Bereitstellung Teil Clustererstellung oder für einen Cluster, der bereits vorhanden ist. Sehen wir uns die Schritte für jedes Szenario.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>Bereitstellen Sie die Diagnose-Erweiterung im Rahmen der Erstellung des Clusters über das portal
Zum Bereitstellen der Diagnose-Erweiterung mit den virtuellen Computern im Cluster als Teil der Clustererstellung verwenden Sie Einstellungspanel Diagnose in der folgenden Abbildung dargestellt. Um zuverlässige Akteure oder zuverlässige Dienste Ereignis aktivieren, daß Diagnose **auf** (Standardeinstellung) festgelegt ist. Nach dem Erstellen des Clusters können nicht Sie dazu über das Portal ändern.

![Azure Diagnostics Einstellungen im Portal Clustererstellung](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Die Azure-Team *benötigt* Unterstützung protokolliert um alle Support-Anfragen zu beheben, die Sie erstellen. Diese Protokolle werden in Echtzeit erfasst und in einem Speicherkonten in der Ressourcengruppe erstellt werden. Die Diagnose konfigurieren Ereignisse auf Anwendungsebene. Hierzu zählen [Zuverlässige Akteure](service-fabric-reliable-actors-diagnostics.md) Ereignisse, [Zuverlässige Dienste](service-fabric-reliable-services-diagnostics.md) Ereignisse und einige Ereignisse auf Systemebene Fabric Service in Azure-Speicher gespeichert werden.

Produkte wie [Elastische suchen](service-fabric-diagnostic-how-to-use-elasticsearch.md) oder Ihren eigenen Prozess erhalten die Ereignisse das Speicherkonto. Derzeit besteht keine Möglichkeit, Filtern und Optimieren Sie die Ereignisse, die an die Tabelle gesendet. Wenn Sie einen Prozess, um Ereignisse aus der Tabelle entfernen nicht implementieren, weiterhin die Tabelle vergrößert.

Wenn Sie einen Cluster mit dem Portal erstellen, empfehlen wir Downloaden der Vorlage *Klicken Sie * *OK*** Cluster erstellen. Einzelheiten Sie zum [Einrichten von Service Fabric-Cluster mithilfe einer Vorlage Azure-Ressourcen-Manager](service-fabric-cluster-creation-via-arm.md). Sie benötigen die Vorlage später ändern, da Sie einige ändern können mithilfe der Portalwebsite.

Mithilfe der folgenden Schritte können Sie Vorlagen aus dem Portal exportieren. Diese Vorlagen können jedoch schwieriger zu verwenden, da sie null-Werte enthalten können, die erforderlichen Informationen fehlen.

1. Öffnen der Ressourcengruppe.
2. Wählen Sie **Einstellungen** den Einstellungen angezeigt.
3. Wählen Sie **Bereitstellung** Bereitstellung Protokollfenster angezeigt.
4. Wählen Sie eine Bereitstellung die Details der Bereitstellung angezeigt.
5. Wählen Sie **Vorlage exportieren** der Vorlage angezeigt.
6. Wählen Sie eine ZIP-Datei exportieren, die Vorlage Parameter und PowerShell Dateien enthält, **in Datei speichern** .

Nach dem export der Dateien müssen Sie eine Änderung vornehmen. Bearbeiten Sie die Datei parameters.json und entfernen Sie **AdminPassword** Element. Dadurch wird eine Aufforderung zur Kennworteingabe beim Ausführen des Bereitstellungsskripts. Wenn Sie das Bereitstellungsskript ausführen, müssen Sie möglicherweise null-Parameterwerte zu beheben.

Die gedownloadete Vorlage verwenden eine Konfiguration aktualisieren:

1. Extrahieren Sie den Inhalt in einen Ordner auf Ihrem lokalen Computer.
2. Ändern Sie den Inhalt der neue Konfiguration entsprechen.
3. Starten der PowerShell und den Ordner, in dem den Inhalt extrahiert.
4. Führen Sie **deploy.ps1** und füllen Sie die Abonnement-ID der Gruppe Ressourcenname (verwenden Sie die gleichnamige Aktualisierung die Konfiguration) und ein eindeutiger Bereitstellung.


### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Bereitstellen Sie Diagnose-Erweiterung im Rahmen der Erstellung des Clusters mithilfe von Azure-Ressourcen-Manager
Erstellen ein Clusters mit dem Ressourcen-Manager müssen Sie die vollständige Cluster Resource Manager Vorlage vor dem Erstellen des Clusters die JSON-Diagnose: Konfiguration hinzufügen. Wir bieten eine Beispielvorlage fünf VM Cluster Resource Manager Diagnose: Konfiguration als Teil unserer Ressourcenmanager Vorlagenbeispiele hinzugefügt. In Azure Samples Gallery an dieser Stelle sehen: [fünf Cluster mit Diagnose Ressourcenmanager Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Diagnose-Einstellung in der Ressourcen-Manager-Vorlage finden öffnen Sie die Datei azuredeploy.json und nach **IaaSDiagnostics**. Erstellen ein Clusters mit dieser Vorlage wählen Sie **Deploy to Azure** über den vorherigen Link.

Auch Sie Ressourcenmanager Beispiel herunterladen, ändern und erstellen Sie einen Cluster mit der geänderten Vorlage mit den `New-AzureRmResourceGroupDeployment` Befehl in einem Azure PowerShell-Fenster. Finden Sie den folgenden Code für die Parameter, die an den Befehl übergeben. Detaillierte Informationen zum Bereitstellen einer Ressourcengruppe mit PowerShell finden Sie unter [Bereitstellen einer Ressourcengruppe mit der Azure-Ressourcen-Manager](../resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>Die Diagnose-Erweiterung zu einem vorhandenen Cluster bereitstellen
Wenn Sie einen vorhandenen Cluster eingesetzte Diagnosen verfügen nicht oder eine vorhandene Konfiguration ändern möchten, können Sie hinzufügen oder aktualisieren. Ändern der Ressourcen-Manager-Vorlage, mit bestehenden Cluster erstellen oder downloaden Sie die Vorlage aus dem Portal, wie oben beschrieben. Ändern Sie die Datei template.json die folgenden Aufgaben durch.

Hinzufügen einer neuen Speicherressourcen Vorlage Ressourcenabschnitt hinzu.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Fügen Sie im Abschnitt Parameter nach Kontoangaben Speicher zwischen `supportLogStorageAccountName` und `vmNodeType0Name`. Ersetzen Sie die Platzhalter Text *hier speicherkontoname* mit dem Namen des Speicherkontos.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Aktualisieren Sie dann die `VirtualMachineProfile` Abschnitt der Datei template.json durch Hinzufügen des folgenden Codes innerhalb des Arrays Extensions. Unbedingt ein Semikolon am Anfang oder Ende, je nachdem, wo eingefügt.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Nach dem Ändern der Datei template.json wie Veröffentlichen der Ressourcen-Manager-Vorlage. Die Vorlage exportiert wurde, dann die deploy.ps1 Datei Vorlage. Nach der Bereitstellung sicher, dass **ProvisioningState** **war erfolgreich**.


## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Aktualisieren Sie Diagnose zum Sammeln und Protokolle aus neuen Quelle laden
Aktualisieren Sie Diagnostics aus, um neue Ereignisquelle Kanäle, die eine neue Anwendung, die Sie bereitstellen darstellen, führen Sie dieselben Schritte wie im [vorherigen Abschnitt](#deploywadarm) für die Einrichtung der Diagnose für einen vorhandenen Cluster Protokolle gesammelt.

Aktualisieren der `EtwEventSourceProviderConfiguration` Abschnitt in die template.json-Datei Einträge für die neue Quelle Kanäle vor dem Anwenden der Konfigurations aktualisieren die `New-AzureRmResourceGroupDeployment` PowerShell-Befehl. Der Name der Ereignisquelle ist als Teil des Codes in der Datei ServiceEventSource.cs generiert Visual Studio definiert.

Wenn Ihre eigene Quelle Ereignisquelle ist, fügen Sie folgenden Code um eigene Quelle in eine Tabelle namens MyDestinationTableName Ereignisse ein.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Um Leistungsindikatoren oder Ereignisprotokolle erfassen, Bearbeiten der Ressourcen-Manager-Vorlage Beispiele in [einem Windows-Computer durch Überwachung und Diagnose mithilfe einer Vorlage Azure-Ressourcen-Manager erstellen](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md). Veröffentlichen Sie die Vorlage Ressourcenmanager.

## <a name="next-steps"></a>Nächste Schritte
Detaillierter Ereignisse Sie achten bei der Fehlerbehebung finden Sie unter Diagnoseereignissen für [Zuverlässige Akteure](service-fabric-reliable-actors-diagnostics.md) und [Zuverlässige Dienste](service-fabric-reliable-services-diagnostics.md)ausgegeben.


## <a name="related-articles"></a>Verwandte Artikel
* [Erfahren Sie, wie Leistungsindikatoren oder Protokolle Sammeln der Diagnose Erweiterung](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)
* [Service Fabric Lösung Protokollanalyse](../log-analytics/log-analytics-service-fabric.md)
