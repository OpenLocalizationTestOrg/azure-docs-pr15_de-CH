<properties
   pageTitle="SQL-Datenbank ermitteln Kompatibilität vor der Migration in Azure SQL-Datenbank mit der SQL Server Management Studio | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank Datenbankmigration, SQL Datenbank-Kompatibilitätsgrad Tier Anwendung Assistent für den Export"
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
   ms.date="08/29/2016"
   ms.author="carlrab"/>

# <a name="use-sql-server-management-studio-to-determine-sql-database-compatibility-before-migration-to-azure-sql-database"></a>Verwenden Sie SQL Server Management Studio, bestimmt SQL Datenbank-Kompatibilitätsgrad vor der Migration in Azure SQL-Datenbank

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Upgrade Advisor](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
 
In diesem Artikel lernen Sie bestimmen, ob eine SQL Server-Datenbank ist kompatibel mit dem Export-Tier-Anwendung-Assistenten in SQL Server Management Studio SQL-Datenbank migrieren.

## <a name="using-sql-server-management-studio"></a>Verwenden SQL Server Management Studio

1. Überprüfen Sie, ob Sie die neueste Version von SQL Server Management Studio. Neue Versionen von Management Studio werden monatliche Updates Azure-Portal synchronisiert bleiben.

     > [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio verwenden, Updates Microsoft Azure und SQL Datenbank synchronisiert bleiben. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Management Studio öffnen Sie und die Datenbank im Objekt-Explorer an.
3. Mit der rechten Maustaste im Objekt-Explorer der Quelldatenbank, **Tasks**zeigen und auf **Datenebene Anwendung exportieren**

    ![Exportieren einer Anwendung auf Datenebene im Aufgaben](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Im Export-Assistenten auf **Weiter**und dann auf **die Registerkarte** konfigurieren Sie exportieren, um die BACPAC zu einem lokalen Speicherort oder Azure Blob speichern. Haben Sie keine Kompatibilitätsprobleme Datenbank eine BACPAC-Datei gespeichert. Wenn Kompatibilitätsprobleme sind werden sie angezeigt auf der Konsole.

    ![Export settings](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS02.png)

5. Zum Exportieren von Daten zu überspringen, klicken Sie auf die **Registerkarte "Erweitert"** und deaktivieren Sie das Kontrollkästchen **Alles auswählen** . Unser Ziel ist zu diesem Zeitpunkt nur die Kompatibilität testen.

    ![Export settings](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS03.png)

6. Klicken Sie auf **Weiter** , und klicken Sie dann auf **Fertig stellen**. Datenbank Kompatibilitätsprobleme angezeigt, wenn, nachdem der Assistent das Schema überprüft.

    ![Export settings](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS04.png)

7. Wird keine Fehler angezeigt werden, die Datenbank kompatibel und können migriert. Wenn Fehler auftreten, müssen Sie beheben. Um die Fehler anzuzeigen, klicken Sie auf **Fehler** **Validating Schema**. 
    ![Export settings](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS05.png)

8.  Wenn die *. BACPAC Datei wird generiert, Ihrer Datenbank ist mit SQL-Datenbank, und Sie migrieren können.

## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version des SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Beheben von Kompatibilitätsproblemen Datenbankmigration](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Migrieren Sie eine SQL Server-kompatible Datenbank mit SQL-Datenbank](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren Sie SQL Server-Datenbanken mithilfe von SQL Server-Migrationsassistenten](http://blogs.msdn.com/b/ssma/)
