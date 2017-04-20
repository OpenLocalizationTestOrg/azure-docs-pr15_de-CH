<properties
    pageTitle="Sicherung und Wiederherstellung für SQL Server | Microsoft Azure"
    description="Beschreibt Aspekte der Sicherung und Wiederherstellung für SQL Server-Datenbanken auf Azure Virtual Machines."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Backup und Wiederherstellung für SQL Server in Azure Virtual Machines

## <a name="overview"></a>Übersicht

Sichern von Daten in SQL Server-Datenbanken ist ein wichtiger Bestandteil der Strategie zum Schutz vor Datenverlust durch Benutzer Fehler. Dies gilt auch für SQL Server auf Azure virtuelle Maschinen (VMs) ausgeführt.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Für SQL Server in Azure VMs ausgeführt können systemeigene Sicherungsprogramm und Techniken angeschlossene Laufwerke für die Bestimmung der Sicherungsdateien wiederherstellen. Es ist jedoch eine Beschränkung für die Anzahl der Datenträger eine Azure virtuellen Computer basierend auf der [Größe des virtuellen Computers](virtual-machines-linux-sizes.md)legen. Außerdem beträgt der Overhead der datenträgerverwaltung berücksichtigen.

Beginnend mit SQL Server 2014 Sie sichern und Wiederherstellen in Microsoft Azure BLOB-Speicher. SQL Server 2016 verbesserter auch diese Option. Darüber hinaus für Datenbankdateien in Microsoft Azure BLOB-Speicher SQL Server 2016 Option fast sofortige Backups und schnelle Wiederherstellung Azure Snapshots. Dieser Artikel bietet eine Übersicht über diese Optionen und Weitere Informationen finden unter [SQL Server Backup und Wiederherstellung mit Microsoft Azure BLOB-Speicher](https://msdn.microsoft.com/library/jj919148.aspx).

>[AZURE.NOTE] Eine Beschreibung der Optionen für sehr große Datenbanken sichern finden Sie unter [Mehreren Terabyte SQL Server Datenbank-Backup-Strategien für Azure Virtual Machines](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).

Die folgenden Abschnitte enthalten Informationen zu den verschiedenen Versionen von SQL Server in Azure virtuellen Maschine unterstützt.

## <a name="sql-server-virtual-machines"></a>SQL Server virtueller Maschinen

Wenn die SQL Server-Instanz ein Azure Virtual Machine ausgeführt wird, befinden sich die Datenbankdateien bereits auf Datenträger in Azure. Diese Datenträger Leben in Azure BLOB-Speicher. Gründe für das Sichern der Datenbank und der Methoden Sie dauern ändern so etwas. Folgendes zu berücksichtigen: 

- Sie müssen nicht mehr Datenbank Sicherung zum Schutz gegen Hardware-oder Medien da Microsoft Azure diesen Schutz als Teil des Dienstes Microsoft Azure bietet.

- Weiterhin müssen Datenbank-Sicherung zum Schutz vor Benutzerfehlern und Archivierung, gesetzlichen Gründen oder administrative Zwecke.

- Sie können die Sicherungsdatei auf Azure speichern. Weitere Informationen finden Sie in den folgenden Abschnitten, die Leitlinien für die verschiedenen Versionen von SQL Server.

## <a name="sql-server-2016"></a>SQL Server 2016

Microsoft SQL Server 2016 unterstützt [Backup und Wiederherstellung mit Azure-Blobs](https://msdn.microsoft.com/library/jj919148.aspx) Funktionen von SQL Server 2014. Aber auch die folgenden Funktionen:

| 2016 Verbesserung               | Details                          |
|---------------------|-------------------------------|
| **Striping**              | Beim Sichern auf Microsoft Azure BLOB-Speicher unterstützt SQL Server 2016 sichern mehrerer Blobs können maximal 12,8 TB großen Datenbanken sichern.      |
| **Snapshot-Sicherung**                | Mithilfe von Azure Snapshots bietet SQL Server Datei Snapshotsicherung nahezu sofortige Backups und schnelle Wiederherstellung für Dateien mit der Dienst Azure BLOB-Speicher gespeichert. Diese Funktion können Sie vereinfachen die Sicherung und Wiederherstellung von Richtlinien. Datei-Snapshot-Sicherung unterstützt Punkt Zeitpunkt wiederherstellen. Weitere Informationen finden Sie unter [Snapshotsicherungen Datenbankdateien in Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx).   |
| **Verwaltete Backup-Planung**            | SQL Server verwaltet Sicherung in Azure unterstützt benutzerdefinierte Zeitpläne. Weitere Informationen finden Sie unter [SQL Server verwaltete Sicherung Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx).   |

Eine der Funktionen von SQL Server 2016 bei Azure BLOB-Speicherung finden Sie unter [Tutorial: 2016 SQL Server-Datenbanken mit Microsoft Azure BLOB-Speicher-Service](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014

SQL Server 2014 umfasst die folgenden Funktionen:

1. **Sicherung und Wiederherstellung in Azure**:

 - *SQL Server-Sicherung URL* unterstützt jetzt in SQL Server Management Studio. Die Option zum Sichern auf Azure steht bei Verwendung von Backup- oder Restore-Vorgang oder Wartung-Assistenten in SQL Server Management Studio. Weitere Informationen finden Sie in der [SQL Server-Sicherung URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
 - *SQL Server verwaltet Backup in Azure* verfügt über neue Funktionen, die automatisiertes backup-Management ermöglicht. Dies ist besonders für die Automatisierung von backup-Management für SQL Server 2014 Instanzen auf einem Computer Azure. Weitere Informationen finden Sie unter [SQL Server verwaltete Sicherung Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
 - *Automatisierte Sicherung* bietet zusätzliche Automatisierung *SQL Server verwaltete Sicherung in Azure* automatisch auf alle vorhandenen und neuen Datenbanken für eine SQL Server-VM in Azure aktivieren. Weitere Informationen finden Sie unter [Automatische Sicherung für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).
 - Finden Sie eine Übersicht über die Optionen für SQL Server 2014 Sicherung in Azure [SQL Server Backup und Wiederherstellung mit Microsoft Azure BLOB-Speicher](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).

1. **Verschlüsselung**: SQL Server 2014 unterstützt Verschlüsselung von Daten beim Erstellen einer Sicherung. Unterstützt verschiedene Verschlüsselungsalgorithmen und die Verwendung osf Zertifikat oder den asymmetrischen Schlüssel. Weitere Informationen finden Sie unter [Backup-Verschlüsselung](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQL Server 2012

Informationen zur SQL Server-Sicherung und Wiederherstellung in SQL Server 2012 finden Sie unter [Sichern und Wiederherstellen der SQL Server-Datenbanken (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Ab SQL Server 2012 SP1 kumulative Update 2 Sie bis zu sichern und Wiederherstellen von Azure BLOB-Speicher-Service. Sichern von SQL Server-Datenbanken auf einem SQL Server auf Azure Virtual Machine oder eine lokale Instanz kann diese Erweiterung verwendet werden. Weitere Informationen finden Sie unter [SQL Server sichern und Wiederherstellen mit Azure BLOB-Speicher](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Einige der Vorteile von Azure BLOB-Speicher Service gehört die Möglichkeit, die 16 Festplattenkapazität für angeschlossenen Festplatten erleichterte Verwaltung, direkte Verfügbarkeit der Sicherungsdatei auf einer anderen Instanz von SQL Server-Instanz auf einem Azure virtuellen Computer oder eine lokale Instanz für Migration oder Disaster Recovery zu umgehen. Eine vollständige Liste der Vorteile einen Azure Blob Service für SQL Server-Sicherung finden Sie in Abschnitt *Vorteile* in [SQL Server sichern und Wiederherstellen mit Azure BLOB-Speicher](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Optimale Vorgehensweisen und Informationen zur Problembehandlung finden Sie unter [Backup und Restore Best Practices (Azure BLOB-Speicher Service)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008

SQL Server-Sicherung und Wiederherstellung in SQL Server 2008 R2 finden Sie unter [Sichern und Wiederherstellen von Datenbanken in SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

SQL Server-Sicherung und Wiederherstellung in SQL Server 2008 finden Sie unter [Sichern und Wiederherstellen von Datenbanken in SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Nächste Schritte

Wenn beim Planen der Bereitstellung von SQL Server in eine Azure-VM finden Sie Bereitstellung Hinweise in der folgenden Anleitung: [Bereitstellen einer SQL Server-VM Azure mit Azure-Ressourcen-Manager](virtual-machines-windows-portal-sql-server-provision.md).

Obwohl Sicherung und Wiederherstellung verwendet werden können, die Daten zu migrieren, sind möglicherweise leichter Migration Datenpfaden SQL Server auf eine Azure-VM. Eine umfassende Erläuterung der Optionen und Vorschläge finden Sie unter [Migrieren von einer Datenbank in SQL Server auf eine Azure-VM](virtual-machines-windows-migrate-sql.md).

Überprüfen Sie weitere [Ressourcen für SQL Server in Azure virtuellen Computer ausgeführt](virtual-machines-windows-sql-server-iaas-overview.md).
