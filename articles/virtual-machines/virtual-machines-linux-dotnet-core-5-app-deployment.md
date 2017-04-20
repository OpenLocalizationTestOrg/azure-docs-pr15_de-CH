<properties
   pageTitle="Automatisierung der Anwendungsbereitstellung mit der virtuellen | Microsoft Azure"
   description="Azure Virtual Machine DotNet Core-Lernprogramm"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Bereitstellung von Azure-Ressourcen-Manager-Vorlagen

Nachdem alle Azure infrastrukturelle Anforderungen identifiziert und eine Bereitstellungsvorlage übersetzt wurden, muss die Bereitstellung der Anwendung behandelt werden. Bereitstellung Hier bezieht Anwendungsbinärdateien auf Azure-Ressourcen installiert. Für die Musik Store Sample .net Core, NGINX und Supervisor installiert und auf jedem virtuellen Computer konfiguriert werden müssen. Musik Shop-Binärdateien auf dem virtuellen Computer installiert werden müssen, und die Musik-Datenbank bereits erstellt.

Dieses Dokument beschreibt, wie Virtual Machine Extensions Bereitstellung und Konfiguration in Azure virtuelle Computer automatisieren können. Alle Abhängigkeiten und eindeutige Konfigurationen werden hervorgehoben. Für optimale Ergebnisse bereits eine Instanz der Lösung der Azure-Abonnement und zusammen mit der Azure-Ressourcen-Manager bereitgestellt. Die vollständige Vorlage – [Bereitstellung von Musik Store on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux)finden Sie hier.

## <a name="configuration-script"></a>Skript

Virtual Machine Extensions sind spezielle Programme für virtuelle Computer Konfiguration Automatisierung ausgeführt. Extensions werden für viele bestimmte Anti-Virus, Protokollierung und Andockfenster Konfiguration. Eine benutzerdefiniertes Skript Erweiterung kann verwendet werden, ein Skript für einen virtuellen Computer ausgeführt. Mit Musik Store Sample obliegt es benutzerdefiniertes Skript Erweiterung Ubuntu virtuelle Computer konfigurieren und die Musik Store-Anwendung installieren.

Überprüfen Sie bevor Details wie virtuellen Extensions in Azure-Ressourcen-Manager-Vorlage deklariert werden das Skript ausgeführt wird. Dieses Skript konfiguriert den Ubuntu virtuellen Computer Musik Store-Anwendung hosten. Beim Ausführen des Skripts alle benötigt und Software installiert, Musik Store-Anwendung vom Datenquellen-Steuerelement zu installieren, Vorbereiten der Datenbank. 

Weitere Informationen zu .net Core Anwendung auf Linux, finden Sie unter [Veröffentlichen einer Linux produktiven Umgebung](https://docs.asp.net/en/latest/publishing/linuxproduction.html). 

> In diesem Beispiel wird zu Demonstrationszwecken.

```none
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## <a name="vm-script-extension"></a>VM-Skript-Erweiterung

VM-Erweiterungen kann mit der erweiterungsressource in Azure-Ressourcen-Manager-Vorlage für einen virtuellen Computer zur Buildzeit ausgeführt werden. Die Erweiterung kann mit dem Assistenten für Visual Studio Ressource oder gültigen JSON in die Vorlage einfügen hinzugefügt werden. Skript-Erweiterung Ressource ist in die VM-Ressource geschachtelt. Dies ist im folgenden Beispiel ersichtlich.

Folgen Sie diesem Link zum Beispiel JSON innerhalb der Ressourcen-Manager-Vorlage- [VM Skript Erweiterung](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359)anzuzeigen. 

Beachten Sie in der folgenden JSON, in GitHub gespeichert ist. Dieses Skript kann auch in Azure BLOB-Speicher gespeichert werden. Außerdem zulassen Azure Resource Manager Vorlagen Skript Ausführungszeichenfolge zu Werten der Parameter für die Ausführung von Skripten als Parameter verwendet werden können In diesem Fall Daten bereitgestellt, wenn die Vorlagen bereitstellen und diese Werte können dann verwendet werden, wenn das Skript ausgeführt.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
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
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Weitere Informationen zur Verwendung von benutzerdefinierten Skripts Erweiterung finden Sie unter [benutzerdefinierte Skript Extensions Resource Manager Vorlagen](./virtual-machines-linux-extensions-customscript.md).

## <a name="next-step"></a>Nächstes

<hr>

[Mehr Azure Ressourcenmanager Vorlagen durchsuchen](https://github.com/Azure/azure-quickstart-templates)
