<properties 
    pageTitle="Dapper elastische Datenbank-Clientbibliothek mit | Microsoft Azure" 
    description="Elastische Datenbank-Clientbibliothek verwenden mit Dapper." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="using-elastic-database-client-library-with-dapper"></a>Verwenden von Clientbibliothek elastische Datenbanken mit Dapper 

Dieses Dokument ist für Entwickler, die Dapper Anwendung abhängig, aber auch zu [elastische Datenbank Werkzeuge](sql-database-elastic-scale-introduction.md) , um Applikationen erstellen, die um die Datenebene skalieren Sharding implementieren.  Dieses Dokument veranschaulicht die Änderung Dapper-basierte Anwendung, die elastische Datenbanktools Integration erforderlich sind. Mittelpunkt leitet auf die elastische Datenbank Splitter und abhängige verfassen mit Dapper. 

**Beispiel**: [elastische Datenbanktools für Azure SQL-Datenbank - gepflegten Integration](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).
 
Integration von **Dapper** und **DapperExtensions** elastische Datenbank-Clientbibliothek für Azure SQL-Datenbank ist einfach. Daten von Arbeitsplan erstellen ändern und neue [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) -Objekte können Ihrer Anwendung [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) Aufruf von der [Clientbibliothek](http://msdn.microsoft.com/library/azure/dn765902.aspx)verwenden. Dies schränkt Änderungen in der Anwendung nur neue Verbindungen erstellt und geöffnet werden. 

## <a name="dapper-overview"></a>Dapper (Übersicht)
**Dapper** ist eine objektrelationale Mapper. Es ordnet .NET Objekte aus der Anwendung in einer relationalen Datenbank (und umgekehrt). Der erste Teil der Beispielcode ist die Integration der Clientbibliothek elastische Datenbank mit Dapper-basierte dargestellt. Der zweite Teil der Beispielcode veranschaulicht wie Dapper und DapperExtensions integriert.  

Die Mapper in Dapper Funktionalität Erweiterungsmethoden für Datenbankverbindungen, die vereinfachen senden T-SQL-Anweisung ausführen oder die Datenbank abzufragen. Beispielsweise erleichtert Dapper Zuordnung zwischen .NET Objekte und die Parameter der SQL-Anweisung **Execute** -Aufrufe oder die Ergebnisse von SQL-Abfragen in .NET Objekte **Abfrage** Aufrufe von Dapper verwendet. 

Bei DapperExtensions müssen Sie nicht die SQL-Anweisung angeben. Erweiterungsmethoden oder **GetList** **Legen Sie** über die Verbindung erstellen der SQL-Anweisung im Hintergrund
 
Ein weiterer Vorteil von Dapper und DapperExtensions ist, dass die Anwendung die Datenbank Verbindung steuert. Dadurch wird die Clientbibliothek elastische Datenbank interagieren die Broker-Basis für die Zuordnung von Shardlets in Datenbanken Datenbank.

Gepflegten Assemblys finden Sie unter [gepflegten Dot net](http://www.nuget.org/packages/Dapper/). Gepflegten Extensions finden Sie unter [DapperExtensions](http://www.nuget.org/packages/DapperExtensions).

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Ein Blick auf die Clientbibliothek elastische Datenbank

Clientbibliothek elastische Datenbank definieren Partitionen Ihrer Anwendungsdaten *Shardlets* aufgerufen, Datenbanken zuordnen und *Sharding*Schlüssel identifizieren. Sie können beliebig viele Datenbanken und die Shardlets auf diese Datenbanken verteilen. Die Zuordnung von Sharding Werte zu den Datenbanken wird durch ein Schema Splitter von der Bibliothek APIs gespeichert. Dies heißt **Splitter Management zuordnen**. Die shardzuordnung dient auch als Broker Datenbankverbindungen für Anfragen, die eine shardingschlüssel enthalten. Diese Funktion wird als **datenabhängiges routing**bezeichnet.

![Splitter-Karten und datenabhängiges routing][1]

Der shardzuordnungs-Manager schützt Benutzer inkonsistente Ansichten Shardlet Daten, die auftreten können, wenn gleichzeitig Shardlet Verwaltungsoperationen auf Datenbanken passiert. Hierzu broker Splitter Karten Datenbankverbindungen für eine Anwendung mit der Bibliothek. Wenn Splitter Verwaltungsvorgänge der Shardlet auswirken, kann Splitter Sitemap-Funktionalität automatisch eine Verbindung zu beenden. 

Anstatt traditionell Verbindungen für Dapper erstellen, müssen die [OpenConnectionForKey-Methode](http://msdn.microsoft.com/library/azure/dn824099.aspx)verwenden. Dadurch erfolgt die Überprüfung und ordnungsgemäß Verbindungen verwaltet werden, wenn Daten zwischen Splitter bewegt.

### <a name="requirements-for-dapper-integration"></a>Vorschriften für gepflegten integration

Beim Arbeiten mit der Clientbibliothek elastische Datenbank und eleganten APIs möchten wir die folgenden Eigenschaften:

* **Scaleout**: Hinzufügen oder Entfernen von Datenbanken von der Datenebene Sharding Anwendung ggf. der Kapazitätsbedarf der Anwendung soll. 

-    **Konsistenz**: Da unsere Anwendung mit Sharding skaliert ist, müssen wir datenabhängiges routing auszuführen. Wir möchten die abhängigen Daten-routing-Funktionen der Bibliothek verwenden. Wir wollen die Validierung insbesondere garantiert Konsistenz von Verbindungen bereitgestellt, die über den shardzuordnungs-Manager vermittelt werden, um Beschädigung oder falsche Ergebnisse zu vermeiden. Dadurch Zugriff auf einen bestimmten Shardlet abgelehnt oder beendet wird (zum Beispiel) der Shardlet derzeit in einem anderen Splitter mit Split-Merge-APIs verschoben.

-    **Objekt zuordnen**: bequem Zuordnung gemäß Dapper übersetzen zwischen Klassen in der Anwendung und den zugrunde liegenden Datenbankstrukturen beibehalten werden soll. 

Der folgende Abschnitt enthält Hinweise zur Anforderungen basierend auf **Dapper** und **DapperExtensions**.

## <a name="technical-guidance"></a>Technische Anleitung
### <a name="data-dependent-routing-with-dapper"></a>Daten abhängige mit Dapper 

Mit Dapper ist die Anwendung in der Regel zum Erstellen und öffnen die Verbindung mit der zugrunde liegenden Datenbank verantwortlich. Bei einem vorgegebenen Typ T die Anwendung Dapper Gibt Abfrageergebnisse zurück wie .NET Sammlungen von Typ t gepflegten führt die Zuordnung von Ergebniszeilen T-SQL-Objekte vom Typ T. Ebenso wird Dapper .NET Objekte in SQL-Werte oder Parameter für DML (Datenbearbeitungssprache)-Anweisungen zugeordnet. Dapper Funktionalität diese über Erweiterungsmethoden für reguläre [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) -Objekt von ADO .NET SQL Client-Bibliotheken. Elastische Skalierung APIs für DDR zurückgegebenen SQL-Verbindung sind ebenfalls regelmäßig [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) -Objekte. Dadurch gepflegten Extensions direkt über von der Clientbibliothek DDR API zurückgegebenen Typ verwenden, wie eine einfache SQL-Clientverbindung.

Diese Stellung nehmen einfach von der Clientbibliothek elastische Datenbank für Dapper vermittelte Verbindungen verwenden.

Dieses Codebeispiel (aus dem zugehörigen Beispiel) zeigt den Ansatz, die shardingschlüssel der Anwendung der Bibliothek sofern, die Verbindung zur richtigen Splitter broker.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

Der Aufruf [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API ersetzt Standard erstellen und Öffnen einer SQL-Clientverbindung. [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) -Aufruf werden die erforderlichen Argumente für datenabhängiges routing: 

-    Shardzuordnung auf die Daten von Routingschnittstellen
-    Shardingschlüssel der Shardlet identifizieren
-    Die Anmeldeinformationen (Benutzername und Kennwort) zum Herstellen des Splitter

Das Zuordnungsobjekt Splitter erstellt eine Verbindung zu der Splitter, der Shardlet für die angegebene shardingschlüssel enthält. Elastische Datenbankclient-APIs auch tag die Verbindung die Konsistenzgarantien implementieren. Der Aufruf [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) reguläre SQL Client-Verbindungsobjekt zurück, folgt der nachfolgende Aufruf der Erweiterungsmethode **Ausführen** von Dapper gepflegten üblich.

Abfragen funktionieren sehr ähnlich – zuerst öffnen Sie die Verbindung mit [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) aus der Client-API. Verwenden Sie reguläre gepflegten Erweiterungsmethoden, die Ergebnisse der SQL-Abfrage in .NET Objekte zuzuordnen:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Hinweis **mit** der DDR-Verbindung blockieren legt alle Datenbankoperationen innerhalb des Blocks ein Splitter, in denen tenantId1 gehalten wird. Die Abfrage gibt nur Blogs auf aktuelle Splitter gespeichert, aber nicht die anderen Splitter gespeichert. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Mit Dapper und DapperExtensions von Daten

Dapper enthält Ökosystem Erweiterungen, die weitere benutzerfreundliche und Abstraktion aus der Datenbank können beim Entwickeln von Datenbanken. DapperExtensions ist ein Beispiel. 

Verwenden von DapperExtensions in der Anwendung nicht geändert wie Datenbankverbindungen erstellt und verwaltet werden. Es obliegt immer noch die Anwendung Verbindungen und reguläre SQL Client Verbindungsobjekte werden durch die Erweiterungsmethoden erwartet. Wir können auf [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) verlassen, wie oben beschrieben. Die folgenden Codebeispiele zeigen, ist lediglich, dass wir nicht mehr die T-SQL-Anweisung schreiben:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

Und hier ist das Codebeispiel für die Abfrage: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Vorübergehende Fehler behandeln

Microsoft Patterns & Practices-Team veröffentlicht die [Vorübergehende Fehler Handling Application Block](http://msdn.microsoft.com/library/hh680934.aspx) Anwendung Entwickler häufig vorübergehende Fehlerzustände, wenn in der Cloud ausgeführt zu verringern. Weitere Informationen finden Sie unter [Ausdauer, geheimen alle Erfolge: die vorübergehende Fehler Handling Application Block mit](http://msdn.microsoft.com/library/dn440719.aspx).

Das Codebeispiel verwendet vorübergehender Fehler Bibliothek vorübergehende Fehler schützen. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** im obigen Code ist definiert als eine **SqlDatabaseTransientErrorDetectionStrategy** mit dem Wiederholungsanzahl 10 und 5 Sekunden Wartezeit zwischen den Wiederholungsversuchen. Bei Verwendung von Transaktionen stellen Sie sicher, dass Ihr Wiederholungsbereich an die Transaktion bei einem vorübergehenden Fehler zurück.

## <a name="limitations"></a>Grenzen

In diesem Dokument beschriebenen Vorgehensweisen umfassen ein paar Nachteile:

* Der Beispielcode für dieses Dokument ist keine Splitter Schema verwalten veranschaulicht.
* Eine Anforderung angegeben, wird angenommen, dass die datenbankverarbeitung in einen Splitter enthalten ist, von der Anforderung bereitgestellten shardingschlüssel. Jedoch ist diese Annahme nicht, z. B. halten Wenn es nicht möglich, ein shardingschlüssel zur Verfügung. Dieses Problem umgehen, enthält die Clientbibliothek elastische Datenbank die [MultiShardQuery Klasse](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). Die Klasse implementiert eine Verbindung Abstraktion für Abfragen über mehrere Splitter. Mit MultiShardQuery in Kombination mit Dapper ist nicht Gegenstand dieses Dokuments.

## <a name="conclusion"></a>Abschluss

Dapper mit DapperExtensions Anwendung profitieren leicht elastische Datenbanktools für Azure SQL-Datenbank. Schritte in diesem Dokument beschriebenen können Anwendungsbereiche das Tool Funktion Daten von Arbeitsplan erstellen ändern und neue [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) -Objekte [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) Aufruf der Clientbibliothek elastische Datenbank verwenden. Dies beschränkt die Anwendung nimmt zu den Orten, wo neue Verbindungen erstellt und geöffnet werden. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
 