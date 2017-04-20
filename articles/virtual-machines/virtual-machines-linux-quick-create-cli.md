<properties
   pageTitle="Mithilfe einer Linux VM in Azure CLI | Microsoft Azure"
   description="Mithilfe einer Linux VM in Azure CLI."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="v-livech"/>


# <a name="create-a-linux-vm-on-azure-by-using-the-cli"></a>Mithilfe einer Linux VM in Azure CLI

Dieser Artikel zeigt, wie schnell bereitstellen eine virtuellen Linux-Maschine (VM) in Azure mithilfe der `azure vm quick-create` Befehl in Azure Befehlszeilenschnittstelle (CLI). Die `quick-create` Befehl setzt einen virtueller Computer einen einfachen, sicheren Infrastruktur mit der Prototyp oder ein Konzept schnell testen. Der Artikel sind erforderlich:

- ein Azure-Konto ([kostenlos testen](https://azure.microsoft.com/pricing/free-trial/)).

- angemeldet bei [Azure CLI](../xplat-cli-install.md) `azure login`.

- Azure-CLI _muss_ Azure-Ressourcen-Manager-Modus `azure config mode arm`.

Sie können auch Linux VM mithilfe der [Azure-Portal](virtual-machines-linux-quick-create-portal.md)bereitstellen.

## <a name="quick-commands"></a>Schnellzugriff

Das folgende Beispiel zeigt, wie CoreOS VM bereitstellen und den Secure Shell (SSH) Schlüssel (der Argumente können abweichen):

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a>Ausführliche Anleitung

Die folgende exemplarische Vorgehensweise hat ein UbuntuLTS VM wird bereitgestellt, Schritt mit jedem Schritt tun.

## <a name="vm-quick-create-aliases"></a>VM Quick-Aliase erstellen

Schnell eine Verteilung auswählen ist der Azure-CLI Aliasnamen häufigsten Distributionen zugeordnet. Die folgende Tabelle listet die Aliase (seit Azure CLI Version 0,10). Alle Installationen mit `quick-create` standardmäßig die VMs von Solid-State Drive (SSD) Speicher mit schneller Bereitstellung und leistungsstarke Festplatte gesichert werden. (Diese Aliase stellen einen kleinen Teil der verfügbaren Distributionen Azure. Suchen Sie weitere Bilder in Azure Marketplace [Bild in PowerShell suchen](virtual-machines-linux-cli-ps-findimage.md), [im Web](https://azure.microsoft.com/marketplace/virtual-machines/)oder [Ihr eigenes Bild hochladen](virtual-machines-linux-create-upload-generic.md).)

| Alias     | Publisher | Angebot        | SKU         | Version |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2         | neueste  |
| CoreOS    | CoreOS    | CoreOS       | Stabil      | neueste  |
| Debian    | credativ  | Debian       | 8           | neueste  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | neueste  |
| RHEL      | Red Hat    | RHEL         | 7.2         | neueste  |
| UbuntuLTS | Kanonische | Ubuntu-Server | 14.04.4-LTS | neueste  |

Die folgenden Abschnitte mit der `UbuntuLTS` Alias für die **ImageURN** -Option (`-Q`) ein Ubuntu 14.04.4 LTS Server bereitstellen.

Die vorherige `quick-create` wird nur aufgerufen die `-M` Flag zu öffentlichen SSH-Schlüssel beim SSH Kennwörter deaktivieren, damit Sie die folgenden aufgefordert hochladen:

- Ressourcengruppennamen (jede Zeichenfolge ist in der Regel gut für Ihre erste Azure-Ressourcengruppe)
- Name des virtuellen Computers
- Speicherort (`westus` oder `westeurope` sind gute Standardwerte)
- Linux (lassen Sie wissen, welches Betriebssystem Sie möchten Azure)
- Benutzername

Das folgende Beispiel gibt die Werte, so dass keine weitere Aufforderung erforderlich ist. Solange Sie einen `~/.ssh/id_rsa.pub` als ssh Rsa Format öffentliche Schlüsseldatei, es funktioniert:

```bash
azure vm quick-create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--admin-username myAdminUser \
--ssh-public-file ~/.ssh/id_rsa.pub \
--image-urn UbuntuLTS
```

Die Ausgabe sieht wie die folgende Ausgabe:

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a>Melden Sie sich bei der neuen VM

Melden Sie sich bei Ihrer VM die öffentliche IP-Adresse in der Ausgabe aufgeführt. Sie können auch den vollqualifizierten Domänennamen (FQDN), der aufgelistet ist:

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

Der Anmeldevorgang sieht etwa wie die folgende Ausgabe:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

myAdminUser@myVM:~$
```

## <a name="next-steps"></a>Nächste Schritte

Die `azure vm quick-create` Befehl ist die Möglichkeit, schnell einen virtuellen Computer bereitstellen, Bash Shell anmelden und arbeiten. Allerdings `vm quick-create` steht Ihnen umfassende Kontrolle noch wird Sie eine komplexere Umgebung erstellen.  Führen Sie diese Artikel, um Linux VM bereitzustellen, die für Ihre Infrastruktur angepasst:

- [Verwenden einer Vorlage Azure-Ressourcen-Manager eine bestimmte Bereitstellung erstellen](virtual-machines-linux-cli-deploy-templates.md)
- [Erstellen Sie Ihrer eigenen benutzerdefinierten Umgebung für eine Azure-CLI-Befehle direkt mit Linux VM](virtual-machines-linux-create-cli-complete.md)
- [Erstellen einer SSH gesichert Linux VM in Azure mithilfe von Vorlagen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

Sie können auch [verwenden die `docker-machine` Azure Treiber mit verschiedenen Befehlen schnell Linux VM als Host Andockfenster erstellen](virtual-machines-linux-docker-machine.md).
