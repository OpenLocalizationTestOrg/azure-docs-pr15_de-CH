<properties
   pageTitle="SQL Server-Datenbank Kompatibilität Probleme vor der Migration zu SQL Datenbank | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank Datenbankmigration, Kompatibilität, SQL Azure-Migrationsassistenten, SSDT"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="migrate-a-sql-server-database-to-azure-sql-database-using-sql-server-data-tools-for-visual-studio"></a>Migrieren einer SQL Server-Datenbank in SQL Azure-Datenbank mithilfe von SQL Server Data Tools für Visual Studio 

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Upgrade Advisor](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

In diesem Artikel lernen Sie erkennen und Beheben von SQL Server Datenbank Kompatibilitätsproblemen mit SQL Server Data Tools für Visual Studio vor der Migration in Azure SQL-Datenbank.

## <a name="using-sql-server-data-tools-for-visual-studio"></a>Mithilfe von SQL Server Data Tools für Visual Studio

Verwenden Sie SQL Server Data Tools für Visual Studio ("SSDT"), ein Visual Studio-Projekt für die Analyse das Datenbankschema importieren. Um zu analysieren, geben Sie die Zielplattform für das Projekt als SQL Datenbank V12 und erstellen Sie das Projekt. Wenn der Build erfolgreich ist, wird die Datenbank kompatibel. Wenn der Buildvorgang fehlschlägt, können Sie Fehler in SSDT (oder eines der in diesem Thema behandelten Tools) beheben. Nachdem das Projekt erfolgreich erstellt wurde, können Sie es als Kopie der Quelldatenbank veröffentlichen. Die Compare-Funktion können in SSDT Sie die Daten aus der Quelldatenbank in Azure SQL-V12-kompatible Datenbank kopieren. Sie können aktualisierte Datenbank migrieren. Um diese Option zu verwenden, herunterladen Sie die [neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)

  ![Diagramm zur Datenmigration VSSSDT](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

  > [AZURE.NOTE] Nur Schema wird, kann das Schema direkt in Azure SQL-Datenbank direkt von Visual Studio veröffentlicht werden. Verwenden Sie diese Methode, wenn das Datenbankschema mehr als allein der Migrations-Assistent behandelt können geändert werden kann.

## <a name="detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Erkennung von Kompatibilitätsproblemen mit SQL Server Data Tools für Visual Studio
   
1.  Öffnen Sie den **SQL Server-Objekt-Explorer** in Visual Studio. **Hinzufügen von SQL Server** Verbindung mit der zu migrierenden Datenbank SQL Server-Instanz verwenden. Suchen Sie die Datenbank im Objekt-Explorer mit der rechten Maustaste der Datenbank und wählen Sie **Neues Projekt erstellen**     
    
    ![Neues Projekt](./media/sql-database-migrate-visualstudio-ssdt/02MigrateSSDT.png)    
   
2.  Konfigurieren der Import **nur anwendungsspezifische Objekte**importieren. Deaktivieren Sie die Optionen zum Importieren von folgenden: Benutzernamen und Berechtigungen sowie Datenbank verwiesen.    

    ![ALT-text](./media/sql-database-migrate-visualstudio-ssdt/03MigrateSSDT.png)    

3.  Klicken Sie auf **Starten** , um die Datenbank zu importieren und das Projekt mit einer T-SQL-Skriptdatei für jedes Objekt in der Datenbank erstellen. Skriptdateien sind in Ordnern innerhalb des Projekts geschachtelt.    

    ![ALT-text](./media/sql-database-migrate-visualstudio-ssdt/04MigrateSSDT.png)    

4.  Im Visual Studio-Projektmappen-Explorer mit der rechten Maustaste des Datenbankprojekts, und wählen Sie Eigenschaften. Konfigurieren Sie auf der Seite **Einstellungen** die Zielplattform Microsoft Azure SQL-Datenbank V12.    
    
    ![ALT-text](./media/sql-database-migrate-visualstudio-ssdt/05MigrateSSDT.png)    
    
5.  Maustaste das Projekt, und wählen Sie zum Erstellen des Projekts **Erstellen** .    
    
    ![ALT-text](./media/sql-database-migrate-visualstudio-ssdt/06MigrateSSDT.png)    
    
6.  Die **Liste** zeigt jede Inkompatibilität. In diesem Fall ist der Benutzername NT-Autorität / Netzwerkdienst nicht kompatibel. Ist nicht kompatibel, können Sie auskommentieren oder entfernen (und welche Auswirkung das Entfernen dieser Anmeldung und die Rolle von datenbanklösung).     
    
    ![ALT-text](./media/sql-database-migrate-visualstudio-ssdt/07MigrateSSDT.png)    
    
## <a name="fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Beheben von Kompatibilitätsproblemen mit SQL Server Data Tools für Visual Studio

1.  Doppelklicken Sie auf das erste Skript öffnen Sie das Skript in ein Abfragefenster, und kommentieren Sie das Skript, und führen Sie das Skript.     
    ![ALT-text](./media/sql-database-migrate-visualstudio-ssdt/08MigrateSSDT.png)

2.  Wiederholen Sie diesen Vorgang für jedes Skript mit Inkompatibilitäten, bis keine Fehler.    
    ![ALT-text](./media/sql-database-migrate-visualstudio-ssdt/09MigrateSSDT.png)
    
3.  Wenn die Datenbank beschädigt ist, wählen Sie das Projekt aus und klicken Sie **Veröffentlichen**. Eine Kopie der Datenbank erstellt und veröffentlicht (es wird empfohlen, zumindest anfänglich eine Kopie verwenden).     
 - Vor dem Veröffentlichen, müssen Sie je nach Quelle SQL Server (früher als SQL Server 2014), Zielplattform zum Aktivieren des Projekts zurücksetzen.     
 - Wenn Sie eine ältere SQL Server-Datenbank migrieren, stellen alle Features in das Projekt nicht in der Quelle SQL Server unterstützt bis zu migrieren Sie die Datenbank auf eine neuere Version von SQL Server.     

        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/10MigrateSSDT.png)    
    
        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/11MigrateSSDT.png)    
        
4.  Im SQL Server-Objekt-Explorer mit der rechten Maustaste der Datenbank und **Datenvergleich**auf. Vergleichen das Projekt in die ursprüngliche Datenbank hilft Ihnen zu verstehen, welche vom Assistenten geändert wurde. Wählen Sie Ihre Azure SQL-V12-Version der Datenbank, und klicken Sie dann auf **Fertig stellen**.    
    
    ![ALT-text](./media/sql-database-migrate-visualstudio-ssdt/12MigrateSSDT.png)    
    
    ![ALT-text](./media/sql-database-migrate-visualstudio-ssdt/13MigrateSSDT.png)    

5.  Überprüfen Sie die Unterschiede ermittelt und dann auf **Ziel aktualisieren** zum Migrieren von Daten aus der Quelldatenbank in Azure SQL-V12-Datenbank.     
    
    ![ALT-text](./media/sql-database-migrate-visualstudio-ssdt/14MigrateSSDT.png)    
    
6.  Auswählen einer Bereitstellungsmethode. Finden Sie unter [eine SQL Server-kompatible Datenbank mit SQL-Datenbank migrieren.](sql-database-cloud-migrate.md)  

## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version des SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren Sie SQL Server-Datenbanken mithilfe von SQL Server-Migrationsassistenten](http://blogs.msdn.com/b/ssma/)
