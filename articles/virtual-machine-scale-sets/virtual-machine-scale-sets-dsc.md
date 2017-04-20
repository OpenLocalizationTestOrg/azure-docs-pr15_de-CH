<properties
   pageTitle="Mit Status Konfiguration mit virtuellen Skalierung gewünscht | Microsoft Azure"
   description="Virtual Machine Maßstab legt Azure DSC-Erweiterung"
   services="virtual-machine-scale-sets"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machine-scale-sets"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Virtual Machine Maßstab legt Azure DSC-Erweiterung

[Virtual Machine Maßstab legt (VMSS)](virtual-machine-scale-sets-overview.md) kann mit der Erweiterung [Azure gewünschten Zustand Konfiguration (DSC)](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md) verwendet werden. VMSS bietet eine Möglichkeit zum Bereitstellen und Verwalten einer großen Anzahl von virtuellen Maschinen und skalierbar und elastisch auf laden. DSC dient zum Konfigurieren der VMs Sie online sind, so dass sie die Produktionssoftware ausgeführt werden.

## <a name="differences-between-deploying-to-vm-and-vmss"></a>Unterschiede zwischen dem Bereitstellen virtueller Computer und VMSS

Die zugrunde liegende Vorlagenstruktur VMSS ist geringfügig von einer einzigen VM. Insbesondere setzt eine VM Extensions unter dem Knoten "VirtualMachines". Gibt es ein Eintrag des Typs "Extensions", in dem die Vorlage DSC hinzugefügt wird

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

VMSS-Knoten enthält einen Abschnitt "Eigenschaften" mit der "VirtualMachineProfile", "ExtensionProfile"-Attribut. DSC ist unter "Extensions" hinzugefügt.

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-vmss"></a>Verhalten VMSS

Das Verhalten VMSS ist identisch mit dem Verhalten einer einzelnen VM. Beim Erstellen eine neue VM wird es automatisch mit der DSC-Erweiterung bereitgestellt. Wenn eine neuere Version des WMF Erweiterung erforderlich ist, startet die VM bevor er online geschaltet. Einmal online, DSC Konfiguration ZIP heruntergeladen und auf dem virtuellen Computer bereitstellen. Weitere Informationen finden in [Azure DSC Erweiterung Overview](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md).

## <a name="next-steps"></a>Nächste Schritte ##
Überprüfen der [Azure-Ressourcen-Manager-Vorlage für DSC-Erweiterung](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md).

Erfahren Sie, wie der [DSC-Erweiterung Anmeldeinformationen sicher behandelt](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md). 

Informationen auf Azure DSC Erweiterung Ereignishandler finden Sie unter [Einführung in Ereignishandler Erweiterung Azure gewünschten Konfiguration](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md). 

Weitere Informationen über PowerShell DSC [finden Sie auf der Mitte der PowerShell-Dokumentation](https://msdn.microsoft.com/powershell/dsc/overview). 


