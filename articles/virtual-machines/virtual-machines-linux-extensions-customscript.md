<properties
   pageTitle="Benutzerdefinierte Skripts auf Linux VMs | Microsoft Azure"
   description="Automatisieren von Konfigurationsaufgaben Linux VM mithilfe der benutzerdefinierten Skript-Erweiterung"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="nepeters"/>

# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Azure benutzerdefiniertes Skript-Erweiterung verwenden mit virtuelle Linux-Computer

Benutzerdefinierte Skripts Erweiterung downloads und Skripts auf Azure virtuellen Maschinen ausgeführt. Diese Erweiterung ist nützlich für Post Bereitstellungskonfiguration, Softwareinstallation oder einer anderen Konfiguration / Management-Aufgabe. Skripts können von Azure-Speicher oder andere zugänglich Internetadresse heruntergeladen oder zur Laufzeit Erweiterung. Die benutzerdefinierten Skripts Erweiterung Azure Resource Manager Vorlagen integriert und kann auch mithilfe der Azure-CLI, PowerShell, Azure-Portal oder die REST-API von Azure Virtual Machine ausgeführt werden.

Dieses Dokument beschreibt, wie benutzerdefinierte Skript Erweiterung von Azure-CLI und einer Vorlage Azure-Ressourcen-Manager und erläutert Schritte zur Problembehandlung bei Linux-Systemen.

## <a name="extension-configuration"></a>Konfiguriert

Das benutzerdefinierte Skript konfiguriert gibt z. B. Skript und der Befehl ausgeführt werden. Diese Konfiguration kann in Konfigurationsdateien gespeichert werden, angegeben in der Befehlszeile oder in einer Vorlage Azure-Ressourcen-Manager. Daten können in eine geschützte Konfiguration verschlüsselt und entschlüsselt nur innerhalb des virtuellen Computers gespeichert werden. Die geschützte Konfiguration eignet sich der Ausführungsbefehl Geheimnisse wie ein Kennwort enthält.

### <a name="public-configuration"></a>Öffentliche Konfiguration

Schema:

- **CommandToExecute**: (string erforderlich) Eintrag Punkt Skript ausführen
- **FileUris**: (optional, Zeichenfolgenarray) die URLs für die Dateien gedownloadet werden.
- **Zeitstempel** (optional, Integer) verwenden Sie dieses Feld nur für eine Wiederholung des Skripts auslösen, indem Sie dieses Feld ändern.

```none
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Geschützte Konfiguration

Schema:

- **CommandToExecute**: (optional, String) Eintrag Point-Skript ausführen. Verwenden Sie dieses Feld, wenn der Befehl geheime Daten wie Kennwörter.
- **StorageAccountName**: (optional, Zeichenfolge) den Namen des Speicherkontos. Wenn Sie Speicher Anmeldeinformationen angeben, müssen alle FileUris URLs für Azure-Blobs sein.
- **StorageAccountKey**: (optional, String) die Taste Speicherkontos.


```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI

Wenn der Azure-CLI das benutzerdefinierte Skript Erweiterung, Erstellen einer Konfigurationsdatei oder Dateien mit mindestens den Datei-Uri und den Ausführungsbefehl Skript.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /scirpt-config.json
```

Gegebenenfalls der Befehl ausgeführt werden kann mit der `--public-config` und `--private-config` Option, die während der Ausführung und eine separate Konfigurationsdatei angegeben werden kann.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI-Beispiele

**Beispiel 1** : öffentliche Konfiguration Skriptdatei.

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
  "commandToExecute": "./hello.sh"
}
```

Azure CLI-Befehl:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Beispiel 2** : öffentliche Konfiguration keine Skriptdatei.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI-Befehl:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Beispiel 3** : öffentliche Datei wird verwendet, um die Skriptdatei URI angeben und eine geschützte Konfiguration zum auszuführenden Befehl angeben.

Öffentliche Konfigurationsdatei:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
}
```

Geschützte Datei:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Azure CLI-Befehl:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path ./public.json --private-config-path ./protected.json
```

## <a name="resource-manager-template"></a>Ressourcen-Manager-Vorlage

Zum Zeitpunkt der Bereitstellung virtueller Computer mithilfe einer Vorlage Ressourcenmanager können Azure benutzerdefinierte Skript Erweiterung ausgeführt werden. Hierzu die Bereitstellungsvorlage fügen Sie korrekt formatierte JSON hinzu.

### <a name="resource-manager-examples"></a>Beispiele für Ressourcenmanager

**Beispiel 1** : öffentliche Konfiguration.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Beispiel 2** : Ausführungsbefehl in geschützten Konfiguration.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Siehe .net Core Musik Store Demo ein vollständiges Beispiel - [Musik Store Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Problembehandlung

Wenn benutzerdefinierte Skript-Erweiterung ausgeführt wird, wird das Skript erstellt oder heruntergeladen wurde, in eine ähnlich dem folgenden Beispiel. Die Ausgabe des Befehls werden ebenfalls in diesem Verzeichnis `stdout` und `stderr` Datei.

```none
/var/lib/azure/custom-script/download/0/
```

Azure Skript Erweiterung erzeugt ein Protokoll Sie hier finden.

```none
/var/log/azure/customscript/handler.log
```

Ausführungsstatus eines benutzerdefinierten Skripts Erweiterung kann auch mit der Azure-CLI abgerufen werden.

```none
azure vm extension get <resource-group> <vm-name>
```

Die Ausgabe sieht wie der folgende Text:

```none
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zu anderen VM Skripterweiterungen Übersicht [Azure Skript-Erweiterung für Linux](./virtual-machines-linux-extensions-features.md).