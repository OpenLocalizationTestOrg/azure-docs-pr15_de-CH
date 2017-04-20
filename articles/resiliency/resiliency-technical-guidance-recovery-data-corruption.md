<properties
   pageTitle="Resiliency technische Anleitung zur Wiederherstellung nach Datenkorruption oder versehentliches Löschen | Microsoft Azure"
   description="Artikel zu verstehen, wie Daten einer Beschädigung von Daten oder versehentliche Löschung, leistungsstarke, hochverfügbare, Fault tolerant Applications entwerfen sowie Planung für die Wiederherstellung"
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

#<a name="azure-resiliency-technical-guidance-recovery-from-data-corruption-or-accidental-deletion"></a>Technische Anleitung Azure Stabilität: Wiederherstellung nach Datenkorruption oder versehentliches Löschen

Teil einer stabilen Business Continuity-Plan hat einen Plan, wenn Ihre Daten beschädigt oder versehentlich gelöscht. Nachfolgend Informationen zur Wiederherstellung nach Daten beschädigt oder versehentlich gelöscht, Anwendungsfehler oder Operator-Fehlern.

##<a name="virtual-machines"></a>Virtuelle Computer

Verwenden Sie zum Schützen Ihrer Azure virtuellen Maschinen (VMs Infrastruktur als Dienst bezeichnet) von Anwendungsfehlern oder versehentliches Löschen [Azure Backup](https://azure.microsoft.com/services/backup/). Azure Backup ermöglicht die Erstellung von Backups auf mehrere VM Datenträger konsistent sind. Darüber hinaus können Backup-Depot Regionen Wiederherstellung von Region Verlust repliziert werden.

##<a name="storage"></a>Speicher

Beachten Sie, dass Azure Storage Data Stabilität durch automatisierte Replikationen bietet dies nicht Ihr Anwendungscode (oder Entwickler-Benutzer) von Daten verhindert durch versehentliche oder unbeabsichtigten Löschen, Updates usw.. Daten Fidelity angesichts Anwendung oder Benutzerfehler sind erweiterte Techniken, wie das Kopieren der Daten auf einen sekundären Speicherort mit ein Überwachungsprotokoll. Entwickler können BLOB- [Snapshot-Funktion](https://msdn.microsoft.com/library/azure/ee691971.aspx)nutzen die schreibgeschützte Point-in-Time-Snapshots der BLOB-Inhalt erstellen können. Dies kann als Grundlage für eine Daten hochwertige Lösung für Azure Storage Blobs verwendet werden.

###<a name="blob-and-table-storage-backup"></a>BLOB und Tabelle Speicher sichern

Blobs und Tabellen langlebig sind, geben sie stets den aktuellen Zustand der Daten. Wiederherstellen von Daten in einen früheren Zustand erfordern wiederherstellen unerwünschte Änderung oder Löschung von Daten. Dies kann erreicht werden, nutzen die Funktionen von Azure speichern und Point-in-Time-Kopien.

Führen Sie für Azure-Blobs Point-in-Time-Backups mit der [BLOB-Snapshot-Funktion](https://msdn.microsoft.com/library/ee691971.aspx). Für jeden Snapshot Zahlen Sie nur für die Unterschiede in den Blob seit der letzten-Status Snapshot speichern Speicher. Snapshots sind abhängige auf das ursprüngliche Blob basiert auf, ein Kopiervorgang anderen Blob oder auch ein anderer Speicherkonto ratsam. Dadurch wird vor dem versehentlichen Löschen geschützte Daten. Azure-Tabellen können Sie Point-in-Time-Kopien zu einer anderen Tabelle oder Azure-Blobs ändern. Detaillierte Hinweise und Beispiele für Sicherungskopien auf Tabellen und Blobs finden Sie hier:

  * [Schützen Ihre Tabellen gegen Anwendungsfehler](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/05/03/protecting-your-tables-against-application-errors/)
  * [Schützen Ihre Blobs gegen Anwendungsfehler](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/04/29/protecting-your-blobs-against-application-errors/)

##<a name="database"></a>Datenbank

Gibt verschiedene [Business Continuity](../sql-database/sql-database-business-continuity.md) (Wiederherstellen) Optionen für Azure SQL-Datenbank. Datenbanken können mit der [Datenbank kopieren](../sql-database/sql-database-copy.md) oder [Exportieren](../sql-database/sql-database-export.md) und [Importieren von](https://msdn.microsoft.com/library/hh710052.aspx) einer SQL Server-Bacpac-Datei kopiert werden. Datenbankkopie bietet transaktionell konsistente Ergebnisse nicht Bacpac (über den Import-/Export-Dienst). Beide Optionen als Warteschlange Dienste innerhalb des Rechenzentrums, und bieten sie nicht gerade eine SLA Fertigstellung.

>[AZURE.NOTE]Die Datenbankoptionen kopieren und importieren/exportieren setzen ein hohes Maß an Load in der Quelldatenbank. Sie können Ressourcenkonflikte oder Drosselung Ereignisse auslösen.

###<a name="sql-database-backup"></a>SQL-Datenbank sichern

Point-in-Time-Backups für Microsoft Azure SQL-Datenbank werden durch [Kopieren der SQL Azure-Datenbank](../sql-database/sql-database-copy.md)erreicht. Diesen Befehl können Sie eine transaktionell konsistente Kopie einer Datenbank auf dem gleichen logischen Datenbankserver oder auf einem anderen Server erstellen. In beiden Fällen ist die Datenbankkopie voll funktionsfähig und unabhängig von der Quelldatenbank. Jede erstellte Kopie, stellt eine Point-in-Time-Recovery-Option. Die neue Datenbank mit der Name der Quelldatenbank umbenennen können Sie den Zustand der Datenbank vollständig wiederherstellen. Alternativ können Sie mithilfe von Transact-SQL-Abfragen eine bestimmte Teilmenge von Daten aus der neuen Datenbank wiederherstellen. Weitere Informationen zur SQL-Datenbank finden Sie unter [Übersicht über Business Continuity mit Azure SQL-Datenbank](../sql-database/sql-database-business-continuity.md).

###<a name="sql-server-on-virtual-machines-backup"></a>SQL Server auf virtuelle Computer sichern

Für SQL Server als ein Dienst virtuelle Maschinen (oft genannt IaaS IaaS VMs) mit Azure-Infrastruktur verwendet, gibt es zwei Optionen: herkömmliche Backups und Protokollversand. Herkömmliche Backups können zu einem bestimmten Zeitpunkt wiederherstellen, aber der Wiederherstellungsprozess ist langsam. Herkömmliche Backups wiederherstellen muss mit einem ersten vollständigen Backup starten und dann alle Sicherungen danach. Die zweite Option ist Protokollversand Sitzung Verzögerung die Wiederherstellung von Sicherungskopien (z. B. durch zwei Stunden) zu konfigurieren. Dadurch wird ein Fehler auf dem primären Wiederherstellung.

##<a name="other-azure-platform-services"></a>Andere Dienste Azure-Plattform

Einige Dienste Azure-Plattform speichern Informationen in einem benutzergesteuerten Speicherkonto oder Azure SQL-Datenbank. Bei Konto oder Speicher Ressource gelöscht oder beschädigt wird, könnte dies Probleme mit dem Dienst. In diesen Fällen ist es wichtig zu Backups, die ermöglichen würden diese Ressourcen neu erstellen, wenn sie gelöscht oder beschädigt wurden.

Azure-Websites und Azure Mobile Dienste müssen sichern und die zugeordneten Datenbanken zu pflegen. Azure Media Service und virtuellen Maschinen müssen Sie zugeordnete Azure Storage-Konto und alle Ressourcen in diesem Konto. Beispielsweise müssen Sie für virtuelle Computer sichern und Verwalten von Datenträgern VM in Azure BLOB-Speicher.

##<a name="checklists-for-data-corruption-or-accidental-deletion"></a>Prüflisten für beschädigte Daten oder versehentliches Löschen

##<a name="virtual-machines-checklist"></a>Checkliste für virtuelle Computer

  1. Überprüfen des virtuellen Maschinen Abschnitts dieses Dokuments.
  2. Sichern und Verwalten von VM Festplatten mit Azure Backup (oder eigene backup-System mithilfe von Azure BLOB-Speicher und VHD Snapshots).

##<a name="storage-checklist"></a>Speicher-Checkliste

  1. Lesen Sie den Abschnitt Speichern dieses Dokuments.
  2. Unverzichtbare Speicherressourcen regelmäßig sichern.
  3. Verwenden Sie die Snapshot-Funktion für Blobs.

##<a name="database-checklist"></a>Datenbank-Checkliste

  1. Überprüfen Sie den Datenbank-Abschnitt dieses Dokuments.
  2. Erstellen Sie Point-in-Time-Backups mit dem Befehl Kopie der Datenbank.

##<a name="sql-server-on-virtual-machines-backup-checklist"></a>SQL Server auf virtuelle Computer Backup-Checkliste

  1. Überprüfen der SQL Server-Backup virtueller Maschinen Abschnitt dieses Dokuments.
  2. Verwenden Sie herkömmliche Backups und Wiederherstellung Techniken.
  3. Erstellen Sie verzögerte Protokollversand Sitzung.

##<a name="web-apps-checklist"></a>Web Apps-Checkliste

  1. Sichern und Verwalten der zugehörigen Datenbank ggf..

##<a name="media-services-checklist"></a>Media Services-Prüfliste

  1. Sichern und Verwalten der zugeordneten Speicherressourcen.

##<a name="more-information"></a>Weitere Informationen

Finden Sie weitere Informationen über Funktionen zum Sichern und Wiederherstellen in Azure [Storage, Backup und Recovery-Szenarien](https://azure.microsoft.com/documentation/scenarios/storage-backup-recovery/).

##<a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist Teil einer Reihe [Azure Resiliency technische](./resiliency-technical-guidance.md)Anleitung. Wenn Sie mehr Flexibilität, Disaster Recovery und hohe Verfügbarkeit Ressourcen suchen, finden Sie in Azure Resiliency technischen Leitlinien [zusätzliche Ressourcen](./resiliency-technical-guidance.md#additional-resources).