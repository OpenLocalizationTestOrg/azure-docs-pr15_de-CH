<properties
    pageTitle="Stretch-Datenbank für eine Datenbank aktivieren | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren einer Datenbank für Stretch-Datenbank."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="enable-stretch-database-for-a-database"></a>Stretch-Datenbank für eine Datenbank aktivieren

Wählen Sie zum Konfigurieren einer vorhandenen Datenbank Stretch Datenbank **Aufgaben | Stretch | Aktivieren Sie** für eine Datenbank in SQL Server Management Studio den **Aktivieren für Stretch** -Assistenten zu öffnen. Sie können auch die Transact\-SQL Datenbank Strecken für eine Datenbank zu aktivieren.

Bei Auswahl von Aufgaben **| Stretch | Aktivieren Sie** für eine einzelne Tabelle und noch nicht die Datenbank Stretch-Datenbank aktiviert haben, wird der Assistent die Datenbank Stretch Datenbank konfiguriert und können Sie Tabellen als Teil des Prozesses auswählen. Führen Sie die Schritte in diesem Thema anstelle der Schritte [Stretch](sql-server-stretch-database-enable-database.md)Datenbank für eine Tabelle aktivieren.

Aktivieren der Stretch-Datenbank auf einer Datenbank oder einer Tabelle Db erfordert\_Besitzerberechtigungen. Aktivieren Stretch-Datenbank in einer Datenbank sind auch Datenbank-Berechtigungen erforderlich.

 >   [AZURE.NOTE] Später deaktivieren Stretch Datenbank daran Stretch-Datenbank für eine Tabelle oder eine Datenbank deaktiviert das remote-Objekt nicht löschen. Wenn Sie die entfernte Tabelle oder der entfernten Datenbank löschen möchten, müssen Sie mithilfe das Azure-Verwaltungsportal ablegen. Die entfernten Objekte weiterhin Azure Kosten, bis Sie sie manuell löschen.

## <a name="before-you-get-started"></a>Bevor Sie beginnen

-   Vor dem Konfigurieren einer Datenbank für Strecken empfohlen Stretch Datenbank Advisor, Datenbanken und Tabellen für Strecken ausführen. Stretch-Datenbank Advisor identifiziert auch blockierenden Probleme. Weitere Informationen finden Sie unter [identifizieren Datenbanken und Tabellen für die Stretch-Datenbank](sql-server-stretch-database-identify-databases.md).

-   Überprüfen Sie [Grenzen für Strecken Datenbank](sql-server-stretch-database-limitations.md).

-   Dehnen Datenbank migriert Daten in Azure. Daher müssen Sie ein Azure-Konto und ein Abonnement für die Abrechnung. Ein Azure-Konto [hier](http://azure.microsoft.com/pricing/free-trial/)abrufen.

-   Haben Sie die Verbindung und die Anmeldung Informationen einen neuen Azure Server erstellen oder einen vorhandenen Azure-Server auswählen.

## <a name="EnableTSQLServer"></a>Voraussetzung: Stretch-Datenbank auf dem Server aktivieren
Stretch-Datenbank auf einer Datenbank oder einer Tabelle zu aktivieren, müssen Sie auf dem lokalen Server aktivieren. Dieser Vorgang erfordert Berechtigungen Sysadmin und Serveradmin.

-   Haben Sie die erforderlichen Administratorberechtigungen konfiguriert der Assistent **Aktivieren Datenbank Stretch** Server für Strecken.

-   Die erforderlichen Berechtigungen besitzen, Administrator ist, aktivieren Sie die Option manuell durch Ausführen von **sp\_konfigurieren** bevor Sie den Assistenten ausführen oder Administrator hat den Assistenten ausführen.

Um Stretch-Datenbank manuell auf dem Server zu aktivieren, führen **sp\_konfigurieren** und aktivieren Sie die Option **remote-Daten-Archiv** . Im folgende Beispiel aktiviert **Remotedaten Archiv** durch den Wert 1 festlegen.

```
EXEC sp_configure 'remote data archive' , '1';
GO

RECONFIGURE;
GO
```
Weitere Informationen finden Sie unter [Konfigurieren der remote-Daten archivieren Serverkonfigurationsoption](https://msdn.microsoft.com/library/mt143175.aspx) und [Sp_configure (Transact-SQL)](https://msdn.microsoft.com/library/ms188787.aspx).

## <a name="Wizard"></a>Verwenden Sie den Assistenten Stretch-Datenbank in einer Datenbank aktivieren
Informationen der Datenbank aktivieren Stretch-Assistenten sowie Informationen, die Sie eingeben müssen die Optionen, die Sie machen, finden Sie unter [Erste Schritte mit der Datenbank aktivieren Stretch-Assistenten](sql-server-stretch-database-wizard.md).

## <a name="EnableTSQLDatabase"></a>Verwenden Sie Transact\-SQL Stretch-Datenbank in einer Datenbank aktivieren
Bevor Sie einzelne Tabellen Stretch Datenbank aktivieren können, müssen Sie die Datenbank aktivieren.

Aktivieren der Stretch-Datenbank auf einer Datenbank oder einer Tabelle Db erfordert\_Besitzerberechtigungen. Aktivieren Stretch-Datenbank in einer Datenbank sind auch Datenbank-Berechtigungen erforderlich.

1.  Bevor Sie beginnen, wählen Sie einen vorhandenen Azure Server für die Stretch-Datenbank migriert Daten oder erstellen Sie einen neuen Azure-Server.

2.  Erstellen Sie eine Firewall-Regel auf Azure Server mit den IP-Adressbereich von SQL Server, mit dem SQL Server die Kommunikation mit dem Remoteserver.

    Finden Sie leicht die Werte müssen und erstellen die Firewallregel Azure Server Object Explorer in SQL Server Management Studio (SSMS) eine Verbindung herstellen möchten. SSMS unterstützt Sie beim Erstellen der Regel öffnen Sie das Dialogfeld die bereits die erforderlichen Werte der IP-Adresse enthält.

    ![Erstellen Sie eine Firewall-Regel in SSMS][FirewallRule]

3.  Zum Konfigurieren der Stretch-Datenbank einer SQL Server-Datenbank muss die Datenbank einen Datenbank-Hauptschlüssel verfügen. Datenbank-Hauptschlüssel sichert die Anmeldeinformationen, die Stretch-Datenbank mit der entfernten Datenbank herstellt. Hier ist ein Beispiel, einen neue Datenbank-Hauptschlüssel erstellt.

    ```tsql
    USE <database>;
    GO

    CREATE MASTER KEY ENCRYPTION BY PASSWORD ='<password>';
    GO
    ```

    Weitere Informationen zu den Datenbank-Hauptschlüssel finden Sie unter [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) und [erstellen einen Datenbank-Hauptschlüssel](https://msdn.microsoft.com/library/aa337551.aspx).

4.  Beim Konfigurieren einer Datenbank für Stretch-Datenbank müssen Sie Anmeldeinformationen für Strecken Datenbank für Kommunikation in lokalen SQL Server und dem Remoteserver Azure bereitzustellen. Sie haben zwei Optionen.

    -   Sie können einen Administrator-Anmeldeinformationen angeben.

        -   Stretch-Datenbank mithilfe des Assistenten zum Aktivieren, können Sie die Anmeldeinformationen gleichzeitig erstellen.

        -   Stretch-Datenbank aktivieren, indem Sie **ALTER DATABASE**ausgeführt werden soll, müssen Sie die Anmeldeinformationen bevor Sie **ALTER DATABASE** Stretch Datenbank manuell zu erstellen.

        Hier ist ein Beispiel, neue Anmeldeinformationen erstellt.

        ```tsql
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>
            WITH IDENTITY = '<identity>' , SECRET = '<secret>';
        GO
        ```

        Weitere Informationen zu den Anmeldeinformationen finden Sie unter [Erstellen Datenbank begrenzt Anmeldeinformationen (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx). Erstellen von Anmeldeinformationen ist ALTER ANY CREDENTIAL-Berechtigungen erforderlich.

    -   Verbundenen Dienstkonto für SQL Server können mit dem Remoteserver Azure kommunizieren, wenn Folgendes wahr sind.

        -   Das Dienstkonto, unter dem die Instanz von SQL Server ausgeführt wird, ist ein Domänenkonto.

        -   Das Domänenkonto gehört zu einer Domäne, deren Active Directory Azure Active Directory verbunden ist.

        -   Azure Remoteserver konfiguriert Azure Active Directory-Authentifizierung unterstützen.

        -   Das Dienstkonto, unter dem die Instanz von SQL Server ausgeführt wird, muss als Dbmanager oder Sysadmin-Konto auf dem Remoteserver Azure konfiguriert werden.

5.  Führen Sie zum Konfigurieren einer Datenbank für Strecken Datenbank den Befehl ALTER DATABASE.

    1.  Für das Argument SERVER Geben Sie den Namen eines vorhandenen Servers Azure einschließlich der `.database.windows.net` Teil des Namens \- z. B. `MyStretchDatabaseServer.database.windows.net`.

    2.  Einen vorhandenen Administrator-Anmeldeinformationen Anmeldeinformationen Argument bzw. Geben Sie verbundene\_SERVICE\_Konto = ON. Das folgende Beispiel stellt bestehende.

    ```tsql
    ALTER DATABASE <database name>
        SET REMOTE_DATA_ARCHIVE = ON
            (
                SERVER = '<server_name>',
                CREDENTIAL = <db_scoped_credential_name>
            ) ;
    GO
    ```

## <a name="next-steps"></a>Nächste Schritte
-   [Aktivieren Stretch Datenbank eine Tabelle](sql-server-stretch-database-enable-table.md) zusätzliche Tabellen aktivieren.

-   [Stretch-Monitor-Datenbank](sql-server-stretch-database-monitor.md) den Status der Datenmigration anzuzeigen.

-   [Anhalten und Fortsetzen von Stretch-Datenbank](sql-server-stretch-database-pause.md)

-   [Verwaltung und Problembehandlung von Stretch-Datenbank](sql-server-stretch-database-manage.md)

-   [Sicherungsdatenbanken Strecken aktiviert](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Siehe auch

[Datenbanken und Tabellen für Strecken Datenbank identifizieren](sql-server-stretch-database-identify-databases.md)

[ALTER DATABASE SET Options (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[FirewallRule]: ./media/sql-server-stretch-database-enable-database/firewall.png
