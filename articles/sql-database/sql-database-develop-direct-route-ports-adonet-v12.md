<properties 
    pageTitle="Ports über 1433 für SQL-Datenbank | Microsoft Azure"
    description="Clientverbindungen von ADO.NET zu Azure SQL-Datenbank V12 manchmal umgehen den Proxy und direkt mit der Datenbank interagieren. Anschlüsse als 1433 Bedeutung."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="" />


<tags 
    ms.service="sql-database" 
    ms.workload="drivers"
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016"
    ms.author="annemill"/>


# <a name="ports-beyond-1433-for-adonet-45-and-sql-database-v12"></a>Ports über 1433 für ADO.NET 4.5 und SQL Datenbank V12


Dieses Thema beschreibt die Änderungen Azure SQL-Datenbank V12 bringt das Verbindungsverhalten der Clients, die ADO.NET 4.5 oder höher.


## <a name="v11-of-sql-database-port-1433"></a>V11 SQL Datenbank: Port 1433


Wenn ein Clientprogramm ADO.NET 4.5 verwendet zu Abfrage mit SQL-Datenbank V11, lautet die interne Reihenfolge


1. ADO.NET versucht, SQL-Datenbank herstellen.

2. ADO.NET verwendet Port 1433 Middleware Modul aufrufen und die Middleware mit SQL-Datenbank verbindet.

3. SQL-Datenbank sendet die Antwort an die Middleware, die auf ADO.NET Port 1433 weiterleitet.


**Terminologie:** Davor liegenden beschrieben darauf hin, dass SQL-Datenbank mit *Proxy weiterleiten*ADO.NET interagiert. Wenn keine Middleware beteiligt waren, würden wir sagen, dass die *direkte Route* verwendet wurde.


## <a name="v12-of-sql-database-outside-vs-inside"></a>V12 SQL Datenbank: außerhalb in Vs


Verbindung zum V12 bitten wir, ob das Clientprogramm *innerhalb oder *außerhalb* * die Azure-Cloud-Grenze ausgeführt wird. Die Unterabschnitten werden zwei Szenarien.


#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Außen:* Läuft auf dem desktop


Anschluss 1433 ist der einzige Port, der auf Ihrem Computer geöffnet sein muss, die die SQL-Datenbank-Client-Anwendung hostet.


#### <a name="inside-client-runs-on-azure"></a>*In:* Läuft auf Azure


Wenn der Client innerhalb der Azure-Cloud ausgeführt wird, verwendet eine *direkte* Interaktion mit der SQL-Datenbankserver nennen wir können. Nach dem Herstellen einer Verbindung eine weitere Interaktion zwischen Client und Datenbank kein Proxy Middleware beinhalten.


Die Reihenfolge lautet wie folgt:


1. ADO.NET 4.5 (oder höher) eine kurze zwischen Azure Cloud initiiert und empfängt eine dynamisch identifizierte Portnummer.
 - Dynamisch identifizierte Portnummer ist im Bereich der 11000 11999 oder 14000 14999.

2. ADO.NET verbindet dann mit der SQL Server direkt mit keine Middleware zwischen.

3. Abfragen an die Datenbank gesendet und Ergebnisse direkt an den Client zurückgegeben.


Stellen Sie sicher, dass Port Bereiche 11000 11999 und 14000-14999 auf dem Clientcomputer Azure für ADO.NET 4.5 Kundeninteraktionen mit SQL Datenbank V12 verfügbar bleiben.

- Insbesondere müssen Ports im Bereich der ausgehenden Blocker frei.

- Die **Windows-Firewall mit erweiterter Sicherheit** Steuerelemente in Azure-VM den Port zu.
 - [Firewall-Benutzeroberfläche](http://msdn.microsoft.com/library/cc646023.aspx) können Sie eine Regel hinzufügen, die das **TCP-** Protokoll mit Portbereich mit Syntax wie **11000 11999**angeben.


## <a name="version-clarifications"></a>Version clarifications


Dieser Abschnitt erläutert die Moniker Versionen beziehen. Außerdem werden einige Kombinationen zwischen Versionen aufgelistet.


#### <a name="adonet"></a>ADO.NET


- ADO.NET 4.0 unterstützt keine 7.4 7.3 TDS-Protokoll.
- ADO.NET 4.5 und höher unterstützt TDS 7.4 Protokoll.


#### <a name="sql-database-v11-and-v12"></a>SQL Datenbank V11 und V12


In diesem Thema werden die Client-Verbindung Unterschiede zwischen SQL Datenbank V11 und V12 hervorgehoben.


*Hinweis:* Die Transact-SQL-Anweisung `SELECT @@version;` gibt einen Wert zurück, der eine Zahl wie "11" starten oder '12' und die unsere Versionsnamen V11 und V12 für SQL-Datenbank.


## <a name="related-links"></a>Verwandte links


- ADO.NET 4.6 wurde am 20. Juli 2015 veröffentlicht. Eine Blog-Ankündigung vom Team .NET steht [hier](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).


- ADO.NET 4.5 wurde am 15. August 2012 veröffentlicht. Eine Blog-Ankündigung vom Team .NET steht [hier](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
 - Ein Blog über ADO.NET 4.5.1 steht [hier](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).


- [Liste der TDS-Protokoll version](http://www.freetds.org/userguide/tdshistory.htm)


- [SQL Datenbank-Anwendungsentwicklung, Überblick](sql-database-develop-overview.md)


- [Azure SQL-Datenbank-firewall](sql-database-firewall-configure.md)


- [Gewusst wie: Konfigurieren der Firewall für SQL-Datenbank](sql-database-configure-firewall-settings.md)

