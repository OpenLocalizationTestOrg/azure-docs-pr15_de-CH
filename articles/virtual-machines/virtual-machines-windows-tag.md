<properties
   pageTitle="Wie eine VM Tag | Microsoft Azure"
   description="Erfahren Sie mehr über die Kennzeichnung von eines Windows-Computers mit dem Ressourcen-Manager-Bereitstellungsmodell Azure erstellt"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Wie Sie einen Windows-Computer in Azure tag


Dieser Artikel beschreibt verschiedene Methoden zum virtuellen Windows-Maschine in Azure durch den Ressourcen-Manager-Bereitstellungsmodell tag. Tags sind benutzerdefinierte Schlüssel-Wert-Paare direkt auf eine Ressource oder eine Ressourcengruppe platziert werden können. Azure unterstützt derzeit bis zu 15 Kategorien pro Ressource und die Ressourcengruppe. Tags für eine Ressource angelegt, zum Zeitpunkt der Erstellung oder einer vorhandenen Ressource hinzugefügt. Beachten Sie, dass Tags für Ressourcen über den Ressourcen-Manager-Bereitstellungsmodell nur unterstützt werden. Ggf. eine virtuellen Linux-Maschine tag finden Sie unter [einem virtuellen Linux-Maschine in Azure tag](virtual-machines-linux-tag.md).

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>PowerShell Tagging

Erstellen, hinzufügen und Löschen von Tags über PowerShell, müssen Sie zunächst die [PowerShell-Umgebung mit Azure-Ressourcen-Manager][]einrichten. Nach Abschluss von Setup können Sie Tags Computing-, Netzwerk- und Speicher-Ressourcen bei der Erstellung oder nach dem Erstellen die Ressource über PowerShell platzieren. Dieser Artikel konzentriert sich auf Anzeigen/Bearbeiten von Tags auf virtuellen Computern gespeichert.

Navigieren Sie zuerst zu einer virtuellen Maschine über die `Get-AzureRmVM` Cmdlet.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Wenn Ihr virtueller Computer bereits Tags enthält, sehen Sie dann alle Tags der Ressource:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Wenn Sie Tags über PowerShell hinzufügen möchten, können Sie die `Set-AzureRmResource` Befehl. Beachten Sie beim Aktualisieren von Tags über PowerShell Tags als Ganzes aktualisiert werden. Wenn Sie ein Tag auf eine Ressource, die bereits Tags hinzufügen, müssen Sie daher alle Tags enthalten, die auf die Ressource platziert werden soll. Es folgt ein Beispiel für eine Ressource über PowerShell-Cmdlets zusätzliche Tags hinzufügen.

Dieses erste Cmdlet legt alle Tags platziert auf *MyTestVM* der Variablen *$tags* mit der `Get-AzureRmResource` und `Tags` Eigenschaft.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

Der zweite Befehl zeigt die Tags für die angegebene Variable.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment

Dritte Befehl hinzugefügt Variable *$tags* zusätzliche Tag. Beachten Sie die **+=** , *$tags* Liste neue Schlüssel-Wert-Paar hinzufügen.

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

Der vierte Befehl legt alle Tags in der Variablen *$tags* der angegebenen Ressource definiert. In diesem Fall ist MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Der fünfte Befehl zeigt alle Tags für die Ressource. Wie Sie sehen, wird *Standort* jetzt als Tag mit *meinStandort* als Wert definiert.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment
        Value       MyLocation
        Name        Location

Weitere Informationen über PowerShell tagging, checken Sie [Azure Ressource Cmdlets][].

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Nächste Schritte

* Über tagging Azure Ressourcen finden Sie unter [Azure Ressourcenmanager Übersicht][] und [Tags Azure Ressourcen zu verwenden][].
* Tags Verwalten von Azure Ressourcen helfen Sie finden Sie [Ihre Azure-Rechnung Verständnis][] und [Einblick in Ihre Microsoft Azure Ressourcenverbrauch][].

[PowerShell-Umgebung mit Azure-Ressourcen-Manager]: ../powershell-azure-resource-manager.md
[Azure Ressource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure-Ressourcen-Manager (Übersicht)]: ../azure-resource-manager/resource-group-overview.md
[Mithilfe von Tags Organisieren Ihrer Azure-Ressourcen]: ../resource-group-using-tags.md
[Verstehen Ihre Azure-Rechnung]: ../billing/billing-understand-your-bill.md
[Einblicke in Ihre Microsoft Azure Ressourcenverbrauch]: ../billing-usage-rate-card-overview.md
