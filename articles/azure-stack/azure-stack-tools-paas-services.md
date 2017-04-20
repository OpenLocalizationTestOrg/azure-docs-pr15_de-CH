<properties
    pageTitle="Tools und PaaS-Dienste für Azure | Microsoft Azure"
    description="Informationen Sie zum Einstieg in Azure Stack PaaS-Dienste."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="tools-for-azure-stack"></a>Tools für Azure Stack

Sie können die nachfolgend beschriebenen Tools. Wenn Sie neue Services und Tools benachrichtigt werden möchten, können folgen Sie #AzureStack auf Twitter.

## <a name="template-tools"></a>Werkzeuge und Vorlagen

### <a name="azure-stack-github-templates"></a>Azure Stapel Github Vorlagen
Untersuchen Sie die wachsende Sammlung von [Azure Stapel GitHub Vorlagen](https://github.com/Azure/AzureStack-QuickStart-Templates). Vorlagen "Schnellstart" Azure Resource Manager sollen wie [Azure](https://github.com/Azure/azure-quickstart-templates)Einstieg in einfache Bausteine und Beispiele in der Microsoft Azure Stapel technischen Vorschau Nachweis der Konzept-Umgebung bereitstellen. Werden erste Partei Arbeitslast für [Ad-nicht-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha), [Sql-2014-nicht-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha), [Sharepoint-2013-nicht-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha), wie sowie einfache 101 Vorlagen [101 einfach Windows Vm](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm).


### <a name="marketplace-item-packaging-tool-and-sample"></a>Marketplace Element Verpackung Werkzeug und
[Downloaden und verwenden Sie das Tool Verpackung](http://www.aka.ms/azurestackmarketplaceitem) Marketplace für Ihre eigenen benutzerdefinierten Vorlagen Marketplace Azure Stapel hinzufügen erstellen. Anleitung zum Erstellen einer Marketplace und Mieter verfügbar machen finden im [Marketplace erstellen Item](azure-stack-create-and-publish-marketplace-item.md).

## <a name="developer-tools"></a>Entwicklertools


### <a name="use-visual-studio-and-azure-stack-tp2-on-the-mas-con01-virtual-machine"></a>Verwenden Sie Visual Studio und Azure Stack TP2 auf dem virtuellen Computer MAS-CON01
Wenn Sie Visual Studio auf der VM-Konsole verwenden, um mit Azure Stapel Vorlagen arbeiten möchten, müssen Sie die richtigen Versionen der erforderlichen Tools installieren. Gehen Sie für TP2 unterstützten Versionen installieren.

1. Mithilfe von Remotedesktopverbindung MAS CON01 virtuellen Computer mit den Azurestack\azurestackadmin-Anmeldeinformationen anmelden.
2. Installieren Sie und öffnen Sie Webplattform-Installer.
3. Suchen und Installieren von **Visual Studio Community 2015 mit Microsoft Azure SDK - 2.9.5**.
4. Verwenden **Software**, deinstallieren Sie **Microsoft Azure PowerShell (September)** , die als Teil des SDK installiert wird.
5. Öffnen Sie den Webplattform-Installer.
6. Suchen und Installieren von **Microsoft Azure PowerShell - Azure Stapel Technical Preview 2**. 
7. Starten Sie MAS CON01 virtuellen Computer neu.
8. Mithilfe von Remotedesktopverbindung MAS CON01 virtuellen Computer mit den Azurestack\azurestackadmin-Anmeldeinformationen anmelden.
9. Visual Studio öffnen Sie und überprüfen Sie, ob der Azure-Stack-Umgebung herstellen, Vorlagen und so weiter. 

### <a name="azure-powershell-sdk"></a>Azure PowerShell SDK
Azure PowerShell ist ein Modul, die zum Verwalten von Azure und Azure Stack mit Windows PowerShell-Cmdlets bereitstellt. Sie können verwenden Cmdlets erstellen, testen, bereitstellen und Verwalten von Projektmappen und Dienstleistungen Azure Stack-Plattform.
[Azure PowerShell SDK downloaden](http://aka.ms/azStackPsh) und [Installieren Sie PowerShell](azure-stack-connect-powershell.md).

### <a name="azure-cross-platform-command-line-interfaces"></a>Azure cross-Plattform-Befehlszeilenschnittstellen
Installieren Sie der Azure-Befehlszeilenschnittstelle (CLI Azure) zur Verwendung der Open Source-Shell basierende Befehle zum Erstellen und Verwalten von Ressourcen in Microsoft Azure Stapel schnell.

[Download von Windows-CLI](http://aka.ms/azstack-windows-cli)

[Mac CLI herunterladen](http://aka.ms/azstack-linux-cli)

[Downloaden Sie die Linux-CLI](http://aka.ms/azstack-mac-cli)

>[AZURE.NOTE]
>
> + Wenn Sie auf einem Mac oder Linux sind, erhalten Sie auch die CLI mithilfe des Befehls`npm install -g azure-cli@0.9.11`</br>
> + Wenn Sie Überprüfungsprobleme Zertifikat erhalten, führen Sie den Befehl`set NODE_TLS_REJECT_UNAUTHORIZED=0`
