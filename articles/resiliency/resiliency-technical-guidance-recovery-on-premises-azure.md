<properties
   pageTitle="Technische Anleitung: Wiederherstellung von lokalen Azure | Microsoft Azure"
   description="Artikel zu verstehen und Entwerfen Recovery Systeme vor-Ort-Infrastruktur in Azure"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-on-premises-to-azure"></a>Technische Anleitung Azure Stabilität: Wiederherstellung von lokalen Azure

Azure bietet eine umfassende Palette von Services für ermöglicht einem lokalen Rechenzentrum in Azure für hohe Verfügbarkeit und Disaster Recovery:

* __Netzwerk__: mit einem virtuellen privaten Netzwerk erweitern Sie sicheren lokalen Netzwerk in die Cloud.
* __Berechnen__: mit Hyper-V für lokale Kunden können "Heben und verschieben" vorhandenen virtuellen Maschinen (VMs) in Azure.
* __Speicher__: StorSimple erweitert das Dateisystem zum Azure-Speicher. Der Azure Backup bietet Backup für Dateien und Datenbanken von SQL Azure-Speicher.
* __Replikation__: mit SQL Server 2014 (oder höher) Verfügbarkeit, implementieren Sie hohe Verfügbarkeit und Disaster Recovery für Ihre lokalen Daten.

##<a name="networking"></a>Netzwerk

Azure Virtual Network können logisch isolierten Bereich in Azure erstellen und sicheren Rechenzentrum vor Ort oder einem einzelnen Clientcomputer mithilfe einer IPSec-Verbindungs herstellen. Mit virtuellen Netzwerk können Sie Infrastruktur skalierbar, bei Bedarf in Azure nutzen und bietet Konnektivität zu Daten und Programme lokal, einschließlich Computern unter Windows Server, Mainframes und UNIX. Weitere Informationen finden Sie unter [Azure Netzwerk Dokumentation](../virtual-network/virtual-networks-overview.md) .

##<a name="compute"></a>Berechnen

Wenn Sie lokal Hyper-V verwenden, Sie können "Heben und verschieben" vorhandenen virtuellen Computern in Azure und Dienstanbieter (oder höher), Windows Server 2012 ausgeführt, ohne die VM Änderungen oder konvertieren VM formatiert. Weitere Informationen finden Sie unter [Festplatten und Festplatten für Azure virtuelle Maschinen](../virtual-machines/virtual-machines-linux-about-disks-vhds.md).

##<a name="azure-site-recovery"></a>Azure Site Recovery

Disaster Recovery bietet als Dienst (DRaaS), Azure [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/). Azure Site Recovery bietet umfassenden Schutz für VMware, Hyper-V und physischen Servern. Mit Azure Site Recovery können Sie einen anderen lokalen Server oder Azure als Recovery-Standort. Weitere Informationen zu Azure Site Recovery finden Sie in [Azure Site Recovery-Dokumentation](https://azure.microsoft.com/documentation/services/site-recovery/).

##<a name="storage"></a>Speicher

Es gibt mehrere Optionen für die Verwendung von Azure als ein Ort für lokale Daten.

###<a name="storsimple"></a>StorSimple

StorSimple kombiniert sichere Cloud-Speicher für lokale Applikationen transparent. Zudem eine einzige Appliance, die leistungsstarke tiered lokale und Cloud-Speicher, live Archivierung cloudbasierten Datenschutz und Disaster Recovery bietet. Weitere Informationen finden Sie auf der [Produktseite StorSimple](https://azure.microsoft.com/services/storsimple/).

###<a name="azure-backup"></a>Azure Backup

Azure Backup ermöglicht Cloud Backups mithilfe der bekannten backup-Tools in Windows Server 2012 (oder höher), Windows Server 2012 Essentials (oder höher) und System Center 2012 Data Protection Manager (oder höher). Diese Tools bieten einen Workflow zum backup-Management, die unabhängig von den Speicherort für die Sicherungskopien oder einer lokalen Festplatte Azure-Speicher. Nachdem Daten in die Cloud gesichert, können autorisierte Benutzer problemlos Backups auf jedem Server wiederherstellen.

Mit inkrementellen Sicherung werden nur die geänderten Dateien in die Cloud übertragen. Dadurch werden effizient Speicherplatz reduzieren Bandbreite und Point-in-Time-Recovery mehrerer Versionen der Daten unterstützt. Sie können auch zusätzliche Features wie Datenaufbewahrungsrichtlinien Datenkompression und Drosselung Datenübertragung verwenden. Mithilfe von Azure als Speicherort der Sicherung hat den Vorteil, dass die Backups automatisch "extern". Dadurch werden zusätzlichen Vorschriften sichern und schützen Sicherungsmedien vor Ort.

Weitere Informationen finden Sie unter [Neuigkeiten Azure Backup?](../backup/backup-introduction-to-azure-backup.md) und [Azure Backup für DPM-Daten konfigurieren](https://technet.microsoft.com/library/jj728752.aspx).

##<a name="database"></a>Datenbank

Sie haben eine Disaster Recovery-Lösung für die SQL Server-Datenbanken in einer Hybrid-IT-Umgebung mit AlwaysOn Availability Gruppen, später Protokollversand und Backup und Wiederherstellung mit Azure BLOB-Speicher. Alle Lösungen verwenden SQL Server auf Azure virtuellen Computer ausgeführt.

AlwaysOn Availability Gruppen können verwendet werden, in einer Hybrid-IT-Umgebung Datenbankreplikate sowohl lokal bestehen und in der Cloud. Dies wird im folgenden Diagramm dargestellt.

![SQL Server AlwaysOn Availability Gruppen in einer hybriden Cloud-Architektur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-3.png)

Später kann auch auf lokalen Servern und der Cloud in einer zertifikatbasierten Installation umfassen. Das folgende Diagramm illustriert dieses Konzept.

![SQL Server-Database mirroring in einer hybriden Cloud-Architektur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-4.png)

Protokollversand kann eine lokale Datenbank mit einer SQL Server-Datenbank in Azure virtuellen Computer verwendet werden.

![SQL Server für den Protokollversand in einer hybriden Cloud-Architektur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-5.png)

Schließlich können Sie eine lokale Datenbank direkt in Azure BLOB-Speicher sichern.

![Sichern von SQL Server Azure BLOB-Speicher in einer hybriden Cloud-Architektur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-6.png)

Weitere Informationen finden Sie unter [hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure virtuellen Maschinen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md) und [Backup und Wiederherstellung für SQL Server in Azure virtuellen Maschinen](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).

##<a name="checklists-for-on-premises-recovery-in-microsoft-azure"></a>Prüflisten für die lokale Recovery in Microsoft Azure

###<a name="networking"></a>Netzwerk

  1. Überprüfen Sie Netzwerk-Abschnitt dieses Dokuments.
  2. Verwenden Sie virtuelles Netzwerk sichere Verbindung lokal in die Cloud.

###<a name="compute"></a>Berechnen

  1. Überprüfen Sie den Compute-Abschnitt dieses Dokuments.
  2. VMs zwischen Hyper-V und Azure verschieben.

###<a name="storage"></a>Speicher

  1. Lesen Sie den Abschnitt Speichern dieses Dokuments.
  2. StorSimple-Dienste für die Verwendung von Cloud-Speicher nutzen.
  3. Nutzen Sie Azure Backup.

###<a name="database"></a>Datenbank

  1. Überprüfen Sie den Datenbank-Abschnitt dieses Dokuments.
  2. Sollten Sie SQL Server in Azure VMs der Sicherung.
  3. AlwaysOn Availability Gruppen einrichten.
  4. Zertifikatbasierte später konfigurieren.
  5. Verwenden des Protokollversands.
  6. Sichern Sie lokalen Datenbanken in Azure BLOB-Speicher.

##<a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist Teil einer Reihe [Azure Resiliency technische](./resiliency-technical-guidance.md)Anleitung. Im nächste Artikel dieser Reihe ist [beschädigte Daten oder versehentliches Löschen](./resiliency-technical-guidance-recovery-data-corruption.md).