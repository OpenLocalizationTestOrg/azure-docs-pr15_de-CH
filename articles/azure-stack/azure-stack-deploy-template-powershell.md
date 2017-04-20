<properties
    pageTitle="Bereitstellen von Vorlagen mit PowerShell in Azure Stapel | Microsoft Azure"
    description="Informationen Sie zum Bereitstellen einer virtuellen Maschine mit einem Ressourcenmanager Vorlage PowerShell."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Bereitstellen von Vorlagen in Azure Stapel mit PowerShell

Mithilfe von PowerShell Azure Resource Manager Vorlagen POC Stapel Azure bereitstellen.  Ressourcen-Manager Vorlagen Bereitstellung und alle Ressourcen für die Anwendung in einer einzigen koordinierten Operation.

## <a name="run-azurerm-powershell-cmdlets"></a>Ausführen von AzureRM-PowerShell-cmdlets

In diesem Beispiel führen Sie ein Skript zum Bereitstellen einer virtuellen Maschine auf Azure Stapel POC Ressourcenmanager Vorlage.  Sicherzustellen Sie bevor Sie fortfahren, dass Sie [installiert und konfiguriert PowerShell](azure-stack-connect-powershell.md)  

Diese Vorlage wird VHD ist ein Standardbild Marketplace (WindowsServer-2012-R2-Datencenter).

1.  Wechseln Sie zu <http://aka.ms/AzureStackGitHub>suchen **101 einfach Windows Vm** -Vorlage und zu speichern: c:\\Vorlagen\\Azuredeploy-101-einfach-Windows-vm.json.

2.  In PowerShell das Skript folgende Bereitstellung. Ersetzen Sie *Benutzername* und *Kennwort* mit Ihrem Benutzernamen und Kennwort ein. Weitere Verwendungen erhöhen Sie den Wert für den Parameter *$myNum* die Bereitstellung überschreiben zu verhindern.

    ```PowerShell
        # Set Deployment Variables
        $myNum = "001" #Modify this per deployment
        $RGName = "myRG$myNum"
        $myLocation = "local"

        # Create Resource Group for Template Deployment
        New-AzureRmResourceGroup -Name $RGName -Location $myLocation

        # Deploy Simple IaaS Template
        New-AzureRmResourceGroupDeployment `
            -Name myDeployment$myNum `
            -ResourceGroupName $RGName `
            -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
            -NewStorageAccountName mystorage$myNum `
            -DnsNameForPublicIP mydns$myNum `
            -AdminUsername <username> `
            -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
            -VmName myVM$myNum `
            -WindowsOSVersion 2012-R2-Datacenter
    ```

3.  Öffnen Sie Stapel Azure-Portal zu, klicken Sie auf **Durchsuchen**klicken Sie auf **virtuellen Maschinen**und suchen Sie den neuen virtuellen Computer (*myDeployment001*).

## <a name="video-example-hybrid-virtual-machine-deployment"></a>Video-Beispiel: Hybrid VM-Bereitstellung

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-hybrid-vm-deployment]

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Vorlagen mit Visual Studio](azure-stack-deploy-template-visual-studio.md)
