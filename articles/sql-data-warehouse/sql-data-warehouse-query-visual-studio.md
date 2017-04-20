<properties
   pageTitle="Abfrage SQL Azure Datawarehouse (Visual Studio) | Microsoft Azure"
   description="Abfrage SQL Datawarehouse mit Visual Studio."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="query-azure-sql-data-warehouse-visual-studio"></a>Abfrage SQL Azure Datawarehouse (Visual Studio)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure maschinelles lernen](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Verwenden Sie Visual Studio Abfrage Azure SQL Data Warehouse in wenigen Minuten. Diese Methode verwendet die Erweiterung SQL Server Data Tools (SSDT) in Visual Studio. 

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm zu verwenden, müssen wie folgt vor:

+ Ein vorhandenes SQL Datawarehouse. Um eine zu erstellen, finden Sie unter [Erstellen einer SQL Data Warehouse][].
+ SSDT für Visual Studio. Wenn Sie Visual Studio haben, haben Sie wahrscheinlich bereits diese. Installations- und Optionen finden Sie unter [Installieren von Visual Studio und SSDT][].
+ Der vollqualifizierte Name für SQL Server. Diese finden Sie [mit SQL Data Warehouse][].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. Verbinden Sie mit SQL Datawarehouse

1. Öffnen Sie Visual Studio 2013 oder 2015.
2. Öffnen Sie SQL Server-Objekt-Explorer. Wählen Sie dazu die **Ansicht** > **SQL Server-Objekt-Explorer**.

    ![SQL Server-Objekt-Explorer][1]

3. Klicken Sie auf **SQL Server hinzufügen** .

    ![SQL Server hinzufügen][2]

4. Füllen Sie die Felder in der Verbindung zum Server.

    ![Verbindung zu Server herstellen][3]

    - **Servername**. Geben Sie den **Servernamen** zuvor identifiziert.
    - **Authentifizierung**. Wählen Sie **SQL Server-Authentifizierung** oder **Active Directory-integrierte Authentifizierung**.
    - **Benutzername** und **Kennwort**. Geben Sie Benutzernamen und Kennwort über SQL Server-Authentifizierung ausgewählt wurde.
    - Klicken Sie auf **Verbinden**.

5. Erweitern Sie zum Untersuchen von Ihrem SQL Azure-Server. Sie können die Datenbanken des Servers anzeigen. Erweitern Sie AdventureWorksDW um die Tabellen in der Datenbank anzuzeigen.

    ![AdventureWorksDW durchsuchen][4]

## <a name="2-run-a-sample-query"></a>2. Führen Sie eine Abfrage

Nun, eine Verbindung zur Datenbank hergestellt wurde, Schreiben wir eine Abfrage.

1. Klicken Sie auf die Datenbank in SQL Server-Objekt-Explorer.

2. Wählen Sie die **neue Abfrage**. Ein neues Abfragefenster wird geöffnet.

    ![Neue Abfrage][5]

3. Kopieren Sie diese TSQL-Abfrage in das Abfragefenster:

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Führen Sie die Abfrage. Dazu klicken Sie auf den grünen Pfeil, oder verwenden Sie die folgende Verknüpfung: `CTRL` + `SHIFT` + `E`.

    ![Abfrage ausführen][6]

5. Sehen Sie sich die Ergebnisse der Abfrage. In diesem Beispiel enthält die Tabelle FactInternetSales 60398 Zeilen.

    ![Abfrageergebnisse][7]

## <a name="next-steps"></a>Nächste Schritte

Sie verbinden und Abfragen können, versuchen Sie die [Visualisierung der Daten mit PowerBI][].

Konfigurieren Sie Ihre Umgebung für Azure Active Directory-Authentifizierung finden Sie unter [Authentifizierung in SQL Data Warehouse][].

<!--Arcticles-->
[Verbinden Sie mit SQL Datawarehouse]: sql-data-warehouse-connect-overview.md
[Erstellen Sie ein SQL Datawarehouse]: sql-data-warehouse-get-started-provision.md
[Installieren von Visual Studio und SSDT]: sql-data-warehouse-install-visual-studio.md
[Authentifizierung mit SQL Datawarehouse]: sql-data-warehouse-authentication.md
[Visualisierung der Daten mit PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
