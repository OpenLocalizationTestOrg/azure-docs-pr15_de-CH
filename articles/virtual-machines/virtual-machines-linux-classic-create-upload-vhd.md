<properties
    pageTitle="Erstellen und Hochladen einer VHD Linux | Microsoft Azure"
    description="Erstellen und Hochladen von Azure virtuelle Festplatte (VHD) mit dem klassischen Bereitstellungsmodell, das Linux-Betriebssystem enthält."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Erstellen und Hochladen einer virtuellen Festplatte mit Linux-Betriebssystem

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Sie können auch [eine benutzerdefinierte Image mit Azure Resource Manager hochladen](virtual-machines-linux-upload-vhd.md).

In diesem Artikel wird das Erstellen und eine virtuelle Festplatte (VHD) hochladen, damit Ihre eigene zum Erstellen von virtuellen Computern in Azure Verwendung veranschaulicht. Erfahren Sie, wie das Betriebssystem vorbereiten, damit Verwendung zum Erstellen von mehreren virtuellen Computern das Bild. 

>  [AZURE.NOTE] Haben Sie einen Moment, Hilfe zu Azure Linux VM-Dokumentation mit dieser [Kurzübersicht](https://aka.ms/linuxdocsurvey) Erfahrungen. Jede Antwort kann wir Ihnen helfen, Ihre Arbeit.

## <a name="prerequisites"></a>Erforderliche Komponenten
Es wird vorausgesetzt, dass Sie über Folgendes verfügen:

- **Linux-Betriebssystem in einer VHD-Datei** – Sie eine [Azure unterstützt Linux-Distribution](virtual-machines-linux-endorsed-distros.md) installiert haben (oder Informationen [für die Verteilung nicht unterstützt](virtual-machines-linux-create-upload-generic.md)) auf einer virtuellen Festplatte in das VHD-Format. Mehrere Tools sind eine VM und VHD erstellen:
    - Installieren und Konfigurieren von [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) oder [KVM](http://www.linux-kvm.org/page/RunningKVM)zu VHD als Ihr Bildformat verwenden. Bei Bedarf können Sie [ein Bild konvertieren](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) mit `qemu-img convert`.
    - Sie können auch Hyper-V [auf Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) oder [Windows Server 2012-2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Neuere VHDX-Format wird in Azure nicht unterstützt. Wenn Sie einen virtuellen Computer erstellen, geben Sie VHD Format. Bei Bedarf können VHDX Festplatten mit VHD konvertieren [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) oder [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-Cmdlet. Darüber hinaus unterstützt Azure nicht dynamische Festplatten hochladen, müssen Sie vor dem Hochladen statische VHDs solche Datenträger konvertieren. Tools wie [Azure VHD Dienstprogramme gehen](https://github.com/Microsoft/azure-vhd-utils-for-go) können dynamische Datenträger bei Azure hochladen konvertieren.

- **Azure-Befehlszeilenschnittstelle** - Installieren der neuesten [Azure-Befehlszeilenschnittstelle](../virtual-machines-command-line-tools.md) die virtuelle Festplatte hochladen.

<a id="prepimage"> </a>
## <a name="step-1-prepare-the-image-to-be-uploaded"></a>Schritt 1: Vorbereiten des Bilds hochgeladen werden

Azure unterstützt verschiedene Linux-Distributionen (siehe [Distributionen unterstützt](virtual-machines-linux-endorsed-distros.md)). Die folgenden Artikel begleiten Sie verschiedene Linux-Distributionen vorbereiten, die in Azure unterstützt werden. Nachdem Sie die Schritte in den folgenden Handbüchern kommen Sie haben Sie eine VHD-Datei, die in Azure hochladen:

- **[Verteilung von centOS](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Andere - Distributionen nicht unterstützt](virtual-machines-linux-create-upload-generic.md)**

> [AZURE.NOTE] Der Azure-Plattform gilt SLA für das Linux-Betriebssystem virtuellen Computern erst gebilligten Distributionen mit Konfigurationsdetails gemäß 'unterstützte Versionen" [Linux auf Azure-Endorsed Verteilung](virtual-machines-linux-endorsed-distros.md)verwendet wird. Alle Linux-Distributionen in Azure Bildergalerie sind gebilligt Distributionen mit der erforderlichen Konfiguration.

Außerdem finden Sie **[Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** allgemeine Tipps zur Linux-Bilder für Azure.


<a id="connect"> </a>
## <a name="step-2-prepare-the-connection-to-azure"></a>Schritt 2: Vorbereiten der Verbindungs zu Azure

Sicherstellen der Azure-CLI im klassischen Bereitstellungsmodell verwenden (`azure config mode asm`), melden Sie sich Ihrem Konto:

```
azure login
```


<a id="upload"> </a>
## <a name="step-3-upload-the-image-to-azure"></a>Schritt 3: Laden Sie das Bild in Azure

Sie benötigen ein Speicherkonto die VHD-Datei hochladen. Sie können entweder ein Storage-Konto oder [eine neue erstellen](../storage/storage-create-storage-account.md)auswählen.

Verwenden der Azure-CLI auf dem Bild mit dem folgenden Befehl:

```bash
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

Im vorherigen Beispiel:

- **BlobStorageURL** ist die URL für das Speicherkonto, das Sie verwenden möchten
- **YourImagesFolder** ist der Container im BLOB-Speicher, wo Sie die Bilder speichern möchten
- **VHDName** ist die Beschriftung im Portal zu der virtuellen Festplatte.
- **PathToVHDFile** ist der vollständige Pfad und Name der VHD-Datei auf Ihrem Computer.

Im folgenden finden ein vollständiges Beispiel:

```bash
azure vm image create UbuntuLTS `
    --blob-url https://teststorage.blob.core.windows.net/vhds/UbuntuLTS.vhd `
    --os Linux /home/ahmet/UbuntuLTS.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>Schritt 4: Erstellen einer VM aus dem Bild
Sie erstellen eine VM `azure vm create` wie reguläre VM. Geben Sie den Namen gab Ihr Bild im vorherigen Schritt. Im folgenden Beispiel verwenden wir im vorherigen Schritt Bezeichnung **UbuntuLTS** Bild:

```bash
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "DeployedUbuntu" UbuntuLTS
```

Erstellen Sie eigene VMs bieten Sie eigene Benutzername + Kennwort Speicherort, DNS-Namen und Image-Name.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in [Azure CLI Referenz Azure klassischen Bereitstellungsmodell](../virtual-machines-command-line-tools.md).

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload
