<properties
   pageTitle="Nicht unterstützt in Azure SQL-Datenbank T-SQL | Microsoft Azure"
   description="Transact-SQL-Anweisungen, die in Azure SQL-Datenbank weniger als vollständig unterstützt werden."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/30/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="azure-sql-database-transact-sql-differences"></a>Azure SQL-Datenbank-Transact-SQL-Unterschiede


Transact-SQL-Funktionen, die Clientanwendungen abhängig sind in Microsoft SQL Server und Azure SQL-Datenbank unterstützt. Eine unvollständige Liste der unterstützten Funktionen für Applikationen folgt:

- Datentypen.
- Operatoren.
- Zeichenfolge, arithmetischen, logischen und Cursor-Funktionen.

Azure SQL-Datenbank dient jedoch Funktionen keine Abhängigkeit von der **master** -Datenbank zu isolieren. Daher viele Server-Aktivitäten sind für SQL-Datenbank und nicht unterstützt. Features, die in SQL Server veraltet werden im Allgemeinen in SQL-Datenbank nicht unterstützt.

> [AZURE.NOTE]
> Dieses Thema beschreibt die Funktionen, die mit SQL-Datenbank in die aktuelle Version aktualisiert werden; SQL Datenbank V12. Weitere Informationen über V12 finden Sie [SQL Datenbank V12 was neu](sql-database-v12-whats-new.md).

Die folgenden Abschnitte enthalten teilweise unterstützte Features und Funktionen, die nicht vollständig unterstützt werden.


## <a name="features-partially-supported-in-sql-database-v12"></a>In SQL-Datenbank V12 teilweise unterstützte Funktionen

SQL Datenbank V12 unterstützt einige, aber nicht alle Argumente, die vorhanden sind in der entsprechenden SQL Server 2016 Transact-SQL-Anweisung. Beispielsweise steht die CREATE PROCEDURE-Anweisung jedoch alle Optionen erstellen nicht verfügbar sind. Finden Sie unter verknüpfte Syntax Themen Informationen zu den unterstützten jede Anweisung.

- Datenbanken: [CREATE](https://msdn.microsoft.com/library/dn268335.aspx )/[Ändern](https://msdn.microsoft.com/library/ms174269.aspx)
- DMVs sind für Funktionen.
- Funktionen: [CREATE](https://msdn.microsoft.com/library/ms186755.aspx)/[Funktion ändern](https://msdn.microsoft.com/library/ms186967.aspx)
- [KILL](https://msdn.microsoft.com/library/ms173730.aspx) 
- Benutzernamen: [CREATE](https://msdn.microsoft.com/library/ms189751.aspx)/[ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx)
- Gespeicherte Prozeduren: [CREATE](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)
- Tabellen: [Erstellen](https://msdn.microsoft.com/library/dn305849.aspx)/[Ändern](https://msdn.microsoft.com/library/ms190273.aspx)
- Typen (Benutzerdefiniert): [Typ erstellen](https://msdn.microsoft.com/library/ms175007.aspx)
- Benutzer: [Erstellen](https://msdn.microsoft.com/library/ms173463.aspx)/[Benutzer ändern](https://msdn.microsoft.com/library/ms176060.aspx)
- Ansichten: [CREATE](https://msdn.microsoft.com/library/ms187956.aspx)/[Ansicht ändern](https://msdn.microsoft.com/library/ms173846.aspx)

## <a name="features-not-supported-in-sql-database"></a>In SQL-Datenbank nicht unterstützte Features

- Sortierung der Objekte
- Verbindung verknüpft: Endpunkt Aussagen, ORIGINAL_DB_NAME. SQL-Datenbank unterstützen Windows-Authentifizierung, unterstützt jedoch ähnliche Azure Active Directory-Authentifizierung. Einigen Authentifizierungsarten müssen die neueste Version von SSMS. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse durch Verwendung von Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).
- Cross-Abfragen mit drei oder vier Namensteilen. (Schreibgeschützte datenbankübergreifenden Abfragen werden mithilfe von [elastische Abfrage](sql-database-elastic-query-overview.md)unterstützt.)
- Cross-Datenbank Besitzketten, VERTRAUENSWÜRDIGEN Einstellung
- Daten Collector
- Datenbankdiagramme
- Database Mail
- DATABASEPROPERTY (DATABASEPROPERTYEX verwenden)
- EXECUTE AS Login
- Verschlüsselungsschlüsseln: Management erweiterbar
- Ereignisse: Ereignisse Benachrichtigungen, Query notifications
- Features zur Datenbank Platzierung, Größe und Datenbankdateien automatisch von Microsoft Azure verwaltet werden.
- Funktionen, die auf hohe Verfügbarkeit beziehen die über Ihr Konto Microsoft Azure verwaltet: Sichern, wiederherstellen, AlwaysOn-Database mirroring, Protokollversand Recovery-Modi. Weitere Informationen finden Sie unter Azure SQL-Datenbank sichern und wiederherstellen.
- Funktionen, die auf SQL Datenbank Protokollleser: Push-Replikation, Change Data Capture.
- Funktionen, die auf der SQL Server-Agent oder die MSDB-Datenbank: Aufträge, Alarme, Operatoren, Policy-basiertes Management, Datenbank-Mail, zentralen Verwaltungsserver.
- FILESTREAM
- Funktionen: Fn_get_sql "fn_virtualfilestats", fn_virtualservernodes
- Globale temporäre Tabellen
- Hardware-bezogene Einstellungen: Speicher, Arbeitsthreads, CPU-Affinität, Ablaufverfolgungsflags usw.. Verwenden Sie Service-Level.
- HAS_DBACCESS
- KILL STATS JOB
- Verbindungsserver OPENQUERY, OPENROWSET, OPENDATASOURCE, BULK INSERT und vierteiligen Namen
- Master-Ziel-Server
- .NET Framework- [CLR-Integration mit SQL Server](http://msdn.microsoft.com/library/ms254963.aspx)
- Ressourcenkontrolle
- Semantische Suche
- Serveranmeldeinformationen. Datenbank Anmeldeinformationen stattdessen beschränkt.
- Trennen Elemente: Server Rollen IS_SRVROLEMEMBER, sys.login_token. Server-Berechtigungen sind nicht verfügbar, obwohl einige Berechtigungen auf Datenbankebene Fassung. Einige DMVs auf Serverebene sind nicht verfügbar, obwohl einige Datenbankebene DMVs Fassung.
- Serverlose Express: Localdb Benutzerinstanzen
- Service broker
- SET REMOTE_PROC_TRANSACTIONS
- HERUNTERFAHREN
- sp_addmessage
- Sp_configure Optionen und konfigurieren. Einige Optionen sind mit [Gültigkeitsbereich Konfiguration ändern](https://msdn.microsoft.com/library/mt629158.aspx).
- sp_helpuser
- sp_migrate_user_to_contained
- SQL Server überwachen. Verwenden Sie stattdessen SQL Datenbank überwachen.
- SQL Server Profiler
- SQL Server-trace
- Ablaufverfolgungsflags. Trace-Flag Elemente wurden Kompatibilitätsmodi verschoben.
- Transact-SQL-Debuggen
- Trigger: Servergültigkeitsbereich oder Logon-Trigger
- USE-Anweisung: um den Datenbankkontext in eine andere Datenbank ändern, müssen Sie eine neue Verbindung mit der neuen Datenbank.


## <a name="full-transact-sql-reference"></a>Vollständige Transact-SQL-Referenz

Weitere Informationen zu Transact-SQL-Grammatik, Syntax und Beispiele finden Sie unter [Transact-SQL-Referenz (Datenbankmodul)](https://msdn.microsoft.com/library/bb510741.aspx) in SQL Server-Onlinedokumentation. 

### <a name="about-the-applies-to-tags"></a>Über die Tags "Gilt für"

Transact-SQL-Referenz enthält Themen im Zusammenhang mit SQL Server Version 2008 bis heute. Es unterhalb des ist ein Symbol, Liste die vier SQL Server-Plattformen und Anwendbarkeit angibt. Verfügbarkeitsgruppen wurden z. B. in SQL Server 2012 eingeführt. [Verfügbarkeit Gruppe erstellen](https://msdn.microsoft.com/library/ff878399.aspx) Thema gibt an, dass die Anweisung gilt ** SQL Server (beginnend mit 2012). Die Anweisung gilt nicht für SQL Server 2008, SQL Server 2008 R2, Azure SQL-Datenbank, Azure SQL Data Warehouse oder Parallel Data Warehouse.

In einigen Fällen kann allgemeine Gegenstand eines Themas in einem Produkt verwendet werden, aber es gibt geringfügige Unterschiede zwischen Produkten. Die Unterschiede werden Mittelpunkte in das Thema angegeben.

