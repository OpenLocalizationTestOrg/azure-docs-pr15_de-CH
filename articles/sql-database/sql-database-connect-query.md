<properties
    pageTitle="Verbinden mit einer Abfrage C# SQL Datenbank | Microsoft Azure"
    description="Schreiben Sie ein Programm in C# Abfragen und SQL-Datenbank. Informationen zu IP-Adressen, Verbindungszeichenfolgen sichere Anmeldung und kostenlose Visual Studio."
    services="sql-database"
    keywords="c#-Datenbankabfrage, c#, Verbinden mit Datenbank SQL C#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>



# <a name="connect-to-a-sql-database-with-visual-studio"></a>Verbinden Sie mit einer SQL-Datenbank mit Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Informationen Sie zum Verbinden mit einer SQL Azure-Datenbank in Visual Studio. 

## <a name="prerequisites"></a>Erforderliche Komponenten


Verbindung mit einer SQL-Datenbank mit Visual Studio benötigen Sie Folgendes: 


- Eine SQL-Datenbank herstellen. In diesem Artikel wird die Beispieldatenbank **AdventureWorks** verwendet. Die Beispieldatenbank AdventureWorks finden Sie unter [Erstellen der Demodatenbank](sql-database-get-started.md).


- Visual Studio 2013-Update 4 (oder höher). Microsoft bietet jetzt Visual Studio Community *kostenlos*.
 - [Visual Studio Community herunterladen](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Weitere Optionen für kostenlose Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## <a name="open-visual-studio-from-the-azure-portal"></a>Öffnen Sie Visual Studio Azure-portal


1. Auf der [Azure-Portal](https://portal.azure.com/)anmelden.

2. Klicken Sie auf **Weitere Dienste** > **SQL-Datenbanken**
3. Öffnen des **AdventureWorks** -Datenbank Blades durch Suchen und auf die *AdventureWorks* -Datenbank.

6. Klicken Sie auf **Extras** , am Anfang der Datenbank Blade:

    ![Neue Abfrage. Verbindung zum SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)

7. Klicken Sie auf **Öffnen in Visual Studio** (Visual Studio, klicken Sie auf den Link herunterladen):

    ![Neue Abfrage. Verbindung zum SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)


8. Visual Studio öffnet mit Fenster **mit Server verbinden** Verbindung zum Server und Datenbank im Portal gewählten bereits festgelegt.  (Klicken Sie auf **Optionen** zu überprüfen, ob die Verbindung mit der richtigen Datenbank.) Geben Sie Ihr Administratorkennwort Server, und klicken Sie auf **Verbinden**.


    ![Neue Abfrage. Verbindung zum SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


8. Wenn Sie eine Firewall-Regel festlegen keinen für IP-Adresse des Computers, erhalten Sie eine Nachricht *keine Verbindung herstellen* . Um eine Firewall-Regel zu erstellen, finden Sie unter [Konfigurieren einer Azure SQL-Datenbank auf Serverebene Firewall-Regel](sql-database-configure-firewall-settings.md).


9. Nach dem erfolgreichen Verbindungsaufbau öffnet der **SQL Server-Objekt-Explorer** eine Verbindung mit der Datenbank.

    ![Neue Abfrage. Verbindung zum SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)


## <a name="run-a-sample-query"></a>Eine Beispielabfrage ausführen

Die folgenden Schritte zeigen nun, dass wir mit der Datenbank verbunden sind, wie eine einfache Abfrage ausgeführt:

2. Maustaste auf die Datenbank und wählen Sie **Neue Abfrage**.

    ![Neue Abfrage. Verbindung zum SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)

3. Kopieren Sie im Abfragefenster und fügen Sie den folgenden Code.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen:

    ![Erfolg. Verbindung zum SQL-Datenbankserver: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## <a name="next-steps"></a>Nächste Schritte

- SQL-Datenbanken in Visual Studio öffnen, verwendet SQL Server Data Tools. Weitere Informationen finden Sie unter [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx).
- Verbindung mit einer SQL-Datenbank finden Sie unter [Verbinden mit SQL-Datenbank mit .NET (C#)](sql-database-develop-dotnet-simple.md).



