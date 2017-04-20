<properties
    pageTitle="Archivieren einer Datenbank SQL Azure eine BACPAC-Datei mithilfe der Azure-Portal"
    description="Archivieren einer Datenbank SQL Azure eine BACPAC-Datei mithilfe der Azure-Portal"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-using-the-azure-portal"></a>Archivieren einer Datenbank SQL Azure eine BACPAC-Datei mithilfe der Azure-Portal

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)

Dieser Artikel beschreibt, wie Sie für die Archivierung der SQL Azure-Datenbank in einer BACPAC Datei (gespeichert in Azure BLOB-Speicher) mithilfe der [Azure-Portal](https://portal.azure.com).

Wenn Sie ein Archiv einer Azure SQL-Datenbank erstellen möchten, können Sie das Datenbankschema und die Daten in eine BACPAC-Datei exportieren. Eine BACPAC-Datei ist einfach eine ZIP-Datei mit der Erweiterung BACPAC. BACPAC Datei kann später in Azure BLOB-Speicher oder im lokalen Speicher in einem lokalen Verzeichnis gespeichert und später importierten zurück in Azure SQL-Datenbank oder in einer SQL Server-lokale Installation. 

***Hinweise***

- Ein Archiv konsistent sein, müssen Sie sicherstellen, dass kein Schreibzugriff Aktivität erfolgt während des Exports oder aus einer [transaktionell konsistenten Kopie](sql-database-copy.md) der SQL Azure-Datenbank exportieren.
- Die maximale Größe einer BACPAC-Datei in Azure BLOB-Speicher archiviert beträgt 200 GB. Um eine größere BACPAC-Datei in den lokalen Speicher zu archivieren, verwenden Sie das Befehlszeilendienstprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . Dieses Dienstprogramm wird mit Visual Studio und SQL Server. Sie können auch die neueste Version von SQL Server Data Tools zu diesem Dienstprogramm [herunterladen](https://msdn.microsoft.com/library/mt204009.aspx) .
- Archivierung in Azure Premium-Speicher mit einer BACPAC-Datei wird nicht unterstützt.
- Wenn der Exportvorgang 20 Stunden überschreitet, kann es abgebrochen. Zum Steigern der Leistung beim Export können Sie folgende Aktionen ausführen:
 - Erhöhen Sie der Dienst vorübergehend.
 - Beenden Sie alle Lese- und Schreibvorgänge während des Exports.
 - Verwenden Sie einen [gruppierten Index](https://msdn.microsoft.com/library/ms190457.aspx) mit Werten ungleich Null für alle großen Tabellen. Ohne gruppierte Indizes möglicherweise ein Export mehr als 6 bis 12 Stunden dauert. Ist der exportdienst muss einen Tabellenscan um ganze Tabelle exportieren abgeschlossen. **DBCC SHOW_STATISTICS** und stellen Sie sicher, dass *RANGE_HI_KEY* nicht null und sein Wert gute ist eine gute Möglichkeit, zu bestimmen, ob die Tabellen für den Export optimiert sind. Weitere Informationen finden Sie unter [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).


> [AZURE.NOTE] BACPACs werden nicht für die Sicherung und Wiederherstellung. Azure SQL-Datenbank erstellt automatisch Backups für jede Benutzerdatenbank. Weitere Informationen finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md).

Zum Abschließen dieses Artikels benötigen Sie Folgendes:

- Ein Azure-Abonnement.
- Ein Azure SQL-Datenbank. 
- Ein [Azure standardspeicherkonto](../storage/storage-create-storage-account.md) mit einem BLOB-Container der BACPAC im Standardspeicher speichern.

## <a name="export-your-database"></a>Ihre Datenbank exportieren

Öffnen Sie das Blade SQL-Datenbank für die Datenbank, die Sie exportieren möchten.

> [AZURE.IMPORTANT] Eine BACPAC Transaktionskonsistenz garantieren Sie zunächst [eine Kopie der Datenbank erstellen](sql-database-copy.md) und exportieren die Datenbankkopie. 

1.  Zum [Azure-Portal](https://portal.azure.com).
2.  Klicken Sie auf **SQL-Datenbanken**.
3.  Klicken Sie auf Datenbank archivieren.
4.  Klicken Sie im Blatt SQL Datenbank öffnen Blade **Datenbank exportieren** **Exportieren** :

    ![Schaltfläche Exportieren][1]

5.  Auf **Speicher** , und wählen Sie Ihr Konto und BLOB-Behälter, die BACPAC gespeichert wird:

    ![Datenbank exportieren][2]

6. Authentifizierungstyp auswählen 
7.  Geben Sie die entsprechenden Anmeldeinformationen für Azure SQL Server-Datenbank, die Sie exportieren.
8.  Klicken Sie auf **OK** , um die Datenbank zu archivieren. Klicken auf **OK** eine Datenbankabfrage Export erstellt und an den Dienst sendet. Die Länge der Zeit wird der Export hängt von der Größe und Komplexität der Datenbank und der Service-Level. Sie erhalten eine Benachrichtigung.

    ![Ausfuhr][3]

## <a name="monitor-the-progress-of-the-export-operation"></a>Überwachen Sie den Fortschritt des Exportvorgangs

1.  Klicken Sie auf **SQL Server**.
2.  Klicken Sie auf den Server mit der ursprünglichen Datenbank (Quelle), gerade archiviert.
3.  Scrollen Sie Vorgänge.
4.  In der SQL Server-Blade Klickverlauf **Importieren/Exportieren**:

    ![Import Export Historie][4]

## <a name="verify-the-bacpac-is-in-your-storage-container"></a>Überprüfen Sie die BACPAC in der Behälter

1.  Klicken Sie auf **Storage-Konten**.
2.  Klicken Sie auf das Speicherkonto, das BACPAC-Archiv gespeichert.
3.  **Container** auf, und wählen Sie den Container die Datenbank in Informationen exportiert, (herunterladen und speichern BACPAC hier).

    ![.bacpac-Info][5]  

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Importieren einer BACPAC mit einer Azure SQL-Datenbank finden Sie unter [Importieren einer BACPCAC mit einer Azure SQL-Datenbank](sql-database-import.md)
- Informationen zum Importieren einer BACPAC mit einer SQL Server-Datenbank finden Sie unter [BACPCAC auf eine SQL Server-Datenbank importieren](https://msdn.microsoft.com/library/hh710052.aspx)



<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png

