<properties
    pageTitle="Erstellen Sie eine spezielle VM in Azure | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer Kopie einer speziellen Windows VM in der Ressourcen-Manager-Bereitstellungsmodell in Azure ausgeführt."
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
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
# <a name="create-a-copy-of-a-specialized-windows-vm-running-in-azure"></a>Erstellen Sie eine Kopie einer speziellen Windows-VM in Azure ausgeführt 

Dieser Artikel veranschaulicht das AZCopy-Tool verwenden, um eine Kopie der virtuellen Festplatte aus einer speziellen Windows-VM zu erstellen, die in Azure ausgeführt wird. Die Kopie der virtuellen Festplatte können Sie einen neuen virtuellen Computer erstellen. 

- Wenn zum Kopieren einer allgemeinen VM finden Sie unter [VM-Image aus einer vorhandenen allgemeinen Azure VM erstellen](virtual-machines-windows-capture-image.md).

- Wenn Sie eine virtuelle Festplatte eine lokale VM wie einem Hyper-V, [Hochladen der virtuellen Festplatte Windows eine lokale VM in Azure](virtual-machines-windows-upload-image.md)Siehe hochladen möchten.


## <a name="before-you-begin"></a>Bevor Sie beginnen

Stellen Sie sicher, dass Sie:

- Verfügen Sie Informationen über die **Quelle und das Ziel Speicherkonten**. VM Quelle müssen Sie auf Storage-Konto und Container. Der Containername werden gewöhnlich **Vhds**. Sie müssen auch ein Speicherkonto Ziel. Wenn nicht bereits vorhanden, können Sie mit dem Portal erstellen (**Weitere Dienste** > Speicherkonten > hinzufügen) oder mit dem [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) -Cmdlet. 

- Azure [PowerShell 1.0](../powershell-install-configure.md) haben (oder höher) installiert.

- Heruntergeladen und installiert [AzCopy Tool](../storage/storage-use-azcopy.md). 


## <a name="deallocate-the-vm"></a>Freigeben der VM

Freigeben der VM frei auf der virtuellen Festplatte kopiert werden. 

- **Portal**: Klicken Sie auf **virtuellen Maschinen** > **MyVM** > Beenden
- **PowerShell**: `Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM` mit dem Namen **"MyVM"** in Ressource Gruppe **MyResourceGroup**VM freigegeben.

Der **Status** der VM in Azure-Portal wird von **beendet** zu **beendet (freigegeben)**.


## <a name="get-the-storage-account-urls"></a>Storage-Konto URLs abrufen

Sie benötigen die URLs der Quell- und Speicherkonten. Die URLs aussehen: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Wenn der Speicher Konto und Container Name schon können Sie nur die Informationen zwischen den Klammern den URL erstellt ersetzen. 

Azure-Portal oder Azure Powershell können Sie die URL:

- **Portal**: Klicken Sie auf **Weitere Dienste** > **Speicherkonten**  >  <storage account> **Blobs** und VHD Quelldatei im Container **Vhds** liegt. Klicken Sie auf **Eigenschaften** für den Container, und kopieren Sie den Text mit der Bezeichnung **URL**. Die URLs von Quelle und Ziel-Container benötigen. 

- **PowerShell**: `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` VM mit dem Namen **"MyVM"** in Ressource Gruppe **MyResourceGroup**informiert. Die Ergebnisse suchen Sie der **Vhd Uri**im Bereich **Speicher-Profil** . Der erste Teil des URIs ist der URL zum Container und des letzten Teils der OS VHD Name für den virtuellen Computer.

## <a name="get-the-storage-access-keys"></a>Zugriffstasten Speicher erhalten

Finden Sie die Zugriffstasten für Quelle und Ziel Speicherkonten. Weitere Informationen zu Zugriffstasten finden Sie unter [über Azure-Speicherkonten](../storage/storage-create-storage-account.md).

- **Portal**: Klicken Sie auf **Weitere Dienste** > **Speicherkonten**  >  <storage account> **Alle** > **Zugriffstasten**. Kopieren Sie den Schlüssel als **key1**.
- **PowerShell**: `Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup` ruft Speicherschlüssel Storage-Konto **Mystorageaccount** in Ressource Gruppe **MyResourceGroup**. Kopieren Sie den Schlüssel als **key1**.


## <a name="copy-the-vhd"></a>Kopieren Sie die VHD-Datei 

Sie können Dateien zwischen Speicherkonten mit AzCopy. Der Zielcontainer wenn angegebene Container vorhanden ist, wird es für Sie erstellt werden. 

AzCopy, öffnen Sie ein Eingabeaufforderungsfenster auf dem lokalen Computer und den Ordner, in dem AzCopy installiert ist. Wie *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*werden. 

Um alle Dateien in einem Container zu kopieren, verwenden Sie **die Befehlszeilenoption** . Hiermit können VHD OS und alle Festplatten mit den Daten im selben Container sind kopieren. Dieses Beispiel zeigt, wie alle Dateien in der Container- **Mysourcecontainer** im Speicher Konto **Mysourcestorageaccount** in den Container **Mydestinationcontainer** im Speicherkonto **Mydestinationstorageaccount** kopieren. Ersetzen Sie die Namen der Speicherkonten und Container durch Ihren eigenen. Ersetzen Sie `<sourceStorageAccountKey1>` und `<destinationStorageAccountKey1>` mit Ihrem eigenen Schlüssel.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Wenn Sie eine bestimmte virtuelle Festplatte mit mehreren Dateien in einem Container kopieren möchten, können Sie auch den Dateinamen /Pattern Switch. In diesem Beispiel wird nur die Datei **myFileName.vhd** kopiert werden.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
        /Pattern:myFileName.vhd
```


Wenn er abgeschlossen ist, erhalten Sie eine Meldung, die etwa so aussieht:

```
  Finished 2 of total 2 file(s).
  [2016/10/07 17:37:41] Transfer summary:
  -----------------
  Total files transferred: 2
  Transfer successfully:   2
  Transfer skipped:        0
  Transfer failed:         0
  Elapsed time:            00.00:13:07
```

## <a name="troubleshooting"></a>Problembehandlung

- Wenn Sie AZCopy, verwenden, wenn Sie die Fehlermeldung "Server konnte die Anforderung authentifiziert. Stellen Sie sicher, dass der Wert der Authorization-Header korrekt unterschreiben gebildet wird." und Sie Schlüssel 2 oder sekundäre Speicherschlüssel Primär- oder 1. Speicherschlüssel verwenden.


## <a name="next-steps"></a>Nächste Schritte

- Sie können einen neuen virtuellen Computer durch [die Kopie der virtuellen Festplatte einer VM als OS Festplatte](virtual-machines-windows-create-vm-specialized.md)erstellen.












