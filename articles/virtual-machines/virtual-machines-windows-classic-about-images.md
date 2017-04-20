<properties
    pageTitle="Bilder für virtuelle Windows-Maschinen | Microsoft Azure"
    description="Erfahren Sie, wie Bilder mit Windows virtuelle Computer in Azure verwendet werden."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="about-images-for-windows-virtual-machines"></a>Bilder für virtuelle Windows-Maschinen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]



## <a name="working-with-images"></a>Arbeiten mit Bildern

Azure PowerShell-Modul können Sie die Bilder zum Azure Abonnement verwalten. Auch können Azure-Verwaltungsportal für einige Aufgaben Bild, aber die Befehlszeile bietet mehr Optionen.


-   **Alle Bilder**:`Get-AzureVMImage`gibt eine Liste aller Bilder in Ihrem aktuellen Abonnement: Bilder sowie von Azure oder Partner. Dies bedeutet, dass Sie eine umfangreiche Liste erhalten. Die nächsten Beispiele zeigen, wie eine kürzere Liste zu.
-   **Bild Familien**:`Get-AzureVMImage | select ImageFamily` Ruft eine Liste von Bild Familien von Zeichenfolgen **ImageFamily** -Eigenschaft angezeigt.
-   **Alle Bilder in einer bestimmten Produktfamilie**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
-   **VM-Images finden**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` dies funktioniert, indem die DataDiskConfiguration-Eigenschaft nur für VM Bilder gilt filtern. In diesem Beispiel filtert auch die Ausgabe auf nur die Bezeichnung und Bild.
-   **Ein generalisiertes Abbild speichern**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
-   **Ein spezialisiertes Abbild speichern**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
>[Azure.Tip] Der Parameter OSState ist einem VM-Image enthält Datenträger sowie die Betriebssystem-CD erstellen möchten. Wenn den Parameter nicht verwenden, erstellt das Cmdlet ein Betriebssystemabbild. Der Wert des Parameters gibt an, ob allgemeine oder spezielle Bild basierend auf, ob Betriebssystem-Datenträger für die erneute Verwendung vorbereitet wurde.
-   **Löschen eines Bilds**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`


## <a name="next-steps"></a>Nächste Schritte

Sie können auch [einen Windows-Computer mit dem Verwaltungsportal erstellen](virtual-machines-windows-classic-tutorial.md)

