<properties
    pageTitle="Hochladen eines Windows VHD für Ressourcenmanager | Microsoft Azure"
    description="Informationen Sie zum Hochladen einer virtuellen Windows-Maschine VHD lokal in Azure mit dem Ressourcen-Manager-Bereitstellungsmodell. Sie können eine virtuelle Festplatte aus entweder eine allgemeine oder spezialisierte VM hochladen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="upload-a-windows-vhd-from-an-on-premises-vm-to-azure"></a>Hochladen Sie Windows virtuelle Festplatte in eine lokale VM in Azure 


Dieser Artikel beschreibt, wie erstellen und Hochladen einer Windows virtuellen Festplatte (VHD) beim Erstellen einer Vm Azure verwendet werden. Sie können eine virtuelle Festplatte eine verallgemeinerte VM oder spezielle VM hochladen. 

Weitere Informationen zu Festplatten und Festplatten in Azure finden Sie unter [Festplatten und Festplatten für virtuelle Computer](virtual-machines-linux-about-disks-vhds.md).


## <a name="prepare-the-vm"></a>Vorbereiten der VM 

Sie können allgemeine und spezialisierte VHDs in Azure hochladen. Jeder Typ erfordert, dass die VM vor vorbereiten.

- **Generalisierte VHD** - allgemeine VHD hat alle Ihre persönlichen Kontoinformationen mit Sysprep entfernt. Wenn Sie die virtuelle Festplatte als Bild verwenden, um neue VMs aus erstellen möchten, sollten Sie:
    - [Vorbereiten einer virtuellen Windows Azure hochladen](virtual-machines-windows-prepare-for-upload-vhd-image.md). 
    - [Verallgemeinern der virtuelle Computer mithilfe von Sysprep](virtual-machines-windows-generalize-vhd.md). 

- **Spezielle VHD** - spezielle VHD verwaltet Benutzerkonten, Applikationen und anderen Zustandsdaten aus Ihrer ursprünglichen VM. Wenn Sie die virtuelle Festplatte als verwenden möchten-einen neuen virtuellen Computer erstellen, stellen Sie sicher, die folgenden Schritte ausgeführt werden. 
    - [Vorbereiten einer virtuellen Windows Azure hochladen](virtual-machines-windows-prepare-for-upload-vhd-image.md). **Nicht** generalize die VM mit Sysprep.
    - Entfernen Sie Gast Virtualisierungs-Tools und des virtuellen Computers (d. h. VMware Tools) installierten Agents.
    - Sicherstellen Sie, dass die VM ziehen Sie seine IP-Adresse und DNS-Clienteinstellungen über DHCP konfiguriert. Dadurch erhält der Server eine IP-Adresse in das VNet beim Starten. 

## <a name="log-in-to-azure"></a>Melden Sie sich bei Azure

Wenn Sie bereits über PowerShell Version 1.4 oder höher installiert ist, lesen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)haben.

1. Azure PowerShell öffnen und Ihre Azure-Konto anmelden. Popup-Fenster für Ihre Azure-Konto-Anmeldeinformationen eingeben.

    ```powershell
    Login-AzureRmAccount
    ```


2. Erhalten Sie das Abonnement-IDs für Abonnements verfügbar.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Legen Sie das richtige Abonnement mit der Abonnement-ID. Ersetzen Sie `<subscriptionID>` mit der ID das richtige Abonnement.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>Das Speicherkonto abrufen

Sie benötigen ein Speicherkonto in Azure hochgeladene VM-Image zu speichern. Sie können ein vorhandenes Speicherkonto verwenden oder eine neue erstellen. 

Um Speicherplatz Konten anzuzeigen, geben Sie Folgendes ein:

```powershell
Get-AzureRmStorageAccount
```

Wenn Sie ein vorhandenes Speicherkonto verwenden möchten, gehen Sie zum Abschnitt [VM-Image hochladen](#upload-the-vm-vhd-to-your-storage-account) .

Benötigen Sie ein Speicherkonto erstellen, gehen Sie folgendermaßen vor:

1. Sie benötigen den Namen der Ressourcengruppe, in dem das Speicherkonto erstellt werden soll. Alle Ressourcengruppen in Ihrem Abonnement feststellen, geben Sie Folgendes ein:

    ```powershell
    Get-AzureRmResourceGroup
    ```

Erstellen Sie eine Ressourcengruppe mit dem Namen **MyResourceGroup** in der Region **West USA** Geben Sie Folgendes ein:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Erstellen Sie ein Speicherkonto namens **Mystorageaccount** in dieser Ressourcengruppe mit dem [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) -Cmdlet:

    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" -SkuName "Standard_LRS" -Kind "Storage"
    ```
            
    Gültige Werte für - SkuName

    - **Standard_LRS** - lokal redundanter Speicher. 
    - **Standard_ZRS** - Zone redundanten Speicher.
    - **Standard_GRS** - Geo redundanten Speicher. 
    - **Standard_RAGRS** - Lesezugriff Geo redundanten Speicher. 
    - **Premium_LRS** - Premium lokal redundanter Speicher. 



## <a name="upload-the-vhd-to-your-storage-account"></a>Die virtuelle Festplatte auf das Speicherkonto hochladen

Verwenden Sie [Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) -Cmdlet, das Bild zu einem Container das Speicherkonto hochzuladen. In diesem Beispiel lädt die Datei **myVHD.vhd** von `"C:\Users\Public\Documents\Virtual hard disks\"` ein Konto namens **Mystorageaccount** in der Ressourcengruppe **MyResourceGroup** . Die Datei in den Schlüsselcontainer **Mycontainer** abgelegt und der Name werden **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Wenn erfolgreich, erhalten Sie eine Antwort, die etwa so aussieht:

```
  C:\> Add-AzureRmVhd -ResourceGroupName myResourceGroup -Destination https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
  MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
  MD5 hash calculation is completed.
  Elapsed time for the operation: 00:03:35
  Creating new page blob of size 53687091712...
  Elapsed time for upload: 01:12:49

  LocalFilePath           DestinationUri
  -------------           --------------
  C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Die Verbindung und der Größe der VHD-Datei kann dieser Befehl eine Weile dauern


## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer VM in Azure über allgemeine VHD](virtual-machines-windows-create-vm-generalized.md)
- [Erstellen einer VM in Azure aus einer speziellen virtuellen](virtual-machines-windows-create-vm-specialized.md) von als OS Festplatte anfügen, wenn Sie einen neuen virtuellen Computer erstellen.


