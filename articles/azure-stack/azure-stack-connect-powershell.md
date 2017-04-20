<properties
    pageTitle="Verbinden mit Azure Stapel mit PowerShell | Microsoft Azure"
    description="Verwalten Sie Azure-Stapel mit PowerShell"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-powershell-and-connect-to-azure-stack"></a>Installieren Sie PowerShell und Azure Stack an
In diesem Handbuch schrittweise wir für die Verbindung mit Azure Stapel mit PowerShell. Abschluss können folgendermaßen dazu verwalten und Bereitstellen von Ressourcen.

## <a name="install-azure-stack-powershell-cmdlets"></a>Azure Stapel PowerShell-Cmdlets installieren

1.  AzureRM-Cmdlets werden aus der Galerie PowerShell installiert. Zunächst öffnen Sie eine Konsole PowerShell MAS CON01 und führen Sie folgenden Befehl eine Liste von PowerShell Repositories verfügbar zurückgeben:

        Get-PSRepository

      ![Screenshot aus laufenden 4Get-PSRepository mit PSGallery aufgelistet](./media/azure-stack-connect-powershell/image1.png)

2.  Führen Sie den folgenden Befehl AzureRM Modul installiert:

        Install-Module -Name AzureRM -RequiredVersion 1.2.6 -Scope CurrentUser

    >[AZURE.NOTE] *-Bereich CurrentUser* ist optional. Ggf. mehr als den aktuellen Benutzer Zugriff auf die Module belegen und lassen Sie *den Bereichsparameter* .

3.  Bestätigen Sie die Installation von AzureRM Module Befehle:

        Get-Command -Module AzureRM.AzureStackAdmin

## <a name="connect-to-azure-stack"></a>Verbinden mit Azure Stapel
Ein Modul ist heruntergeladen, die Konfiguration von PowerShell Verbindung Azure Stapel behandelt.  Modul und zusätzliche Schritte finden Sie unter [Azure Stack-Tools](http://aka.ms/ConnectToAzureStackPS) . 

## <a name="retrieve-a-list-of-subscriptions"></a>Abrufen einer Liste von Abonnements
In diesem Abschnitt überprüfen Sie, dass PowerShell-Cmdlets abrufen und Auswählen eines Abonnements für die Verwendung auf Azure Stapel ausführen.

Führen Sie den folgenden Befehl zum Abrufen einer Liste mit Ihrem Konto verbundene Azure Stapel Abonnements:

    Get-AzureRmSubscription


## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen von Vorlagen mit PowerShell](azure-stack-deploy-template-powershell.md)

[Verbinden mit Azure CLI](azure-stack-connect-cli.md)

[Bereitstellen von Vorlagen mit Visual Studio](azure-stack-deploy-template-visual-studio.md)


