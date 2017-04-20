<properties
pageTitle="Vorbereiten einer Oracle virtuellen Linux-Maschine für Azure | Microsoft Azure"
description="Schritt-Konfiguration eines virtuellen Computers Oracle unter Linux in Microsoft Azure."
services="virtual-machines-linux"
authors="bbenz"
manager="timlt"
documentationCenter="virtual-machines"
tags="azure-service-management,azure-resource-manager"
/>

<tags
ms.service="virtual-machines-linux"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-linux"
ms.workload="infrastructure-services"
ms.date="06/22/2015"
ms.author="bbenz" />

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Vorbereiten einer virtuellen Maschine Oracle Linux für Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


-   [Vorbereiten einer virtuellen Maschine Oracle Linux 6.4 + für Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

-   [Vorbereiten einer virtuellen Maschine Oracle Linux 7.0 + für Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

## <a name="prerequisites"></a>Erforderliche Komponenten
Es wird vorausgesetzt, dass Sie bereits ein Oracle Linux Betriebssystem auf einer virtuellen Festplatte installiert haben. Es gibt mehrere Tools VHD-Dateien, z. B. eine Virtualisierungslösung wie Hyper-V. Eine Anleitung finden Sie [Installieren Sie Hyper-V und erstellen einen virtuellen Computer](http://technet.microsoft.com/library/hh846766.aspx).

**Oracle Installationshinweise für Linux**

- Oracle Red Hat kompatibel Kernel und die UEK3 (unverwüstliche Enterprise Kernel) werden auf Hyper-V und Azure unterstützt. Für optimale Ergebnisse unbedingt auf den neuesten Kernel beim Vorbereiten der Oracle Linux VHD aktualisieren.

- Oracle UEK2 nicht auf Hyper-V und Azure werden als nicht die benötigten Treiber enthalten ist.

- Neuere VHDX-Format wird in Azure nicht unterstützt. Hyper-V-Manager oder das Cmdlet Vhd konvertieren, um die Festplatte in VHD-Format konvertieren.

- Wenn Sie das Linux-System installieren, empfehlen wir Standardpartitionen als LVM (oft Standard für viele Installationen) verwenden. Dies vermeidet LVM Namenskonflikte mit geklonten VMs, insbesondere dann, wenn ein Betriebssystemdatenträger muss immer eine andere VM zur Problembehandlung an. LVM oder [RAID](virtual-machines-linux-configure-raid.md) kann wahlweise auf Datenträger verwendet werden.

- NUMA ist für größere VM aufgrund eines Fehlers in Linux Kernel-Versionen unter 2.6.37 nicht unterstützt. Dieses Problem betrifft hauptsächlich Distributionen, mit denen die Red Hat 2.6.32 Kernel. Manuelle Installation des Agenten Azure Linux (Waagent) deaktiviert automatisch NUMA in GRUB-Konfiguration für den Linux-Kernel. Weitere Informationen finden in den folgenden Schritten.

- Konfigurieren Sie eine Swap-Partition auf dem Betriebssystem-Laufwerk. Linux-Agent kann konfiguriert eine Auslagerungsdatei auf dem temporären Datenträger erstellen. Weitere Informationen finden in den folgenden Schritten.

- Alle die VHDs müssen Größen, die Vielfache von 1 MB sind.

- Stellen Sie sicher, dass die `Addons` Repository aktiviert ist. Wählen Sie die Datei `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) oder `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), und ändern Sie die Zeile `enabled=0` , `enabled=1` **[ol6_addons]** oder **[ol7_addons]** in der Datei.


## <a name="oracle-linux-64"></a>Oracle Linux 6.4 +
Sie müssen bestimmte Konfigurationsschritte im Betriebssystem des virtuellen Computers in Azure ausgeführt.

1. Wählen Sie im mittleren Bereich der Hyper-V-Manager den virtuellen Computer.

2. Klicken Sie auf **Verbinden** , um das Fenster für den virtuellen Computer.

3. Deinstallieren Sie NetworkManager durch Ausführen des folgenden Befehls:

        # sudo rpm -e --nodeps NetworkManager

    >[AZURE.NOTE] Das Paket noch nicht installiert ist, wird dieser Befehl mit einer Fehlermeldung fehl. Wird erwartet.

4. Erstellen Sie eine Datei namens **Netzwerk** im Verzeichnis/etc/Sysconfig, das folgenden Text enthält:

    `NETWORKING=yes`  
    `HOSTNAME=localhost.localdomain`

5.  Erstellen Sie eine Datei mit dem Namen **Ifcfg eth0** in der /etc/sysconfig/network-scripts / Verzeichnis mit dem folgenden Text:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
            TYPE=Ethernet
            USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

6.  Verschieben oder entfernen Udev-Regeln zu statische Regeln für die Ethernet-Schnittstelle generiert. Diese Regeln Probleme beim Klonen eines virtuellen Computers in Azure oder Hyper-v

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2\>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2\>/dev/null

7.  Sicherstellen Sie, dass der Netzwerkdienst beim Booten gestartet wird, durch den folgenden Befehl ausführen:

        # chkconfig network on

8.  Installieren Sie Python-pyasn1 durch den folgenden Befehl ausführen:

        # sudo yum install python-pyasn1

9.  Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Öffnen Sie hierzu "/ boot/grub/menu.lst" in einem Texteditor und sicherstellen, dass der Kernel standardmäßig die folgenden Parameter:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Dies wird auch sichergestellt, dass alle Konsole den ersten seriellen Anschluss Nachrichten sind die Azure unterstützen können mit Debuggen Probleme. Dadurch wird aufgrund eines Fehlers in Oracle Red Hat kompatibel Kernel NUMA deaktiviert.

    Darüber hinaus sollten Sie *Entfernen* die folgenden Parameter:

        rhgb quiet crashkernel=auto

    Grafik- und quiet Boot eignen sich nicht in einer Cloudumgebung, wo wir alle Protokolle an den seriellen Anschluss gesendet werden soll.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass dieser Parameter die VM von mindestens 128 MB Arbeitsspeicher reduziert. VM-kleinere Probleme möglicherweise.

10.  Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet. Dies ist normalerweise die Standardeinstellung.

11.  Installieren der Azure Linux-Agent durch Ausführen des folgenden Befehls:

        # <a name="sudo-yum-install-walinuxagent"></a>Sudo Yum installieren WALinuxAgent

    Beachten Sie, dass WALinuxAgent Paket Entfernen der NetworkManager NetworkManager Gnome-Pakete, wenn sie nicht bereits entfernt wurden, wie in 2 Schritt.

12.  Swap-Bereich wird nicht auf dem Betriebssystem-Datenträger erstellt.

    Azure Linux-Agent kann Auslagerungsspeicher automatisch mit der lokalen Ressource, der die VM zugeordnet ist, nach der Bereitstellung in Azure konfigurieren. Beachten Sie, dass die lokale Festplatte *temporärer* Speicherplatz und gelöscht werden, können Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in /etc/waagent.conf entsprechend:

        ResourceDisk.Format=y

        ResourceDisk.Filesystem=ext4

        ResourceDisk.MountPoint=/mnt/resource

        ResourceDisk.EnableSwap=y

        ResourceDisk.SwapSizeMB=2048 ## Hinweis: Legen Sie den Wert auf alles.

13.  Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # <a name="sudo-waagent--force--deprovision"></a>Sudo Waagent-force - entziehen
        # <a name="export-histsize0"></a>Exportieren Sie HISTSIZE = 0
        # <a name="logout"></a>Abmelden

14.  Klicken Sie auf **Action -\> fahren Sie** in Hyper-V-Manager. Da Ihre VHD Linux kann jetzt in Azure hochgeladen werden.

## <a name="oracle-linux-70"></a>Oracle Linux 7.0 +
**Ändert in Oracle Linux 7**

Azure Vorbereiten einer virtuellen Maschine Oracle Linux 7 ähnelt dem Verfahren für Oracle Linux 6. Es gibt jedoch einige wichtige Unterschiede zu beachten:

-   Red Hat kompatibel Kernel und Oracle UEK3 werden in Azure unterstützt. Den Kernel UEK3 empfohlen.

-   Das Paket NetworkManager widerspricht Azure Linux-Agent nicht mehr. Dieses Paket wird standardmäßig installiert, und wir empfehlen nicht entfernt.

-   Das Verfahren zum Bearbeiten von Kernel-Parameter geändert wurde (siehe unten) wird GRUB2 jetzt als Standard-Bootloader verwendet.

-   XFS ist nun die Standardeinstellung. Ext4 Dateisystem kann weiterhin verwendet werden.

**Konfigurationsschritte**

1.  Wählen Sie im Hyper-V-Manager den virtuellen Computer.

2.  Klicken Sie auf **Verbinden** , um ein Konsolenfenster für den virtuellen Computer zu öffnen.

3.  Erstellen Sie eine Datei namens **Netzwerk** im Verzeichnis/etc/Sysconfig, das folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Erstellen Sie eine Datei mit dem Namen **Ifcfg eth0** in der /etc/sysconfig/network-scripts / Verzeichnis mit dem folgenden Text:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

5.  Verschieben oder entfernen Udev-Regeln zu statische Regeln für die Ethernet-Schnittstelle generiert. Diese Regeln verursachen Probleme, wenn Sie in Microsoft Azure oder Hyper-V einen virtuellen Computer klonen.

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2>/dev/null

6.  Sicherstellen Sie, dass der Netzwerkdienst beim Booten gestartet wird, durch den folgenden Befehl ausführen:

        # sudo chkconfig network on

7.  Installieren Sie das Paket Python-pyasn1 durch den folgenden Befehl ausführen:

        # sudo yum install python-pyasn1

8.  Führen Sie den folgenden Befehl die aktuellen Yum Metadaten und installieren:

        # sudo yum clean all
        # sudo yum -y update

9.  Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Dazu öffnen "/ Etc/Default/Grub" in einem Texteditor und bearbeiten GRUB\_CMDLINE\_LINUX-Parameter, beispielsweise:

        GRUB\_CMDLINE\_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"

    Auch dadurch alle Konsole werden Nachrichten an den ersten seriellen Anschluss Azure helfen kann mit debugging verzichten. Darüber hinaus sollten Sie *Entfernen* die folgenden Parameter:

        rhgb quiet crashkernel=auto

    Grafik- und quiet Boot eignen sich nicht in einer Cloudumgebung, wo wir alle Protokolle an den seriellen Anschluss gesendet werden soll.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass dieser Parameter die VM von mindestens 128 MB Arbeitsspeicher reduziert. VM-kleinere Probleme möglicherweise.

10.  Sobald Sie fertig sind, bearbeiten "/ Etc/Default/Grub", führen Sie folgenden Befehl GRUB-Konfiguration neu erstellen:

        # <a name="sudo-grub2-mkconfig--o-bootgrub2grubcfg"></a>Sudo grub2-Mkconfig - o /boot/grub2/grub.cfg

11.  Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet. Dies ist normalerweise die Standardeinstellung.

12.  Installieren der Azure Linux-Agent durch Ausführen des folgenden Befehls:

        # <a name="sudo-yum-install-walinuxagent"></a>Sudo Yum installieren WALinuxAgent

13.  Swap-Bereich wird nicht auf dem Betriebssystem-Datenträger erstellt.

    Azure Linux-Agent kann Auslagerungsspeicher automatisch mit der lokalen Ressource, der die VM zugeordnet ist, nach der Bereitstellung in Azure konfigurieren. Beachten Sie, dass die lokale Festplatte *temporärer* Speicherplatz und gelöscht werden, können Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in /etc/waagent.conf entsprechend:

        ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## Hinweis: Legen Sie den Wert auf alles.

14.  Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # <a name="sudo-waagent--force--deprovision"></a>Sudo Waagent-force - entziehen
        # <a name="export-histsize0"></a>Exportieren Sie HISTSIZE = 0
        # <a name="logout"></a>Abmelden

15.  Klicken Sie auf **Action -\> fahren Sie** in Hyper-V-Manager. Da Ihre VHD Linux kann jetzt in Azure hochgeladen werden.
