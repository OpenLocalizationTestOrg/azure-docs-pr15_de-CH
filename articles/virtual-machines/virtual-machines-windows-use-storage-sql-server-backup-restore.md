<properties
    pageTitle="Verwenden von Azure-Speicher für SQL Server Backup und Wiederherstellen | Microsoft Azure"
    description="Informationen Sie zum Sichern von SQL Server auf Azure-Speicher. Erläutert die Vorteile von Datenbanken SQL Azure-Speicher sichern."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="MikeRayMSFT"
    manager="jhubbard"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="mikeray"/>

# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Verwenden von Azure-Speicher für SQL Server Backup und Wiederherstellung

## <a name="overview"></a>Übersicht

Beginnend mit SQL Server 2012 SP1 CU2 können Sie jetzt SQL Server-Backups direkt an den Dienst Azure BLOB-Speicher schreiben. Diese Funktionalität können Sie bis zu sichern und Wiederherstellen von Azure BLOB-Dienst mit einer lokalen SQL Server-Datenbank oder einer SQL Server-Datenbank in Azure VM. Backup-to-cloud bietet Vorteile Verfügbarkeit unbegrenzt Geo repliziert Aufbewahrung und einfache Migration von Daten aus der Cloud. Backup- oder RESTORE-Anweisung ausstellen mithilfe der Transact-SQL oder SMO.

SQL Server 2016 führt neue Funktionen; [Datei Snapshot-Sicherung](http://msdn.microsoft.com/library/mt169363.aspx) können Sie nahezu direkte Backups und Restores unglaublich schnell durchführen.

Dieses Thema erläutert, warum für Backups von SQL Azure-Speicher verwenden können und beschreibt die Komponenten. Am Ende des Artikels Ressourcen können Walkthroughs und Weitere Informationen zu Ihrer SQL Server-Backups mit diesem Dienst.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Vorteile von Azure Blob-Dienst für SQL Server-Sicherung

Es gibt mehrere Probleme beim Sichern von SQL Server bestehen. Dazu zählen Ausfallrisiko Speicherung, Aufbewahrung und Hardwarekonfiguration auf Speicher-Management. Viele dieser Probleme werden behandelt, unter Verwendung von Azure BLOB-Speicher für SQL Server-Backups. Berücksichtigen Sie die folgenden Vorteile:

- **Benutzerfreundlich**: Speicherung der Sicherungskopien in Azure-Blobs kann praktisch sein, eine flexible und einfach externe Option. Aufbewahrung Ihrer SQL Server-Backups erstellen kann so einfach wie das Ändern der Skripts/Arbeitsplätze **BACKUP URL** -Syntax verwenden. Lagerung sollte in der Regel weit vom Produktionsort Datenbank zu einer Katastrophe, die sowohl die externe und Produktion Datenbankpfade auswirken. [Geo-Replikation der Azure-blobs](../storage/storage-redundancy.md)auswählen, müssen Sie eine zusätzliche Schutzebene Notfall, die gesamte Region auswirkt.
- **Backup-Archiv**: der Azure BLOB-Speicher-Dienst bietet eine bessere Alternative Option häufig verwendete Band Backups archiviert. Bänder müssen physikalischen Transport zum externen Standort und Maßnahmen der Medien. Speichern von Sicherungskopien in Azure BLOB-Speicher bietet sofort, hochgradig verfügbar und eine dauerhafte Archivierung Option.
- **Verwaltete Hardware**: entsteht kein Leistungsaufwand Hardware-Management mit Azure. Azure Services verwalten Hardware und Geo-Replikation für Redundanz und Schutz vor Hardwarefehlern.
- **Unbegrenzte Speicherung**: direkte Sicherung auf Azure-Blobs aktivieren, haben Sie Zugriff auf Speicher praktisch unbegrenzt. Alternativ hat sichern ein Azure virtuellen Datenträger Grenzwerte basierend auf Computer-Größe. Gibt es eine Beschränkung für die Anzahl von Datenträgern zu einer Azure Virtual Machine für Backups legen. Diese Grenze ist 16 Festplatten für eine sehr große Instanz und weniger für kleinere Instanzen.
- **Backup-Verfügbarkeit**: Backups in Azure-Blobs gespeichert sind überall und jederzeit verfügbar und leicht erreichbar für die Wiederherstellung einer lokalen SQL Server oder einer anderen SQL Server läuft in einer Azure Virtual Machine, ohne Datenbank verbinden/trennen oder herunterladen und die virtuelle Festplatte anfügen.
- **Kosten**: Zahlen nur für den Dienst verwendet wird. Kann optional externe und backup-Archiv sein. Siehe [Azure Preise Rechner](http://go.microsoft.com/fwlink/?LinkId=277060 "Rechner Preise")und die [Artikel Azure Preise](http://go.microsoft.com/fwlink/?LinkId=277059 "Preise Artikel") Weitere Informationen.
- **Storage Snapshots**: Wenn Datenbankdateien in Azure Blob gespeichert und SQL Server 2016 verwenden, können Sie [Datei Snapshotsicherung](http://msdn.microsoft.com/library/mt169363.aspx) nahezu sofortige Backups und Restores unglaublich schnell durchführen.

Weitere Informationen finden Sie unter [SQL Server sichern und Wiederherstellen mit Azure BLOB-Speicher](http://go.microsoft.com/fwlink/?LinkId=271617).

Den folgenden zwei Abschnitten Azure BLOB-Speicher Service, einschließlich der erforderlichen SQL Server-Komponenten. Es ist wichtig zu verstehen, die Komponenten und deren Interaktion erfolgreich sichern und Wiederherstellen von Azure BLOB-Speicher-Dienst.

## <a name="azure-blob-storage-service-components"></a>Dienstkomponenten von Azure BLOB-Speicher

Folgender Azure werden verwendet, wenn der Dienst Azure BLOB-Speicher sichern.

| Komponente               | Beschreibung                          |
|---------------------|-------------------------------|
| **Konto** | Das Speicherkonto ist der Ausgangspunkt für alle Speicher-Services. Zugriff auf einen Dienst Azure BLOB-Speicher zuerst erstellen Sie Azure Storage-Konto. Weitere Informationen zum Dienst Azure BLOB-Speicher finden Sie unter [Azure Blob Storage Service verwenden](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Container** | Ein Container stellt eine Gruppierung einer Gruppe von Blobs und kann eine unbegrenzte Anzahl von Blobs speichern. Schreiben von SQL Server backup Azure Blob-Dienst mindestens den Stammcontainer erstellt muss. |
| **BLOB** | Eine Datei von Typ und Größe. BLOBs sind adressierbare mit URL: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Informationen zur Seitenblobs finden Sie unter [Understanding und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>SQL Server-Komponenten

Die folgenden SQL Server-Komponenten werden verwendet, wenn der Dienst Azure BLOB-Speicher sichern.

| Komponente               | Beschreibung                          |
|---------------------|-------------------------------|
| **URL** | Eine URL gibt einen URI Uniform Resource Identifier () in eine eindeutige Sicherungsdatei. Speicherort und Name der SQL Server-Sicherungsdatei ist die URL verwendet. Die URL muss auf eine tatsächliche Blob nicht nur Container verweisen. Wenn das Blob nicht vorhanden ist, wird es erstellt. Wenn ein vorhandenes Blob angegeben, Sicherung schlägt fehl, wenn die > WITH FORMAT angegeben wurde. Im folgenden ist ein Beispiel der URL Sie in der BACKUP-Befehl geben: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS wird empfohlen, jedoch nicht erforderlich. |
| **Anmeldeinformationen** | Die Informationen zu Azure BLOB-Speicherdienst authentifizieren wird als Referenz gespeichert.  In Reihenfolge für SQL Server-Backups Azure Blob oder Wiederherstellung aus schreiben muss SQL Server-Anmeldeinformationen erstellt werden. Weitere Informationen finden Sie in der [SQL Server-Anmeldeinformationen](https://msdn.microsoft.com/library/ms189522.aspx). |

> [AZURE.NOTE] Möchten Sie kopieren und eine Sicherungsdatei auf der Dienst Azure BLOB-Speicher hochladen, verwenden Sie ein BLOB-Seite als Storage Option Wenn Sie diese Datei für die Wiederherstellung verwenden möchten. Fehler bei der Wiederherstellung von einem Block BLOB-Typ.

## <a name="next-steps"></a>Nächste Schritte

1. Erstellen Sie ein Azure-Konto Wenn Sie noch keines haben. Sollten Sie beim Auswerten von Azure [Testen](https://azure.microsoft.com/free/).

1. Gehen Sie durch die folgenden Tutorials, die Sie erstellen ein Speicherkonto und Wiederherstellungsvorgang durchlaufen.

    - **SQL Server 2014**: [Tutorial: SQL Server 2014 sichern und Wiederherstellen in Microsoft Azure BLOB-Speicher-Service](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
    - **SQL Server 2016**: [Tutorial: 2016 SQL Server-Datenbanken mit Microsoft Azure BLOB-Speicher-Service](https://msdn.microsoft.com/library/dn466438.aspx)

1. Lesen Sie weitere Dokumentation mit [SQL Server Backup und Wiederherstellung mit Microsoft Azure BLOB-Speicher](https://msdn.microsoft.com/library/jj919148.aspx).

Wenn Sie Probleme haben, wiederholen Sie das Thema [SQL Server Backup URL Best Practices und Problembehandlung](https://msdn.microsoft.com/library/jj919149.aspx).

Für andere SQL Server-Sicherung und Wiederherstellungsoptionen, [Backup und Wiederherstellung für SQL Server in Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
