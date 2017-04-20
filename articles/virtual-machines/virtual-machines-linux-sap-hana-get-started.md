<properties
   pageTitle="Schnellstart-Handbuch für die manuelle Installation von SAP HANA Azure VMs | Microsoft Azure"
   description="Schnellstart-Handbuch für die manuelle Installation von SAP HANA Azure VMs"
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

# <a name="quickstart-guide-for-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Schnellstart-Handbuch für die manuelle Installation von Einzelinstanz-SAP HANA Azure VMs

## <a name="introduction"></a>Einführung

Dieses Schnellstarthandbuch soll eine Einzelinstanz-SAP HANA Prototyp-Demo System auf Azure VMs durch eine manuelle Installation von SAP NetWeaver 7.5 und SAP HANA SP12 einrichten.
Das Handbuch setzt voraus, dass der Leser Azure IaaS Grundlagen wie virtuelle Maschinen oder virtuelle Netzwerke Azure-Portal oder Powershell/CLI inklusive Json Vorlagen bereitstellen. Darüber hinaus wird erwartet, dass der Leser mit SAP HANA, SAP NetWeaver und lokal installieren.

Es wird erwartet, dass der Leser über die allgemeine SAP Azure-Dokumentation im Abschnitt Allgemeine Informationen am Ende dieses Artikels genannten.

Durch die Beschränkung auf nicht produktiven Systemen dieses Handbuch deckt keine Themen wie HA, backup, DR, hohe Leistung oder besondere Sicherheitsaspekte.

Erfolgte die Installation des Beispiels mit zwei virtuellen Maschinen zu einer verteilten Installation SAP NetWeaver Azure-Ressourcen-Manager-Modell wie SAP-Linux-Azure nur auf Azure-Ressourcen-Manager und nicht das klassische Modell unterstützt wird. Weitere Informationen zu Azure-Ressourcen-Manager finden im Abschnitt "Allgemeine Informationen" am Ende dieses Artikels.

Diese wurden zwei VMs für die Installation der Beispieldaten verwendet:

* Hana Appsrv (Typ DS3) zum Hosten der NW 7.5 ASC Instanz + PAS
* Hana-Dbsrv (Typ GS4) Host HANA SP12
* Beide VMs gehörte eine Azure virtual Network (Azure-Hana-Test-Vnet)
* In beiden Fällen OS wurde SLES 12 SP1


Sein bewusst, dass ab Juli 2016 SAP HANA nur für OLAP (BW)-Produktionssysteme auf Azure VM GS5 unterstützt wird. Zu Testzwecken, eine offiziellen SAP Support nicht erwartet, ist es etwas kleiner wie z.B. GS4 gut.
Was sollte immer verwendet werden SAP HANA Azure Storage für HANA Daten und Protokolldateien - Azure Premium ist finden Sie Abschnitt "Disk Setup" weiteren unten. Weitere Details zu dem SAP-Produkte auf Azure unterstützt werden überprüfen Sie im Abschnitt Allgemeine Informationen am Ende dieses Artikels.


Das Handbuch beschreibt zwei verschiedene Verfahren SAP HANA in Azure VMs manuell zu installieren:

* Installieren von SAP HANA über SAP Software Bereitstellung Manager (SWPM) als Teil einer verteilten NetWeaver-Installation im Schritt "Instanz"
* Installieren Sie Hdblcm Tool HANA Lebenszyklus Manager mit SAP HANA und anschließend NetWeaver danach

Es kann auch SWPM und alle Komponenten (SAP HANA SAP-Anwendungsserver, ASC beispielsweise SAP-GUI) in einer einzigen VM installieren. Diese Option ist nicht in diesem Artikel beschriebenen, aber die zu berücksichtigenden Elemente sind identisch.

Abschnitt nach unten Prüflisten zum Einrichten von VMs Azure testen sollten vor Installation einige grundlegende Fehler vermeiden, die eintritt, wenn nur eine Standardkonfiguration Azure VM mit gelesen werden.


## <a name="checklist-sap-hana-installation-via-sap-swpm"></a>Prüfliste SAP HANA Installation über SAP SWPM

Dies ist eine einfache Checkliste der wichtigsten Elemente im Zusammenhang mit einer manuellen Installation der Einzelinstanz-SAP HANA für Demo oder Prototypen Pursposes über SAP SWPM Ausführen einer verteilten SAP NW 7.5 installieren. Die einzelnen Elemente erläutert und Screenshots in diesem Artikel ausführlicher dargestellt:

* Erstellen Sie ein Azure virtuelles Netzwerk umfasst zwei VMs 
* Bereitstellen von zwei Azure VMs mit OS SLES 12 SP1 Azure-Ressourcen-Manager-Modell 
* der Anwendungsserver VM (75GB und 500GB) zwei standardmäßige Datenträger zuordnen
* HANA-Datenbankserver VM - 2 standard-Datenträger wie für Anwendungsserver VM + 2 Premium-Datenträger (z.B. 2x512GB) vier Laufwerke zuordnen
* Je nach Größe oder Durchsatz fügen Sie mehrere Festplatten und entweder Erstellen von Stripesetvolumes auf Betriebssystemebene innerhalb des virtuellen Computers Lvm oder Mdadm verwenden
* XFS Dateisysteme erstellen, auf den angeschlossenen Festplatten / logische Volumes
* Bereitstellen der neuen XFS Dateisysteme auf Betriebssystemebene. Ein Dateisystem die SAP-Software und die andere Sapmnt Verzeichnis und vielleicht Backups zu verwenden. SAP HANA DB-Server Bereitstellen der XFS Dateisysteme auf die Premium-Datenträger als /hana und /usr/sap ist zu vermeiden, dass das Root-Dateisystem die Groß unter Linux Azure VMs füllt alle erforderlichen
* Geben Sie die lokale IP-Adressen des Tests VMs in/Etc/hosts
* Geben Sie den Parameter Nofail in/etc/fstab
* Kernel-Parameter nach HANA SLES 12 Hinweis (Siehe Details weiter unten im Abschnitt Parameter Kernel) festlegen
* Swap-Bereich hinzufügen
* -Wenn einen grafischen Desktop auf die VMs installiert. Andernfalls verwenden Sie remote Sapinst installieren
* die SAP-Software von SAP Service Marketplace herunterladen
* Installieren Sie die SAP ASC-Instanz auf dem Anwendungsserver VM
* Verzeichnis der Sapmnt über NFS unter Test VMs (Anwendungsserver VM ist der NFS-Server)
* Installieren Sie die Datenbankinstanz einschließlich HANA über SWPM auf dem-Datenbankserver VM
* PAS auf Anwendungsserver VM installieren
* SAP MC starten und verbinden Sie z. B. über SAP GUI / HANA Studio 



## <a name="checklist-sap-hana-installation-via-hdblcm"></a>Prüfliste SAP HANA Installation über hdblcm

Dies ist eine einfache Checkliste der wichtigsten Elemente im Zusammenhang mit einer manuellen Installation der Einzelinstanz-SAP HANA für Demo oder Prototypen Pursposes über SAP SWPM Ausführen einer verteilten SAP NW 7.5 installieren. Die einzelnen Elemente erläutert und Screenshots in diesem Artikel ausführlicher dargestellt:

* Erstellen Sie ein Azure virtuelles Netzwerk umfasst zwei VMs 
* Bereitstellen von zwei Azure VMs mit OS SLES 12 SP1 Azure-Ressourcen-Manager-Modell 
* der Anwendungsserver VM (75GB und 500GB) zwei standardmäßige Datenträger zuordnen
* HANA-Datenbankserver VM - 2 Standardspeicher wie Anwendungsserver VM + 2 Premium-Datenträger (z.B. 2x512GB) vier Laufwerke zuordnen
* Je nach Größe oder Durchsatz fügen Sie mehrere Festplatten und entweder Erstellen von Stripesetvolumes auf Betriebssystemebene innerhalb des virtuellen Computers Lvm oder Mdadm verwenden
* XFS Dateisysteme erstellen, auf den angeschlossenen Festplatten / logische Volumes
* Bereitstellen der neuen XFS Dateisysteme auf Betriebssystemebene. Ein Dateisystem die SAP-Software und die andere Sapmnt Verzeichnis und vielleicht Backups zu verwenden. SAP HANA DB-Server Bereitstellen der XFS Dateisysteme auf die Premium-Datenträger als /hana und /usr/sap ist zu vermeiden, dass das Root-Dateisystem die Groß unter Linux Azure VMs füllt alle erforderlichen
* Geben Sie die lokale IP-Adressen des Tests VMs in/Etc/hosts
* Geben Sie den Parameter Nofail in/etc/fstab
* Kernel-Parameter nach HANA SLES 12 Hinweis (Siehe Details weiter unten im Abschnitt Parameter Kernel) festlegen
* Swap-Bereich hinzufügen
* -Wenn einen grafischen Desktop auf die VMs installiert. Andernfalls verwenden Sie remote Sapinst installieren
* die SAP-Software von SAP Service Marketplace herunterladen
* Gruppen-Id 1001 auf HANA DB Server VM erstellen Sie Gruppe "Sapsys"
* Installieren von SAP HANA auf dem-Datenbankserver VM mit Hdblcm
* Installieren Sie die SAP ASC-Instanz auf dem Anwendungsserver VM
* Verzeichnis der Sapmnt über NFS unter Test VMs (Anwendungsserver VM ist der NFS-Server)
* Installieren Sie die Datenbankinstanz einschließlich HANA über SWPM auf dem-Datenbankserver VM
* PAS auf Anwendungsserver VM installieren
* SAP MC starten und verbinden Sie z. B. über SAP GUI / HANA Studio 




## <a name="prepare-azure-vms-for-manual-installation-of-sap-hana"></a>Manuelle Installation von SAP HANA Azure VMs vorbereiten

Dieses Kapitel zur Vorbereitung der Azure-VMs für die manuelle Installation von SAP HANA besteht aus fünf Abschnitten die folgende Themen behandelt:

* Disk Setup
* Kernel-Parameter
* Dateisysteme
* / Etc/hosts
* / Etc/fstab


### <a name="disk-setup"></a>Festplattenkonfiguration

Das Root-Dateisystem in einer Linux-VM auf Azure ist beschränkter Größe. Daher ist es notwendig, einen virtuellen Computer für die Ausführung von SAP Speicherplatz an. Bei einer SAP-Anwendungsserver, VM in eine reine Prototyp-Demo verwendet, reicht Azure standard-Datenträger verwenden. Für die SAP HANA DB Daten und Protokolldateien - Azure Premium-Datenträger auch in einer nicht produktiven Landschaft verwendet werden soll.

Einige Einzelheiten darüber, wie Linux VM Festplatten an [hier](virtual-machines-linux-add-disk.md)

Bei Azure zwischenspeichern - müssen eine "None" für Festplatten, die zum Speichern von Transaktionsprotokollen HANA verwenden. Finden Sie Dateien mit ok ist HANA Zwischenspeichern. Wie HANA wird eine Arbeitsspeicher-Datenbank hängt von insgesamt Verwendungsmuster Umfang der Lesecache auf Azure Datenträger (HANA starten und Lesen von Daten von der Festplatte in den Arbeitsspeicher) verbessern.

Detaillierte Informationen zu Azure Storage Premium [hier](../storage/storage-premium-storage.md)

[Hier](https://github.com/Azure/azure-quickstart-templates) finden Sie Json Beispielvorlagen VMs zu erstellen.
Der '101-Vm-einfach-Linux' dargestellt eine Basisvorlage wie einschließlich Lagerbereich, der einen 100-GB-Datenträger hinzugefügt.

[Dieser Artikel](virtual-machines-linux-sap-on-suse-quickstart.md) enthält Informationen dazu, wie ein SUSE Bild über Powershell oder CLI und die Bedeutung einer Festplatte über UUID.


Je nach Größe der System und Durchsatz möglicherweise mehrere Datenträger statt einer schließen und später ein Stripeset über die Datenträger auf Betriebssystemebene erstellen müssen. Dies sind zwei Aspekte warum ein Stripeset über mehrere Azure Festplatten erstellen möchten:

* Datendurchsatz
* benötigen Sie eine einzelnes Dateisystem > 1TB, wie das aktuelle Azure Disk Limit 1 TB (Juli 2016)


Weitere Informationen über die zwei wichtigsten Tools Striping konfigurieren finden Sie hier:

[Artikel mit Mdadm Linux Software-Raid auf Azure-VM konfigurieren](virtual-machines-linux-configure-raid.md)

[Artikel zur Konfiguration von Logical Volume Manager auf Linux Azure VM](virtual-machines-linux-configure-lvm.md)



![](./media/virtual-machines-linux-sap-hana-get-started/image003.jpg)

Im Test wurden zwei Azure standard Speicherdatenträger Umgebung SAP-Anwendungsserver VM zugeordnet. Zum einen wurde die SAP-Software für die Installation (z. B. SAP GUI NetWeaver 7.5 SAP HANA... speichern ) und was möglicherweise erforderliche Platz haben (z. B. backup, Daten) sowie Sapmnt-Verzeichnis (z.B. SAP Profile) für alle virtuellen Computer freigegeben werden die gleichen SAP-Landschaft gehören.

![](./media/virtual-machines-linux-sap-hana-get-started/image004.jpg)

VM vier Festplatten wurden entgegen der Anwendungsserver SAP HANA Server VM zugeordnet. Wie zwei Festplatten für SAP Software (eine konnte auch den SAP-Software-Datenträger über NFS freigegeben) halten und genügend Speicherplatz z.B. Backup verwendet wurden. Die weiteren zwei Festplatten wurden Azure Premium-Datenträger zu SAP HANA Daten- und Protokolldateien sowie das Verzeichnis /usr/sap.


### <a name="kernel-parameters"></a>Kernel-Parameter


SAP HANA erfordert Linux Kernel Einstellungen, sind nicht Teil der standardmäßigen Azure Galeriebilder manuell festgelegt werden. Es gibt einen bestimmten Hinweis beschreibt die Einstellungen. 


Hinweis SAP HANA DB: Empfohlene betriebssystemeinstellungen für SLES 12 / SLES für SAP Applikationen 12: [SAP-Hinweis 2205917](https://launchpad.support.sap.com/#/notes/2205917)

Eine weitere Thema Reagrding Seitencache mit SAP HANA auf SLES ausgeführt [hier](https://www.suse.com/documentation/sles_for_sap/singlehtml/sles_for_sap_guide/sles_for_sap_guide.html#sec.s4s.configure.page-cache) in Kapitel 6.1 Kernel: Seite Cache-Grenzwert

Es gibt auch ein Hinweis bezüglich der Seite Cache-Grenzwert [SAP-Hinweis 1557506](https://service.sap.com/sap/support/notes/1557506)

SLES 12 ist ein neues Tool ersetzt das alte Sapconf-Dienstprogramm. Ist "optimiert Adm" und ein SAP HANA Profil verfügbar ist. Weitere Informationen zu diesem Tool, die folgenden zwei Links finden.

SLES Dokumentation optimiert Adm Profil Sap Hana finden Sie [hier](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html)

SLES Dokumentation optimiert Adm Sap-Hana - Profil Kapitel 6.2 Tuning Systeme für SAP Arbeitslasten mit optimiert-Adm - finden Sie [hier](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf)


![](./media/virtual-machines-linux-sap-hana-get-started/image005.jpg)

Eine hier wie "optimiert Adm" die Transparent_hugepage sowie die Numa_balancing entsprechend SAP HANA erforderlich geändert.


Um SAP HANA Kernel Einstellungen permanent hat einen grub2 auf SLES 12 verwenden. Weitere Informationen über grub2 finden Sie [hier](https://www.suse.com/documentation/sled-12/book_sle_admin/data/sec_grub2_file_structure.html)


![](./media/virtual-machines-linux-sap-hana-get-started/image006.jpg)

Dieses Bildschirmabbild zeigt, wie der Kernel wurden in der Konfigurationsdatei geändert und dann "Kompilieren" über grub2-mkconfig


![](./media/virtual-machines-linux-sap-hana-get-started/image007.jpg)

Eine andere Möglichkeit wäre die über Yast die Bootloader-Kernel-Parameter ändern.


### <a name="filesystems"></a>Dateisysteme 

![](./media/virtual-machines-linux-sap-hana-get-started/image008.jpg)

Hier sieht man zwei Dateisysteme auf SAP-Anwendungsserver VM auf den beiden Datenträgern angeschlossenen Azure-Speicher erstellt wurden. Beide Dateisysteme sind vom Typ XFS und /sapdata und /sapsoftware.

Es ist nicht notwendig, dies. Es gibt verschiedene Optionen wie steinlaibung Speicherplatz.
Der wichtigste Aspekt ist zu vermeiden, dass das Root-Dateisystem Speicherplatz ausgeführt wird. 


![](./media/virtual-machines-linux-sap-hana-get-started/image009.jpg)

Über SAP HANA DB VM wichtig während der Datenbankinstallation über Sapinst (Swpm) ist und nur mit der Option einfach "normalen" Installation werden installiert Sachen standardmäßig unter /hana und /usr/sap. Die Standardeinstellung für SAP HANA Sicherung ist z.B. unter /usr/sap.
Wie Schlüssel zu vermeiden ist das Root-Dateisystem der Platz ausgeht. Daher sollte eine sicherstellen, dass vor der Installation von SAP HANA über Swpm ist ausreichend Speicherplatz unter /hana und /usr/sap.

[In diesem Artikel](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm) von SAP beschreibt das Layout standard Dateisystem SAP HANA 


![](./media/virtual-machines-linux-sap-hana-get-started/image010.jpg)

Beim Installieren von SAP NetWeaver auf standard SLES 12 Azure Bild werden eine Meldung, dass es keine austauschen. Um diese Meldung zu entfernen konnte eine z.B. eine Swap-Datei manuell in dieses Dokument Dd, Mkswap und Swapon. Suchen Sie einfach nach "Hinzufügen eines Swap-Datei manuell" in [diesem Artikel](https://www.suse.com/documentation/sled-12/book_sle_deployment/data/sec_yast2_i_y2_part_expert.html)

Eine weitere Option ist Auslagerungsspeicher über Linux VM-Agenten konfigurieren. Weitere Informationen finden Sie [hier](virtual-machines-linux-agent-user-guide.md)


### <a name="etchosts"></a>/ Etc/hosts

![](./media/virtual-machines-linux-sap-hana-get-started/image011.jpg)

Wichtig vor SAP installieren werden Hostnamen und IP-Adressen der SAP VMs in der Datei/etc/Hosts enthalten. Eine SAP VMs innerhalb eines virtuellen Netzwerks Azure bereitzustellen und die interne IP-Adressen.

### <a name="etcfstab"></a>/ Etc/fstab

![](./media/virtual-machines-linux-sap-hana-get-started/image000c.jpg)

Während der Testphase war es eine gute Idee Fstab Nofail Parameter hinzugefügt. Wenn etwas mit den Laufwerken schiefgeht würde VM noch kommen und nicht des Startvorgangs hängen. Aber wie aufpassen, zusätzlichen Speicherplatz möglicherweise nicht zur Verfügung und Prozesse konnte das Root-Dateisystem füllen. Wenn /hana fehlt starten nicht SAP HANA wenn überhaupt.


## <a name="install-graphical-gnome-desktop-on-sles-12"></a>Installieren Sie SLES 12 grafisch Gnome-desktop

Dieses Kapitel besteht aus zwei Secitions die folgenden Themen:

* Installation von Gnome-Desktop und Xrdp auf SLES 12
* Java-basierte SAP MC mit Firefox auf SLES 12 ausführen

Eine können alternativen wie Xterminal, VNC aber Sep 2016 nur beschrieben Xrdp.

### <a name="installation-of-gnome-desktop-and-xrdp-on-sles-12"></a>Installation von Gnome-Desktop und Xrdp auf SLES 12

Speziell für Microsoft Windows-Hintergrund und grafischen Desktop direkt in SAP Linux VMs Firefox, Sapinst, SAP-GUI, SAP MC oder HANA Studio ausführen und vielleicht die VM über RDP auf einem Microsoft Windows-Computer verwenden möchte ist eine einfache Möglichkeit, dies zu erreichen. Während dies z. B. für einen Produktions-Datenbankserver möglicherweise ist es für eine reine Prototyp-Demo-Umgebung. Hier werden die Schritte zur Installation von Gnome-Desktop auf einem virtuellen Computer Azure SLES 12:

Installieren Sie Gnome-Desktop durch den folgenden Befehl (z. B. in einem Putty Fenster):

   Zypper -t Muster Gnome-basic

Installieren Sie Xrdp Verbindung VM über RDP können:

   Zypper in xrdp

/etc/sysconfig/windowmanager bearbeiten und den Standard-Windows-Manager auf Gnome festgelegt:

   DEFAULT_WM = "Gnome"

Chkconfig um sicherzustellen, dass diese Xrdp nach einem Neustart automatisch ausgeführt: 

  Chkconfig-Xrdp auf Ebene 3

für den Fall, dass ein Problem mit RDP-Verbindung sollte versuchen Sie (vielleicht aus dem Putty) neu starten:

  /etc/xrdp/xrdp.sh starten

Xrdp Neustart als erwähnt nicht Check ist eine .pid-Datei arbeiten und entfernen:

  Überprüfen von /var/run und xrdp.pid suchen   
  Entfernen Sie es, und wiederholen Sie den Neustart



### <a name="sap-mc"></a>SAP MC


Starten der graphical Java-basierte SAP MC von Firefox auf einem Azure SLES 12 VM nach der Installation von Gnome erhalten Dekstop wie im vorherigen Abschnitt einen Fehler aufgrund fehlender Java-Browser-Plugin.

Die URL zu der SAP MC <server>: 5 < Instance_number > 13

Weitere Informationen finden Sie [hier](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm)


![](./media/virtual-machines-linux-sap-hana-get-started/image013.jpg)

Auf dem Screenshot kann man sehen, wie die Fehlermeldung aussehen könnte, wenn Java-Browser-Plugin nicht vorhanden ist. 

![](./media/virtual-machines-linux-sap-hana-get-started/image014.jpg)

Eine Möglichkeit zur Lösung des Problems ist einfach fehlende Plug-in über Yast installieren.

![](./media/virtual-machines-linux-sap-hana-get-started/image015.jpg)

Wiederholte SAP MC URL bringt ein wenig Dialogfeld das erste Mal aufgefordert, das Plugin zu aktivieren.


Eine weitere Frage Popup-möglicherweise wird eine Fehlermeldung über eine fehlende Datei: Dies ist wahrscheinlich der Installation Java 1,8 für SAP GUI 7.4 ist javafx.properties

Die IBM Java Version über Yast enthalten nicht diese Datei. Die Lösung ist ein Java-Download von Oracle.
Artikel zu diesem Problem spricht finden Sie [hier](https://scn.sap.com/thread/3908306)



## <a name="manual-sap-hana-installation-via-swpm-as-part-of-a-netweaver-75-installation"></a>Manuelle Installation von SAP HANA über SWPM als Teil der Installation NetWeaver 7.5


Die folgenden Screenshots zeigt die wichtigsten Schritte SAP NetWeaver 7.5 und SAP HANA SP12 über SWPM (Sapinst). NW 7.5 enthält Installation SWPM Funktionen auch die HANA-Datenbank als einzelne Instanz installieren.


![](./media/virtual-machines-linux-sap-hana-get-started/image012.jpg)

Für den Beispieltest wurde Umgebung nur eine ABAP-Anwendungsserver installiert. Die Option "Distributed System" wurde zur ASC-Instanz und die primäre Anwendung-Server-Instanz in einer Azure VM und SAP HANA als Datenbanksystem in einer anderen Azure-VM installieren.


![](./media/virtual-machines-linux-sap-hana-get-started/image016.jpg)

ASC-Instanz auf dem Anwendungsserver VM installiert ist und "Grün" in der SAP MC Sapmnt Verzeichnis umfasst z. B. SAP Profilverzeichnis ist mit SAP HANA-Datenbankserver VM festgelegt ist.
DB-Installationsschritt benötigt Zugriff auf diese Informationen. Am besten ist NFS verwenden, die mit Yast konfiguriert werden können.


![](./media/virtual-machines-linux-sap-hana-get-started/image017b.jpg)

Mithilfe der Optionen "Rw" und "No_root_squash" sollte die Anwendung VM Sapmnt Directory Server über NFS freigegeben werden. Standardwert ist "Ro" und "Root_squash" das zu Problemen führen kann, wenn die Instanz installieren.


![](./media/virtual-machines-linux-sap-hana-get-started/image018b.jpg)

SAP HANA-Datenbankserver VM Freigeben der Sapmnt vom app-Server hat VM über "NFS-Client" konfiguriert werden (z.B. mit Hilfe der Yast)


![](./media/virtual-machines-linux-sap-hana-get-started/image019.jpg)

Dann muss man zu SAP HANA-Datenbankserver VM Anmeldung SWPM um eine verteilte Installation der NW 7.5 - "Instanz" Nächstes erreichen.


![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Im Zusammenhang mit SAP HANA Installation sich nicht tatsächlich zu viele nach einem "normalen" Installation auswählen. Außerdem muss der Pfad zu Installatiom Media eine SID DB, den Hostnamen und die Instanznummer DB Sys Admin-Kennwort eingeben.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Als Nächstes ist das Kennwort für das Schema DBACOCKPIT eingeben.



![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Dann kommt die Frage SAPABAP1 Schema Kennwort.


![](./media/virtual-machines-linux-sap-hana-get-started/image023.jpg)

Am Ende sollte nur grüne Häkchen vor jeder Phase des Installationsvorgangs DB und wenig Meldungsfeld die erscheint sage "Ausführung von... Datenbankinstanz abgeschlossen".

 
![](./media/virtual-machines-linux-sap-hana-get-started/image024.jpg)

Nach der Installation sollte der SAP MC auch Datenbankinstanz als "Grün" und die vollständige Liste der SAP HANA Prozesse (Hdbindexserver, Hdbcompileserver) anzeigen


![](./media/virtual-machines-linux-sap-hana-get-started/image025.jpg)

Dieses Bildschirmabbild zeigt Teile der Dateistruktur unter /hana/shared SWPM Installation HANA erstellt. Es gab keine Möglichkeit, einen anderen Pfad angeben. Deshalb so Speicherplatz unter /hana vor SAP HANA über SWPM zu vermeiden, dass das Root-Dateisystem freier Speicherplatz wird bereitgestellt.


![](./media/virtual-machines-linux-sap-hana-get-started/image026.jpg)

Hier sieht man dasselbe wie für das Verzeichnis /usr/sap beschrieben.


![](./media/virtual-machines-linux-sap-hana-get-started/image027b.jpg)

Der letzte Schritt der verteilten ABAP Installation ist die "primäre Application Server-Instanz"


![](./media/virtual-machines-linux-sap-hana-get-started/image028b.jpg)

Wenn PAS sowie SAP GUI installiert kann eine Transaktion "Dbacockpit", die mit SAP HANA Installation alles sieht überprüfen.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Als letzte konnte Schritt SAP HANA Studio in SAP-Anwendungsserver VM und anschließen auf dem-Datenbankserver VM SAP HANA-Instanz.




## <a name="manual-sap-hana-installation-via-hana-life-cycle-manager-tool-hdblcm"></a>Manuelle Installation von SAP HANA über HANA Lifecycle Manager Tool hdblcm


Neben Installieren von SAP HANA als Teil einer verteilten Installation über SWPM ist es auch möglich, zunächst HANA eigenständige mit Hdblcm und installieren dann z. B. SAP NetWeaver 7.5 danach. Die folgenden Screenshots stellt hierzu.

Hier sind drei Informationen HANA Hdblcm:

[Auswählen der richtigen SAP HANA HDBLCM für Ihre Aufgabe](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)

[SAP HANA Lifecycle Management-Tools](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)

[SAP HANA Serverinstallation and Update Guide](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)



![](./media/virtual-machines-linux-sap-hana-get-started/image030.jpg)

Zum Vermeiden Probleme mit einer Gruppen-Id-Standardeinstellung für die \<HANA SID\>ADM-Benutzer (mit dem Hdblcm erstellt) sollte eine neue Gruppe namens "Sapsys" mit der Kennung 1001 vor der Installation von SAP HANA mit Hdblcm definieren.




![](./media/virtual-machines-linux-sap-hana-get-started/image031.jpg)

Startzeit der ersten Hdblcm werden eine einfache Startmenü hat eine Option 1 "neues System installieren"



![](./media/virtual-machines-linux-sap-hana-get-started/image032.jpg)

Auf diesem Bild sieht man die wichtigsten Optionen vor eingegeben wurden. Wichtig - Verzeichnisse der HANA Protokoll- und Volumes sowie den Installationspfad (Hana/in diesem Beispiel geteilt) benannt wurden und/Usr/Sap sollte nicht Teil des Root-Dateisystems aber gehören Azure Datenträger die VM gemäß Abschnitt Setup Azure VM zugeordnet wurden. Dies verhindert das Risiko, das Root-Dateisystem Speicherplatz kann.
Man sieht auch, dass HANA Administrator Benutzer-Id 1005 und Bestandteil der Sapsys (Id 1001) vor der Installation definiert.



![](./media/virtual-machines-linux-sap-hana-get-started/image033.jpg)

Eine überprüfen die HANA \<HANA SID\>Benutzerdetails in/Etc/Passwd Adm (in diesem Beispiel Azdadm)



![](./media/virtual-machines-linux-sap-hana-get-started/image034.jpg)

Nach der Installation von SAP HANA mit Hdblcm in SAP HANA ersichtlich. SAPABAP1-Schema umfasst z. B. alle SAP NetWeaver Tabellen ist noch nicht verfügbar.



![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Nachdem eine SAP HANA SAP NetWeaver darauf installieren können. "Verteilte Installation" mithilfe über SWPM im entsprechenden Abschnitt beschriebenen der in diesem Beispiel vorgenommen wurde.
Installation nur die Datenbankinstanz über eine SWPM verfügt die gleichen Daten wie vor mit Hdblcm (z. B. Hostname, HANA SID Instanznummer) eingeben. SWPM wird die vorhandene HANA Installation und zusätzliche Schemas hinzufügen.



![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Dies ist das Bild Installationsschritt SWPM hat eine Daten über DBACOCKPIT Schemas eingeben.


![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Dann erwartet SWPM Daten über SAPABAP1 Schemas eingeben.


![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Nach Abschluss der SWPM Datenbank Instanz Installation sieht man das Schema SAPABAP1 HANA Studio.



![](./media/virtual-machines-linux-sap-hana-get-started/image039b.jpg)

Und schließlich nach Installation des SAP-Anwendungsserver und der SAP-GUI sollte man HANA Datenbankinstanz Transaktion "Dbacockpit" überprüfen.




## <a name="general-information-related-to-sap-azure-certifications-running-sap-hana-on-azure-and-sap-software-download"></a>Allgemeine Informationen zum SAP Azure Zertifizierungen unter SAP HANA Azure und SAP-Software-download

* Allgemeine SAP Azure Doku über SAP auf Azure mit Windows-Betriebssystem im klassischen Modus ausgeführt: [Mithilfe von SAP auf virtuelle Windows-Maschinen in Azure](virtual-machines-windows-classic-sap-get-started.md)

* Informationen zu SAP-Vorlagen für die Verwendung durch Kunden: [Azure Schnellstart Vorlagen für SAP](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/)

* Allgemeine SAP Azure Doku zur Ausführung von SAP auf Azure mit Linux-Betriebssystem in Azure-Ressourcen-Manager-Modell: [Mithilfe von SAP auf Linux virtuellen Maschinen (VMs)](virtual-machines-linux-sap-get-started.md)

* zertifizierte SAP HANA Hardware Directory Listet die Azure VM für die Produktion unterstützt werden: [Zertifizierte SAP HANA® Hardware Verzeichnis](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)

* Informationen zu virtuellen Größen speziell für Linux-Workloads: [Größen für virtuelle Computer in Azure](virtual-machines-linux-sizes.md)

* Hinweis Die Liste SAP-Produkten auf Azure und Azure VM-Typen für SAP: [SAP-Hinweis 1928533](https://launchpad.support.sap.com/#/notes/1928533/E)

* Hinweis zu SAP "Erweiterte Überwachung" Linux VMs auf Azure: [SAP-Hinweis 2191498](https://launchpad.support.sap.com/#/notes/2191498/E)

* SAP HANA auf Azure "Große Instanzen". Es ist wichtig zu verstehen, dass dies nicht zur Ausführung von SAP HANA auf Azure VMs aber in eine hybride Umgebung, in der SAP-Anwendungsserver in Azure VMs aber SAP HANA auf blanken Servern ausgeführt wird: [SAP-Hinweis 2316233](https://launchpad.support.sap.com/#/notes/2316233/E)

* Hinweis Informationen über SAPOSCOL Linux: [SAP-Hinweis 1102124](https://launchpad.support.sap.com/#/notes/1102124/E)

* Schlüssel überwachen Metriken für SAP auf Microsoft Azure: [Hinweis 2178632](https://launchpad.support.sap.com/#/notes/2178632/E)

* Informationen zum Azure-Ressourcen-Manager: [Azure-Ressourcen-Manager (Übersicht)](../azure-resource-manager/resource-group-overview.md)

* Informationen zur Bereitstellung von Linux VMs über Vorlagen: [Bereitstellen und managen von virtuellen Computern mithilfe von Azure-Ressourcen-Manager Vorlagen und Azure-CLI](virtual-machines-linux-cli-deploy-templates.md)

* Vergleich der Bereitstellung zwischen Azure-Ressourcen-Manager und Classic Modelle: [Azure Resource Manager im Vergleich zu klassischen Bereitstellung: verstehen Bereitstellungsmodelle und den Zustand Ihrer Ressourcen](../resource-manager-deployment-model.md)

* NetWeaver 7.5 für Linux/HANA von SAP Service Marketplace herunter![](./media/virtual-machines-linux-sap-hana-get-started/image001.jpg)

* HANA SP12 Platform Edition von SAP Service Marketplace herunter![](./media/virtual-machines-linux-sap-hana-get-started/image002.jpg)

