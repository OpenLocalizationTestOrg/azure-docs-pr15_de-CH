<properties
    pageTitle="Verschiedene Verfahren zum Erstellen einer Linux VM | Microsoft Azure"
    description="Lernen Sie die verschiedenen Methoden zum Erstellen einer virtuellen Linux-Maschine auf Azure, einschließlich Links zu Tools und Lernprogrammen für jede Methode."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Verschiedene Verfahren zum Erstellen einer virtuellen Linux-Maschine in Azure

Sie haben die Möglichkeit, in Azure eine virtuellen Linux-Maschine (VM) Tools und bequem Sie Workflows erstellen. Dieser Artikel beschreibt diese Unterschiede und Beispiele für Ihre Linux-VMs erstellen.


## <a name="azure-cli"></a>Azure CLI 

Azure-CLI wird auf Plattformen über ein Npm Paket, Distribution bereitgestellte Pakete oder Andockfenster Container. Weitere Informationen [zum Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md). Die folgenden Lernprogramme Beispiele zur Verwendung der Azure-CLI. Jeder Artikel Weitere Informationen zu CLI Schnellstart-Befehle:

- [Azure-CLI für Entwicklungs- und erstellen Sie Linux VM](virtual-machines-linux-quick-create-cli.md)
    - Im folgenden Beispiel wird ein CoreOS VM, mit einem öffentlichen Schlüssel mit dem Namen `azure_id_rsa.pub`:

    ```bash
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
        --image-urn CoreOS
    ```

- [Erstellen einer sicheren Linux VM mit einer Azure Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
    - Im folgenden Beispiel wird einen virtueller Computer mithilfe einer Vorlage auf GitHub gespeichert:

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

- [Erstellen einer vollständigen Linux Umgebung der Azure-CLI](virtual-machines-linux-create-cli-complete.md)
    - Erstellen ein System zum Lastenausgleich und mehrere VMs in einem Verfügbarkeit enthält.

- [Hinzufügen eines Datenträgers zu Linux VM](virtual-machines-linux-add-disk.md)
    - Im folgenden Beispiel wird eine vorhandene Bezeichnung VM 5Gb Festplatte hinzugefügt `TestVM`:

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Azure-portal

[Azure-Portal](https://portal.azure.com) können Sie schnell einen virtuellen Computer erstellen, da es keine auf Ihrem System installiert. Mit der VM erstellt Azure-Portal:

- [Erstellen einer Linux VM mit Azure-portal](virtual-machines-linux-quick-create-portal.md) 
- [Legen Sie einen Datenträger mithilfe des Azure-Portals](virtual-machines-linux-attach-disk-portal.md)


## <a name="operating-system-and-image-choices"></a>Betriebssystem und Auswahlmöglichkeiten
Beim Erstellen einer VM wählen Sie ein Bild basierend auf dem Betriebssystem ausführen möchten. Azure und seine Partner bieten viele Bilder die Programme installiert sind. Oder Hochladen von Bildern (siehe [folgenden Abschnitt](#use-your-own-image)).

### <a name="azure-images"></a>Azure Bilder
Verwenden der `azure vm image` CLI-Befehle von Publisher Distribution Release und Builds verfügbar ist.

Liste verfügbaren Verleger wie folgt:

```bash
azure vm image list-publishers --location WestUS
```

Liste von verfügbaren Produkte (Angebote) einem bestimmten Herausgeber wie folgt:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Liste Verfügbare SKUs (Distribution Releases) ein Angebot wie folgt:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Liste aller verfügbaren Bilder für einen bestimmten folgt:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Weitere Beispiele durchsuchen und Bilder finden Sie unter [Navigieren und select Azure virtuellen Computerimages mit der Azure-CLI](virtual-machines-linux-cli-ps-findimage.md).

Die `azure vm quick-create` und `azure vm create` Befehle sind Aliase häufigeren Distributionen und die aktuellsten Versionen zugreifen können. Verwenden von Aliasen ist häufig schneller als Verleger, Angebot, SKU und Version bei jedem eine VM erstellen angeben:

| Alias     | Publisher | Angebot        | SKU         | Version |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2         | neueste  |
| CoreOS    | CoreOS    | CoreOS       | Stabil      | neueste  |
| Debian    | credativ  | Debian       | 8           | neueste  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | neueste  |
| RHEL      | RedHat    | RHEL         | 7.2         | neueste  |
| SLES      | SLES      | SLES         | 12 SP1      | neueste  |
| UbuntuLTS | Kanonische | UbuntuServer | 14.04.4-LTS | neueste  |

### <a name="use-your-own-image"></a>Verwenden Sie ein eigenes Bild

Benötigen bestimmte Anpassungen können Sie ein Bild basierend auf einer vorhandenen Azure VM *erfassen* die VM. Sie können auch ein Bild vor Ort hochladen. Weitere Informationen zu unterstützten Distributionen und wie Sie Ihre eigenen Bilder finden Sie in folgenden Artikeln:

- [Azure unterstützt Distributionen](virtual-machines-linux-endorsed-distros.md)

- [Informationen für die Verteilung nicht unterstützt](virtual-machines-linux-create-upload-generic.md)

- [Erfassen einer virtuellen Linux-Maschine als Ressourcen-Manager-Vorlage](virtual-machines-linux-capture-image.md).
    - Schnellstart Beispielbefehle vorhandene VM erfassen:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie Linux VM- [Portal](virtual-machines-linux-quick-create-portal.md)mit der [CLI](virtual-machines-linux-quick-create-cli.md)oder mit einer [Azure-Ressourcen-Manager-Vorlage](virtual-machines-linux-cli-deploy-templates.md)

- Nach dem Erstellen einer Linux VM [Datenträger hinzufügen](virtual-machines-linux-add-disk.md).

- Schritte zum [Zurücksetzen eines Kennworts oder SSH-Schlüssel und Benutzer verwalten](virtual-machines-linux-using-vmaccess-extension.md)
