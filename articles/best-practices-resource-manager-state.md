<properties
    pageTitle="Behandlung Status im Ressourcenmanager Vorlagen | Microsoft Azure"
    description="Zeigt empfohlene Vorgehensweisen mit komplexen Objekten Azure Ressourcenmanager und verknüpften Vorlagen Daten freigeben"
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="tomfitz"/>

# <a name="sharing-state-in-azure-resource-manager-templates"></a>Freigabe von Status im Ressourcenmanager Azure Vorlagen

Dieses Thema zeigt die best Practices für die Verwaltung und Freigabe von Status in Vorlagen. Die Parameter und Variablen in diesem Thema dargestellt sind Beispiele für Objekte können Bereitstellung Bedarf bequem organisieren. Anhand dieser Beispiele können Sie Objekte mit Eigenschaftswerten implementieren, die für Ihre Umgebung sinnvoll.

Dieses Thema ist Teil eines umfangreicheren Whitepapers. Vollständigen Artikel lesen herunterladen Sie [World Class Resource Manager Vorlagen und bewährte Methoden](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf)


## <a name="provide-standard-configuration-settings"></a>Standard-Konfigurationen bereitstellen

Ein gemeinsames Muster werden anstatt bieten eine Vorlage, die Flexibilität und zahlreiche Varianten zur Auswahl der bekannten Konfigurationen. Benutzer können, standard T-shirt-Größen wie kleine, mittlere und große Sandbox auswählen. Weitere Beispiele für T-shirt-Größen sind Produktangebote Community Edition oder Enterprise Edition. In anderen Fällen möglicherweise Workload-spezifische Konfigurationen einer Technologie – wie Karte reduzieren oder kein Sql.

Bei komplexen Objekten können Variablen erstellen, die Sammlung von Daten, manchmal auch als "Eigenschaftensammlungen" enthalten und verwenden diese Daten auf die Ressource-Deklaration in der Vorlage. Dieser Ansatz bietet Kunden gute bekannte Konfigurationen mit unterschiedlichen Größen vorkonfiguriert sind. Ohne bekannte Konfigurationen müssen Benutzer der Vorlage Cluster selbst anpassen ermitteln, Faktor Ressourcengründen Plattform und rechnen zum Identifizieren der resultierende Partitionierung Speicherkonten und anderen Ressourcen (aus Cluster Größe und Ressourcen). Neben optimal für Kunden, einige bekannte Konfigurationen sind einfacher zu unterstützen und können Sie eine höhere Dichte liefern.

Im folgenden Beispiel wird veranschaulicht, wie Variablen, die komplexe Objekte zum Darstellen von Sammlungen von Daten enthalten. Sammlungen definieren Sie Werte für die Größe des virtuellen Computers, Einstellungen, Einstellung und verfügbarkeitseinstellungen verwendet werden.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Beachten Sie, dass die **TshirtSize** -Variable verkettet T-shirt-Größe, die über einen Parameter (**klein**, **Mittel**, **Groß**) bereitgestellt, Text **TshirtSize**. Sie verwenden diese Variable Variable zugeordnete komplexen Objekts, T-shirt-Größe abgerufen.

Sie können dann diese Variablen in der Vorlage verweisen. Die Möglichkeit, benannte Variablen und deren Eigenschaften verweisen die Vorlagensyntax vereinfacht und erleichtert Kontext. Das folgende Beispiel definiert eine Ressource bereitstellen mithilfe zuvor gezeigten Werte festgelegt. Virtueller Speicher wird z. B. festgelegt durch Abrufen des Werts für `variables('tshirtSize').vmSize` zwar den Wert für die Festplattengröße entnommen `variables('tshirtSize').diskSize`. Darüber hinaus wird der URI verknüpfte Vorlage festgelegt, mit dem Wert für `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-to-a-template"></a>Zustand an eine Vorlage übergeben

Sie Freigabezustand in einer Vorlage Parameter, die während der Bereitstellung bieten.

Die folgende Tabelle enthält häufig verwendete Parameter in Vorlagen.

Name | Wert | Beschreibung
---- | ----- | -----------
Speicherort    | Eine Zeichenfolge aus einer eingeschränkten Liste von Azure-Regionen   | Der Speicherort, in dem die Ressourcen bereitgestellt werden.
storageAccountNamePrefix    | Zeichenfolge    | Eindeutigen DNS-Namen für das Speicherkonto die VM-Festplatten platziert
Domänenname  | Zeichenfolge    | Domänenname des öffentlich zugänglichen Jumpbox VM im Format: **{Domänenname}. {} Ort}.cloudapp.com** Beispiel: **mydomainname.westus.cloudapp.azure.com**
adminUsername   | Zeichenfolge    | Benutzername für VMs
adminPassword   | Zeichenfolge    | Kennwort für VMs
tshirtSize  | Zeichenfolge aus einer eingeschränkten Liste von Angeboten, T-shirt-Größen   | Die Größe der benannten Maßstab bereitstellen. Z. B. "Small", "Medium", "Groß"
virtualNetworkName  | Zeichenfolge    | Name des virtuellen Netzwerks Consumer verwenden möchte.
enableJumpbox   | Eine Zeichenfolge aus einer eingeschränkten Liste (aktiviert/deaktiviert) | Parameter, der angibt, ob eine Jumpbox für die Umgebung aktivieren. Werte: "aktiviert", "deaktiviert"

Im vorherigen Abschnitt verwendete **TshirtSize** -Parameter wird folgendermaßen definiert:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a>Verknüpfte Vorlagen Zustand übergeben

Verbindung mit verknüpften Vorlagen Sie oft eine Kombination aus statischen und generiert Variablen.

### <a name="static-variables"></a>Statische Variablen

Statische Variablen werden häufig zur Basiswerte wie URLs bereitstellen, die in einer Vorlage verwendet werden.

Im folgenden Auszug Vorlage `templateBaseUrl` gibt das Stammverzeichnis für die Vorlage im GitHub. Die nächste Zeile erstellt eine neue Variable `sharedTemplateUrl` , die die base-URL mit den Namen der Vorlage Ressourcen verkettet. Unterhalb dieser Zeile eine komplexe Objektvariable verwendet, um ein T-shirt-Größe gespeichert, die base-URL wird mit bekannten Konfiguration Vorlagenpfad verkettet und die `vmTemplate` Eigenschaft.

Der Vorteil dieses Ansatzes ist, ändert der Vorlagenpfad müssen nur die statische Variable an einer Stelle ändern, der in allen verknüpften Vorlagen übergibt.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>Generierte Variablen

Neben statischen Variablen werden mehrere Variablen dynamisch generiert. In diesem Abschnitt werden einige der häufig verwendeten generierten Variablen.

#### <a name="tshirtsize"></a>tshirtSize

Sie kennen diese generierte Variable aus den oben aufgeführten Beispielen.

#### <a name="networksettings"></a>networkSettings

In Kapazität, Funktion oder Lösungsvorlage bewertete End-to-End-erstellen die verknüpften Vorlagen normalerweise Ressourcen in einem Netzwerk. Ein einfacher Ansatz ist die Verwendung ein komplexes Objekts Einstellungen speichern und verknüpften Vorlagen.

Beispiel für Kommunikation Einstellungen angezeigt.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings

Ressourcen in verknüpften Vorlagen befinden sich häufig in einem Verfügbarkeit. Im folgenden Beispiel der Namen Verfügbarkeit angegeben und auch Fehlerdomäne und Aktualisieren mit Anzahl verwenden.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Benötigen Sie mehrere Verfügbarkeit legt fest (z. B. eine master-Knoten) und weitere Datenknoten einen Namen als Präfix verwenden können, geben Sie mehrere Verfügbarkeit und befolgen Sie das zuvor für eine Variable für eine bestimmte T-shirt-Größe angezeigt.

#### <a name="storagesettings"></a>storageSettings

Speicherdetails werden häufig mit verknüpften Vorlagen freigegeben. Im folgenden Beispiel stellt ein *StorageSettings* Objekt Details über die Storage-Konto und Container.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings

Mit verknüpften Vorlagen müssen Sie verschiedene Knoten über andere bekannte Konfigurationstypen Einstellung übergeben. Ein komplexes Objekt ist eine einfache Möglichkeit zum Speichern und Freigeben von Informationen zum Betriebssystem und erleichtert auch mehrere Betriebssystemoptionen für die Bereitstellung zu unterstützen.

Das folgende Beispiel zeigt ein Objekt für *OsSettings*:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings

Generierte Variable *MachineSettings* ist ein komplexes Objekt enthalten Core-Variablen für einen virtuellen Computer erstellen. Die Variablen umfassen Administrator-Benutzernamen und ein Kennwort, ein Präfix für die VM-Namen und einen Bildverweis Betriebssystem.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Hinweis, *OsImageReference* aus der *OsSettings* -Variablen in der Haupt-Vorlage definiert die Werte abruft. Das bedeutet einfach können das Betriebssystem für eine VM-vollständig oder die Einstellung eines Vorlage.

#### <a name="vmscripts"></a>vmScripts

*VmScripts* -Objekt enthält Details über die Skripts downloaden und Ausführen einer VM-Instanz einschließlich Verweise und außerhalb. Außerhalb nehmen die Infrastruktur. Innere Referenzen installierte Software Installation und Konfiguration

Listen Sie die Skripts für die VM herunterladen verwenden Sie die *ScriptsToDownload* -Eigenschaft. Dieses Objekt enthält außerdem Verweise auf Befehlszeilenargumente für verschiedene Aktionen. Dazu gehören die Standardinstallation für jeden Knoten eine Installation ausgeführt wird, nachdem alle Knoten bereitgestellt werden und zusätzliche Skripts, die für eine bestimmte Vorlage ausführen.

Dieses Beispiel ist eine Vorlage zur Bereitstellung MongoDB, Vermittler zu hohe Verfügbarkeit erfordern. *VmScripts* den Arbiterserver installiert wurde *ArbiterNodeInstallCommand* hinzugefügt.

Variablenabschnitt finden Sie die Variablen, die bestimmten Text zum Ausführen des Skripts mit den richtigen Werten definieren.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Zurücksetzen Zustand aus einer Vorlage

Nicht nur können Sie übergeben Daten in einer Vorlage können Sie auch Daten an die aufrufende Vorlage freigeben. Im Abschnitt **gibt** eine verknüpfte Vorlage erhalten Sie Schlüssel-Wert-Paare, die von der Vorlage verwendet werden kann.

Das folgende Beispiel zeigt wie die private IP-Adresse in eine verknüpfte Vorlage generiert.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

Innerhalb der main Vorlage können Sie die Daten mit der folgenden Syntax:

    "[reference('master-node').outputs.masterip.value]"

Dieser Ausdruck oder Ausgaben im Abschnitt Resources-Abschnitt der wichtigsten Vorlage können. Sie können den Ausdruck im Variablenabschnitt verwenden, da Laufzeitstatus beruht. Wenn dieser Wert von der Hauptvorlage verwenden:

    "outputs": { 
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }
     
Ein Beispiel Ausgaben Teil verknüpfte Vorlage Datenträger für einen virtuellen Computer zurückgegeben finden Sie unter [mehrere Datenträger für einen virtuellen Computer erstellen](resource-group-create-multiple.md#creating-multiple-data-disks-for-a-virtual-machine).

## <a name="define-authentication-settings-for-virtual-machine"></a>Einstellungen Sie Authentifizierung für virtuelle Computer

Das gleiche Muster für die Konfiguration gezeigt können Authentifizierungseinstellungen für einen virtuellen Computer. Parameter für die Übergabe erstellt in die Art der Authentifizierung.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

Variablen für die verschiedenen Authentifizierungstypen hinzufügen und eine Variable zum Speichern der für diese Bereitstellung basierend auf dem Wert des Parameters verwendet.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

Beim virtuellen Computer definieren, legen Sie **OsProfile** der Variablen, die Sie erstellt haben.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Nächste Schritte
- Abschnitte der Vorlage finden Sie unter [Erstellen von Azure Ressourcenmanager Vorlagen](resource-group-authoring-templates.md)
- Die Funktionen, die in einer Vorlage finden Sie unter [Azure Ressourcenmanager Vorlage Funktionen](resource-group-template-functions.md)

