<properties
   pageTitle="Problembehandlung bei Windows VM-Erweiterung-Fehler | Microsoft Azure"
   description="Informationen Sie zur Problembehandlung bei Azure Windows VM-Erweiterung-Fehler"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Problembehandlung bei Azure Windows VM-Erweiterung-Fehler

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Erweiterungsstatus anzeigen
Azure Ressourcenmanager Vorlagen können von Azure PowerShell ausgeführt werden. Nach Ausführung die Vorlage kann den Erweiterungsstatus Azure Resource Explorer oder Befehlszeilentools angezeigt.

Hier ist ein Beispiel:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Hier ist die Ausgabe:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Problembehandlung bei Erweiterung-Fehler

### <a name="re-running-the-extension-on-the-vm"></a>Die Erweiterung Ausführen des virtuellen Computers erneut

Wenn Skripts auf dem virtuellen Computer ausführen benutzerdefinierte Skript Erweiterung können Sie manchmal ausführen ein Fehler, VM wurde erstellt, aber das Skript ist fehlgeschlagen. Unter diesen Conditons wird empfohlen, diese Fehler zu beheben Erweiterung entfernen und erneut die Vorlage erneut.
Hinweis: In Zukunft würden diese Funktionalität erweitert werden um die Notwendigkeit die Erweiterung deinstallieren.


#### <a name="remove-the-extension-from-azure-powershell"></a>Entfernen Sie die Erweiterung von Azure PowerShell

    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Gelöschte Erweiterung kann die Vorlage erneut die Skripts auf dem virtuellen Computer aus ausgeführt werden.
