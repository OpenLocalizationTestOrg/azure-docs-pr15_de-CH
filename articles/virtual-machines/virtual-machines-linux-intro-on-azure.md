<properties
    pageTitle="Einführung in Azure Linux | Microsoft Azure"
    description="Erfahren Sie mehr über Azure virtuelle Linux-Computer auf."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

#<a name="introduction-to-linux-on-azure"></a>Einführung in Linux auf Azure

Dieses Thema enthält eine Übersicht über einige Aspekte der virtuelle Linux-Computer in der Azure-Cloud verwenden. Bereitstellen einer virtuellen Linux-Maschine ist ein einfacher Prozess mit einem Bild aus der Galerie.


## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Authentifizierung: Benutzernamen, Kennwörter und SSH-Schlüssel

Beim Erstellen einer virtuellen Linux-Maschine mit klassischen Azure-Portal werden Sie aufgefordert, einen Benutzernamen, Kennwort oder einen öffentlichen SSH-Schlüssel bereitzustellen. Die Wahl eines Benutzernamens für die Bereitstellung einer virtuellen Linux-Maschine auf Azure unterliegt folgende Einschränkung: Namen Systemkonten (UID < 100) bereits auf dem virtuellen Computer vorhanden sind nicht zulässig, 'root' angenommen.


 - Siehe [Erstellen einer virtuellen Maschine unter Linux](virtual-machines-linux-quick-create-cli.md)
 - Informationen Sie [zum Verwenden von SSH mit Linux auf Azure](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="obtaining-superuser-privileges-using-sudo"></a>Abrufen mit Superuser-Berechtigungen`sudo`

Die während der Bereitstellung von virtuellen Instanz auf Azure angegebenen Benutzerkontos ist ein privilegiertes Konto. Dieses Konto wird vom Azure Linux-Agent Berechtigungen mit Stamm (Super) erhöhen können konfiguriert die `sudo` Dienstprogramm. Nach der Anmeldung mit diesem Benutzerkonto werden Sie Befehle mit der Syntax ausgeführt

    # sudo <COMMAND>

Sie können optional Root-Shell mit **Sudo s**erhalten.

- Finden Sie unter [Using Root-Rechte für virtuelle Linux-Computer in Azure](virtual-machines-linux-use-root-privileges.md)


## <a name="firewall-configuration"></a>Firewall-Konfiguration

Azure bietet einen eingehende Paketfilter, der Konnektivität im klassischen Azure-Portal angegebenen Ports beschränkt. Standardmäßig ist der einzige zulässige Port SSH. Sie können zusätzliche Anschlüsse auf Ihrem virtuellen Linux-Maschine Öffnen durch Konfigurieren von Endpunkten im klassischen Azure-Portal:

 - Siehe: [Endpunkte einer virtuellen Maschine einrichten](virtual-machines-windows-classic-setup-endpoints.md)

Linux Bilder in der Galerie Azure sind nicht *Iptables* Firewall standardmäßig aktiviert. Bei Bedarf kann die Firewall konfiguriert werden zusätzliche Filter.


## <a name="hostname-changes"></a>Hostname ändert

Wenn Sie zunächst eine Instanz eines Bilds Linux bereitstellen, müssen Sie einen Hostnamen für den virtuellen Computer bereitstellen. Sobald der virtuelle Computer ausgeführt wird, erscheint dieser Hostname Plattform DNS-Server, so dass mehrere virtuelle Maschinen miteinander verbunden mit Hostnamen IP-Adress-Suchvorgänge durchführen.

Hostname ändert gegebenenfalls werden nach der Bereitstellung eines virtuellen Computers verwenden Sie den Befehl

    # sudo hostname <newname>

Azure Linux-Agent umfasst Funktionen automatisch diese Änderung und den virtuellen Computer diese Änderung beibehalten und veröffentlichen diese Änderung der Plattform DNS-Server entsprechend konfigurieren.

 - [Azure Linux-Agent-Benutzerhandbuch](virtual-machines-linux-agent-user-guide.md)

### <a name="cloud-init"></a>Cloud-Initialisierung
**Ubuntu** und **CoreOS** Bilder nutzen Cloud Init Azure bietet zusätzliche Funktionen für den Neustart einer virtuellen Maschine.

 - [Wie Sie benutzerdefinierte Daten einfügen](virtual-machines-windows-classic-inject-custom-data.md)
 - [Benutzerdefinierte Daten und Microsoft Azure Cloud-Initialisierung](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Partitionen Sie Azure Swap mit Cloud-Initialisierung](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [Verwenden von CoreOS auf Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="virtual-machine-image-capture"></a>Virtual Machine-Aufnahme

Azure bietet die Möglichkeit, den Zustand des vorhandenen virtuellen Computer in ein Bild aufzuzeichnen, die später zum Bereitstellen zusätzlicher virtueller Computerinstanzen verwendet werden. Azure Linux-Agent möglicherweise Rollback zum Teil der Anpassung, die beim Bereitstellungsprozess durchgeführt wurden. Sie können die folgenden virtuellen Computer als Bild erfassen Schritte:

1. Ausführen **Waagent-entziehen** Bereitstellung Anpassung rückgängig machen. Oder **Waagent-Identitätsintegrationsprodukts + User** optional angegebene während der Bereitstellung und alle zugehörigen Daten gelöscht.

2. Nach unten/schalten Sie den virtuellen Computer herunterfahren.

3. Im klassischen Azure-Portal auf *erfassen* oder verwenden Sie die Powershell oder CLI auf dem virtuellen Computer als Bild erfassen.

 - Siehe: [wie einer virtuellen Linux-Maschine als Vorlage verwenden](virtual-machines-linux-classic-capture-image.md)


## <a name="attaching-disks"></a>Anfügen von Datenträgern

Jeder virtuelle Computer verfügt eine temporäre lokale *Datenträger* zugeordnet. Da Daten auf einem Datenträger dauerhafte Neustart möglicherweise nicht, ist es oft Applikationen und Prozesse auf dem virtuellen Computer für transiente und **temporäre** Speicherung von Daten verwendet. Es wird auch zum Speichern der Seite oder Auslagerungsdateien des Betriebssystems.

Unter Linux die Festplatte in der Regel von Azure Linux-Agent verwaltet und automatisch **/mnt/resource** (oder **mnt** Ubuntu Bilder) bereitgestellt.


>[AZURE.NOTE] Beachten Sie, dass die Festplatte ein **temporärer** Speicherplatz ist möglicherweise gelöscht und neu formatiert, wenn die VM neu gestartet wird.

Unter Linux kann der Datenträger vom Kernel als Namen `/dev/sdc`, und Benutzer müssen partitionieren, formatieren und diese Ressource bereitstellen. Dies wird im Lernprogramm schrittweise behandelt: [wie ein Datenträger mit einem virtuellen Computer](virtual-machines-linux-classic-attach-disk.md).

 - **Siehe auch:** [Konfigurieren von Software-RAID auf Linux](virtual-machines-linux-configure-raid.md)  &  [LVM auf Linux VM in Azure konfigurieren](virtual-machines-linux-configure-lvm.md)

