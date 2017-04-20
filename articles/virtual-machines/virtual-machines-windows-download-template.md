<properties
    pageTitle="Erstellen eines Abbilds VM von Azure-VM | Microsoft Azure"
    description="Erstellen Sie ein generalisiertes Abbild VM aus einer vorhandenen Azure VM in der Ressourcen-Manager-Bereitstellungsmodell erstellt"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>


# <a name="download-the-template-for-a-vm"></a>Downloaden Sie die Vorlage für einen virtuellen Computer

Beim Erstellen einer VM in Azure über das Portal oder PowerShell ist eine Vorlage Resource Manager automatisch für Sie erstellt. Diese Vorlage können Sie eine Bereitstellung schnell duplizieren. Die Vorlage enthält Informationen über alle Ressourcen in einer Ressourcengruppe. Für einen virtuellen Computer bedeutet dies die Vorlagencontainer alles für die VM Gruppe, einschließlich der Netzwerkressourcen entsteht.

## <a name="download-the-template-using-the-portal"></a>Downloaden Sie die Vorlage über das portal

1. Auf der [Azure-Portal](https://portal.azure.com/)anmelden.
2. Hub-Menü Wählen Sie **virtuellen Maschinen**.
3. Wählen Sie den virtuellen Computer aus.
5. **Automatisierungsskript**auswählen
6. Wählen Sie **herunterladen** und speichern Sie die ZIP-Datei auf dem lokalen Computer.
7. Öffnen Sie die ZIP-Datei und extrahieren Sie die Dateien in einen Ordner. Die ZIP-Datei enthält:
    
    - Deploy.ps1
    - Deploy.sh 
    - deployer.RB
    - DeploymentHelper.cs
    - Parameters.JSON
    - Template.JSON

Die JSON-Datei ist die Vorlage.
    
## <a name="download-the-template-using-powershell"></a>Downloaden Sie die Vorlage mithilfe von PowerShell

Sie können auch mithilfe des [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) -Cmdlet JSON-Vorlagendatei. Sie können die `-path` Parameter Dateiname und Pfad für die JSON-Datei anzugeben. In diesem Beispiel wird veranschaulicht, wie Vorlagen für die Ressourcengruppe mit dem Namen **MyResourceGroup** in den Ordner **C:\users\public\downloads** auf dem lokalen Computer.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Bereitstellung von Ressourcen mithilfe von Vorlagen finden Sie unter [Exemplarische Vorgehensweise Ressourcenmanager Vorlage](../resource-manager-template-walkthrough.md).