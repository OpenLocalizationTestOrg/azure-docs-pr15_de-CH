<properties
    pageTitle="Best Practices für die SQL Server-Leistung | Microsoft Azure"
    description="Bietet Tipps zum Optimieren der Leistung von SQL Server in Microsoft Azure VMs."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/07/2016"
    ms.author="jroth" />

# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>Best Practices zur Performance für SQL Server in Azure Virtual Machines

## <a name="overview"></a>Übersicht

Dieses Thema enthält Tipps zum Optimieren der Leistung von SQL Server in Microsoft Azure Virtual Machine. Beim Ausführen von SQL Server in Azure Virtual Machines, sollten Sie weiterhin dieselbe Datenbankperformance tuning-Optionen für SQL Server in lokalen Server-Umgebung verwenden. Die Leistung einer relationalen Datenbank in einer öffentlichen Cloud hängt jedoch viele Faktoren wie die Größe eines virtuellen Computers und der Konfiguration der Datenträger.

Wenn SQL Server Bilder [sollten Sie Ihre VMs in Azure-Portal-Bereitstellung](virtual-machines-windows-portal-sql-server-provision.md)zu erstellen. SQL Server VMs bereitgestellt im Portal mit Ressourcen-Manager implementieren alle diese best Practices, einschließlich der Speicherkonfiguration.

Dieser Artikel konzentriert sich auf *die Leistung von SQL Server* auf Azure VMs. Die Arbeitslast ist weniger anspruchsvoll, benötigen Sie nicht alle aufgeführten Optimierung. Betrachten Sie Ihre Leistung und Auslastung Muster Bewertung dieser Vorschläge.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="quick-check-list"></a>Kurze Checkliste

Nachfolgend eine kurze Checkliste für eine optimale Leistung von SQL Server auf Azure-Computer:

|Bereich|Optimierungen|
|---|---|
|[Virtueller Speicher](#vm-size-guidance)|[DS3](virtual-machines-windows-sizes.md#standard-tier-ds-series) für SQL Enterprise Edition.<br/><br/>[DS2](virtual-machines-windows-sizes.md#standard-tier-ds-series) für SQL Standard Edition und Web Edition.|
|[Speicher](#storage-guidance)|Verwenden Sie [Premium-Speicherung](../storage/storage-premium-storage.md). Standardspeicher ist nur für Test-/empfohlen.<br/><br/>Halten Sie [Konto](../storage/storage-create-storage-account.md) und SQL Server-VM im selben Bereich.<br/><br/>Deaktivieren Sie Azure- [Geo-redundanten Speicher](../storage/storage-redundancy.md) (Geo-Replikation) für das Speicherkonto.|
|[Datenträger](#disks-guidance)|Verwenden Sie mindestens 2 [P30 Datenträger](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) (1 für Protokolldateien; 1 für Datendateien und TempDB).<br/><br/>Verwenden Sie Betriebssystem oder temporären Datenträger Datenbankspeicher oder protokollieren.<br/><br/>Aktivieren Lese-Cache auf den Datenträgern hosting Daten- und TempDB.<br/><br/>Aktivieren Sie nicht Zwischenspeichern auf Datenträgern hosting der Protokolldatei.<br/><br/>Wichtig: Beenden des SQL Server-Dienstes beim Cache-Einstellungen für einen Datenträger Azure VM ändern.<br/><br/>Mehrere Azure Datenträger e/a-Durchsatz zu verteilen.<br/><br/>Formatieren Sie mit dokumentierten Belegungsgrößen.|
|[E/A](#io-guidance)|Aktivieren Sie Datenbank seitenkomprimierung.<br/><br/>Aktivieren Sie für Datendateien sofort initialisieren.<br/><br/>Einschränken Sie oder deaktivieren Sie die automatische Vergrößerung der Datenbank.<br/><br/>Deaktivieren Sie die automatische Verkleinerung der Datenbank.<br/><br/>Verschieben Sie alle Datenbanken auf Datenträger, einschließlich Datenbanken.<br/><br/>Wechseln Sie SQL Server-Fehler Serverprotokoll und Verzeichnisse zum Datenträger<br/><br/>Backup und Datenbank Standardspeicherorte einrichten.<br/><br/>Aktivieren Sie gesperrte Seiten.<br/><br/>SQL Server-Leistung Updates anwenden.|
|[Bestimmte Funktion](#feature-specific-guidance)|Direkt auf BLOB-Speicher sichern.|

Weitere Informationen darüber, *wie* und *Warum* diese Optimierungen zu überprüfen Sie die Details und in den folgenden Abschnitten.

## <a name="vm-size-guidance"></a>VM Größe Anleitung

Leistung vertrauliche handelt empfiehlt es sich, die [Größe der virtuellen Computer](virtual-machines-windows-sizes.md)verwenden:

- **SQL Server Enterprise Edition**: DS3 oder höher

- **SQL Server Standard Edition und Web Edition**: DS2 oder höher


## <a name="storage-guidance"></a>Speicher-Hinweise

DS-Serie (mit DSv2 und GS-Serie) VMs unterstützt [Storage Premium](../storage/storage-premium-storage.md). Premium-Speicher für alle Produktionsarbeitslasten empfohlen.

> [AZURE.WARNING] Standardspeicher hat unterschiedliche Latenz und Bandbreite und sollte nur für Test-/Arbeitslasten. Produktions-Workloads verwenden Premium-Speicher.

Darüber hinaus empfiehlt Microsoft Azure Storage-Konto im gleichen Rechenzentrum als Ihr SQL Server virtuelle Computer übertragen verzögert zu erstellen. Beim Erstellen ein Speicherkonto deaktivieren Sie Geo-Replikation, da nicht konsistente Schreibreihenfolge über mehrere Festplatten gewährleistet. Stattdessen Sie eine SQL Server-Disaster Recovery-Technologie zwischen zwei Azure Rechenzentren konfigurieren. Weitere Informationen finden Sie unter [hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Festplatten-Hinweise

Es gibt drei wichtigsten Datenträgertypen auf Azure-VM:

- **Betriebssystem-Datenträger**: beim Erstellen von Azure Virtual Machine anfügen die Plattform mindestens ein Datenträger (als Laufwerk **C** ) für die VM für Ihre Betriebssystem-CD. Diese Diskette ist eine virtuelle Festplatte als Seitenblob im Speicher gespeichert.
- **Temporärer Speicherplatz**: Azure Virtual Machines enthalten einen anderen Datenträger bezeichnet die temporäre Diskette (als **D**: Laufwerk). Dies ist ein Datenträger auf dem Knoten für Speicherbereich verwendet werden kann.
- **Datenträger**: Sie können auch weitere Datenträger zum virtuellen Computer anfügen, als Datenträger und diese werden als Seitenblobs im Speicher gespeichert werden.

Den folgenden Abschnitten werden empfohlene Vorgehensweisen zur Verwendung dieser unterschiedlichen Datenträgern.

### <a name="operating-system-disk"></a>Betriebssystem-CD

Ein Betriebssystem-Datenträger ist eine virtuelle Festplatte, die Sie starten und als ausgeführte Version eines Betriebssystems bereitstellen und als Laufwerk **C** gekennzeichnet.

Standardcachingrichtlinie auf dem Betriebssystem-Datenträger ist **Schreibgeschützt**. Für vertrauliche Applikationen Leistung empfiehlt Datenträger statt der Betriebssystem-CD verwenden. Abschnitt unten Datenträger.

### <a name="temporary-disk"></a>Temporärer Speicherplatz

Vorübergehender Laufwerk **D**gekennzeichnet: Laufwerk, Azure Blob-Speicher nicht beibehalten. Speichern Sie die Benutzerdatenbank-Dateien oder Benutzer Transaktionsprotokolldateien auf **D**: Laufwerk.

D-Serie, Dv2-Serie und G-Serie VMs basiert auf das temporäre Laufwerk auf die VMs SSD. Wenn Ihre Arbeitslast TempDB verwendet wird (z.B. temporäre Objekte oder komplexen Joins) führen TempDB auf Laufwerk **D** gespeichert TempDB Datendurchsatz und geringere Latenz TempDB.

VMs, die Premium-Speicher (DS-Serie, DSv2- und GS-Serie) unterstützen, empfehlen wir Speichern von TempDB oder Puffer Pool Extensions auf einem Datenträger, der Premium-Speicher unterstützt mit Lese-Cache aktiviert. Es gibt eine Ausnahme von dieser Empfehlung; ist der Verbrauch TempDB schreibintensiv erreichen Sie höheren Leistung von TempDB auf dem lokalen Laufwerk **D** , die auch SSD basierend auf diesen Computer speichern.

### <a name="data-disks"></a>Datenträger

- **Datenträger für Daten und Protokolldateien verwenden**: Verwenden Sie mindestens 2 Premium [P30 Laufwerke](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) , wobei eine Festplatte Protokolldatei(en) und anderen Datendateien und TempDB enthält.

- **Festplatten-Striping**: mehr Durchsatz können Sie zusätzliche Datenträger hinzufügen und Disk Striping. Ermitteln Sie die Anzahl der Datenträger müssen Sie die Anzahl von IOPS für Daten- und Protokolldateien Datenträger analysieren. Diese Informationen finden Sie unter Tabellen auf IOPS pro [VM Dateigröße](virtual-machines-windows-sizes.md) sowie im folgenden Artikel: [Using Premium Storage für Festplatten](../storage/storage-premium-storage.md). Die folgenden Leitlinien:

    - Verwenden Sie für Windows 8 / Windows Server 2012 oder höher [Storage Spaces](https://technet.microsoft.com/library/hh831739.aspx). Stripe-Größe auf 64 KB für OLTP-Arbeitslasten und 256 KB für Data warehousing Arbeitslasten zu Leistungseinbußen Partition Versatz festgelegt. Außerdem festlegen, Anzahl der Spalten = Anzahl physikalischer Datenträger. PowerShell (nicht Server Manager UI) verwenden Sie konfigurieren einen Speicherplatz mit mehr als 8 Festplatten explizit die Anzahl der Spalten entsprechend die Anzahl der Datenträger. Weitere Informationen zum Konfigurieren von [Storage Spaces](https://technet.microsoft.com/library/hh831739.aspx)finden Sie unter [Storage Spaces Cmdlets in Windows PowerShell](https://technet.microsoft.com/library/jj851254.aspx)

    - Für Windows 2008 R2 oder früher können Sie dynamische Datenträger (Volumes verteilt OS) und die Stripe-Größe ist immer 64 KB. Beachten Sie, dass diese Option ab Windows 8 / Windows Server 2012 veraltet ist. Informationen finden Sie im Support-Anweisung am [Dienst für virtuelle Datenträger Windows Storage Management-API wechselt](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

    - Wenn Ihre Arbeitslast nicht intensiv Protokoll und keine dedizierte IOPs benötigt, können Sie nur ein Speicher-Pool konfigurieren. Andernfalls erstellen Sie zwei Speicher-Pools für die Log-Dateien und anderen Speicher-Pool für Datendateien und TempDB. Bestimmen Sie die Anzahl der Datenträger, basierend auf Ihrem laden jedes Speicher-Pool zugeordnet. Denken Sie daran, dass VM-Größen unterschiedliche angeschlossenen Datenträger ermöglichen. Weitere Informationen finden Sie unter [Größe für virtuelle Computer](virtual-machines-windows-sizes.md).

    - Wenn Sie Premium-Speicher (Test-/Szenarien) nicht verwenden, empfiehlt es sich, die maximale Anzahl von Datenträgern unterstützt Ihr [virtueller Speicher](virtual-machines-windows-sizes.md) hinzufügen und verwenden Disk Striping.

- **Cachingrichtlinie**: Storage für Premium-Datenträger Zwischenspeichern Lesen auf Datenträger hosten nur Ihre Datendateien und TempDB. Wenn Sie Premium-Speicher nicht verwenden, aktivieren Sie keinen Zwischenspeicher auf alle Datenträger. Informationen zum Konfigurieren von Datenträgerspeicher, finden Sie unter den folgenden Themen: [Set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) und [AzureDataDisk festlegen](https://msdn.microsoft.com/library/azure/jj152851.aspx).

    >[AZURE.WARNING] Beenden Sie den SQL Server-Dienst beim cacheeinstellung der Azure-VM Datenträger um eine Beschädigung der Datenbank zu vermeiden.

- **Größe der Zuordnungseinheiten NTFS**: beim Formatieren des Datenträger wird empfohlen, eine Größe der Zuordnungseinheiten von 64 KB für Daten-und Protokolldateien sowie TempDB zu verwenden.

- **Best Practices-Disk Management**: Wenn einen Datenträger entfernen oder Ändern der Cachetyp beenden den SQL Server-Dienst während der Änderung. Wenn die Zwischenspeichern auf dem Betriebssystem-Datenträger geändert werden, Azure beendet die VM ändert den Cache und die VM neu gestartet. Wenn Cache der Datenträger ändern VM nicht beendet, aber der Datenträger während der Änderung von der VM getrennt und anschließend angefügt.

    >[AZURE.WARNING] Während dieser Vorgänge zu den SQL Server-Dienst kann eine Beschädigung der Datenbank führen.

## <a name="io-guidance"></a>E/a-Hinweise

- Die besten Ergebnisse mit Premium werden ermöglicht, wenn Ihre Anwendung und Anfragen parallelisieren. Premium-Speicher ist für Szenarios konzipiert e/a-Warteschlangenlänge größer als 1, ist damit wenig oder keine Leistungssteigerung Singlethread-serielle Anfragen angezeigt werden (auch wenn sie intensiven Speicher). Dies konnte z. B. die Singlethread-Testergebnisse Performance-Analyse-Tools wie SQLIO auswirken.

- Verwenden Sie [Datenbank seitenkomprimierung](https://msdn.microsoft.com/library/cc280449.aspx) als e/a-intensive Arbeitslasten verbessern helfen. -Komprimierung kann die CPU-Nutzung auf dem Datenbankserver erhöhen.

- Instant initialisieren reduzieren die anfängliche Dateizuordnungstabellen in Betracht. Um sofort initialisieren nutzen zu können, erteilen Sie den SQL Server (MSSQLSERVER) mit SE_MANAGE_VOLUME_NAME und Sicherheitsrichtlinien **Durchführen von Volumewartungsaufgaben** hinzufügen. Bei Verwendung einer SQL Server-Plattform-Images für Azure nicht das Standarddienstkonto (NT Service\MSSQLSERVER) Sicherheitsrichtlinien **Durchführen von Volumewartungsaufgaben** hinzugefügt. Instant initialisieren ist also in einer Azure SQL Server-Plattform-Images nicht aktiviert. Starten Sie nach dem Hinzufügen des SQL Server-Dienstkontos Sicherheitsrichtlinien **Volume-Wartungsaufgaben durchführen** , des SQL Server-Dienstes. Sicherheitsaspekte bei Verwendung dieses Features könnte. Weitere Informationen finden Sie unter [Datenbank initialisieren](https://msdn.microsoft.com/library/ms175935.aspx).

- **Automatische Vergrößerung** gilt nur ein Notfall bei unerwartetem anwachsen. Verwalten Sie Ihr Wachstum Daten- und Protokolldateien täglich Vergrößerung nicht. Automatische Vergrößerung wird bereits vergrößern Sie die Größe Option Datei.

- Stellen Sie sicher, dass **Autoshrink** deaktiviert, um unnötigen Aufwand zu vermeiden, der sich negativ auf die Leistung auswirken kann.

- Verschieben Sie alle Datenbanken auf Datenträger, einschließlich Datenbanken. Weitere Informationen finden Sie unter [Verschieben von Systemdatenbanken](https://msdn.microsoft.com/library/ms345408.aspx).

- Wechseln Sie SQL Server-Fehler Serverprotokoll und Verzeichnisse zum Datenträger In SQL Server Configuration Manager hierzu werden die SQL Server-Instanz und wählen Sie Eigenschaften. Die Fehler protokollieren und Trace Einstellungen können auf der Registerkarte **Start-Parameter** geändert werden. Das Sicherungsverzeichnis ist in **der Registerkarte** angegeben. Der folgende Screenshot zeigt, wo sich die Startparameter Fehler protokollieren.

    ![SQL-Fehlerprotokoll Screenshot](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

- Backup und Datenbank Standardspeicherorte einrichten. Die Informationen in diesem Thema verwenden und im Eigenschaftenfenster Server ändern. Eine Anleitung finden Sie unter [anzeigen oder Ändern der Standardspeicherorte für Daten- und Protokolldateien (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). Der folgende Screenshot zeigt, wo Sie diese ändern.

    ![SQL Data Protokoll-und Backup](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)

- Aktivieren Sie gesperrte Seiten IO und Aktivitäten Paging. Weitere Informationen finden Sie unter [Aktivieren der Sperren von Seiten im Speicheroption (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

- Ausführen von SQL Server 2012 installieren Sie Service Pack 1 Kumulatives Update 10. Dieses Update enthält die Korrektur für Leistung an beim Ausführen von Select in temporary Table-Anweisung in SQL Server 2012. Informationen finden Sie in diesem [Knowledge Base-Artikel](http://support.microsoft.com/kb/2958012).

- Berücksichtigen Sie Komprimieren von Datendateien bei ein-/Azure.

## <a name="feature-specific-guidance"></a>Spezifische Funktion

Einige Installationen können zusätzliche Leistung mit erweiterten Konfiguration Vorteile. Die folgende Liste zeigt einige SQL Server-Features, die Ihnen helfen, eine bessere Leistung zu erzielen:

- **Backup auf Azure-Speicher**: bei Backups für SQL Server in Azure virtuellen Maschinen ausführen, können Sie [SQL Server-Sicherung URL](https://msdn.microsoft.com/library/dn435916.aspx). Dieses Feature ist mit SQL Server 2012 SP1 CU2 ab und empfohlen für die angeschlossenen Datenträger sichern. Wenn Sie Backup und Wiederherstellung nach Azure Storage folgen Vorschläge an [SQL Server Backup URL Best Practices, Problembehandlung und Wiederherstellung von Backups in Azure-Speicher](https://msdn.microsoft.com/library/jj919149.aspx) Sie können auch diese Backups mittels [Automatisierte Backups für SQL Server in Azure Virtual Machines](virtual-machines-windows-classic-sql-automated-backup.md)automatisieren.

    SQL Server 2012 können Sie [SQL Server-Sicherung Azure-Tool](https://www.microsoft.com/download/details.aspx?id=40740). Dieses Tool können mehrere backup Stripe Vewendung backup-Durchsatz erhöhen.

- **SQL Server-Datendateien in Azure**: dieser neuen [SQL Server-Datendateien in Azure](https://msdn.microsoft.com/library/dn385720.aspx)ist SQL Server 2014 ab. Dateien in Azure mit SQL Server zeigt vergleichbare Leistungsmerkmale wie Azure Datenträger.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie SQL Server und Storage Premium näher interessieren, finden Sie im Artikel [Verwenden Azure Premium-Speicher mit SQL Server auf virtuellen Computern](virtual-machines-windows-classic-sql-server-premium-storage.md).

Bewährte Sicherheitsmethoden finden Sie unter [Security Considerations for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-security.md).

Überprüfen Sie andere Themen SQL Server virtueller Computer [Auf virtuellen Computern Azure SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).
