<properties
   pageTitle="Migrieren von SQL Server-Datenbank in SQL Datenbank Datenbank Microsoft Azure-Datenbank-Assistenten bereitstellen | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank Datenbankmigration Microsoft Azure-Datenbank-Assistent"
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

# <a name="migrate-sql-server-database-to-sql-database-using-deploy-database-to-microsoft-azure-database-wizard"></a>Migrieren von SQL Server-Datenbank in SQL Datenbank Datenbank Microsoft Azure-Datenbank-Assistenten bereitstellen


> [AZURE.SELECTOR]
- [SSMS Migrationsassistenten](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [BACPAC-Datei exportieren](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [BACPAC importieren](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Microsoft Azure-Datenbank-Assistenten in SQL Server Management Studio Datenbank bereitstellen migriert einer [SQL Server-kompatible Datenbank](sql-database-cloud-migrate.md) direkt in Ihre Azure SQL-Datenbankserver.

## <a name="use-the-deploy-database-to-microsoft-azure-database-wizard"></a>Microsoft Azure-Datenbank-Assistenten Datenbank bereitstellen

> [AZURE.NOTE] Die folgenden Schritte gehen haben Sie eine [Datenbank von SQL Server bereitgestellt](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/).

1. Überprüfen Sie, ob Sie die neueste Version von SQL Server Management Studio. Neue Versionen von Management Studio werden monatliche Updates Azure-Portal synchronisiert bleiben.

    > [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio verwenden, Updates Microsoft Azure und SQL Datenbank synchronisiert bleiben. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Management Studio öffnen und Verbinden mit der SQL Server-Datenbank im Objekt-Explorer migriert werden.
3. Mit der rechten Maustaste im Objekt-Explorer Datenbank, **Tasks**zeigen und auf **Microsoft Azure SQL Datenbank Datenbank bereitstellen**

    ![Im Menü Aufgaben auf Azure bereitstellen](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.  Deployment-Assistenten klicken Sie auf **Weiter**und dann auf **Verbinden** , um die Verbindung mit dem SQL-Datenbankserver.

    ![Im Menü Aufgaben auf Azure bereitstellen](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. Geben Sie in das Dialogfeld Verbindung mit Server Verbindungsinformationen für die Verbindung mit dem SQL-Datenbankserver.

    ![Im Menü Aufgaben auf Azure bereitstellen](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.  Geben Sie folgende [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -Datei, die mit diesem Assistenten während des Migrationsprozesses erstellt:

 - Der **Name der neuen Datenbank** 
 - Die **Edition des Microsoft Azure SQL-Datenbank** ([Service-Ebene](sql-database-service-tiers.md))
 - Die **Maximale Datenbankgröße**
 - Das **Service-Ziel** (Performance Level)
 - Der **Name der temporären Datei**  

    ![Export settings](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.  Führen Sie den Assistenten. Je nach Größe und Komplexität der Datenbank möglicherweise Bereitstellung von wenigen Minuten zu mehrere Stunden dauern. Erkennt dieses Assistenten Kompatibilitätsprobleme, Fehler auf dem Bildschirm angezeigt und die Migration nicht weiter. Anleitung zum Beheben von Kompatibilitätsproblemen Datenbank finden Sie unter [Beheben von Kompatibilitätsproblemen Datenbank](sql-database-cloud-migrate-fix-compatibility-issues.md).

7.  Schließen Sie in Objekt-Explorer an die migrierte Datenbank in der Azure SQL-Datenbankserver.
8.  Zeigen Sie Azure-Portal Ihre Datenbank und deren Eigenschaften.

## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version des SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren Sie SQL Server-Datenbanken mithilfe von SQL Server-Migrationsassistenten](http://blogs.msdn.com/b/ssma/)
