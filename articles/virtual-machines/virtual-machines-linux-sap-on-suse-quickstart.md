<properties
   pageTitle="Testen von Sapnetweaver auf Microsoft Azure SUSE Linux VMs | Microsoft Azure"
   description="Testen von Sapnetweaver auf Microsoft Azure SUSE Linux VMs"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Laufenden Sapnetweaver auf Microsoft Azure SUSE Linux VMs


Dieser Artikel beschreibt verschiedene Dinge berücksichtigen Sie SAP NetWeaver auf Microsoft Azure SUSE Linux virtuellen Maschinen (VMs) ausführen. Ab Mai 19 2016 wird SAP NetWeaver auf SUSE Linux VMs auf Azure offiziell unterstützt. Alle Details zu Linux-Versionen und SAP Kernel-Versionen finden Sie im SAP-Hinweis 1928533 "SAP-Applikationen in Azure: unterstützte Produkte und Azure VM".
Weitere Dokumentationen zum SAP auf Linux VMs finden Sie hier: [Mithilfe von SAP auf Linux virtuellen Maschinen (VMs)](virtual-machines-linux-sap-get-started.md).


Die folgende Informationen sollen einige potenzielle Probleme zu vermeiden.

## <a name="suse-images-on-azure-for-running-sap"></a>SUSE Bilder auf Azure für die Ausführung von SAP

Für SAP NetWeaver auf Azure ausführen, verwenden nur SUSE Linux Enterprise Server SLES 12 (SPx): Siehe auch Hinweis 1928533. Spezielles SUSE-Image in Azure Marketplace ("SLES 11 SP3 für SAP CAL") ist dies nicht für die allgemeine Verwendung vorgesehen. Dieses Bild kann nicht verwendet werden, da die Lösung [SAP Cloud Appliance Library](https://cal.sap.com/) reserviert ist.  

Sie sollten für alle neuen Tests und Installationen auf Azure Azure-Ressourcen-Manager verwenden. SUSE SLES Bilder und Versionen suchen Azure PowerShell oder Azure Befehlszeilenschnittstelle (CLI), verwenden Sie die folgenden Befehle. Dann können die Ausgabe beispielsweise in einer JSON-Vorlage für die Bereitstellung einer neuen SUSE Linux VM Betriebssystemabbild definieren.
Diese PowerShell-Befehle sind für Azure PowerShell Version 1.0.1 und höher.

* Suchen Sie nach vorhandenen Herausgeber SUSE einschließlich:

   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```

* Suchen Sie nach vorhandenen Angebote SUSE:

   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```

* SUSE SLES Angebote suchen:

   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   CLI : azure vm image list-skus westeurope SUSE SLES
   ```

* Suchen Sie nach einer bestimmten Version einer SLES SKU:

   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12"
   CLI : azure vm image list westeurope SUSE SLES 12
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Installieren von WALinuxAgent in einer VM SUSE

Der Agent aufgerufen WALinuxAgent gehört SLES Bilder in Azure Marketplace. Informationen (z. B. beim Hochladen einer SLES OS virtuellen Festplatte (VHD) lokal) manuell installieren finden Sie unter:

- [OpenSUSE] (http://software.opensuse.org/package/WALinuxAgent)

- [Azure] (virtual-Maschinen-Linux-unterstützt-distros.md)

- [SUSE] (https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP "enhanced" Überwachung

SAP "Überwachung enhanced" ist obligatorische Voraussetzung für SAP in Azure ausgeführt. Überprüfen Sie die Details im SAP 2191498 "SAP auf Linux mit Azure: Erweiterte Überwachung" beachten.

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Ein Azure Linux VM zuweisen Azure Datenträger

Sie sollten nie Mount Azure-Datenträger eine Azure Linux VM mit Geräte-ID. Verwenden Sie stattdessen den universally unique Identifier (UUID). Seien Sie vorsichtig, wenn Sie Grafiktools Mount Azure-Datenträger verwenden. Überprüfen Sie die Einträge in/etc/fstab.

Das Problem mit der Geräte-ID ist, Sie ändern können und legen Azure VM-Startvorgang möglicherweise. Zum Beheben des Problems können Sie den Parameter Nofail in/etc/fstab hinzufügen. Aber Vorsicht mit Nofail Applikationen können Sie den Mount-Punkt als vor und können bei ein externen Azure Datenträger während des Startvorgangs geladen war nicht in das Root-Dateisystem schreiben.

Die einzige Ausnahme mithilfe UUID ist ein Betriebssystemdatenträger Problembehandlung anfügen, wie im folgenden Abschnitt beschrieben.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Problembehandlung bei SUSE VM, der nicht mehr verfügbar ist

Möglicherweise Situationen, hängt eine VM SUSE Azure Startvorgang (z. B. mit einen Fehler Datenträger bereitstellen). Sie können dieses Problem mit der Boot-Diagnose für Azure Virtual Machines v2 in Azure-Portal überprüfen. Weitere Informationen finden Sie unter [Boot Diagnose] (https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Eine Möglichkeit zur Lösung des Problems ist ein anderes SUSE VM auf Azure OS Festplatte beschädigt VM an. Dann ändern Sie entsprechende wie/etc/fstab bearbeiten oder Entfernen von Udev Netzwerkregeln wie im nächsten Abschnitt beschrieben.

Es ist wichtig zu berücksichtigen. Bereitstellen von mehreren SUSE VMs aus dem gleichen Azure Marketplace-Bild (z. B. SLES 11 SP4) wird OS Festplatte immer durch dieselbe UUID bereitgestellt werden. Daher führt UUID OS Festplatte aus einer anderen VM angefügt, die mit der gleichen Azure Marketplace bereitgestellt wurde mit zwei identische UUIDs. Das konnte führt zu Problemen und sollte für die Behandlung von VM tatsächlich vom Datenträger angeschlossen und beschädigt OS anstelle des ursprünglichen startet.

Es gibt zwei Arten vermeiden:

* Verwenden Sie ein anderes Azure Marketplace-Bild für die Problembehandlung VM (z. B. SLES 11 SPx SLES 12).
* Nicht angeschlossen beschädigt OS aus anderen VM UUID - Verwendung mit etwas anderes.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Eine VM SUSE lokal Azure hochladen

Eine Beschreibung der Schritte ein VM SUSE lokal Azure hochladen finden Sie unter [Vorbereitung einer SLES oder OpenSUSE Virtual Machine für Azure] (virtual-machines-linux-suse-create-upload-vhd.md).

Wenn Sie VM ohne die entziehen Ende (z. B. zum Beibehalten einer vorhandenen SAP-Installation sowie der Hostname) hochladen möchten, überprüfen Sie Folgendes:

* Sicherstellen Sie, dass der Betriebssystem-Datenträger eingehängt mit UUID und nicht die Geräte-ID. Ändern in/etc/fstab UUID reicht nicht für den Betriebssystem-Datenträger. Außerdem vergessen Sie Bootloader über YaST oder durch Bearbeiten der /boot/grub/menu.lst anzupassen.
* VHDX Format für SUSE Betriebssystemdatenträger verwenden und zum Azure hochladen in VHD konvertieren ist sehr wahrscheinlich, dass das Netzwerkgerät von eth0 auf eth1 geändert wird. Um Probleme zu vermeiden, wenn Sie auf Azure später starten, ändern, eth0 gemäß [Festsetzung eth0 geklonte SLES 11 VMware] (https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Neben wie im Artikel beschrieben ist wird empfohlen, diese zu entfernen:

   /lib/udev/Rules.d/75-persistent-NET-Generator.Rules

Sie können auch Azure Linux-Agent (Waagent) um potenzielle Probleme zu vermeiden, solange nicht mehrere Netzwerkkarten vorhanden sind.

## <a name="deploying-a-suse-vm-on-azure"></a>Bereitstellen einer VM SUSE Azure

Erstellen Sie neue SUSE VMs mit JSON-Vorlagendateien im neuen Azure-Ressourcen-Manager-Modell. Nachdem die JSON-Vorlagendatei erstellt, können Sie die VM mit folgenden CLI-Befehl als Alternative zu PowerShell bereitstellen:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Weitere Informationen über JSON-Vorlagendateien finden Sie unter [Azure Ressourcenmanager erstellen Vorlagen] (Resource-Gruppe-authoring-templates.md) und [Azure Schnellstart Vorlagen] (https://azure.microsoft.com/documentation/templates/).

Weitere Informationen über CLI und Azure Resource Manager finden Sie unter [Nutzung der Azure-CLI für Mac, Linux und Windows Azure Ressourcenmanager] (befehlsschnittstelleninstanz-Cli-Azure-Ressource-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP-Lizenz und Hardware-Schlüssel

Für die Zulassung von SAP Azure wurde ein neuer Mechanismus SAP-Hardwareschlüssel berechnen, für die SAP-Lizenz eingeführt. SAP-Kernel angepasst werden musste dies. Frühere SAP Kernel-Versionen für Linux enthalten nicht dazu Code. In bestimmten Situationen (z. B. Azure VM Größe), SAP-Hardwareschlüssel geändert und SAP-Lizenz ist nicht mehr gültig. Dies ist in den neuesten SAP Linux-Kernels gelöst. Details überprüfen Sie SAP-Hinweis 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE Sapconf Paket / Adm optimiert

SUSE bietet ein Paket mit dem Namen "Sapconf", die SAP-spezifischen Einstellungen verwaltet. Weitere Informationen zur Funktionsweise dieses Pakets und zum Installieren und verwenden, finden Sie unter [mit Sapconf eine SUSE Linux Enterprise Server SAP-Systemen ausführen vorbereiten] (https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) und [Sapconf Neuigkeiten oder einem SUSE Linux Enterprise Server Vorbereiten für SAP-Systeme?] (http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

In der Zwischenzeit ist ein neues Tool ersetzt Sapconf - ADM optimiert. Weitere Informationen zu diesem Tool, die folgenden zwei Links finden.

SLES Dokumentation optimiert Adm Profil Sap Hana finden Sie [hier](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

Systeme für SAP Arbeitslasten mit optimiert Adm - Tuning finden [hier](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) in Kapitel 6.2


## <a name="nfs-share-in-distributed-sap-installations"></a>NFS-Freigabe Bereitstellen verteilter SAP-Installationen

Haben Sie eine verteilte Installation – zum Beispiel soll die Datenbank und die SAP-Anwendungsserver in separate VMs - installieren können Sie das /sapmnt-Verzeichnis über Network File System (NFS) freigeben. Haben Sie Probleme mit den Installationsschritten nach NFS-Freigabe für /sapmnt erstellen, überprüfen Sie, ob die Freigabe "No_root_squash" festgelegt ist.

## <a name="logical-volumes"></a>Logische volumes

In der Vergangenheit bei Bedarf einen großen logischen Datenträger über mehrere Azure Datenträger (z.B. SAP-Datenbank), wurde empfohlen, Lvm auf Azure noch vollständig validiert wurde nicht Mdadm verwenden. Wie Linux RAID auf Azure mithilfe von Mdadm finden Sie unter [Configure Software-RAID auf Linux](virtual-machines-linux-configure-raid.md). In der Zwischenzeit ab Anfang Mai 2016 Lvm auf Azure unterstützt auch als Alternative zu Mdadm verwendet werden. Weitere Informationen zu Lvm auf Azure finden Sie unter [LVM auf Linux VM in Azure konfigurieren](virtual-machines-linux-configure-lvm.md).


## <a name="azure-suse-repository"></a>Azure SUSE-repository

Haben Sie ein Problem mit der standardmäßigen Azure SUSE Repository können einen einfachen Befehl Sie es zurücksetzen. Dies kann passieren, wenn private Betriebssystemabbild in Azure Region erstellen und kopieren Sie das Abbild in eine andere Region Bereitstellen neuer VMs, die auf diese private Betriebssystemabbild werden soll. Führen Sie einfach folgenden Befehl innerhalb des virtuellen Computers:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>GNOME-desktop

Gnome-Desktop mit der kompletten SAP Demo-Systems in einer einzigen VM - einschließlich SAP-GUI installieren verwenden Sie Browser und SAP-Verwaltungskonsole – Hinweis auf Azure SLES Bilder zu installieren:

   Für SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Für SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>SAP-Unterstützung für Oracle unter Linux in der cloud

Es gibt eine Einschränkung Support von Oracle unter Linux in einer virtualisierten Umgebung. Obwohl dies kein Azure-spezifische Thema, ist es wichtig, zu verstehen. SAP unterstützt Oracle auf SUSE oder Red Hat in der öffentlichen Cloud wie Azure nicht. Um dieses Thema direkt bei Oracle.
