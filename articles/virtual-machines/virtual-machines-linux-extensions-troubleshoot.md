<properties
   pageTitle="Problembehandlung bei Linux VM-Erweiterung-Fehler | Microsoft Azure"
   description="Informationen Sie zur Problembehandlung bei Azure Linux VM-Erweiterung-Fehler"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-linux-vm-extension-failures"></a>Problembehandlung bei Azure Linux VM-Erweiterung-Fehler

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Erweiterungsstatus anzeigen
Azure Ressourcenmanager Vorlagen können von Azure-CLI ausgeführt werden. Nach Ausführung die Vorlage kann den Erweiterungsstatus Azure Resource Explorer oder Befehlszeilentools angezeigt.

Hier ist ein Beispiel:

Azure CLI:

      azure vm get-instance-view


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

## <a name="troubleshooting-extenson-failures"></a>Problembehandlung bei Extenson-Fehler:

### <a name="re-running-the-extension-on-the-vm"></a>Die Erweiterung Ausführen des virtuellen Computers erneut

Wenn Skripts auf dem virtuellen Computer ausführen benutzerdefinierte Skript Erweiterung können Sie manchmal ausführen ein Fehler, VM wurde erstellt, aber das Skript ist fehlgeschlagen. Unter diesen Conditons wird empfohlen, diese Fehler zu beheben Erweiterung entfernen und erneut die Vorlage erneut.
Hinweis: In Zukunft würden diese Funktionalität erweitert werden um die Notwendigkeit die Erweiterung deinstallieren.

#### <a name="remove-the-extension-from-azure-cli"></a>Entfernen Sie die Erweiterung von Azure-CLI

      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

"Verteiler-Name" entspricht den Erweiterungstyp aus der Ausgabe von "Azure Vm Get-Instanz-Ansicht" wobei Name ist der Name der erweiterungsressource aus der Vorlage

Gelöschte Erweiterung kann die Vorlage erneut die Skripts auf dem virtuellen Computer aus ausgeführt werden.
