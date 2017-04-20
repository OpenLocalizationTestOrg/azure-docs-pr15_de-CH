<properties
    pageTitle="Erstellen und Hochladen einer Red Hat Enterprise Linux VHD in Azure verwendet"
    description="Informationen Sie zum Erstellen und Hochladen einer Azure virtuellen Festplatte (VHD) mit einer Red Hat Linux-Betriebssystem."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/17/2016"
    ms.author="mingzhan"/>


# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Vorbereiten eines Red Hat-basierten virtuellen Computers für Azure

In diesem Artikel erfahren Sie, wie eine virtuelle Maschine von Red Hat Enterprise Linux (RHEL) für die Verwendung in Azure vorzubereiten. Versionen von RHEL, die in diesem Artikel behandelt werden 6.7, 7.1 und 7.2. Hypervisoren Vorbereitung in diesem Artikel behandelt werden Hyper-V, Kernel-basierte virtuelle Maschine (KVM) und VMware. Weitere Teilnahmebedingungen für Red Hat Cloud Access Programm finden Sie unter [Red Hat Cloud Access-Website](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) und [RHEL in Azure ausgeführt](https://access.redhat.com/articles/1989673).

[Vorbereiten eines RHEL 6.7 virtuellen Computers von Hyper-V-Manager](#rhel67hyperv)

[Vorbereiten eines RHEL 7.1/7.2 virtuellen Computers von Hyper-V-Manager](#rhel7xhyperv)

[Vorbereiten einer virtuellen Maschine RHEL 6.7 von KVM](#rhel67kvm)

[Vorbereiten einer virtuellen Maschine RHEL 7.1/7.2 von KVM](#rhel7xkvm)

[Vorbereiten einer virtuellen Maschine RHEL 6.7 von VMware](#rhel67vmware)

[Vorbereiten einer RHEL 7.1/7.2 virtuellen Maschine von VMware](#rhel7xvmware)

[Vorbereiten eines virtuellen Computers RHEL 7.1/7.2 Kickstart-Datei](#rhel7xkickstart)


## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Vorbereiten eines Red Hat-basierten virtuellen Computers von Hyper-V-Manager
### <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Abschnitt wird vorausgesetzt, dass Sie bereits installiert RHEL Bild (aus einer ISO-Datei, die Red Hat-Website entnommen) in eine virtuelle Festplatte (VHD). Weitere Informationen zur Verwendung von Hyper-V-Manager ein Betriebssystemabbild installieren finden Sie unter [Installieren der Hyper-V-Rolle und konfigurieren einen virtuellen Computer](http://technet.microsoft.com/library/hh846766.aspx).

**Installationshinweise für RHEL**

- Siehe auch [Allgemeine Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) auf Azure Linux vorbereiten.

- Neuere VHDX-Format wird in Azure nicht unterstützt. Hyper-V-Manager oder das PowerShell-Cmdlet **Vhd konvertieren** , um die Festplatte in VHD-Format konvertieren.

- VHDs müssen erstellt werden als "fixed" – dynamische Festplatten werden nicht unterstützt.

- Wenn Sie das Linux-System installieren, empfehlen wir Standardpartitionen als LVM (oft Standard für viele Installationen) verwenden. Dies vermeidet LVM Namenskonflikte mit geklonten VMs, insbesondere dann, wenn ein Betriebssystemdatenträger muss immer eine andere VM zur Problembehandlung an. LVM oder [RAID](virtual-machines-linux-configure-raid.md) kann wahlweise auf Datenträger verwendet werden.

- Konfigurieren Sie eine Swap-Partition auf dem Betriebssystem-Laufwerk. Linux-Agent eine Auslagerungsdatei auf der Festplatte temporäre Ressource erstellen können. Weitere Informationen zu diesem steht in den folgenden Schritten.

- Alle die VHDs müssen Größen, die Vielfache von 1 MB sind.

- Verwendung **Qemu Img** Datenträgerabbilder VHD-Format konvertieren, beachten Sie, dass ein bekanntes Problem in Img Qemu Versionen 2.2.1 oder höher. Dieser Fehler führt eine falsch formatierte VHD-Datei. Das Problem soll in einer zukünftigen Version des Img Qemu festgesetzt. Jetzt wir empfehlen die Verwendung von Qemu Img Version 2.2.0 oder früher.

### <a id="rhel67hyperv"> </a>Vorbereiten eines RHEL 6.7 virtuellen Computers von Hyper-V-Manager###


1.  Wählen Sie im Hyper-V-Manager den virtuellen Computer.

2.  Klicken Sie auf **Verbinden** , um ein Konsolenfenster für den virtuellen Computer zu öffnen.

3.  Deinstallieren Sie NetworkManager durch Ausführen des folgenden Befehls:

        # sudo rpm -e --nodeps NetworkManager

    Hinweis: Wenn das Paket noch nicht installiert ist, wird dieser Befehl mit einer Fehlermeldung fehl. Wird erwartet.

4.  Erstellen Sie eine Datei mit dem Namen **Netzwerk** der `/etc/sysconfig/` Verzeichnis, den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Erstellen Sie eine Datei mit dem Namen **Ifcfg eth0** in der `/etc/sysconfig/network-scripts/` Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Verschieben oder entfernen Udev-Regeln zu statische Regeln für die Ethernet-Schnittstelle generiert. Diese Regeln Probleme beim Klonen eines virtuellen Computers in Microsoft Azure oder Hyper-v

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Sicherstellen Sie, dass der Netzwerkdienst beim Booten gestartet wird, durch den folgenden Befehl ausführen:

        # sudo chkconfig network on

8.  Registrieren Sie Ihr Abonnement Red Hat, um die Installation von Paketen aus dem Repository RHEL aktivieren, indem Sie den folgenden Befehl ausführen:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Das WALinuxAgent-Paket `WALinuxAgent-<version>` Red Hat Extras Repository abgelegt wurde. Aktivieren Sie Extras-Repository durch Ausführen des folgenden Befehls:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Öffnen Sie hierzu `/boot/grub/menu.lst` in einem Texteditor und sicherstellen, dass der Kernel standardmäßig die folgenden Parameter:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Dies wird auch sichergestellt, dass alle Konsole den ersten seriellen Anschluss Nachrichten sind die Azure unterstützen können mit Debuggen Probleme. Dies deaktiviert NUMA aufgrund eines Fehlers in der Kernelversion, die von RHEL 6 verwendet wird.

    Zusätzlich die oben genannte Aktion wird empfohlen, die folgenden Parameter entfernen:

        rhgb quiet crashkernel=auto

    Grafik- und quiet Boot eignen sich nicht in einer Cloudumgebung, wo wir alle Protokolle an den seriellen Anschluss gesendet werden soll.

    Die Crashkernel-Option kann Links auf Wunsch konfigurieren, aber beachten Sie, dass dieser Parameter die VM von mindestens 128 MB Arbeitsspeicher reduziert. VM-kleinere Probleme möglicherweise.

11. Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet. Dies ist normalerweise die Standardeinstellung. Ändern Sie /etc/ssh/sshd_config, um die folgende Zeile einzuschließen:

        ClientAliveInterval 180

12. Installieren der Azure Linux-Agent durch Ausführen des folgenden Befehls:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

    Beachten Sie, dass WALinuxAgent Paket Entfernen der NetworkManager NetworkManager Gnome-Pakete, wenn sie nicht bereits entfernt wurden, wie in 2 Schritt.

13. Swap-Bereich wird nicht auf dem Betriebssystem-Datenträger erstellt.
Azure Linux-Agent kann automatisch Auslagerungsspeicher konfigurieren, mit der lokale Ressource, die die VM zugeordnet ist, nachdem die VM auf Azure bereitgestellt wird. Beachten Sie, dass die lokale Festplatte temporärer Speicherplatz ist und gelöscht werden, kann Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in /etc/waagent.conf entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Registrierung aufheben des Abonnements (falls erforderlich) den folgenden Befehl ausführen:

        # sudo subscription-manager unregister

15. Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Klicken Sie auf **Aktion > Herunterfahren** in Hyper-V-Manager. Da Ihre VHD Linux kann jetzt in Azure hochgeladen werden.
 

### <a id="rhel7xhyperv"> </a>Von Hyper-V-Manager einen virtuellen Computer RHEL 7.1/7.2 Vorbereiten###

1.  Wählen Sie im Hyper-V-Manager den virtuellen Computer.

2.  Klicken Sie auf **Verbinden** , um ein Konsolenfenster für den virtuellen Computer zu öffnen.

3.  Erstellen Sie eine Datei mit dem Namen **Netzwerk** der `/etc/sysconfig/` Verzeichnis, den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Erstellen Sie eine Datei mit dem Namen **Ifcfg eth0** in der `/etc/sysconfig/network-scripts/` Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

5.  Sicherstellen Sie, dass der Netzwerkdienst beim Booten gestartet wird, durch den folgenden Befehl ausführen:

        # sudo chkconfig network on

6.  Registrieren Sie Ihr Abonnement Red Hat, um die Installation von Paketen aus dem Repository RHEL aktivieren, indem Sie den folgenden Befehl ausführen:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Öffnen Sie hierzu `/etc/default/grub` in einem Texteditor und Bearbeiten der **GRUB_CMDLINE_LINUX** -Parameter. Zum Beispiel:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Dies wird auch sichergestellt, dass alle Konsole den ersten seriellen Anschluss Nachrichten sind die Azure unterstützen können mit Debuggen Probleme. Zusätzlich die oben genannte Aktion wird empfohlen, die folgenden Parameter entfernen:

        rhgb quiet crashkernel=auto

    Grafik- und quiet Boot eignen sich nicht in einer Cloudumgebung, wo wir alle Protokolle an den seriellen Anschluss gesendet werden soll.
    Die Crashkernel-Option kann Links auf Wunsch konfigurieren, aber beachten Sie, dass dieser Parameter die VM von mindestens 128 MB Arbeitsspeicher reduziert. VM-kleinere Probleme möglicherweise.

8.  Sie anschließend bearbeiten `/etc/default/grub`, führen Sie folgenden Befehl GRUB-Konfiguration neu erstellen:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9.  Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet. Dies ist normalerweise die Standardeinstellung. Ändern `/etc/ssh/sshd_config` die folgende Zeile enthalten:

        ClientAliveInterval 180

10. Das WALinuxAgent-Paket `WALinuxAgent-<version>` Red Hat Extras Repository abgelegt wurde. Aktivieren Sie Extras-Repository durch Ausführen des folgenden Befehls:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Installieren der Azure Linux-Agent durch Ausführen des folgenden Befehls:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

12. Swap-Bereich wird nicht auf dem Betriebssystem-Datenträger erstellt. Azure Linux-Agent kann automatisch Auslagerungsspeicher konfigurieren, mit der lokale Ressource, die die VM zugeordnet ist, nachdem die VM auf Azure bereitgestellt wird. Beachten Sie, dass die lokale Festplatte temporärer Speicherplatz ist und gelöscht werden, kann Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in `/etc/waagent.conf` entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Wenn Sie das Abonnement abmelden möchten, führen Sie den folgenden Befehl ein:

        # sudo subscription-manager unregister

14. Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Klicken Sie auf **Aktion > Herunterfahren** in Hyper-V-Manager. Da Ihre VHD Linux kann jetzt in Azure hochgeladen werden.
 


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Vorbereiten eines Red Hat-basierten virtuellen Computers von KVM

### <a id="rhel67kvm"> </a>Vorbereiten eine virtuellen Maschine RHEL 6.7 von KVM###


1.  KVM-Bild RHEL 6.7 von Red Hat-Website herunterladen.

2.  Festlegen Sie ein Root-Kennwort.

    Generieren Sie ein verschlüsseltes Kennwort zu, und kopieren Sie die Ausgabe des Befehls:

        # openssl passwd -1 changeme

    Legen Sie ein Passwort mit Guestfish:

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Ändern Sie im zweite Feld der Root-Benutzer von "!!" das verschlüsselte Kennwort.

3.  Erstellen Sie einen virtuellen Computer aus dem Bild qcow2 KVM Typ auf **qcow2**festgelegt und das Gerätemodell virtuelles Netzwerk-Schnittstelle auf **Virtio**festgelegt. Dann starten Sie den virtuellen Computer, und melden Sie sich als Root.

4.  Erstellen Sie eine Datei mit dem Namen **Netzwerk** der `/etc/sysconfig/` Verzeichnis, den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Erstellen Sie eine Datei mit dem Namen **Ifcfg eth0** in der `/etc/sysconfig/network-scripts/` Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Verschieben oder entfernen die Udev-Regeln zu vermeiden, dass statische Regeln für die Ethernet-Schnittstelle. Diese Regeln Probleme beim Klonen eines virtuellen Computers in Microsoft Azure oder Hyper-v

        # mkdir -m 0700 /var/lib/waagent
        # mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Sicherstellen Sie, dass der Netzwerkdienst beim Booten gestartet wird, durch den folgenden Befehl ausführen:

        # chkconfig network on

8.  Registrieren Sie Ihr Abonnement Red Hat, um die Installation von Paketen aus dem Repository RHEL aktivieren, indem Sie den folgenden Befehl ausführen:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Öffnen Sie hierzu `/boot/grub/menu.lst` in einem Texteditor und sicherstellen, dass der Kernel standardmäßig die folgenden Parameter:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Dies wird auch sichergestellt, dass alle Konsole den ersten seriellen Anschluss Nachrichten sind die Azure unterstützen können mit Debuggen Probleme. Dies deaktiviert NUMA aufgrund eines Fehlers in der Kernelversion, die von RHEL 6 verwendet wird.

    Zusätzlich die oben genannte Aktion wird empfohlen, die folgenden Parameter entfernen:

        rhgb quiet crashkernel=auto

    Grafik- und quiet Boot eignen sich nicht in einer Cloudumgebung, wo wir alle Protokolle an den seriellen Anschluss gesendet werden soll.
    Die Crashkernel-Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass dieser Parameter die VM von mindestens 128 MB Arbeitsspeicher reduziert. VM-kleinere Probleme möglicherweise.

10. Fügen Sie Initramfs mit Hyper-V-Module:  

    Bearbeiten `/etc/dracut.conf` und Inhalt hinzufügen: Add_drivers += "Hv_vmbus Hv_netvsc Hv_storvsc"

    Rebuild Initramfs: # Dracut – f - V

11. Cloud-Initialisierung zu deinstallieren:

        # yum remove cloud-init

12. Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten starten:

        # chkconfig sshd on

    Ändern Sie /etc/ssh/sshd_config, um die folgenden Zeilen enthalten:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Starten Sie Sshd:

        # service sshd restart

13. Das WALinuxAgent-Paket `WALinuxAgent-<version>` Red Hat Extras Repository abgelegt wurde. Aktivieren Sie Extras-Repository durch Ausführen des folgenden Befehls:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Installieren der Azure Linux-Agent durch Ausführen des folgenden Befehls:

        # yum install WALinuxAgent
        # chkconfig waagent on

15. Azure Linux-Agent kann automatisch Auslagerungsspeicher konfigurieren, mit der lokale Ressource, die die VM zugeordnet ist, nachdem die VM auf Azure bereitgestellt wird. Beachten Sie, dass die lokale Festplatte temporärer Speicherplatz ist und gelöscht werden, kann Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in **/etc/waagent.conf** entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Registrierung aufheben des Abonnements (falls erforderlich) den folgenden Befehl ausführen:

        # subscription-manager unregister

17. Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. VM KVM Herunterfahren.

19. Konvertieren Sie qcow2-Bild in VHD-Format.
    Konvertieren Sie das Bild zuerst in raw-Format:

         # qemu-img convert -f qcow2 –O raw rhel-6.7.qcow2 rhel-6.7.raw
    Stellen Sie sicher, dass die Größe der raw-Bild 1 MB ausgerichtet wird. Größe mit 1 MB, gerundet: # MB = ((1024*1024)$) # Größe = $(Img Qemu Info -f raw - Ausgabe Json "Rhel-6.7.raw" | \ Gawk ' übereinstimmen ($0 / "virtuelle Größe": ([0-9] +), Val) {drucken val[1]}') # Rounded_size = $((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-6.7.raw $rounded_size

    Konvertieren Sie raw-Festplatte in einer virtuellen Festplatte fester Größe:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xkvm"> </a>Vorbereiten eine virtuellen Maschine RHEL 7.1/7.2 von KVM###


1.  KVM-Bild RHEL 7.1 (oder 7.2) von Red Hat-Website herunterladen. Wir verwenden das Beispiel RHEL 7.1.

2.  Festlegen Sie ein Root-Kennwort.

    Generieren Sie ein verschlüsseltes Kennwort zu, und kopieren Sie die Ausgabe des Befehls:

        # openssl passwd -1 changeme

    Legen Sie ein Passwort mit Guestfish.

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Ändern Sie im zweite Feld des Root-Benutzers von "!!" das verschlüsselte Kennwort.

3.  Erstellen Sie einen virtuellen Computer aus dem Bild qcow2 KVM Typ auf **qcow2**festgelegt und das Gerätemodell virtuelles Netzwerk-Schnittstelle auf **Virtio**festgelegt. Dann starten Sie den virtuellen Computer, und melden Sie sich als Root.

4.  Erstellen Sie eine Datei mit dem Namen **Netzwerk** der `/etc/sysconfig/` Verzeichnis, den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Erstellen Sie eine Datei mit dem Namen **Ifcfg eth0** in der `/etc/sysconfig/network-scripts/` Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Sicherstellen Sie, dass der Netzwerkdienst beim Booten gestartet wird, durch den folgenden Befehl ausführen:

        # chkconfig network on

7.  Registrieren Sie Ihr Abonnement Red Hat, um Pakete aus dem Repository RHEL ermöglichen durch Ausführen des folgenden Befehls:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8.  Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Öffnen Sie hierzu `/etc/default/grub` in einem Texteditor und Bearbeiten der **GRUB_CMDLINE_LINUX** -Parameter. Zum Beispiel:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Dies wird auch sichergestellt, dass alle Konsole den ersten seriellen Anschluss Nachrichten sind die Azure unterstützen können mit Debuggen Probleme. Zusätzlich die oben genannte Aktion wird empfohlen, die folgenden Parameter entfernen:

        rhgb quiet crashkernel=auto

    Grafik- und quiet Boot eignen sich nicht in einer Cloudumgebung, wo wir alle Protokolle an den seriellen Anschluss gesendet werden soll.
    Die Crashkernel-Option kann Links auf Wunsch konfigurieren, aber beachten Sie, dass dieser Parameter die VM von mindestens 128 MB Arbeitsspeicher reduziert. VM-kleinere Probleme möglicherweise.

9.  Sie anschließend bearbeiten `/etc/default/grub`, führen Sie folgenden Befehl GRUB-Konfiguration neu erstellen:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Fügen Sie Initramfs mit Hyper-V-Module:

    Bearbeiten `/etc/dracut.conf` und Inhalt hinzufügen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Neu erstellen Sie Initramfs:

        # dracut –f -v

11. Cloud-Initialisierung zu deinstallieren:

        # yum remove cloud-init

12. Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten starten:

        # systemctl enable sshd

    Ändern Sie /etc/ssh/sshd_config, um die folgenden Zeilen enthalten:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Starten Sie Sshd:

        systemctl restart sshd

13. Das WALinuxAgent-Paket `WALinuxAgent-<version>` Red Hat Extras Repository abgelegt wurde. Aktivieren Sie Extras-Repository durch Ausführen des folgenden Befehls:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Installieren der Azure Linux-Agent durch Ausführen des folgenden Befehls:

        # yum install WALinuxAgent

    Aktivieren Sie den Dienst Waagent:

        # systemctl enable waagent.service

15. Swap-Bereich wird nicht auf dem Betriebssystem-Datenträger erstellt. Azure Linux-Agent kann automatisch Auslagerungsspeicher konfigurieren, mit der lokale Ressource, die die VM zugeordnet ist, nachdem die VM auf Azure bereitgestellt wird. Beachten Sie, dass die lokale Festplatte temporärer Speicherplatz ist und gelöscht werden, kann Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in `/etc/waagent.conf` entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Registrierung aufheben des Abonnements (falls erforderlich) den folgenden Befehl ausführen:

        # subscription-manager unregister

17. Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Fahren Sie den virtuellen Computer in KVM.

19. Konvertieren Sie qcow2-Bild in VHD-Format.

    Konvertieren Sie das Bild zuerst in raw-Format:

         # qemu-img convert -f qcow2 –O raw rhel-7.1.qcow2 rhel-7.1.raw

    Stellen Sie sicher, dass die Größe der raw-Bild 1 MB ausgerichtet wird. Größe mit 1 MB, gerundet:

         # MB=$((1024*1024))
         # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                  gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
         # rounded_size=$((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-7.1.raw $rounded_size

    Konvertieren Sie raw-Festplatte in einer virtuellen Festplatte fester Größe:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Vorbereiten eines Red Hat-basierten virtuellen Computers von VMware
### <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Abschnitt wird vorausgesetzt, dass Sie einen virtuellen Computer RHEL in VMware installiert haben. Weitere Informationen zum Installieren eines Betriebssystems in VMware anzeigen Sie [VMware Gast-Betriebssystem-Installationshandbuch](http://partnerweb.vmware.com/GOSIG/home.html)

- Beim Installieren des Betriebssystems Linux empfohlen Standardpartitionen als LVM (oft Standard für viele Installationen) verwenden. Dies vermeidet LVM Namenskonflikte mit geklonten VMs, insbesondere dann, wenn ein Betriebssystemdatenträger muss immer eine andere VM zur Problembehandlung an. LVM oder RAID kann wahlweise auf Datenträger verwendet werden.

- Konfigurieren Sie eine Swap-Partition auf dem Betriebssystem-Laufwerk. Linux-Agent eine Auslagerungsdatei auf der Festplatte temporäre Ressource erstellen können. Weitere Informationen finden in den folgenden Schritten.

- Wenn Sie die virtuelle Festplatte erstellen, wählen Sie **Informationsspeicher virtuelle Datenträger als einzelne Datei aus**



### <a id="rhel67vmware"> </a>Eine RHEL 6.7 virtuelle Maschine von VMware vorbereiten###

1.  Deinstallieren Sie NetworkManager durch Ausführen des folgenden Befehls:

         # sudo rpm -e --nodeps NetworkManager

    Hinweis: Wenn das Paket noch nicht installiert ist, wird dieser Befehl mit einer Fehlermeldung fehl. Wird erwartet.

2.  Erstellen Sie eine Datei namens **Netzwerk** im Verzeichnis/etc/Sysconfig, das folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3.  Erstellen Sie eine Datei mit dem Namen **Ifcfg eth0** in der /etc/sysconfig/network-scripts / Verzeichnis mit dem folgenden Text:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4.  Verschieben oder entfernen die Udev-Regeln zu vermeiden, dass statische Regeln für die Ethernet-Schnittstelle. Diese Regeln Probleme beim Klonen eines virtuellen Computers in Microsoft Azure oder Hyper-v

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

5.  Sicherstellen Sie, dass der Netzwerkdienst beim Booten gestartet wird, durch den folgenden Befehl ausführen:

        # sudo chkconfig network on

6.  Registrieren Sie Ihr Abonnement Red Hat, um die Installation von Paketen aus dem Repository RHEL aktivieren, indem Sie den folgenden Befehl ausführen:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Das WALinuxAgent-Paket `WALinuxAgent-<version>` Red Hat Extras Repository abgelegt wurde. Aktivieren Sie Extras-Repository durch Ausführen des folgenden Befehls:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8.  Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Öffnen Sie hierzu "/ boot/grub/menu.lst" in einem Texteditor und sicherstellen, dass der Kernel standardmäßig die folgenden Parameter:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Dies wird auch sichergestellt, dass alle Konsole den ersten seriellen Anschluss Nachrichten sind die Azure unterstützen können mit Debuggen Probleme. Dies deaktiviert NUMA aufgrund eines Fehlers in der Kernelversion, die von RHEL 6 verwendet wird.
    Zusätzlich die oben genannte Aktion wird empfohlen, die folgenden Parameter entfernen:

        rhgb quiet crashkernel=auto

    Grafik- und quiet Boot eignen sich nicht in einer Cloudumgebung, wo wir alle Protokolle an den seriellen Anschluss gesendet werden soll.
    Die Crashkernel-Option kann Links auf Wunsch konfigurieren, aber beachten Sie, dass dieser Parameter die VM von mindestens 128 MB Arbeitsspeicher reduziert. VM-kleinere Probleme möglicherweise.

9.  Fügen Sie Initramfs mit Hyper-V-Module:

        Edit `/etc/dracut.conf` and add content:

            add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

        Rebuild initramfs:

            # dracut –f -v

10. Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet. Dies ist normalerweise die Standardeinstellung. Ändern `/etc/ssh/sshd_config` die folgende Zeile enthalten:

        ClientAliveInterval 180

11. Installieren der Azure Linux-Agent durch Ausführen des folgenden Befehls:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

12. Swap-Bereich wird nicht auf dem Betriebssystem-Datenträger erstellt:

    Azure Linux-Agent kann automatisch Auslagerungsspeicher konfigurieren, mit der lokale Ressource, die die VM zugeordnet ist, nachdem die VM auf Azure bereitgestellt wird. Beachten Sie, dass die lokale Festplatte temporärer Speicherplatz ist und gelöscht werden, kann Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in `/etc/waagent.conf` entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Registrierung aufheben des Abonnements (falls erforderlich) den folgenden Befehl ausführen:

        # sudo subscription-manager unregister

14. Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. VM Herunterfahren und die VMDK-Datei in eine VHD-Datei konvertieren.

    Konvertieren Sie das Bild zuerst in raw-Format:

        # qemu-img convert -f vmdk –O raw rhel-6.7.vmdk rhel-6.7.raw

    Stellen Sie sicher, dass die Größe der raw-Bild 1 MB ausgerichtet wird. Größe mit 1 MB, gerundet:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.7.raw" | \
                gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.7.raw $rounded_size

    Konvertieren Sie raw-Festplatte in einer virtuellen Festplatte fester Größe:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xvmware"> </a>Vorbereiten eine RHEL 7.1/7.2 virtuellen Maschine von VMware###

1.  Erstellen Sie eine Datei namens **Netzwerk** im Verzeichnis/etc/Sysconfig, das folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2.  Erstellen Sie eine Datei mit dem Namen **Ifcfg eth0** in der /etc/sysconfig/network-scripts / Verzeichnis mit dem folgenden Text:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

3.  Sicherstellen Sie, dass der Netzwerkdienst beim Booten gestartet wird, durch den folgenden Befehl ausführen:

        # sudo chkconfig network on

4.  Registrieren Sie Ihr Abonnement Red Hat, um die Installation von Paketen aus dem Repository RHEL aktivieren, indem Sie den folgenden Befehl ausführen:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5.  Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Öffnen Sie hierzu `/etc/default/grub` in einem Texteditor und Bearbeiten der **GRUB_CMDLINE_LINUX** -Parameter. Zum Beispiel:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Dies wird auch sichergestellt, dass alle Konsole den ersten seriellen Anschluss Nachrichten sind die Azure unterstützen können mit Debuggen Probleme. Zusätzlich die oben genannte Aktion wird empfohlen, die folgenden Parameter entfernen:

        rhgb quiet crashkernel=auto

    Grafik- und quiet Boot eignen sich nicht in einer Cloudumgebung, wo wir alle Protokolle an den seriellen Anschluss gesendet werden soll.
    Die Crashkernel-Option kann Links auf Wunsch konfigurieren, aber beachten Sie, dass dieser Parameter die VM von mindestens 128 MB Arbeitsspeicher reduziert. VM-kleinere Probleme möglicherweise.

6.  Sie anschließend bearbeiten `/etc/default/grub`, führen Sie folgenden Befehl GRUB-Konfiguration neu erstellen:

         # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7.  Fügen Sie Initramfs mit Hyper-V-Module:

    Bearbeiten `/etc/dracut.conf`, Inhalt hinzufügen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Neu erstellen Sie Initramfs:

        # dracut –f -v

8.  Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet. Dies ist normalerweise die Standardeinstellung. Ändern `/etc/ssh/sshd_config` die folgende Zeile enthalten:

        ClientAliveInterval 180

9.  Das WALinuxAgent-Paket `WALinuxAgent-<version>` Red Hat Extras Repository abgelegt wurde. Aktivieren Sie Extras-Repository durch Ausführen des folgenden Befehls:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Installieren der Azure Linux-Agent durch Ausführen des folgenden Befehls:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

11. Swap-Bereich wird nicht auf dem Betriebssystem-Datenträger erstellt. Azure Linux-Agent kann automatisch Auslagerungsspeicher konfigurieren, mit der lokale Ressource, die die VM zugeordnet ist, nachdem die VM auf Azure bereitgestellt wird. Beachten Sie, dass die lokale Festplatte temporärer Speicherplatz ist und gelöscht werden, kann Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in `/etc/waagent.conf` entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. Wenn Sie das Abonnement abmelden möchten, führen Sie den folgenden Befehl ein:

        # sudo subscription-manager unregister

13. Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. VM Herunterfahren und die VMDK-Datei in das VHD-Format konvertieren.

    Konvertieren Sie das Bild zuerst in raw-Format:

        # qemu-img convert -f vmdk –O raw rhel-7.1.vmdk rhel-7.1.raw

    Stellen Sie sicher, dass die Größe der raw-Bild 1 MB ausgerichtet wird. Größe mit 1 MB, gerundet:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                 gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.1.raw $rounded_size

    Konvertieren Sie raw-Festplatte in einer virtuellen Festplatte fester Größe:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Vorbereiten eines Red Hat-basierten virtuellen Computers eine ISO mit Kickstart-Datei automatisch


### <a id="rhel7xkickstart"> </a>Vorbereiten eines virtuellen Computers RHEL 7.1/7.2 Kickstart-Datei###


1.  Erstellen Sie Kickstart-Datei mit dem folgenden Inhalt, und speichern Sie die Datei. Details Kickstart Installation finden Sie im [Installationshandbuch Kickstart](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).



        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
        auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff



        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=yes
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2.  Legen Sie die Kickstart-Datei an einem Ort über das Installationsprogramm.

3.  Erstellen Sie im Hyper-V-Manager einen neuen virtuellen Computer. Wählen Sie auf der Seite **virtuelle Festplatte verbinden** **Anfügen einer virtuellen Festplatte später**, und führen Sie den Assistenten.

4.  VM-Optionen zu öffnen:

    ein.  Fügen Sie eine neue virtuelle Festplatte für die VM. Vergewissern Sie sich **VHD-Format** und **Feste Größe**.

    b.  Das DVD-Laufwerk die Installation ISO zuordnen.

    c.  Legen Sie im BIOS zum Starten von CD.

5.  Starten Sie VM. Erscheint im Installationshandbuch drücken Sie **Registerkarte** bootoptionen konfigurieren.

6.  Geben Sie `inst.ks=<the location of the kickstart file>` am Ende der Startoptionen und drücken Sie die **EINGABETASTE**.

7.  Warten Sie, bis die Installation abgeschlossen. Wenn er abgeschlossen ist, wird die VM automatisch heruntergefahren. Da Ihre VHD Linux kann jetzt in Azure hochgeladen werden.

## <a name="known-issues"></a>Bekannte Probleme
Es bestehen bekannte Probleme bei Verwendung von RHEL 7.1 in Hyper-V und Azure.

### <a name="disk-io-freeze"></a>Datenträger-e/a fixieren

Häufige Speicher Datenträger e/a-Aktivitäten mit RHEL 7.1 in Hyper-V und Azure kann dieses Problem auftreten.   

Repro-Rate:

Dieses Problem tritt zeitweise. Es tritt jedoch häufig während Datenträger e/a-Operationen in Hyper-V und Azure häufiger.   


[AZURE.NOTE] Dieses bekannte Problem wurde von Red Hat bereits behoben. Führen Sie den folgenden Befehl, um die zugehörigen Updates installieren:

    # sudo yum update

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Hyper-V-Treiber konnte nicht die ursprüngliche RAM-Datenträger enthalten, wenn Hyper-V-Hypervisor verwenden

In einigen Fällen u. Installationsprogramme Linux Treiber für Hyper-V in der ursprünglichen RAM-Disk (Initrd oder Initramfs) nicht, wenn festgestellt wird, dass es in einer Umgebung mit Hyper-V ausgeführt wird.

Wenn Sie Linux-Image Vorbereiten eines anderen Virtualisierung (Virtualbox, Xen, etc.) verwenden, müssen Sie eventuell neu erstellen Initrd sicherstellen, dass mindestens die Hv_vmbus und Hv_storvsc-Kernel-Module stehen die ursprüngliche RAM-Disk. Dies ist ein bekanntes Problem mindestens auf Systemen upstream Red Hat-Distribution.

Um dieses Problem zu beheben, müssen Sie Hyper-V-Module in Initramfs und erstellen Sie ihn neu:

Bearbeiten `/etc/dracut.conf` und Inhalt hinzufügen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

Neu erstellen Sie Initramfs:

        # dracut –f -v

Weitere Informationen finden Sie in den Informationen [Initramfs](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Nächste Schritte
Sie nun können mit Red Hat Enterprise Linux virtuelle Festplatte erstellen neuen virtueller Maschinen in Azure. Ist dies das erste Mal, dass Sie die VHD-Datei in Azure hochladen, finden Sie die Schritte 2 und 3 [Erstellen und Hochladen einer virtuellen Festplatte mit dem Betriebssystem Linux](virtual-machines-linux-classic-create-upload-vhd.md).

Weitere Informationen zu den Hypervisoren zertifiziert sind Red Hat Enterprise Linux anzeigen Sie [Red Hat-website](https://access.redhat.com/certified-hypervisors)
