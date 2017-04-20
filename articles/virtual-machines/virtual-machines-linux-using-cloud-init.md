<properties
    pageTitle="Mit Cloud-Init Linux VM während der Erstellung anpassen | Microsoft Azure"
    description="Verwenden von Cloud Init Linux VM während der Erstellung anpassen."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="v-livech"
/>

# <a name="using-cloud-init-to-customize-a-linux-vm-during-creation"></a>Mithilfe von Cloud-Init Linux VM während der Erstellung anpassen

Dieser Artikel veranschaulicht, wie ein Cloud-Init-Skript Hostname, installierte Pakete festlegen und Verwalten von Benutzerkonten.  Die Cloud-Init-Skripts werden während des Erstellens der VM von Azure CLI bezeichnet.  Der Artikel sind erforderlich:

- ein Azure-Konto ([kostenlos testen](https://azure.microsoft.com/pricing/free-trial/)).

- angemeldet bei [Azure CLI](../xplat-cli-install.md) `azure login`.

- Azure-CLI _muss_ Azure-Ressourcen-Manager-Modus `azure config mode arm`.

## <a name="quick-commands"></a>Schnellzugriff

Erstellen Sie ein Cloud-init.txt-Skript legt den Hostnamen, die alle Pakete aktualisiert und Linux Sudo Benutzer hinzugefügt.

```bash
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Erstellen einer Ressourcengruppe VMs zu starten.

```bash
azure group create myResourceGroup westus
```

Erstellen einer Linux VM mit Cloud-Initialisierung beim Start konfigurieren.

```bash
azure vm create \
-g myResourceGroup \
-n myVM \
-l westus \
-y Linux \
-f myVMnic \
-F myVNet \
-P 10.0.0.0/22 \
-j mySubnet \
-k 10.0.0.0/24 \
-Q canonical:ubuntuserver:14.04.2-LTS:latest \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Ausführliche Anleitung

### <a name="introduction"></a>Einführung

Beim Starten einer neuen Linux VM erhalten einen Standard-Linux VM mit angepassten oder bereit für Ihre Bedürfnisse Sie. [Cloud-Initialisierung](https://cloudinit.readthedocs.org) ist ein Skript oder Configuration Settings in diesem Linux VM eingefügt, wie es zum ersten Mal gestartet wird.

Azure gibt es drei verschiedene Arten, wie er auf einer Linux-VM ändern bereitgestellt oder gestartet.

- Mit Cloud-Init-Skripts einfügen.
- Fügen Sie Skripts Azure [VMAccess-Erweiterung](virtual-machines-linux-using-vmaccess-extension.md)verwenden.
- Ein Azure-Vorlage mit Cloud-Initialisierung.
- Ein Azure-Vorlage mit [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md).

Skripts zu einem beliebigen Zeitpunkt nach dem Booten eingefügt:

- SSH Befehle direkt ausführen
- Fügen Sie Skripts mit Azure [VMAccess Erweiterung](virtual-machines-linux-using-vmaccess-extension.md)imperativ oder in Azure Vorlage ein
- Configuration Management-Tools wie Ansible, Salz, Chef und Marionetten.

>[AZURE.NOTE]: VMAccess Extension executes a script as root in the same way using SSH can.  However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Cloud-Init Verfügbarkeit Azure VM Image Aliase schnell erstellen:

| Alias     | Publisher | Angebot        | SKU         | Version | Cloud-Initialisierung |
|:----------|:----------|:-------------|:------------|:--------|:-----------|
| CentOS    | OpenLogic | CentOS       | 7.2         | neueste  | Nein         |
| CoreOS    | CoreOS    | CoreOS       | Stabil      | neueste  | Ja        |
| Debian    | credativ  | Debian       | 8           | neueste  | Nein         |
| openSUSE  | SUSE      | openSUSE     | 13.2        | neueste  | Nein         |
| RHEL      | RedHat    | RHEL         | 7.2         | neueste  | Nein         |
| UbuntuLTS | Kanonische | UbuntuServer | 14.04.4-LTS | neueste  | Ja        |

Microsoft arbeitet mit unseren Partnern zu Cloud Init enthalten und arbeiten in den Bildern, die sie in Azure bereitstellen.

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Die VM-Erstellung mit CLI Azure hinzufügen Cloud-Init-Skript

Geben Sie ein Cloud-Init-Skript starten beim Erstellen einer VM in Azure mit CLI Azure Cloud-Init-Datei `--custom-data` wechseln.

Erstellen einer Ressourcengruppe VMs zu starten.

```bash
azure group create myResourceGroup westus
```

Erstellen einer Linux VM mit Cloud-Initialisierung beim Start konfigurieren.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Erstellen einer Cloud Init um den Hostnamen des Linux VM

Eine der einfachsten und wichtigsten für alle Linux VM wäre der Hostname. Wir können einfach festlegen dieses Skript mit Cloud-Initialisierung.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Cloud-Init-Beispielskript mit dem Namen `cloud_config_hostname.txt`.

``` bash
#cloud-config
hostname: myservername
```

Beim ersten Start der VM wird dieses Cloud Init Hostname auf `myservername`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_hostname.txt
```

Benutzernamen und den Hostnamen des neuen VM überprüfen.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a>Erstellen einer Cloud Init Linux aktualisieren

Aus Sicherheitsgründen möchten Sie Ubuntu VM beim ersten Start zu aktualisieren.  Mithilfe von Cloud-Init, dass mit dem folgenden Skript je nach Linux-Distribution können Sie verwenden.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Cloud-Init-Beispielskript `cloud_config_apt_upgrade.txt` für Debian-Familie

```bash
#cloud-config
apt_upgrade: true
```

Nachdem Linux gestartet wurde, werden alle installierten Pakete über `apt-get`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_apt_upgrade.txt
```

Anmeldung und überprüfen Sie, ob alle Pakete werden aktualisiert.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a>Erstellen eine Cloud-Init-Skript zum Hinzufügen eines Benutzers zu Linux

Eine der ersten Aufgaben für alle neuen Linux VM wird zum Hinzufügen eines Benutzers für sich selbst oder mit `root`. SSH Schlüssel sind empfohlen für Sicherheit und Nutzbarkeit und sie dazu die `~/.ssh/authorized_keys` mit dieser Cloud-Init-Skript.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Cloud-Init-Beispielskript `cloud_config_add_users.txt` für Debian-Familie

```bash
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Nachdem Linux gestartet wurde, werden die aufgelisteten Benutzer erstellt und Sudo Gruppe hinzugefügt.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_add_users.txt
```

Anmeldung und überprüfen Sie den neu erstellten Benutzer.

```bash
ssh myVM
cat /etc/group
```

Ausgabe

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Nächste Schritte

Cloud-Init wird ein Standardverfahren zum Ändern Ihrer Linux VM starten. Azure verfügt außerdem über VM-Erweiterungen, die die LinuxVM beim Starten oder während der Ausführung ändern können. Azure-VMAccessExtension können Sie z. B. SSH oder Benutzerinformationen zurückgesetzt, während die VM ausgeführt wird. Mit Cloud-Init müssten Sie einen Neustart das Kennwort zurücksetzen.

[VM-Erweiterungen zu Funktionen](virtual-machines-linux-extensions-features.md)

[Verwalten von Benutzern, SSH und Kontrollkästchen oder Notfalldisketten in Azure Linux VMs mit VMAccess-Erweiterung](virtual-machines-linux-using-vmaccess-extension.md)
