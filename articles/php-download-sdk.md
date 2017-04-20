<properties
    pageTitle="Für PHP Azure SDK herunterladen"
    description="Informationen Sie zum Herunterladen und Installieren von Azure SDK für PHP."
    documentationCenter="php"
    services="app-service\web"
    authors="allclark"
    manager="douge"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="06/01/2016"
    ms.author="allclark;yaqiyang"/>

#<a name="download-the-azure-sdk-for-php"></a>Für PHP Azure SDK herunterladen

## <a name="overview"></a>Übersicht

Azure SDK für PHP enthält Komponenten, die Sie entwickeln, bereitstellen und Verwalten von PHP-Applikationen für Azure ermöglichen. Azure SDK für PHP umfasst Folgendes:

* **Die PHP-Clientbibliotheken für Azure**. Diese Klassenbibliotheken bieten eine Schnittstelle für den Zugriff auf Azure Funktionen wie Datenmanagement Services und cloud-Services.  
* **Azure Befehlszeilenschnittstelle für Mac, Linux und Windows (Azure CLI)**. Dies ist eine Reihe von Befehlen zum Bereitstellen und Verwalten von Azure Services wie Azure Websites und Azure Virtual Machines. Die Arbeit auf allen Plattformen, einschließlich Mac, Linux und Windows Azure-CLI.
* **Azure PowerShell (nur Windows)**. Dies ist ein Satz von PowerShell-Cmdlets zum Bereitstellen und Verwalten von Azure Services wie Cloud-Diensten und virtuellen Maschinen.
* **Azure-Emulatoren (nur Windows)**. Computing- und Emulatoren sind lokale Emulatoren Cloud-Dienste und Management-Services, die Sie eine Anwendung lokal testen. Azure-Emulatoren ausführen nur unter Windows.

In den folgenden Abschnitten beschrieben beschriebenen Komponenten herunterladen und installieren.

Die Informationen in diesem Thema annehmen, dass Sie [PHP] [ install-php] installiert.

> [AZURE.NOTE] PHP-Clientbibliotheken Azure mit PHP 5.5 oder höher erforderlich.

##<a name="php-client-libraries-for-azure"></a>PHP-Clientbibliotheken für Azure

PHP-Clientbibliotheken für Azure bieten eine Schnittstelle für den Zugriff auf Azure Funktionen wie Datenmanagement Services und cloud-Dienste von einem Betriebssystem. Diese Bibliotheken können über die Composer installiert.

Informationen zur Verwendung von PHP-Clientbibliotheken für Azure finden Sie unter [Verwendung des BLOB-][blob-service], [wie die Tabelle Service] [ table-service] und [Verwendung der Warteschlangendienst][queue-service].

###<a name="install-via-composer"></a>Über Composer installieren

1. [Installieren Sie Git][install-git].


    > [AZURE.NOTE] Unter Windows müssen Sie auch die Umgebungsvariable PATH ausführbare Git hinzufügen.

2. Erstellen Sie eine Datei mit dem Namen **composer.json** im Stammverzeichnis des Projekts und fügen Sie den folgenden Code hinzu:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Download ** [composer.phar] [ composer-phar] ** in der Projektstamm.

4. Öffnen Sie ein Eingabeaufforderungsfenster und führen Sie dies in der Projektstamm

        php composer.phar install

##<a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell und Azure Emulatoren

Azure PowerShell ist ein Satz von PowerShell-Cmdlets zum Bereitstellen und Verwalten von Azure Services (wie Cloud-Dienste und virtuelle Computer). Azure-Emulatoren werden Emulatoren Cloud-Dienste und Management-Services, die Sie eine Anwendung lokal testen. Diese Komponenten werden unterstützt nur Windows.

Mit [Microsoft-Webplattform-Installer]wird empfohlen Azure PowerShell und Emulatoren Azure installieren[download-wpi]. Beachten Sie, dass Sie auch andere Entwicklungskomponenten wie PHP, SQL Server, Microsoft Drivers für SQL Server für PHP und WebMatrix installieren können.

Informationen zur Verwendung von Azure PowerShell finden Sie unter [Verwenden von Azure PowerShell][powershell-tools].

##<a name="azure-cli"></a>Azure CLI

Azure-CLI ist eine Reihe von Befehlen zum Bereitstellen und Verwalten von Azure Services wie Azure Websites und Azure Virtual Machines. Informationen zum Installieren von Azure CLI finden Sie in der [Azure-CLI installieren](xplat-cli-install.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [PHP-Entwicklercenter](/develop/php/).


[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
