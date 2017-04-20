<properties
   pageTitle="Abfragen von Azure SQL Data Warehouse (Sqlcmd) | Microsoft Azure"
   description="Abfragen von Azure SQL Data Warehouse mit dem Befehlszeilenprogramm Sqlcmd."
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
   ms.date="09/06/2016"
   ms.author="barbkess;sonyama"/>

# <a name="query-azure-sql-data-warehouse-sqlcmd"></a>Abfrage Azure SQL Data Warehouse (Sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure maschinelles lernen](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Diese exemplarische Vorgehensweise verwendet das Befehlszeilenprogramm [Sqlcmd][] einer Azure SQL Data Warehouse abgefragt.  

## <a name="1-connect"></a>1. verbinden

Erste Schritte mit [Sqlcmd][], öffnen Sie das Eingabeaufforderungsfenster, und geben **Sqlcmd** gefolgt von Verbindungszeichenfolge für SQL Data Warehouse-Datenbank. Die Verbindungszeichenfolge benötigt die folgenden Parameter:

+ **Server (-S):** Server in Form `<`Servername`>`. database.windows.net
+ **Datenbank (-d):** Name der Datenbank.
+ **Aktivieren Bezeichnern (-I):** Verbindung zu SQL Data Warehouse-Instanz müssen Bezeichner in Anführungszeichen aktiviert sein.

Um SQL Server-Authentifizierung verwenden, müssen Sie die Benutzername/Kennwort-Parameter hinzufügen:

+ **Benutzer (-U):** Serverbenutzer `<`Benutzer`>`
+ **Kennwort (-P):** Kennwort des Benutzers.

Die Verbindungszeichenfolge könnte beispielsweise folgendermaßen aussehen:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Um Azure Active Directory-integrierte Authentifizierung verwenden, müssen Sie Parameter Azure Active Directory hinzufügen:

+ **Azure Active Directory-Authentifizierung (-G):** Azure Active Directory für die Authentifizierung verwenden

Die Verbindungszeichenfolge könnte beispielsweise folgendermaßen aussehen:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [AZURE.NOTE] Authentifizierung mit Active Directory müssen [Azure Active Directory-Authentifizierung aktivieren](sql-data-warehouse-authentication.md) .

## <a name="2-query"></a>2. Abfrage

Nach Verbindung können Sie alle unterstützten Transact-SQL-Anweisungen die Instanz ausgeben.  In diesem Beispiel werden Abfragen interaktiv übermittelt.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Diese weiter Beispiele wie Abfragen im Batch-Modus die Option-Q verwenden oder Ihre SQL Sqlcmd piping ausführen.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Nächste Schritte

Weitere [Dokumentation Sqlcmd][Sqlcmd] Details über die Optionen Sqlcmd.

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[Sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
