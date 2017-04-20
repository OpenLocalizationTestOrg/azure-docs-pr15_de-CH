<properties
   pageTitle="Website-Protokolle mit Azure Data Lake Analytics analysieren | Azure"
   description="Informationen Sie zum Website-Protokolle mit See Datenanalyse zu analysieren. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-analyze-website-logs-using-azure-data-lake-analytics"></a>Lernprogramm: Analysieren Sie Website-Protokolle mit Azure Data Lake Analytics

Informationen Sie zum Website-Protokolle über See Datenanalyse besonders herausfinden, welche Quellen Fehler ausgeführt wurde, als sie der Website analysieren.

>[AZURE.NOTE] Wenn Sie die Anwendung arbeiten möchten, spart Zeit [mit Azure Data Lake Analytics interaktive Tutorials](data-lake-analytics-use-interactive-tutorials.md)zu. Dieses Lernprogramm basiert auf dem gleichen Szenario und denselben Code. Dieses Lernprogramm soll Entwicklern das Erstellen und Ausführen einer Anwendung See Datenanalyse zu vermitteln.

## <a name="prerequisites"></a>Komponenten:

- **Visual Studio 2015, Visual Studio 2013 4 oder Visual C++ installiert Visual Studio 2012 aktualisieren**.
- **Microsoft Azure SDK für .NET Version 2.5 oder höher**.  Installieren Sie mit dem [Webplattform-Installer](http://www.microsoft.com/web/downloads/platform.aspx).
- **[Data Lake-Tools für Visual Studio](http://aka.ms/adltoolsvs)**.

    Sobald Daten See Tools für Visual Studio installiert ist, sehen Sie **Daten See** Menüs in Visual Studio:

    ![U-SQL Visual Studio-Menü](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **Grundkenntnisse über See Datenanalyse und See-Tools für Visual Studio**. Zum Einstieg finden Sie unter:

    - [Erste Schritte mit Azure See Datenanalyse mithilfe von Azure-Portal](data-lake-analytics-get-started-portal.md).
    - [U-SQL entwickeln Skript Daten See-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

- **Ein Konto See Datenanalyse.**  Siehe [Erstellen einer Azure Data Lake Analytics-Konto](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).

    Die See-Tools unterstützen nicht erstellen See Datenanalyse Konten.  Sie müssen also mit Azure-Portal, Azure PowerShell, .NET SDK oder Azure CLI erstellen.
- **Hochladen Sie die Beispieldaten See Datenanalyse Konto.** Finden Sie unter [SearchLog.tsv auf See Datenspeicher Standardkonto hochladen](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

    Zum Ausführen eines Auftrags See Datenanalyse benötigen Sie einige Daten. Obwohl die See Tools Hochladen von Daten unterstützt, verwenden Sie das Portal die Beispieldaten zu diesem Lernprogramm leichter hochladen.

## <a name="connect-to-azure"></a>Verbinden mit Azure

Vor dem Erstellen und U-SQL-Skripts testen, müssen Sie zunächst in Azure verbinden.

**Verbindung mit dem Datenanalyse**

1. Öffnen Sie Visual Studio.
2. Klicken Sie im Menü **Daten See** **-Optionen**.
4. Klicken Sie auf **Anmelden**oder **Benutzer wechseln** , wenn ein Benutzer angemeldet hat und gehen.
5. Klicken Sie auf **OK** , um das Dialogfeld-Optionen zu schließen.

**Datenanalyse See Konten durchsuchen**

1. Öffnen Sie von Visual Studio **Server-Explorer** durch **STRG + ALT + S**drücken.
2. **Server-Explorer**erweitern Sie **Azure**, und dann **See Datenanalyse**. Sie werden Ihre Datenanalyse See Kontenliste finden gibt. Sie können nicht aus dem Studio See Datenanalyse Konten erstellen. Zum Erstellen eines Kontos finden Sie unter [Erste Schritte mit Azure See Datenanalyse mithilfe von Azure-Portal](data-lake-analytics-get-started-portal.md) oder [Erste Schritte mit Azure Data Lake Analytics Azure PowerShell verwenden](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>U-SQL-Anwendung entwickeln

U-SQL-Anwendung ist meist ein U-SQL-Skript. Erfahren Sie mehr über U-SQL finden Sie unter [Erste Schritte mit U-SQL](data-lake-analytics-u-sql-get-started.md).

Sie können die Anwendung zusätzlich benutzerdefinierte Operatoren hinzufügen.  Weitere Informationen finden Sie in der [U-SQL Entwickeln benutzerdefinierter Operatoren für Datenanalysen See Aufträge](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**Zum Erstellen und Übermitteln eines Auftrags See Datenanalyse**

1. Klicken Sie im Menü **Datei** auf **neu**und klicken Sie auf **Projekt**.
2. U-SQL Projekttyp ausgewählt.

    ![Neues U SQL Visual Studio-Projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Klicken Sie auf **OK**. Visual Studio erstellt eine Lösung mit einer Script.usql-Datei.
4. Geben Sie das folgende Skript in der Datei Script.usql:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            PARTITIONED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    U-SQL finden Sie unter [Data Lake Analytics U-SQL-Sprache beginnen](data-lake-analytics-u-sql-get-started.md).    

5. Dem Projekt fügen Sie ein neues U-SQL-Skript hinzu, und geben Sie Folgendes ein:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();

6. Wechseln Sie zu dem ersten U-SQL-Skript und neben die Schaltfläche **Absenden** , geben Sie Ihr Konto Analytics an.
7. **Projektmappen-Explorer**klicken Sie mit der rechten Maustaste auf **Script.usql**und klicken Sie dann auf **Skript erstellen**. Überprüfen Sie die Ergebnisse im Ausgabebereich.
8. **Projektmappen-Explorer**klicken Sie mit der rechten Maustaste auf **Script.usql**und klicken Sie dann auf **Skript senden**.
9. Überprüfen Sie **Analytics Konto** dem soll die Stapelverarbeitung, und klicken Sie auf **Senden**. Übermittlung Ergebnisse und Auftrag Link stehen in die See-Tools für Visual Studio-Fenster, wenn die Übermittlung abgeschlossen ist.
10. Warten Sie, bis der Auftrag abgeschlossen ist.  Wenn der Auftrag nicht wahrscheinlich fehlt die Quelldatei.  Den erforderlichen Abschnitt dieser praktischen Einführung anzeigen Weitere Informationen zur Problembehandlung finden Sie unter [Überwachung und Problembehandlung von Azure Data Lake Analytics-Aufträge](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Wenn der Auftrag abgeschlossen ist, wird den folgenden Bildschirm angezeigt:

    ![Datenanalyse See analysieren Weblogs Website Protokolle](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)

11. Jetzt wiederholen Sie Schritte 7 bis 10 für **Script1.usql**.

>[AZURE.NOTE]Sie können nicht aus lesen oder Schreiben einer U-SQL-Tabelle, die erstellt oder im selben Skript geändert wurde.  Deshalb verwenden zwei Skripts für dieses Beispiel.

**Die Auftragsausgabe an**

1. **Server-Explorer** **Azure**erweitern **See Datenanalyse**zu erweitern, erweitern Sie Ihr Konto See Datenanalyse, **Speicherkonten**, Maustaste das Standardkonto See Datenspeicher und anschließend klicken Sie auf **Explorer**.
2.  **Beispiele** zum Öffnen des Ordners Doppelklicken Sie und dann auf **Ausgaben**.
3.  Doppelklicken Sie auf **UnsuccessfulResponsees.log**.
4.  Sie können auch die Ausgabedatei in der Diagrammansicht des Auftrags doppelklicken, navigieren direkt in die Ausgabe.

## <a name="see-also"></a>Siehe auch

Zunächst mit See Datenanalyse mit anderen Tools finden Sie unter:

- [Erste Schritte mit See Datenanalyse mithilfe von Azure-Portal](data-lake-analytics-get-started-portal.md)
- [Erste Schritte mit See Datenanalyse mithilfe von Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [Erste Schritte mit See Datenanalyse mit .NET SDK](data-lake-analytics-get-started-net-sdk.md)

Um weitere Themen anzuzeigen:

- [Entwickeln Sie U-SQL-Skripts mit Daten See Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Erste Schritte mit Azure Data Lake Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md)
- [Entwickeln Sie U-SQL benutzerdefinierte Operatoren für Datenanalysen See Projekte](data-lake-analytics-u-sql-develop-user-defined-operators.md)
