<properties
    pageTitle="Verwenden Sie die Erweiterung CustomScript auf Linux VM | Microsoft Azure"
    description="Erfahren Sie, wie mit der Erweiterung CustomScript Applikationen auf virtuelle Linux-Computer in Azure mit klassischen Bereitstellungsmodell erstellt bereitstellen."
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="gbowerman"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="multiple"
    ms.tgt_pltfrm="linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

#<a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Bereitstellen einer LEUCHTE app mit Azure CustomScript-Erweiterung für Linux#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Microsoft Azure CustomScript-Erweiterung für Linux bietet eine Möglichkeit zum Anpassen der virtuellen Maschinen (VMs) durch Ausführen von beliebigem Code von VM (z. B. Python und Bash) unterstützten Skriptsprachen geschrieben. Dies ermöglicht sehr flexibel anwendungsbereitstellung auf mehreren Computern automatisieren.

Sie können die CustomScript Erweiterung der Azure-Verwaltungsportal, Windows PowerShell oder in der Azure-Befehlszeilenschnittstelle (CLI Azure) bereitstellen.

In diesem Artikel verwenden wir erstellt Azure CLI bereitstellen eine einfache Anwendung LAMPE ein Ubuntu VM mit klassischen Bereitstellungsmodell.

## <a name="prerequisites"></a>Erforderliche Komponenten

Erstellen Sie für dieses Beispiel zunächst zwei Azure VMs mit Ubuntu 14.04 oder höher. VMs werden *Skript-Vm* und *Lampe Vm*bezeichnet. Verwenden Sie eindeutige Namen beim Erstellen der VMs. Einer dient zum CLI-Befehle ausführen und eine LAMPE app bereitgestellt wird.

Sie benötigen auch ein Azure Storage-Konto und einen Schlüssel für den Zugriff (von klassischen Azure-Portal erhalten Sie).

Finden Sie Hilfe zum Erstellen von Linux VMs Azure erstellen [Einen virtuellen Linux ausgeführt](virtual-machines-linux-classic-createportal.md).

Befehle Install Ubuntu angenommen, aber die Installation für alle unterstützten Linux-Distribution anpassen.

Skript-Vm-VM muss Azure-CLI mit Verbindung zum Azure installiert. Hilfe hierzu finden Sie unter [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md).

## <a name="upload-a-script"></a>Hochladen von Skripts

Die Erweiterung CustomScript verwenden wir ein Skript auf einer entfernten VM zu LAMPE Stapel eine PHP-Seite erstellen. Um Zugriff auf das Skript werden wir diese als Azure Blob hochladen.

### <a name="script-overview"></a>Skripts (Übersicht)

Das Skriptbeispiel LAMPE Stapel Ubuntu (einschließlich eine automatische Installation von MySQL) installiert, ein einfaches PHP-Datei und Apache startet.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Skript hochladen

Speichern Sie das Skript als Textdatei, z. B. *install_lamp.sh*, und Laden Sie sie in Azure-Speicher. Sie können problemlos Azure CLI dazu. Das folgende Beispiel lädt die Datei in einen Speichercontainer namens "Scripts". Wenn der Container nicht vorhanden ist, müssen Sie zuerst erstellen.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Erstellen Sie eine JSON-Datei, die das Skript aus dem Azure-Speicher herunterladen beschreibt auch. Speichern Sie diese als *public_config.json* ("Mystorage" durch den Namen Ihres Speicherkontos ersetzen):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>Bereitstellen der Erweiterung

Jetzt können Sie den nächsten Befehl Linux CustomScript Erweiterung der remote VM mit der Azure-CLI bereitgestellt.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

Der vorherige Befehl downloads und führt das Skript *install_lamp.sh* auf dem virtuellen Computer *Lampe Vm*bezeichnet.

Da die Anwendung einen Webserver enthält, müssen Sie einen HTTP-Abhör-Port der remote VM weiter öffnen.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Überwachung und Problembehandlung

Sie können wie das benutzerdefinierte Skript führt anhand der Protokolldatei auf remote VM überprüfen. SSH *Lampe Vm* zu Ende der Protokolldatei mit dem nächsten Befehl.

    cd /var/log/azure/customscript
    tail -f handler.log

Nach dem Ausführen der CustomScript-Erweiterung können Sie PHP-Seite erstellte Informationen durchsuchen. PHP-Seite für das Beispiel in diesem Artikel ist *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Die gleichen Schritte können Sie komplexere apps bereitstellen. In diesem Beispiel wurde das Installationsskript als öffentliche Blob in Azure-Speicher gespeichert. Eine sicherere wäre das Installationsskript als sichere Blob mit [Sicheren Zugriff Signatur](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS) speichern.

Zusätzliche Ressourcen für Azure CLI, Linux und die Erweiterung CustomScript werden nachfolgend aufgeführt.

[Automatisieren von Linux VM Anpassungsaufgaben CustomScript Erweiterung](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Azure Linux Extensions (GitHub)](https://github.com/Azure/azure-linux-extensions)

[Linux- und Open Source-Computing in Azure](virtual-machines-linux-opensource-links.md)
