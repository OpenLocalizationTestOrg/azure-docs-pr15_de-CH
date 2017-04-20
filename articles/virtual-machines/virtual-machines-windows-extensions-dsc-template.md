<properties
   pageTitle="Gewünschten Status Resource Manager Konfigurationsvorlage | Microsoft Azure"
   description="Ressourcenmanager Vorlagendefinition für die gewünschte Konfiguration in Azure, Beispiele und Problembehandlung"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>Windows VMSS und gewünschte Konfiguration von Azure-Ressourcen-Manager-Vorlagen
Dieser Artikel beschreibt Ressourcenmanager Vorlage für den [gewünschten Zustand Erweiterung Konfigurationshandler](virtual-machines-windows-extensions-dsc-overview.md). 

## <a name="template-example-for-a-windows-vm"></a>Vorlage, z.B. Windows-VM

Im folgenden Codeausschnitt wird die Ressource-Abschnitt der Vorlage.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }

```

## <a name="template-example-for-windows-vmss"></a>Beispiel für Windows VMSS Vorlage

VMSS-Knoten enthält einen Abschnitt "Eigenschaften" mit der "VirtualMachineProfile", "ExtensionProfile"-Attribut. DSC wird unter "Extensions" hinzugefügt. 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
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

## <a name="detailed-settings-information"></a>Detaillierte Einstellungen

Das folgende Schema ist für den Einstellungen Teil Azure DSC-Erweiterung in einer Vorlage Azure-Ressourcen-Manager.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Details
| Eigenschaftenname | Typ | Beschreibung |
| --- | --- | --- |
| settings.wmfVersion | Zeichenfolge | Gibt die Version von Windows Management Framework, die auf der VM installiert werden soll. Wenn diese Eigenschaft auf 'letzte' installiert die aktualisierte Version des WMF. Der einzige mögliche Werte für diese Eigenschaft sind **"4.0", "5,0" ' 5.0PP' und 'letzte'**. Diese Werte werden aktualisiert. Der Standardwert ist 'letzte'.|
| Settings.Configuration.URL | Zeichenfolge | Gibt die URL-Adresse, Ihre DSC Konfiguration Zip-Datei herunterladen. Die bereitgestellte URL ein SAS-Token für den Zugriff benötigt, müssen Sie die protectedSettings.configurationUrlSasToken-Eigenschaft auf den Wert des SAS-Tokens festgelegt. Diese Eigenschaft ist settings.configuration.script oder settings.configuration.function definiert sind. |
| Settings.Configuration.Script | Zeichenfolge | Gibt den Dateinamen des Skripts mit der Definition der DSC-Konfiguration. Dieses Skript muss sich im Stammverzeichnis der Zip-Datei von der configuration.url-Eigenschaft angegebenen URL heruntergeladen. Diese Eigenschaft ist settings.configuration.url oder settings.configuration.script definiert sind. |
| Settings.Configuration.Function | Zeichenfolge | Gibt den Namen der DSC-Konfiguration. Die Konfiguration muss im Skript festgelegten configuration.script enthalten sein. Diese Eigenschaft ist settings.configuration.url oder settings.configuration.function definiert sind. |
| settings.configurationArguments | Auflistung | Definiert alle Parameter, die Sie zur Konfiguration DSC übergeben möchten. Diese Eigenschaft wird nicht verschlüsselt. |
| settings.configurationData.url | Zeichenfolge | Gibt die URL der Konfigurationsdatei (.pds1) herunterladen, als Eingabe für die DSC-Konfiguration verwenden. Die bereitgestellte URL ein SAS-Token für den Zugriff benötigt, müssen Sie die protectedSettings.configurationDataUrlSasToken-Eigenschaft auf den Wert des SAS-Tokens festgelegt.|
| settings.privacy.dataEnabled | Zeichenfolge | Aktiviert oder deaktiviert die Telemetrie Auflistung. Die einzig möglichen Werte für diese Eigenschaft sind **'Aktivieren', 'Deaktivieren', '', oder $null**. Lassen diese Eigenschaft leer oder null ermöglicht Telemetrie. Der Standardwert ist ". [Weitere Informationen](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings | Auflistung | Alternative Speicherorte, WMF herunterladen definiert. [Weitere Informationen](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments | Auflistung | Definiert alle Parameter, die Sie zur Konfiguration DSC übergeben möchten. Diese Eigenschaft ist verschlüsselt. |
| protectedSettings.configurationUrlSasToken | Zeichenfolge | Gibt das SAS-Token configuration.url festgelegten URL zuzugreifen. Diese Eigenschaft ist verschlüsselt. |
| protectedSettings.configurationDataUrlSasToken | Zeichenfolge | Gibt das SAS-Token configurationData.url festgelegten URL zuzugreifen. Diese Eigenschaft ist verschlüsselt. |

## <a name="settings-vs-protectedsettings"></a>Einstellungen im Vergleich zu ProtectedSettings
Alle werden in einer Textdatei Einstellungen des virtuellen Computers gespeichert.
Eigenschaften unter "Settings" sind öffentliche Eigenschaften, da sie nicht in der Text verschlüsselt werden.
Eigenschaften unter 'ProtectedSettings' mit einem Zertifikat verschlüsselt und werden nicht im Klartext in der Datei auf dem virtuellen Computer angezeigt.

Wenn die Konfiguration Anmeldeinformationen erforderlich ist, können sie in ProtectedSettings aufgenommen:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Beispiel

Im folgende Beispiel wird der [DSC Erweiterung Handler Seite](virtual-machines-windows-extensions-dsc-overview.md)im Abschnitt "Erste Schritte" abgeleitet.
Dieses Beispiel verwendet Ressource-Manager Vorlagen statt Cmdlets zum Bereitstellen der Erweiterung. "IisInstall.ps1" Konfiguration speichern, platzieren Sie es in ein. ZIP-Datei, und Laden Sie die Datei in einen gültigen URL. Dieses Beispiel verwendet Azure BLOB-Speicher, aber es ist herunterladen. ZIP-Dateien von jedem beliebigen Standort.

In der Vorlage Azure-Ressourcen-Manager weist der folgende Code die VM die richtige Datei herunterladen und Ausführen der entsprechenden PowerShell-Funktion:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Das vorherige Format aktualisieren
Alle in das vorherige Format (mit den öffentlichen Eigenschaften ModulesUrl, ConfigurationFunction, SasToken oder Eigenschaften) automatisch in das aktuelle Format anpassen und wie zuvor.

Das folgende Schema ist vom vorherigen Settings Schema aussah:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Hier ist wie das vorherige Format in das aktuelle Format passt:

| Eigenschaftenname | Vorherige Schema entspricht |
| --- | --- |
| settings.wmfVersion | Einstellungen. WMFVersion |
| Settings.Configuration.URL | Einstellungen. ModulesUrl |
| Settings.Configuration.Script | Erste Teil Settings. ConfigurationFunction (vor '\\\\") |
| Settings.Configuration.Function | Zweiten Teil Settings. ConfigurationFunction (nach "\\\\") |
| settings.configurationArguments | Einstellungen. Eigenschaften |
| settings.configurationData.url | protectedSettings.DataBlobUri (ohne SAS-Token) |
| settings.privacy.dataEnabled | Einstellungen. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings | Einstellungen. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments | protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken | Einstellungen. SasToken |
| protectedSettings.configurationDataUrlSasToken | SAS-Token aus protectedSettings.DataBlobUri |


## <a name="troubleshooting---error-code-1100"></a>Problembehandlung - Fehlercode 1100
Fehlercode 1100 gibt an, dass es ein Problem mit der DSC-Erweiterung.
Der Text dieser Fehler Variable und ändern kann.
Hier sind einige der Fehler, die auftreten können und wie Sie behoben werden können.

### <a name="invalid-values"></a>Ungültige Werte
"Privacy.dataCollection ist '{0}'. Die einzig möglichen Werte sind "" Aktivieren"und"Deaktivieren"" "WmfVersion ist '{0}'. Mögliche Werte sind... und 'letzte' "

Problem: Der angegebene Wert ist nicht zulässig.

Lösung: Ändern Sie den ungültigen Wert auf einen gültigen Wert. Siehe die Tabelle im Abschnitt Details.

### <a name="invalid-url"></a>Ungültige URL
"ConfigurationData.url ist '{0}'. Dies ist keine gültige URL""DataBlobUri ist '{0}'. Dies ist keine gültige URL""Configuration.url ist '{0}'. Dies ist keine gültige URL"

Problem: Ein bereitgestellt-URL ist ungültig.

Lösung: Überprüfen Sie die angegebenen URLs. Stellen Sie sicher, dass alle URLs auf gültige Speicherorte aufgelöst, die Erweiterung auf dem Remotecomputer zugreifen kann.

### <a name="invalid-configurationargument-type"></a>Ungültiger ConfigurationArgument-Typ
"Ungültige ConfigurationArguments Typ {0}"

Problem: Die ConfigurationArguments-Eigenschaft kann nicht in ein Hashtable-Objekt aufgelöst. 

Lösung: Stellen der ConfigurationArguments-Eigenschaft einer Hashtabelle. Führen Sie das Format im Beispiel bereitgestellt. Achten Sie auf Anführungszeichen, Kommas und Klammern.

### <a name="duplicate-configurationarguments"></a>Doppelte ConfigurationArguments
"Gefunden Sie doppelte Argumente '{0}' in öffentliche und geschützte ConfigurationArguments"

Problem: Die ConfigurationArguments öffentlich und ConfigurationArguments in geschützten Einstellungen Eigenschaften mit demselben Namen enthalten.

Lösung: Entfernen eines doppelten Eigenschaften.

### <a name="missing-properties"></a>Fehlende Eigenschaften
"Configuration.function muss configuration.url oder configuration.module angegeben werden"

"Configuration.url erfordert angegeben, configuration.script"

"Configuration.script erfordert angegeben, configuration.url"

"Configuration.url erfordert angegeben, configuration.function"

"ConfigurationUrlSasToken erfordert angegeben, configuration.url"

"ConfigurationDataUrlSasToken erfordert angegeben, configurationData.url"

Problem: Definierte Eigenschaft benötigt eine andere Eigenschaft fehlt.

Solutions: 
- Bereitstellen Sie fehlende Eigenschaft.
- Entfernen Sie die Eigenschaft, die die Eigenschaft fehlende.


## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über DSC und virtuellen Skalierung in [Mithilfe virtueller Computer Maßstab legt Azure DSC-Erweiterung](../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

Weitere Informationen finden Sie auf [DSC sichere Verwaltung von Anmeldeinformationen](virtual-machines-windows-extensions-dsc-credentials.md). 

Informationen auf Azure DSC Erweiterung Ereignishandler finden Sie unter [Einführung in Ereignishandler Erweiterung Azure gewünschten Konfiguration](virtual-machines-windows-extensions-dsc-overview.md). 

Weitere Informationen über PowerShell DSC [finden Sie auf der Mitte der PowerShell-Dokumentation](https://msdn.microsoft.com/powershell/dsc/overview). 
