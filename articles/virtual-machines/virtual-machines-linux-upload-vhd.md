<properties
    pageTitle="Erstellen und Hochladen einer Linux-Abbilds | Microsoft Azure"
    description="Erstellen und Hochladen einer virtuellen Festplatte (VHD) in Azure mit einer benutzerdefinierten Linux mit dem Ressourcen-Manager-Bereitstellungsmodell."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

# <a name="upload-and-create-a-linux-vm-from-custom-disk-image"></a>Uploaden und Linux VM von benutzerdefinierten Image Erstellen

Dieser Artikel beschreibt, wie eine virtuelle Festplatte (VHD) mit dem Ressourcen-Manager-Bereitstellungsmodell Azure hochladen und Linux VMs aus diesem benutzerdefiniertes Bild erstellen. Diese Funktionalität ermöglicht das Installieren und konfigurieren eine Linux-Distribution an Ihre Bedürfnisse verwenden diese VHD zum schnellen Azure virtuelle Maschinen (VMs) erstellen.

## <a name="quick-commands"></a>Schnellzugriff
Möchten Sie schnell, die folgenden Abschnitt Details die Base Aufgabe Befehle eine VM in Azure hochladen. Ausführliche Informationen und Kontext für jeden Schritt können den Rest des Dokuments [hier](#requirements)gefunden.

Achten Sie [Azure-CLI](../xplat-cli-install.md) angemeldet und Ressourcen-Manager-Modus:

```bash
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Parameternamen wird durch Ihre eigenen Werte. Beispiel-Parameternamen enthalten `myResourceGroup`, `mystorageaccount`, und `myimages`.

Erstellen Sie zunächst eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUs` Speicherort:

```bash
azure group create myResourceGroup --location "WestUS"
```

Erstellen Sie ein Speicherkonto Ihre virtuellen Laufwerke enthält. Im folgenden Beispiel wird ein Speicherkonto namens `mystorageaccount`:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Liste der Zugriffstasten für das Speicherkonto. Notieren Sie `key1`:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Erstellen Sie einen Container innerhalb Ihres Speicherkontos Taste Speicher, die Sie erhalten. Im folgenden Beispiel wird einen Container namens `myimages` mit Speicher-Schlüsselwert aus `key1`:

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Laden Sie schließlich Ihre VHD erstellten Container. Geben Sie den lokalen Pfad auf der virtuellen Festplatte unter `/path/to/disk/mydisk.vhd`:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Sie können nun eine VM aus der hochgeladenen Laufwerk [Ressourcenmanager Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd)erstellen. Sie können auch die CLI durch Angabe des URI auf der Festplatte (`--image-urn`). Im folgenden Beispiel wird einen virtueller Computer mit dem Namen `myVM` mit dem virtuellen Laufwerk bereits hochgeladen:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

Storage Konto muss identisch, in dem virtuellen Datenträger hochgeladen. Müssen Sie auch angeben oder Antwort aufgefordert, alle weiteren erforderlichen Parameter der `azure vm create` Befehl wie virtuelle Netzwerke, öffentliche IP-Adresse, Benutzername und SSH-Schlüssel. Erfahren Sie mehr über die [CLI Ressourcenmanager Parameter](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Vorschriften
Die folgenden Schritte müssen wie folgt vor:

- **Linux-Betriebssystem in einer VHD-Datei installiert** - Installieren einer [Azure unterstützt Linux-Distribution](virtual-machines-linux-endorsed-distros.md) (oder Informationen [für die Verteilung nicht unterstützt](virtual-machines-linux-create-upload-generic.md)) auf einer virtuellen Festplatte in das VHD-Format. Mehrere Tools sind eine VM und VHD erstellen:
    - Installieren und Konfigurieren von [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) oder [KVM](http://www.linux-kvm.org/page/RunningKVM)zu VHD als Ihr Bildformat verwenden. Bei Bedarf können Sie [ein Bild konvertieren](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) mit `qemu-img convert`.
    - Sie können auch Hyper-V [auf Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) oder [Windows Server 2012-2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Neuere VHDX-Format wird in Azure nicht unterstützt. Wenn Sie einen virtuellen Computer erstellen, geben Sie VHD Format. Bei Bedarf können VHDX Festplatten mit VHD konvertieren [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) oder [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-Cmdlet. Darüber hinaus unterstützt Azure nicht dynamische Festplatten hochladen, müssen Sie vor dem Hochladen statische VHDs solche Datenträger konvertieren. Tools wie [Azure VHD Dienstprogramme gehen](https://github.com/Microsoft/azure-vhd-utils-for-go) können dynamische Datenträger bei Azure hochladen konvertieren.

- VMs erstellt das benutzerdefinierte Abbild müssen in demselben Speicherkonto als das Bild selbst befinden.
    - Erstellen Sie ein Speicherkonto und Container zu Ihr benutzerdefiniertes Bild und erstellten VMs
    - Nachdem Sie Ihre virtuellen Computer erstellt haben, können Sie sicher das Bild löschen

Achten Sie [Azure-CLI](../xplat-cli-install.md) angemeldet und Ressourcen-Manager-Modus:

```bash
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Parameternamen wird durch Ihre eigenen Werte. Beispiel-Parameternamen enthalten `myResourceGroup`, `mystorageaccount`, und `myimages`.


<a id="prepimage"> </a>
## <a name="prepare-the-image-to-be-uploaded"></a>Vorbereiten des Abbilds hochgeladen werden.

Azure unterstützt verschiedene Linux-Distributionen (siehe [Distributionen unterstützt](virtual-machines-linux-endorsed-distros.md)). Die folgenden Artikel begleiten Sie verschiedene Linux-Distributionen vorbereiten, die in Azure unterstützt werden:

- **[Verteilung von centOS](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Andere - Distributionen nicht unterstützt](virtual-machines-linux-create-upload-generic.md)**

Außerdem finden Sie **[Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** allgemeine Tipps zur Linux-Bilder für Azure.

> [AZURE.NOTE] Der [Azure-Plattform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gilt für VMs mit Linux erst gebilligten Distributionen mit Konfigurationsdetails gemäß 'unterstützte Versionen" [Linux auf Azure-Endorsed Verteilung](virtual-machines-linux-endorsed-distros.md)verwendet wird.


## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Ressourcengruppen zusammenstellen logisch Azure Ressourcen zur Unterstützung der virtuelle Computer, virtuelle Netzwerke und Speicher. Weitere Informationen über [hier Azure Ressourcengruppen](../azure-resource-manager/resource-group-overview.md). Bevor das benutzerdefinierte Festplattenabbild hochladen und VMs erstellen, müssen Sie zuerst eine Ressourcengruppe erstellen. 

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUS` Speicherort:

```bash
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Ein Speicherkonto erstellen
VMs werden als Seitenblobs in ein Speicherkonto gespeichert. Weitere Informationen über [hier Azure BLOB-Speicher](../storage/storage-introduction.md#blob-storage). Sie erstellen ein Speicherkonto für benutzerdefinierte Datenträgerabbild und VMs. Von benutzerdefinierten Image Erstellen VMs müssen in demselben Speicherkonto als das Abbild.

Im folgenden Beispiel wird ein Speicherkonto namens `mystorageaccount` in der zuvor erstellten Ressourcengruppe:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Speicherkontoschlüssel Liste
Azure generiert zwei 512-Bit-Zugriffstasten für jedes Storage-Konto. Zugriffstasten wird bei Storage-Konto wie Schreibvorgänge durchzuführen. Mehr über das [Verwalten des Zugriffs auf Speicher hier](../storage/storage-create-storage-account.md#manage-your-storage-account). Zugriffstasten mit Anzeigen der `azure storage account keys list` Befehl.

Zeigen Sie die Zugriffstasten für das Speicherkonto erstellten an:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Die Ausgabe ähnelt:

```
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
Notieren Sie `key1` es verwendet das Speicherkonto in den nächsten Schritten interagieren.

## <a name="create-a-storage-container"></a>Erstellen eines
Dabei, die verschiedene Verzeichnissen logisch organisieren im lokalen Dateisystem erstellen, erstellen Sie Container ein Speicherkonto auf Ihre virtuellen Laufwerke und Bilder. Ein Speicherkonto kann eine beliebige Anzahl von Containern enthalten. 

Im folgenden Beispiel wird einen Container namens `myimages`, Angabe der Zugriffstaste im vorherigen Schritt abgerufen (`key1`):

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>Virtuelle Festplatte hochladen
Jetzt können Sie Ihre benutzerdefinierten Datenträgerabbild tatsächlich hochladen. Wie alle virtuellen Laufwerke von VMs verwendet Sie hochladen und speichern das benutzerdefinierte Festplattenabbild als Seitenblob.

Geben Sie die Zugriffstaste im vorherigen Schritt und den Pfad der benutzerdefinierten Festplattenabbild auf dem lokalen Computer erstellten Container an:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Benutzerdefiniertes Bild VM erstellen
Beim Erstellen von VMs von benutzerdefinierten Image Geben Sie den URI in der Abbildung. Stellen Sie sicher, dass der Speicherort der benutzerdefinierten Datenträgerabbild Ziel Storage Konto entspricht. Sie können die VM mit der Azure-CLI oder Ressourcenmanager JSON-Vorlage erstellen.


### <a name="create-a-vm-using-the-azure-cli"></a>Erstellen Sie einen virtuellen Computer mit der Azure-CLI
Geben Sie die `--image-urn` mit der `azure vm create` Befehl benutzerdefinierte Festplattenabbild auf. Sicherstellen, dass `--storage-account-name` das Speicherort der benutzerdefinierten Datenträgerabbild Speicherkonto entspricht. Sie haben nicht den gleichen Container als benutzerdefinierte Datenträgerabbild verwenden, um Ihre virtuellen Computer speichern. Sollten Sie zusätzliche Container wie die früheren Schritte vor dem Hochladen benutzerdefinierter Datenträgerabbilder erstellen.

Im folgenden Beispiel wird einen virtueller Computer mit dem Namen `myVM` von benutzerdefinierten Image:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Sie müssen angeben oder Antwort aufgefordert, alle weiteren Parameter erforderlich die `azure vm create` Befehl wie virtuelle Netzwerke, öffentliche IP-Adresse, Benutzername und SSH-Schlüssel. Weitere Informationen über die [CLI Ressourcenmanager Parameter](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Erstellen Sie einen virtuellen Computer mit einer JSON-Vorlage
Azure Ressourcenmanager Vorlagen sind JavaScript Object Notation (JSON)-Dateien, die die Umgebung definieren, die Sie erstellen möchten. Die Vorlagen sind in verschiedenen Anbietern wie Compute oder unterteilt. Sie können Vorlagen verwenden oder eigene. Weitere Informationen über [Ressourcen-Manager und Vorlagen](../azure-resource-manager/resource-group-overview.md).

In der `Microsoft.Compute/virtualMachines` Anbieter der Vorlage haben eine `storageProfile` Knoten, die Konfigurationsdetails für Ihre virtuellen Computer enthält. Die zwei wichtigsten Parameter Bearbeiten der `image` und `vhd` URIs, die Ihre benutzerdefinierten Datenträgerabbild zu Ihrer neuen VM-Laufwerk verweisen. Folgende zeigt ein Beispiel für die Verwendung einer benutzerdefinierten Image JSON:

```bash
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Mit [dieser Vorlage erstellen Sie einen virtuellen Computer aus ein benutzerdefiniertes Bild](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) oder [Erstellen eigener Vorlagen Azure Ressourcenmanager](../resource-group-authoring-templates.md)lesen. 

Haben Sie eine Vorlage erstellen Ihre VMs mit den `azure group deployment create` Befehl. Geben Sie den URI der JSON-Vorlage mit der `--template-uri` Parameter:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Wenn JSON-Datei lokal auf Ihrem Computer gespeichert haben, können Sie die `--template-file` Parameter stattdessen:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie vorbereitet und benutzerdefinierte virtuelle Festplatte hochladen, mehr über [Ressourcen-Manager und Vorlagen](../azure-resource-manager/resource-group-overview.md). Auch kann [einen Datenträger](virtual-machines-linux-add-disk.md) Hinzufügen neuer VMs möchten. Haben Sie Programme auf Ihre VMs zugreifen müssen, müssen Sie [Ports und Endpunkte zu öffnen](virtual-machines-linux-nsg-quickstart.md).