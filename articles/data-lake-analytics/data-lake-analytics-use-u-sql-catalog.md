<properties
   pageTitle="Einführen von Azure Data Lake Analytics U-SQL Katalog | Azure"
   description="Einführen von Azure Data Lake Analytics U-SQL-Katalog"
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

# <a name="use-u-sql-catalog"></a>U-SQL-Katalog

U-SQL Katalog dient und strukturiert, damit sie von U-SQL-Skripts verwendet werden können. Der Katalog ermöglicht die höchste Leistung mit Daten in Azure Data Lake.

Jede Azure Data Lake Analytics-Konto verfügt über genau ein U-SQL Katalog zugeordnet. U-SQL-Katalog kann nicht gelöscht werden. U-SQL Kataloge kann derzeit zwischen Konten See Datenspeicher verwendet werden.

Jedes U-SQL-Katalog enthält eine Datenbank namens **Master**. Der Master-Datenbank kann nicht gelöscht werden.  Jeder Katalog U-SQL kann weitere zusätzlichen Datenbanken enthalten.

U-SQL-Datenbank enthält:

- Baugruppen-Freigabecode .NET zwischen U-SQL-Skripts.
- Werte Funktionen – weitergeben U-SQL-Code zwischen U-SQL-Skripts
- Tabellen – Daten zwischen U-SQL-Skripts.
- Schemas - Freigabe Tabellenschemas zwischen U-SQL-Skripts.

## <a name="manage-catalogs"></a>Verwalten von Katalogen
Jedes Konto Azure Data Lake Analytics hat eine standardmäßige Azure See Datenspeicher Konto zugeordnet. Dieses Konto See Datenspeicher wird als Standardkonto See Datenspeicher bezeichnet. U-SQL Katalog wird im Datenspeicher See Standardkonto/Catalog-Ordner gespeichert. Löschen Sie alle Dateien im Ordner "/ Catalog" nicht.

### <a name="use-azure-portal"></a>Azure-Portal nutzen

Siehe [Verwalten See Datenanalyse mithilfe von portal](data-lake-analytics-manage-use-portal.md#view-u-sql-catalog)


### <a name="use-data-lake-tools-for-visual-studio"></a>Verwenden Sie Data Lake-Tools für Visual Studio.

Data Lake-Tools für Visual Studio können Sie den Katalog verwalten.  Weitere Informationen zu den Tools finden Sie unter [Using Data See Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

**Katalog verwalten**

1. Öffnen Sie Visual Studio und Azure an. Die Anleitung finden Sie unter [Verbinden in Azure](data-lake-analytics-data-lake-tools-get-started.md#connect-to-azure).
1. Öffnen Sie **Server-Explorer** , indem Sie **STRG + ALT + S**drücken.
2. Im **Server-Explorer**erweitern Sie **Azure**, erweitern Sie **See Datenanalyse**zu, erweitern Sie Ihr Konto See Datenanalyse, erweitern Sie **Datenbanken**und dann **master**.



    - Zum Hinzufügen einer neuen Datenbank Maustaste auf **Datenbank**, und klicken Sie auf **Datenbank erstellen**.
    - Neue Assembly hinzufügen, mit der rechten Maustaste **Assemblys**und **Assembly registrieren**klicken.
    - Fügen Sie ein neues Schema klicken Sie **Schemata**und dann auf "Create Schema **.
    - Zum Hinzufügen einer neuen Tabelle Maustaste auf **Tabellen**, und klicken Sie auf "" erstellen Tabelle **.
    - Eine neue Tabellenwertfunktion finden Sie unter [U-SQL Entwickeln benutzerdefinierter Operatoren für Datenanalysen See Aufträge](data-lake-analytics-u-sql-develop-user-defined-operators.md).


![U-SQL Visual Studio Kataloge durchsuchen](./media/data-lake-analytics-use-u-sql-catalog/data-lake-analytics-browse-catalogs.png)


## <a name="see-also"></a>Siehe auch

- Erste Schritte
    - [Erste Schritte mit See Datenanalyse mit Azure-portal](data-lake-analytics-get-started-portal.md)
    - [Erste Schritte mit See Datenanalyse mithilfe von Azure PowerShell](data-lake-analytics-get-started-powershell.md)
    - [Erste Schritte mit See Datenanalyse mit Azure .NET SDK](data-lake-analytics-get-started-net-sdk.md)
    - [Entwickeln Sie U-SQL-Skripts mit Daten See Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
    - [Erste Schritte mit Azure Data Lake Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md)

- U-SQL und Entwicklung
    - [Erste Schritte mit Azure Data Lake Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md)
    - [Verwenden Sie Fensterfunktionen U-SQL Azure Data Lake Analytics Aufträge](data-lake-analytics-use-window-functions.md)
    - [Entwickeln Sie U-SQL benutzerdefinierte Operatoren für Datenanalysen See Projekte](data-lake-analytics-u-sql-develop-user-defined-operators.md)

- Management
    - [Verwalten Sie Azure Data Lake Analytics mit Azure-portal](data-lake-analytics-manage-use-portal.md)
    - [Verwalten von Azure See Datenanalyse mithilfe von Azure PowerShell](data-lake-analytics-manage-use-powershell.md)
    - [Überwachung und Problembehandlung von Azure Data Lake Analytics Aufträge mithilfe von Azure-portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

- End-to-End-Lernprogramm
    - [Verwenden Sie interaktive Tutorials Azure Data Lake Analytics](data-lake-analytics-use-interactive-tutorials.md)
    - [Analysieren Sie Website-Protokolle mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md)
