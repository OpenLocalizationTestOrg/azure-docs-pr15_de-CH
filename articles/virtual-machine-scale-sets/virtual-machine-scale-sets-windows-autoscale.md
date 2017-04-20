<properties
    pageTitle="Skalierungsgröße Windows Virtual Machine Maßstab legt | Microsoft Azure"
    description="Skalierung für ein Windows Virtual Machine Skalieren mithilfe von Azure PowerShell einrichten"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Skalieren Sie Computer in einer virtuellen Maschine Skalierung automatisch

Virtual Machine Maßstab legt erleichtern das Bereitstellen und Verwalten von identischen virtuellen Computern als Gruppe. Maßstab legt bieten eine hochgradig skalierbaren und anpassbare Compute-Ebene für Hyperscale und Plattform-Images von Windows, Linux-Plattform-Images, benutzerdefinierte Bilder und Extensions unterstützen. Weitere Informationen über Maßstab finden Sie in der [Virtual Machine Maßstab legt fest](virtual-machine-scale-sets-overview.md).

In diesem Lernprogramm wird das Skalieren Windows virtuelle Computer erstellen und skaliert automatisch die Computer in der Gruppe veranschaulicht. Skalierung und Skalierung über eine Azure-Ressourcen-Manager-Vorlage erstellen und Azure PowerShell Bereitstellung erstellen. Weitere Informationen zu Vorlagen finden Sie unter [Erstellen von Azure Resource Manager Vorlagen](../resource-group-authoring-templates.md). Über Maßstab legt automatische Skalierung finden Sie unter [Automatische Skalierung und Virtual Machine skalieren](virtual-machine-scale-sets-autoscale-overview.md).

In diesem Artikel werden die folgenden Ressourcen und Extensions bereitstellen:

- Microsoft.Storage/storageAccounts
- Microsoft.Network/virtualNetworks
- Microsoft.Network/publicIPAddresses
- Microsoft.Network/loadBalancers
- Microsoft.Network/networkInterfaces
- Microsoft.Compute/virtualMachines
- Microsoft.Compute/virtualMachineScaleSets
- Microsoft.Insights.VMDiagnosticsSettings
- Microsoft.Insights/autoscaleSettings

Weitere Informationen zu Ressourcen-Manager-Ressourcen finden Sie unter [Azure Compute, Netzwerk und Speicher-Anbieter unter Azure-Manager](../virtual-machines/virtual-machines-windows-compare-deployment-models.md).

## <a name="step-1-install-azure-powershell"></a>Schritt 1: Installieren von Azure PowerShell

Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen und Azure anmelden.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Schritt 2: Erstellen einer Ressourcengruppe und ein Speicherkonto

1. **Erstellen einer Ressourcengruppe** – alle Ressourcen einer Ressourcengruppe bereitgestellt werden müssen. Verwenden Sie [New AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) , eine Ressourcengruppe mit dem Namen **vmsstestrg1**erstellen.

2. **Erstellen ein Speicherkonto** – Speicher-Konto ist, in dem die Vorlage gespeichert wird. Verwenden Sie [Neue AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) erstellen ein Speicherkonto namens **Vmsstestsa**.

## <a name="step-3-create-the-template"></a>Schritt 3: Erstellen der Vorlage
Eine Azure-Ressourcen-Manager-Vorlage ermöglicht bereitstellen und Verwalten von Azure Ressourcen zusammen mit Ressourcen und zugehörigen Bereitstellungsparameter eine JSON.

1. Erstellen Sie in Ihrem bevorzugten Editor die Datei C:\VMSSTemplate.json und die anfängliche JSON Struktur zur Unterstützung der Vorlage.

        {
          "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
          ]
        }

2. Parameter sind nicht immer erforderlich, aber sie können Werte eingeben, wenn die Vorlage bereitgestellt wird. Fügen Sie diese Parameter unter Parameter übergeordnete Element, das der Vorlage hinzugefügt.

        "vmName": { "type": "string" },
        "vmSSName": { "type": "string" },
        "instanceCount": { "type": "string" },
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" },
        "resourcePrefix": { "type": "string" }

    - Legen Sie ein Namen für den virtuellen Computer, der auf den Computern in der Skala verwendet wird.
    - Der Name des Speicherkontos, in dem die Vorlage gespeichert wird.
    - Die Anzahl von virtuellen Maschinen zu Beginn in der festlegen.
    - Name und Kennwort des Administratorkontos auf den virtuellen Computern.
    - Ein Präfix für die Ressourcen, die erstellt werden, um die Skalierung unterstützt.

3. Variablen können in einer Vorlage verwendet werden, an Werte, die sich häufig ändern können oder Werte, die aus einer Kombination von Parameterwerten erstellt werden. Fügen Sie unter Variablen übergeordnete Element, das der Vorlage hinzugefügt.

        "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
        "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
        "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
        "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
        "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
        "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
        "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
        "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
        "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
          "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
        "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

  - DNS-Namen von Netzwerkschnittstellen.
    - IP-Adressnamen und Präfixe für virtuelles Netzwerk und Subnetze.
    - Die Namen und IDs des virtuellen Netzwerks laden Balancer und Netzwerkschnittstellen.
    - Namen für die Konten mit den Computern in der Speicher festgelegt.
    - Standardeinstellungen für die Diagnose-Erweiterung, die auf virtuellen Computern installiert. Weitere Informationen zur Diagnose-Erweiterung finden Sie unter [Erstellen einer Windows Virtual Machine bei der Überwachung und Diagnose mit Azure Ressourcenmanager Vorlage](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).

4. Fügen Sie Konto Speicherressource unter Ressourcen übergeordnete Element, das der Vorlage hinzugefügt hinzu. Diese Vorlage verwendet eine Schleife empfohlenen fünf Storage-Konten erstellen, die Betriebssystem-Datenträger und Diagnosedaten gespeichert. Dieser Satz von Konten kann bis zu 100 VMs in einer Skala unterstützen das aktuelle Maximum ist. Jedes Storage-Konto heißt mit einem Buchstaben, der in kombinierten mit Präfix, das die Parameter für die Vorlage bereitzustellen Variablen definiert wurde.

        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. Virtuelles Netzwerkressource hinzufügen. Weitere Informationen finden Sie unter [Network Ressourcenanbieter](../virtual-network/resource-groups-networking.md).

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[variables('virtualNetworkName')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "subnet1",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },

6. Fügen Sie die öffentlichen IP-Adresse Lastenausgleich und Netzwerkschnittstelle verwendeten Ressourcen hinzu.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP1')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName1')]"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP2')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName2')]"
            }
          }
        },

7. Fügen Sie Load Balancer Ressource, die mit der Skala verwendet wird. Weitere Informationen finden Sie unter [Azure Resource Manager Unterstützung für Lastenausgleich](../load-balancer/load-balancer-arm.md).

        {
          "apiVersion": "2015-06-15",
          "name": "[variables('loadBalancerName')]",
          "type": "Microsoft.Network/loadBalancers",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
          ],
          "properties": {
            "frontendIPConfigurations": [
              {
                "name": "loadBalancerFrontEnd",
                "properties": {
                  "publicIPAddress": {
                    "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
                  }
                }
              }
            ],
            "backendAddressPools": [ { "name": "bepool1" } ],
            "inboundNatPools": [
              {
                "name": "natpool1",
                "properties": {
                  "frontendIPConfiguration": {
                    "id": "[variables('frontEndIPConfigID')]"
                  },
                  "protocol": "tcp",
                  "frontendPortRangeStart": 50000,
                  "frontendPortRangeEnd": 50500,
                  "backendPort": 3389
                }
              }
            ]
          }
        },

8. Hinzuzufügen Sie die Netzwerkschnittstellenressource, mit der die virtuellen Computer. Da Computer in einer Skalierung über eine öffentliche IP-Adresse nicht, ein virtuellen Computer im gleichen virtuellen Netzwerk Remote auf Computer entsteht.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[variables('nicName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
            "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": {
                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
                  },
                  "subnet": {
                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                  }
                }
              }
            ]
          }
        },

9. Hinzufügen der virtuellen Computer im selben Netzwerk wie der Skalierung.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[parameters('vmName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "storageLoop",
            "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_A1" },
            "osProfile": {
              "computername": "[parameters('vmName')]",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "[concat(parameters('resourcePrefix'), 'os1')]",
                "vhd": {
                  "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"        
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
                }
              ]
            }
          }
        },

10. Fügen Sie die virtuellen Ressourcen festzulegen, und geben Sie diagnoseerweiterung, die auf allen virtuellen Computern in der Skalierungsgruppe installiert ist. Viele der für diese Ressource ähneln die VM-Ressource. Die Hauptunterschiede sind die Kapazität, mit denen die Anzahl der virtuellen Maschinen im Skalierungsgruppe und UpgradePolicy, die angibt, wie virtuelle Computer aktualisiert werden. Skalierung festlegen wird nicht erstellt, bis alle Speicherkonten erstellt wurden mit DependsOn-Element angegeben.

            {
              "type": "Microsoft.Compute/virtualMachineScaleSets",
              "apiVersion": "2016-03-30",
              "name": "[parameters('vmSSName')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "storageLoop",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
              ],
              "sku": {
                "name": "Standard_A1",
                "tier": "Standard",
                "capacity": "[parameters('instanceCount')]"
              },
              "properties": {
                "upgradePolicy": {
                  "mode": "Manual"
                },
                "virtualMachineProfile": {
                  "storageProfile": {
                    "osDisk": {
                      "vhdContainers": [
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
                      ],
                      "name": "vmssosdisk",
                      "caching": "ReadOnly",
                      "createOption": "FromImage"
                    },
                    "imageReference": {
                      "publisher": "MicrosoftWindowsServer",
                      "offer": "WindowsServer",
                      "sku": "2012-R2-Datacenter",
                      "version": "latest"
                    }
                  },
                  "osProfile": {
                    "computerNamePrefix": "[parameters('vmSSName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                  },
                  "networkProfile": {
                    "networkInterfaceConfigurations": [
                      {
                        "name": "networkconfig1",
                        "properties": {
                          "primary": "true",
                          "ipConfigurations": [
                            {
                              "name": "ip1",
                              "properties": {
                                "subnet": {
                                  "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                                },
                                "loadBalancerBackendAddressPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                                  }
                                ],
                                "loadBalancerInboundNatPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                                  }
                                ]
                              }
                            }
                          ]
                        }
                      }
                    ]
                  },
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
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                          }
                        }
                      }
                    ]
                  }
                }
              }
            },

11. Fügen Sie die AutoscaleSettings-Ressource hinzu, die definiert, wie die Skalierungsgruppe basierend auf die Prozessorverwendung auf den Computern in der Skalierungsgruppe passt.

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
                          "metricName": "\\Processor(_Total)\\% Processor Time",
                          "metricNamespace": "",
                          "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                          "timeGrain": "PT1M",
                          "statistic": "Average",
                          "timeWindow": "PT5M",
                          "timeAggregation": "Average",
                          "operator": "GreaterThan",
                          "threshold": 50.0
                        },
                        "scaleAction": {
                          "direction": "Increase",
                          "type": "ChangeCount",
                          "value": "1",
                          "cooldown": "PT5M"
                        }
                      }
                    ]
                  }
                ],
                "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              }
            }

    Diese Werte sind für dieses Lernprogramm wichtig:

    - **MetricName** - entspricht dieser Wert der Leistungsindikator in der Wadperfcounter-Variablen definiert. Mit Variablen, die diagnoseerweiterung sammelt die **Prozessor(_Total)\% Prozessorzeit** Zähler.
    - **MetricResourceUri** - Wert ist der Ressourcenbezeichner des virtuellen Computers Skalierung festlegen.
    - **TimeGrain** – ist dieser Wert die Granularität der Metriken gesammelt werden. In dieser Vorlage ist es auf 1 Minute festgelegt.
    - **Statistik** – dieser Wert bestimmt, wie die Metriken kombiniert werden, um die automatische Skalierung Aktion aufzunehmen. Mögliche Werte sind: Mittelwert, Min, Max. In dieser Vorlage werden durchschnittliche gesamte CPU-Nutzung virtueller Maschinen.
    - **Zeitfenster** – dieser Wert ist die Zeitspanne, in der Instanz Daten. Es muss zwischen 5 und 12 Stunden.
    - **TimeAggregation** – dieser Wert bestimmt, wie die gesammelten Daten mit der Zeit kombiniert werden. Der Standardwert ist Durchschnitt. Mögliche Werte sind: Durchschnitt mindestens höchstens, letzte, Summe, Count.
    - **Operator** -ist dieser Wert der Operator, mit dem die Daten und den Schwellenwert verglichen. Mögliche Werte sind: NotEquals, GreaterThan GreaterThanOrEqual, LessThan, LessThanOrEqual entspricht.
    - **Schwellenwert** – dieser Wert ist der Wert, der die Aktion ausgelöst. In dieser Vorlage werden Maschinen der Maßstab wird die durchschnittliche CPU-Auslastung zwischen Computern in der Gruppe über 50 % hinzugefügt.
    - **Richtung** – dieser Wert bestimmt die Aktion, die durchgeführt wird, wenn der Schwellenwert erreicht ist. Mögliche Werte sind erhöhen oder verringern. In dieser Vorlage ist die Anzahl von virtuellen Maschinen in den Maßstab erhöht der Schwellenwert über 50 % im definierten Zeitfenster.
    - **Typ** – dieser Wert ist der Typ, erfolgen und muss auf ChangeCount festgelegt.
    - **Wert** ist dieser Wert die Anzahl virtueller Computer, die hinzugefügt oder entfernt aus der Skalierung. Dieser Wert muss mindestens 1 sein. Der Standardwert ist 1. In dieser Vorlage Anzahl der Computer in der erhöht um 1 Wenn der Schwellenwert erreicht wird.
    - **Abklingzeit** – ist dieser Wert die Zeitspanne seit der letzten Skalierung Aktion warten muss, bevor die nächste Aktion auftritt. Dieser Wert muss zwischen 1 Minute und einer Woche.

12. Speichern der Vorlagendatei.    

## <a name="step-4-upload-the-template-to-storage"></a>Schritt 4: Die Vorlage in den Speicher hochladen

Die Vorlage kann hochgeladen werden, solange Sie kennen den Namen und den primären Schlüssel für das Speicherkonto, das Sie in Schritt 1 erstellt haben.

1.  Legen Sie im Fenster Microsoft Azure PowerShell eine Variable mit dem Namen des Speicherkontos, die Sie in Schritt 1 erstellt haben.

            $storageAccountName = "vmstestsa"

2.  Eine Variable, die den primären Schlüssel für das Speicherkonto festgelegt.

            $storageAccountKey = "<primary-account-key>"

    Dieser Schlüssel erhalten durch Klicken auf das Schlüsselsymbol beim Konto Speicherressourcen im Azure-Portal anzeigen.

3.  Erstellen des Storage-Konto Kontextobjekts, mit Speicher-Konto zu überprüfen.

            $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

4.  Erstellen Sie den Container die Vorlage speichern.

            $containerName = "templates"
            New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob

5.  Hochladen der Vorlage in den neuen Container.

            $blobName = "VMSSTemplate.json"
            $fileName = "C:\" + $BlobName
            Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx

## <a name="step-5-deploy-the-template"></a>Schritt 5: Bereitstellen der Vorlage

Erstellen die Vorlage können Sie starten, Ressourcen bereitstellen. Befehl zum Starten des Prozesses:

    New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"

Drücken Sie eingeben, werden Werte für die Variablen bereitgestellt, die Ihnen zugewiesen. Enthalten Sie diese Werte:

    vmName: vmsstestvm1
      vmSSName: vmsstest1
      instanceCount: 5
      adminUserName: vmadmin1
      adminPassword: VMpass1
      resourcePrefix: vmsstest

Es dauert ca. 15 Minuten für alle Ressourcen erfolgreich bereitgestellt werden.

>[AZURE.NOTE] Das Portal Möglichkeit können Sie die Ressourcen. Verwenden Sie diesen Link: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"

## <a name="step-6-monitor-resources"></a>Schritt 6: Überwachen von Ressourcen

Einige Informationen zu virtuellen Maßstab legt mit diesen Methoden erhalten:

 - Azure-Portal - erhalten Sie derzeit eine begrenzte Informationen über das Portal.
 - Der [Azure-Ressourcen-Explorer](https://resources.azure.com/) - dieses Tool ist am besten zu den aktuellen Zustand der Skalierung festlegen. Folgen Sie diesem Pfad, und sehen Sie die Instanzansicht Skalierung festlegen, die Sie erstellt haben:

        subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

 - Azure PowerShell – mit diesem Befehl Informationen:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

        Or

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView

 - Verbinden Sie mit der virtuellen Computer würde andere Rechner und können dann die virtuellen Computer in der einzelnen Prozesse überwachen Remote zugreifen.

>[AZURE.NOTE] Eine vollständige REST-API zum Abrufen von Informationen über Maßstab finden Sie im [Virtual Machine Maßstab legt fest](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-the-resources"></a>Schritt 7: Entfernen der Ressourcen

Da in Azure verwendeten Ressourcen berechnet ist es immer empfohlen, nicht mehr benötigte, Ressourcen löschen. Sie müssen nicht jede Ressource aus einer Ressourcengruppe einzeln löschen. Die Ressourcengruppe löschen und alle Ressourcen automatisch gelöscht.

    Remove-AzureRmResourceGroup -Name vmsstestrg1

Ggf. die Ressource Gruppe können Sie den Maßstab nur löschen.

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"

## <a name="next-steps"></a>Nächste Schritte

- Verwalten der Skalierung, gerade erstellt anhand der Informationen in der [Verwaltung virtueller Computer in einem virtuellen Computer skalieren](virtual-machine-scale-sets-windows-manage.md).
- Erfahren Sie mehr über vertikale Skalierung [vertikale Skalieren mit virtuellen Skalierung](virtual-machine-scale-sets-vertical-scale-reprovision.md) überprüfen
- Beispiele von Azure Monitor Überwachungsfunktionen in [Azure Monitor PowerShell quick start Beispiele](../monitoring-and-diagnostics/insights-powershell-samples.md)
- Erfahren Sie mehr über Benachrichtigungsfunktionen [Autoscale Aktionen zum Senden von e-Mail- und Webhook Benachrichtigungen in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md) verwenden
- Erfahren Sie, wie Sie [Überwachungsprotokolle zum Senden von e-Mail- und Webhook Benachrichtigungen in Azure Monitor verwenden](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
