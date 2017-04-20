<properties
    pageTitle="Replizieren lokalen virtuellen VMware Maschinen oder physischen Servern an einem sekundären Standort | Microsoft Azure"
    description="Anhand dieses Artikels virtuelle VMware-Computer oder Windows-Linux-Servern an einem sekundären Standort mit Azure Site Recovery replizieren."
    services="site-recovery"
    documentationCenter=""
    authors="nsoneji"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="nisoneji"/>


# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Replizieren Sie lokalen VMware virtuellen Computern oder Servern an einem sekundären Standort


## <a name="overview"></a>Übersicht

InMage Scout in Azure Site Recovery bietet Echtzeit-Replikation zwischen lokalen VMware. InMage Scout ist in Azure Site Recovery Service Abonnements enthalten.


## <a name="prerequisites"></a>Erforderliche Komponenten

**Ein Azure-Konto**: Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Sie können eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)starten. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Site Recovery Preise.


## <a name="step-1-create-a-vault"></a>Schritt 1: Erstellen eines Depots
1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.
2. Klicken Sie auf Neu > Verwaltung > Backup und Site Recovery (OMS). Sie können auch auf Durchsuchen klicken > Recovery Services Depot > hinzufügen.
3. **Name** Geben Sie einen Anzeigenamen zu Tresor an. Haben Sie mehrere Abonnements wählen Sie davon.
4. In **Ressourcengruppe** eine neue Ressourcengruppe erstellen oder bestehende auswählen. Geben Sie eine Azure Region erforderlichen Felder ausfüllen. 
5.  Wählen Sie **Speicherort**die geografische Region für das Depot. Überprüfen Sie unterstützte Regionen [Azure Site Recovery-Preise](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Möchten Sie schnell das Depot aus dem Dashboard auf Pin Dashboard und klicken Sie auf erstellen.
6. Neue Depot wird im Schaltpult angezeigt > alle Ressourcen und Recovery Hauptdienste Tresoren Blade.

## <a name="step-2-configure-the-vault-and-download-inmage-scout-components"></a>Schritt 2: Konfigurieren Sie das Depot und Komponenten InMage Scout
7. Recovery Services Depots Blade Vault und klicken Sie auf Settings.
8. **Einstellungen** > **Erste Schritte** klicken Sie auf **Site Recovery** > Schritt 1: **Vorbereiten Infrastruktur** > **Schutzziel**.
9. **Schutzziel** Recovery-Standort wählen Sie aus, und wählen Sie Ja, mit VMware vSphere Hypervisor. Klicken Sie auf OK.
10. **Scout Setup**klicken Sie auf Download Download InMage Scout 8.0.1 GA Software und Registrierung Schlüssel. Die Installationsdateien für alle erforderlichen Komponenten sind in der heruntergeladenen ZIP-Datei.


## <a name="step-3-install-component-updates"></a>Schritt 3: Komponente installieren

Lesen Sie die neuesten [Updates](#updates). Sie installieren die Update-Dateien auf Servern in der folgenden Reihenfolge:

1. RX-Server ist eine
2. Konfigurationsserver
3. Prozess-Server
3. Master Zielserver
4. vContinuum Server
5. Quellserver (Windows und Linux-Server)

Installieren Sie die Updates wie folgt:

1. [Aktualisieren](https://aka.ms/asr-scout-update4) ZIP-Datei herunterladen Diese ZIP-Datei enthält die folgenden Dateien:

    - RX_8.0.4.0_GA_Update_4_8725872_16Sep16.TAR.gz
    - CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
    - UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe
    - UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
    - vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe
    - UA update4 Bits für RHEL5, OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz 
    
2. Extrahieren Sie die ZIP-Dateien.<br>
3. **Server für RX**: Kopie **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** Server RX und. Führen Sie im extrahierten **Ordner/Installieren**.<br>
4. **Für Server-Prozess Konfigurationsserver**: Kopie **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** Konfigurationsserver und Prozess-Server. Doppelklicken Sie auf ausführen.<br>
5. **Master für die Windows-Zielserver**: Aktualisieren Sie einheitlichen Agent **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** auf master Zielserver kopieren. Doppelklicken Sie auf ausführen. Beachten Sie, dass der einheitliche Agent auch auf dem Quellserver. Auf dem Quellserver als auch sollte Installation wie in dieser Liste.<br>
7. **Für den Server vContinuum**: **vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe** mit dem vContinuum-Server kopieren.  Stellen Sie sicher, dass Sie den vContinuum Assistenten geschlossen haben. Doppelklicken Sie auf die Datei, um Sie auszuführen.<br>
8. **Master Zielserver für Linux**: Aktualisieren Sie einheitlichen Agent **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** auf dem master Zielserver kopieren und extrahieren Sie sie. Führen Sie im extrahierten **Ordner/Installieren**.<br>
9. **Für die Windows-Quellserver**: Aktualisieren Sie einheitlichen Agent kopieren Sie **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** auf dem Quellserver. Doppelklicken Sie auf ausführen.<br>
10. **Für Linux-Quellserver**: Aktualisieren Sie einheitlichen Agent entsprechende Version UA Datei Linux-Server kopieren und extrahieren Sie sie. Führen Sie im extrahierten **Ordner/Installieren**.  Beispiel: RHEL 6,7 64-Bit-Server **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** auf den Server kopieren und extrahieren. Führen Sie im extrahierten **Ordner/Installieren**.

## <a name="step-4-set-up-replication"></a>Schritt 4: Einrichten von Replikation
1. Replikation zwischen eingerichtet und VMware Websites ausgerichtet.
2. Verwenden Sie Anleitung InMage Scout-Dokumentation, die heruntergeladen wird mit dem Produkt. Alternativ können Sie die Dokumentation wie folgt zugreifen:

    - [Versionshinweise](https://aka.ms/asr-scout-release-notes)
    - [Kompatibilitätsmatrix für](https://aka.ms/asr-scout-cm)
    - [Benutzerhandbuch](https://aka.ms/asr-scout-user-guide)
    - [RX-Benutzerhandbuch](https://aka.ms/asr-scout-rx-user-guide)
    - [Schnellinstallations-Handbuch](https://aka.ms/asr-scout-quick-install-guide)


## <a name="updates"></a>Updates

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Site Recovery Scout 8.0.1 Update 4
Scout Update 4 ist ein kumulatives Update. Es sind alle Updates von update1 bis update3 und folgende neue Fehlerbehebungen und Optimierungen.

**Neue Plattformen** 

- Unterstützung für vCenter/vSphere 6.0, 6.1 und 6.2 hinzugefügt
- Für die folgenden Linux-Betriebssysteme unterstützt
    - Red Hat Enterprise Linux (RHEL) 7.0, 7.1 und 7.2 
    - CentOS 7.0, 7.1 und 7.2
    - Red Hat Enterprise Linux (RHEL) 6,8
    - CentOS 6.8

>[AZURE.NOTE]
>
> RHEL/CentOS 7 64-Bit- **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** Lieferumfang Basispaket Scout GA **InMage_Scout_Standard_8.0.1 GA.zip**. Paket Scout GA Portal wie in [Schritt 1](site-recovery-vmware-to-vmware.md#Step 1: Create a vault)beschrieben.

**Fehlerbehebungen und Optimierungen** 

- Verbessertes Herunterfahren für die folgenden Linux OSes und Clones unerwünschte Resync Probleme.
    - Red Hat Enterprise Linux (RHEL) 6.x
    - Oracle Linux (OL) 6.x
- Unter Linux schwerhörig Ordner Berechtigungen einheitliches Verzeichnis jetzt nur für den lokalen Benutzer beschränkt werden.
- Windows geladen Timeout Problem beim Ausstellen allgemeine verteilte Konsistenz Lese-Zeichen auf stark verteilte Programme wie SQL und Freigabe.
- Zusätzliche Protokolldateien verwandte Fix CX-Basis Installer.
- Windows-Master Ziel Basis Installer VMware vCLI 6.0 Downloadlink hinzugefügt.
- Weitere überprüft und für Netzwerk-Konfigurationen ändert hinzugefügt während Failover und DR Werkzeuge.
- Die CX wird nicht irgendwann Aufbewahrung Informationen gemeldet.  
- Für physische Cluster ausfällt Volume Größe Vorgang vContinuum Assistenten als Quell-Volume verkleinern geschah.
- Clusterschutz ist fehlgeschlagen "Konnte die Datenträgersignatur zu" Wenn Clusterdatenträger PRDM Datenträger ist.
- Cxps transport Serverausfalls aufgrund einer Ausnahme außerhalb des Bereichs. 
- Servername und IP-Spalten werden auf Push-Installation des vContinuum-Assistenten geändert.
- RX-API Enhancements
    - Bietet fünf neuesten verfügbaren allgemeinen Konsistenz (nur Tags).
    - Kapazität und Speicherplatz Details bietet für alle geschützten Geräte.
    - Bietet Scout Treiberzustand auf. 
    
>[AZURE.NOTE] 
>
>- **InMage_Scout_Standard_8.0.1_GA.zip** -Basispaket wurde jetzt CX-Basis Installer **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** und Windows-Master Ziel Basis Installer **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**aktualisiert. Verwenden Sie für alle neuen Installation neue CX und Windows Master Ziel GA Bits.
>- Update 4 direkt einsetzbar auf 8.0.1 GA.
>- Konfigurationsserver und RX-Updates können nicht zurückgesetzt werden, nachdem sie auf das System angewendet werden.

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Site Recovery Scout 8.0.1 Update 3
Update 3 enthält die folgenden Fehlerbehebungen und Optimierungen:

- Konfigurationsserver und RX fehl Site Recovery Depot registrieren, wenn sie hinter dem Proxy.
- Die Anzahl der Stunden, die Recovery Point Objective (RPO) nicht erfüllt wird nicht im Statusbericht aktualisiert.
- Der Konfigurationsserver Synchronisierung ist nicht mit RX ESX Hardwaredetails oder Netzwerkdaten UTF-8-Zeichen enthalten.
- Windows Server 2008 R2-Domänencontroller nicht nach Wiederherstellung starten.
- Offline-Synchronisierung funktioniert nicht wie erwartet.
- Nach dem virtuellen Computer (VM) Failover Replikations-Paar löschen in der CX-UI lange bleibt und Benutzer können das Failback abgeschlossen oder Vorgang fortsetzen.
- Insgesamt wird Snapshot-Konsistenz Auftrag durch Vorgänge optimiert haben Anwendung reduzieren wie SQL-Clients getrennt.
- Die Leistung des Werkzeugs Konsistenz (VACP.exe) verbessert dadurch die Speicherverwendung für das Erstellen von Snapshots für Windows erforderlich ist.
- Der Push installieren stürzt ab, wenn das Kennwort länger als 16 Zeichen ist.
- vContinuum nicht überprüfen und neue vCenter Anmeldeinformationen anzufordern, wenn die Anmeldeinformationen nicht geändert werden.
- Unter Linux downloadet master Ziel Cache-Manager (Cachemgr) nicht vom Prozess-Server Dateien die Replikation Paar Einschränkung führt.
- Wenn die Festplattenreihenfolge physischen Failover Cluster (MSCS) nicht auf allen Knoten ist, ist für einige Cluster-Volumes nicht Replikation festgelegt.
<br/>Beachten Sie, dass Cluster reprotected sein, um dieses Update zu nutzen.  
- SMTP-Funktionen funktioniert nicht wie erwartet, nachdem RX Scout 8.0.1 Scout 7.1 aktualisiert wird.
- Weitere Statistiken wurden im Protokoll vom Rollbackvorgang nachverfolgt, der die an.
- Unterstützung wurde für Linux-Betriebssysteme auf dem Quellserver hinzugefügt:
    - Red Hat Enterprise Linux (RHEL) 6 Update 7
    - CentOS 6 aktualisieren 7
- CX und RX Benutzeroberfläche jetzt zeigen die Benachrichtigung für das Paar die in Bitmap-Modus wechselt.
- Die folgenden Sicherheitsupdates wurden im RX hinzugefügt:

**Beschreibung**|**Umsetzung**
---|---
Autorisierung über Manipulierung umgehen|Beschränkt den Zugriff auf nicht für Benutzer.
Fälschung von Cross-Site-Anfragen|Implementiert das Konzept Seite Token nach dem Zufallsprinzip für jede Seite generiert. <br/>Dadurch sehen Sie: <li> Es ist nur eine einzige Anmeldung Instanz für denselben Benutzer.</li><li>Seite Aktualisieren funktioniert nicht – werden Sie zum Dashboard umgeleitet.</li>
Datei hochladen|Eingeschränkte Dateien Erweiterungen. Zugelassen werden: 7z, Aiff, Asf, Avi, Bmp, Csv, Doc, Docx fla, Flv, Gif, Gz, Gzip, Jpeg, Jpg, Protokoll mid, Mov, mp3, mp4, Mpc, Mpeg, mpg, ods Odt, Pdf, Png, ppt, Pptx, Pxd, qt, Ram, Rar, Rm, Rmi, Rmvb, Rtf, deza, Sitd, Swf, Sxc, Sxw, Tar, Tgz, Tif, Tiff, Txt, Vsd, Wav, Wma, Wmv, Xls, Xlsx, Xml, und Zip.
Permanente Cross-Site scripting | Input Validierung hinzugefügt.


>[AZURE.NOTE]
>
>-  Alle Site Recovery-Updates sind kumulativ. Update 3 sind alle Updates Update 1 und Update 2. Update 3 direkt einsetzbar auf 8.0.1 GA.
>-  Konfigurationsserver und RX-Updates können nicht zurückgesetzt werden, nachdem sie auf das System angewendet werden.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Site Recovery Scout 8.0.1 Update (Update 03 15 Dec) 2

Problembehebungen in Update 2:

- **Konfigurationsserver**: Update für ein Problem, das die 31 Tage kostenlose metering-Funktion verhindert bei der Konfigurationsserver Site Recovery erfasst wurde.
- **Unified Agent**: ein Problem in Update 1 nicht installiert auf dem master Zielserver beim Aktualisieren von Version 8.0 8.0.1 Update zu beheben.


### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Site Recovery Scout 8.0.1 Update 1

Update 1 enthält die folgenden Fehlerkorrekturen und neue Funktionen:

- 31 Tage lang kostenlosen Schutz pro Server-Instanz. Dadurch können Sie Funktionalität testen oder ein Proof-of-Concept.
    - Alle Vorgänge auf dem Server, einschließlich Failover und Failback können die ersten 31 Tage ab, die ein Server mit Site Recovery geschützt ist.
    - Ab dem 32. Tag jeden geschützten Server standard Instanz Rate Azure Site Recovery Schutz einer Site im Besitz des Kunden berechnet.
    - Gleichzeitig ist die Anzahl der geschützten Server derzeit geladen werden auf der Dashboardseite Depot Azure Site Recovery.
- Unterstützt für vSphere Befehlszeilenschnittstelle (vCLI) 5.5 Update 2.
- Unterstützung für Linux-Betriebssysteme auf dem Quellserver:
    - RHEL 6 Update 6
    - RHEL 5 11 aktualisieren
    - CentOS 6 Update 6
    - CentOS 5 Update 11
- Fehler behebt die folgenden Probleme:
    - Vault-Registrierung Konfigurationsserver oder RX Server schlägt fehl.
    - Cluster-Volumes nicht wie erwartet, wenn virtuelle Clustercomputer reprotected werden, wenn sie wieder angezeigt.
    - Failback schlägt fehl, wenn der master Zielserver auf einem anderen Server ESXi aus lokalen Produktion virtuelle Computer gehostet wird.
    - Konfiguration Dateiberechtigungen werden beim Aktualisieren auf 8.0.1, Schutz und Vorgänge geändert.
    - Resynchronisation Schwellenwert nicht durchgesetzt, wie erwartet zu inkonsistenten Replikationsverhalten führt.
    - RPO-Einstellungen werden in der Konfigurationsschnittstelle Server nicht korrekt angezeigt. Nicht komprimierte Datenwert zeigt fälschlicherweise den komprimierten Wert.
    -  Der Entfernungsvorgang nicht erwartungsgemäß in den vContinuum-Assistenten löschen und Replikation nicht von Server-Konfigurationsschnittstelle gelöscht.
    -  Im vContinuum-Assistenten ist der Datenträger automatisch deaktiviert, wenn **Details** an Datenträger beim Schutz von MSCS virtuelle Computer klicken.
    - Während des physisch zu virtuell (P2V) Szenarios nicht erforderliche HP Services wie CIMnotify und CqMgHost, manuelle Wiederherstellung virtueller Computer verschoben. Dies führt zu zusätzlicher booten.
    - Linux VM Schutz schlägt bei mehr als 26 Laufwerke auf dem master Zielserver.

## <a name="next-steps"></a>Nächste Schritte

Fragen Sie in [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)zu buchen.
