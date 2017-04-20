<properties 
    pageTitle="Mit Entity Framework elastische Datenbank-Clientbibliothek | Microsoft Azure" 
    description="Verwenden Sie elastische Datenbank-Clientbibliothek und Entity Framework zum Codieren von Datenbanken" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="elastic-database-client-library-with-entity-framework"></a>Elastische Datenbank-Clientbibliothek mit Entity Framework 
 
Dieses Dokument zeigt eine Entity Framework-Anwendung, die [elastische Datenbanktools](sql-database-elastic-scale-introduction.md)integrieren. Der Schwerpunkt liegt auf [Splitter Management zugeordnet](sql-database-elastic-scale-shard-map-management.md) und mit dem Entity Framework **Code First** [datenabhängiges routing](sql-database-elastic-scale-data-dependent-routing.md) verfassen. [Code First-neue Datenbank](http://msdn.microsoft.com/data/jj193542.aspx) Lernprogramm für EF dient als Beispiel in diesem Dokument. Im Beispielcode zu diesem Dokument gehört elastische Datenbanktools der Beispiele in den Visual Studio.
  
## <a name="downloading-and-running-the-sample-code"></a>Herunterladen und Ausführen des Beispielcodes
Den Code für diesen Artikel herunterladen:

* Visual Studio 2012 oder höher ist erforderlich. 
* Starten Sie Visual Studio. 
* Visual Studio-wählen Sie Datei > Neues Projekt. 
* Navigieren Sie im Dialogfeld "Neues Projekt" **Online-Beispiele** für **Visual C#** und Typ "elastische Db" in das Suchfeld oben rechts.
    
    ![Entity Framework und elastische Datenbank Beispiel-app][1] 

    Stichprobe **Elastische DB Tools für SQL Azure-Entity Framework-Integration**bezeichnet. Akzeptieren Sie die Lizenz, lädt das Beispiel. 

Um das Beispiel auszuführen, müssen Sie drei leere Datenbanken in Azure SQL-Datenbank zu erstellen:

* Splitter Karte Manager-Datenbank
* Splitter 1 Datenbank
* Splitter 2-Datenbank

Füllen Sie die Platzhalter in **Program.cs** der Datenbankname Azure SQL Server den Datenbanknamen und die Anmeldeinformationen für die Verbindung zu Datenbanken, erstellte Datenbanken. Erstellen Sie die Projektmappe in Visual Studio. Visual Studio lädt die erforderlichen NuGet-Pakete für die Clientbibliothek elastische Datenbank Entity Framework und vorübergehender Fehler als Teil des Buildprozesses herunter. Stellen Sie sicher, dass wiederherstellen NuGet-Pakete für die Projektmappe aktiviert ist. Sie können diese Einstellung mit der rechten Maustaste auf die Projektmappendatei im Projektmappen-Explorer von Visual Studio. 

## <a name="entity-framework-workflows"></a>Entity Framework workflows 

Entity Framework-Entwickler beruhen auf einer der folgenden vier Arbeitsabläufe Anwendung und Dauerhaftigkeit für Anwendungsobjekte sicherzustellen: 

* **Code First (neue Datenbank)**: der EF Entwickler erstellt das Modell im Anwendungscode und EF generiert der Datenbank aus. 
* **Code First (bestehende Datenbank)**: Entwickler kann EF Anwendungscode für das Modell aus einer vorhandenen Datenbank generieren.
* **Erste Modell**: Entwickler erstellt das Modell in der EF-Designers und EF Datenbank aus dem Modell.
* **Erste Datenbank**: der Entwickler verwendet, um das Modell aus einer vorhandenen Datenbank ableiten EF. 

Diese Methoden basieren auf Klasse DbContext Datenbankverbindungen und Datenbankschema für eine Anwendung transparent zu verwalten. Bootstrapping und Schema Erstellen der Datenbank wie in das Dokument später ausführlicher erläutert werden verschiedene Konstruktoren der DbContext-Basisklasse für verschiedene Ebenen der Verbindung Erstellung steuern können. Probleme entstehen in erster Linie aus der Datenbank verbindungsmanagement von EF bereitgestellte Verbindung Verwaltungsfunktionen von Daten von routing bereitgestellten Schnittstellen schneidet von der Clientbibliothek elastische Datenbank. 

## <a name="elastic-database-tools-assumptions"></a>Elastische Datenbanktools Annahmen 

Begriffsdefinitionen finden Sie unter [elastische Datenbank Tools Glossar](sql-database-elastic-scale-glossary.md).

Elastische Datenbank-Clientbibliothek definieren Sie Partitionen Ihrer Anwendungsdaten Shardlets aufgerufen. Shardlets durch ein shardingschlüssel gekennzeichnet und Datenbanken zugeordnet sind. Eine Anwendung kann so viele Datenbanken und verteilen Shardlets genügend Kapazität oder aktuellen Geschäftserfordernissen die Performance. Die Zuordnung von Sharding Werte zu den Datenbanken wird durch ein Schema Splitter von elastische Datenbankclient-APIs gespeichert. Wir nennen dies **Splitter Karte**oder SMM kurz. Die shardzuordnung dient auch als Broker Datenbankverbindungen für Anfragen, die eine shardingschlüssel enthalten. Wir bezeichnen dies als **datenabhängiges routing**. 
 
Der shardzuordnungs-Manager schützt Benutzer inkonsistente Ansichten Shardlet Daten, die auftreten können, wenn gleichzeitige Shardlet Management-Vorgänge (z. B. das Verschieben von Daten von einem Splitter zu einem anderen) geschehen. Hierzu verwalteten Splitter Karten von Client Library Broker Datenbankverbindungen für eine Anwendung. Dadurch Splitter Sitemap-Funktionalität zu automatisch eine Verbindung bei Splitter Verwaltungsvorgänge der Shardlet auswirken, die für die Verbindung erstellt wurde. Dieser Ansatz muss einige EF-Funktionen wie das Erstellen neuer Verbindungen aus einer vorhandenen Datenbank Vorhandensein überprüft integrieren. Im Allgemeinen wurde unserer arbeiten standard DbContext-Konstruktoren nur arbeiten zuverlässig geschlossenen Datenbankverbindungen für EF sicher geklont werden können. Das Entwurfsmodell elastische Datenbank ist nur geöffnete Verbindungen broker. Man könnte meinen, dass eine Verbindung von der Clientbibliothek vor Übergabe EF DbContext vermittelt dieses Problem möglicherweise behoben. Aber durch die Verbindung geschlossen und auf EF wieder geöffnet, verzichtet eine Überprüfung und Konsistenz Kontrollen der Bibliothek. Migrationen Funktionalität in EF verwendet dabei jedoch das zugrunde liegende Datenbankschema auf eine Weise verwalten, die für die Anwendung transparent. Idealerweise möchten wir behalten und kombiniert alle diese Funktionen aus der Clientbibliothek elastische Datenbank und EF in derselben Anwendung. Im folgenden Abschnitt werden diese Eigenschaften und Anforderungen ausführlicher. 


## <a name="requirements"></a>Vorschriften 

Beim Arbeiten mit der Clientbibliothek elastische Datenbank und Entity Framework-APIs möchten wir die folgenden Eigenschaften: 

* **Skalierung**: Hinzufügen oder Entfernen von Datenbanken von der Datenebene Sharding Anwendung ggf. der Kapazitätsbedarf der Anwendung. Das bedeutet Kontrolle über das Erstellen und Löschen von Datenbanken und verwenden den elastische Datenbank Splitter-Manager-APIs zum Verwalten von Datenbanken und Zuordnung von Shardlets zugeordnet. 

* **Konsistenz**: die Anwendung Sharding beschäftigt, und die abhängigen Daten-routing-Funktionen der Clientbibliothek. Um Beschädigung oder falsche Ergebnisse zu vermeiden, werden Verbindungen über den shardzuordnungs-Manager vermittelt. Außerdem behält Validierung und Konsistenz.
 
* **Code First**: bequem von EF Code erste Paradigma beibehalten. Im ersten Codebeispiel werden Klassen in der Anwendung transparent zugrunde liegenden Datenbankstrukturen zugeordnet. Der Anwendungscode interagiert mit DbSets, die meisten Aspekte der zugrunde liegenden datenbankverarbeitung zu maskieren.
 
* **Schema**: Entity Framework Ausgangsdatenbank erstellen und nachfolgende Schema Entwicklung durch Migration behandelt. Diese Funktionen beibehalten, ist zur Anpassung Ihrer Anwendung einfach Daten entwickelt. 

Die folgende Anleitung weist wie diese Anforderungen für Code First Applikationen elastische Datenbanktools verwenden. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Daten von routing mit DbContext EF 

Datenbankverbindungen mit Entity Framework werden normalerweise durch Unterklassen der **DbContext**verwaltet. Erstellen Sie diese Unterklassen **DbContext**ableiten. Dies ist, definieren Sie die **DbSets** , die die Datenbank gesichert Sammlungen von CLR-Objekte für Ihre Anwendung implementieren. Im Kontext des datenabhängiges routing können wir mehrere nützliche Eigenschaften identifizieren, die nicht unbedingt für andere EF Code erste Anwendungsszenarios halten: 

* Die Datenbank ist bereits vorhanden und wurde in shardzuordnung elastische Datenbank registriert. 
* Das Schema der Anwendung wurde bereits mit der Datenbank (siehe unten) bereitgestellt. 
* Datenabhängiges routing Verbindungen mit der Datenbank werden durch die shardzuordnung vermittelt. 

**Ef4** Integration mit datenabhängiges routing für dezentrales Skalieren:

1. Erstellen Sie physische Datenbankverbindungen durch die elastische Client-Schnittstellen der shardzuordnungs-manager 
2. Umbrechen Sie die Verbindung mit der **DbContext** -Unterklasse
3. Weitergeben der Verbindungs in die **DbContext** Basisklassen um sicherzustellen, dass die Verarbeitung auf EF sowie geschieht. 

Im folgenden Codebeispiel veranschaulicht diesen Ansatz. (Dieser Code ist auch das zugehörige Visual Studio-Projekt)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed to the proper shard by the shard map manager. 
        // Note that the base class c'tor call will fail for an open connection
        // if migrations need to be done and SQL credentials are used. This is the reason for the 
        // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map to broker a validated connection for the given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Hauptpunkte
* Ein neuer Konstruktor ersetzt den Standardkonstruktor DbContext-Unterklasse 
* Der neue Konstruktor akzeptiert die erforderlichen Argumente für datenabhängiges routing über Clientbibliothek elastische Datenbank:
    * die shardzuordnung auf dem datenabhängigen routing-Schnittstellen
    * die shardingschlüssel der Shardlet identifizieren
    * eine Verbindungszeichenfolge mit den Anmeldeinformationen für datenabhängiges routing Verbindung mit dem Splitter. 
 
* Der Aufruf an den Basisklassenkonstruktor berücksichtigt Gebiet eine statische Methode, die alle Schritte führt für datenabhängiges routing. 
   * Aufruf OpenConnectionForKey elastische Datenbank Client-Schnittstellen verwendet auf der Karte Splitter geöffneten Verbindung.
   * Splitter-Karte erstellt offene Verbindung mit Splitter, der Shardlet für die angegebene shardingschlüssel enthält.
   * Diese offene Verbindung wird an den Basisklassenkonstruktor DbContext gibt an, dass diese Verbindung von EF verwendet werden, anstatt automatisch eine neue Verbindung erstellen EF übergeben. Auf diese Weise die Verbindung wurde durch die elastische Datenbankclient-API so gewährleisten sie Konsistenz unter Splitter Karte Verwaltungsvorgänge markiert.
 
  
Verwenden Sie den new-Konstruktor für die Unterklasse DbContext anstelle der standardmäßigen Konstruktor im Code. Hier ist ein Beispiel: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

Der new-Konstruktor öffnet die Verbindung zu den Splitter, der mit den Daten für Shardlet den Wert des **tenantid1**identifiziert. -Block **verwenden** bleibt unverändert auf **DbSet** für Blogs mit EF auf den Splitter für **tenantid1**. Semantik für den Code in der dadurch blockiert, dass alle Datenbankoperationen jetzt auf einen Splitter beschränkt werden, in denen **tenantid1** gehalten. Beispielsweise würde eine LINQ-Abfrage über Blogs **DbSet** nur Blogs auf aktuelle Splitter gespeichert, aber nicht die anderen Splitter gespeichert zurück.  

#### <a name="transient-faults-handling"></a>Vorübergehende Fehler behandeln
Microsoft Patterns & Practices-Team veröffentlicht [Die vorübergehende Fehler Handling Application Block](https://msdn.microsoft.com/library/dn440719.aspx). Die Bibliothek wird Clientbibliothek elastische Skalierung in Kombination mit EF verwendet. Allerdings sicherstellen Sie, dass vorübergehende Ausnahme zu stellen gibt, in denen wir sicherstellen, dass neue Konstruktor nach vorübergehender Fehler verwendet wird, damit alle neuen Verbindungsversuch mit Konstruktoren, die wir optimiert. Andernfalls eine Verbindung mit dem richtigen Splitter nicht garantiert und gibt keine Garantien, die Änderungen Splitter-Karte die Verbindung aufrechterhalten wird. 

Das folgende Codebeispiel veranschaulicht die Verwendung von SQL wiederholen Politik um neuen **DbContext** Unterklasse Konstruktoren: 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** im obigen Code ist definiert als eine **SqlDatabaseTransientErrorDetectionStrategy** mit dem Wiederholungsanzahl 10 und 5 Sekunden Wartezeit zwischen den Wiederholungsversuchen. Dieser Ansatz ähnelt der Anleitung für EF und Benutzer initiierte Transaktionen (siehe [mit Ausführungsstrategien wiederholen (ab EF6)](http://msdn.microsoft.com/data/dn307226). Beide Situationen erfordern, dass die Anwendung den Bereich steuert, vorübergehende Ausnahme gibt: die Transaktion öffnen oder neu erstellen (siehe) vom Kontext der richtige Konstruktor verwendet elastische Datenbank-Clientbibliothek.

Die Notwendigkeit, wohin vorübergehende Ausnahmen im Bereich führen schließt auch die Verwendung von mit EF integrierten **SqlAzureExecutionStrategy** . **SqlAzureExecutionStrategy** eine Verbindung öffnen, aber nicht **OpenConnectionForKey** verwenden und daher umgehen die Validierung, die Bestandteil der **OpenConnectionForKey** -Aufruf ausgeführt wird. Stattdessen verwendet das Codebeispiel mit EF integrierte **DefaultExecutionStrategy** . Im Gegensatz zu **SqlAzureExecutionStrategy**funktioniert es in Kombination mit der Wiederholung von vorübergehenden Fehler. Die Ausführungsrichtlinie wird in der **ElasticScaleDbConfiguration** -Klasse festgelegt. Beachten Sie, dass wir nicht **DefaultSqlExecutionStrategy** verwenden, da es vorschlägt, vorübergehende Ausnahmen auftreten, die zu falsch Verhalten wie - **SqlAzureExecutionStrategy** verwenden. Weitere Informationen zu anderen wiederholungsrichtlinien und EF finden Sie unter [Verbindung Stabilität EF](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Konstruktor schreibt
Codebeispiele veranschaulichen die standardmäßige Konstruktor schreibt für Ihre Anwendung erforderlich, Daten von routing mit Entity Framework. In der folgenden Tabelle verallgemeinert dadurch andere Konstruktoren. 


Aktuelle Konstruktor  | Neu Konstruktor für Daten | Basiskonstruktor | Notizen
---------- | ----------- | ------------|----------
MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection Bool) |Die Verbindung muss eine Funktion der shardzuordnung und datenabhängiges routing Schlüssel. Sie müssen Bypass automatische Verbindung erstellen von EF und Splitter-Karte, broker-Verbindung verwenden. 
MyContext(string)|ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection Bool) |Die Verbindung ist eine Funktion der shardzuordnung und datenabhängiges routing Schlüssel. Einen festen Namen oder Verbindungszeichenfolge funktioniert nicht wie Bypass-Validierung der Splitter-Karte. 
MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, Bool) |Die Verbindung erhalten für die Karte und Sharding angegebenen Splitter mit dem Muster erstellt. Das kompilierte Modell werden auf der Basis c'tor weitergegeben.
MyContext (DbConnection Bool) |ElasticScaleContext (ShardMap, TKey, Bool) |DbContext (DbConnection Bool) |Die Verbindung muss aus der shardzuordnung und der Schlüssel abgeleitet werden. Es kann als Eingabe bereitgestellt werden (sofern die Eingabe der shardzuordnung und der Schlüssel bereits verwendet wurde). Der boolesche Wert wird übergeben. 
MyContext (String, DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, Bool) |Die Verbindung muss aus der shardzuordnung und der Schlüssel abgeleitet werden. Es kann als Eingabe bereitgestellt werden (es sei denn, die Eingabe der shardzuordnung und den Schlüssel wurde). Das kompilierte Modell werden weitergegeben. 
MyContext (ObjectContext Bool) |ElasticScaleContext (ShardMap, TKey ObjectContext Bool) |DbContext (ObjectContext Bool) |Des neuen Konstruktors muss sicherstellen, dass alle ObjectContext als Eingabe zum verwaltet durch flexible Verbindung ist. Eine ausführliche Erläuterung der ObjectContext ist nicht Gegenstand dieses Dokuments.
MyContext (DbConnection, DbCompiledModel, Bool) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel, Bool)| DbContext (DbConnection, DbCompiledModel, Bool); |Die Verbindung muss aus der shardzuordnung und der Schlüssel abgeleitet werden. Die Verbindung kann als Eingabe bereitgestellt werden (sofern die Eingabe der shardzuordnung und der Schlüssel bereits verwendet wurde). Modell und Boolean sind an den Basisklassenkonstruktor übergeben. 

## <a name="shard-schema-deployment-through-ef-migrations"></a>Splitter Schema Bereitstellung über EF-Migrationen 

Automatische Schema Management ist eine Vereinfachung von Entity Framework bereitgestellt. In Zusammenhang mit elastische Datenbanktools Applikationen möchten wir behalten diese Funktion Schema neu erstellten Splitter automatisch bereitstellen, wenn Sharding Anwendung Datenbanken hinzugefügt werden. Der primäre Anwendungsfall ist Speicherkapazität auf der Datenebene für Sharding mit EF erhöhen. Auf EF-Funktionen für Schema-Management verringert den Datenbank-Verwaltung mit einer Sharding Anwendung auf EF. 

Schema-Bereitstellung über EF Migrationen funktioniert am besten bei **geschlossene Verbindung**. Im Gegensatz zum Szenario für Daten von ist routing, die auf die geöffnete Verbindung elastische Datenbankclient-API bereitgestellt. Ein weiterer Unterschied ist Konsistenz erforderlich: während wünschenswert, Konsistenz für alle datenabhängigen Routingverbindungen zum gleichzeitigen Splitter Karte Manipulation Schutz ist nicht mit dem ursprünglichen Schema-Bereitstellung in einer neuen Datenbank, die noch nicht in der shardzuordnung eingetragen und nicht zu Shardlets zugeordnet. Wir können daher auf reguläre Datenbankverbindungen für diese Szenarien datenabhängiges routing verlassen.  

Dadurch einen Ansatz, ist Schema Bereitstellung über EF Migrationen eng an die Registrierung der neuen Datenbank als Splitter in der Anwendung shardzuordnung gekoppelt. Dies beruht auf folgenden Komponenten: 

* Die Datenbank wurde bereits erstellt. 
* Die Datenbank ist leer – Es enthält keine Benutzerschema und keine Benutzerdaten.
* Die Datenbank kann noch durch elastische Datenbankclient-APIs für datenabhängiges routing zugegriffen werden. 

Mit diesen Komponenten an schaffen wir eine reguläre ungeöffnet **SqlConnection** EF Migrationen Schema Bereitstellung starten. Das folgende Codebeispiel veranschaulicht diesen Ansatz. 

        // Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
        // and kick off EF intialization of the database to deploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query to engage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register the mapping of the tenant to the shard in the shard map. 
            // After this step, data-dependent routing on the shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 
 

Dieses Beispiel zeigt die Methode **RegisterNewShard** , die registriert des Splitter in der shardzuordnung, setzt das Schema über EF Migrationen und speichert eine Zuordnung von shardingschlüssel shard. Es basiert auf einem Konstruktor der **DbContext** -Unterklasse (**ElasticScaleContext** im Beispiel), die eine SQL-Verbindungszeichenfolge als Eingabe akzeptiert. Der Code dieses Konstruktors ist einfach, wie im folgenden Beispiel gezeigt: 


        // C'tor to deploy schema and migrations to a new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that the schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 
 
Möglicherweise verwendet eine Version der Konstruktor der Basisklasse geerbt. Jedoch muss der Code sicher, dass der Standard für EF beim Verbinden-Initialisierung. Damit der kurzen Umweg statische Methode vor dem Aufrufen des Basisklassenkonstruktors mit der Verbindungszeichenfolge. Beachten Sie, dass die Registrierung der Splitter sollte in einer anderen Anwendungsdomäne oder um sicherzustellen, dass die Initialisierung Einstellungen für EF nicht in Konflikt stehen. 


## <a name="limitations"></a>Grenzen 

In diesem Dokument beschriebenen Vorgehensweisen umfassen ein paar Nachteile: 

* EF-Applikationen, mit denen **LocalDb** zunächst in eine reguläre SQL Server-Datenbank migrieren, bevor elastische Datenbank Clientbibliothek. Scaling-out einer Anwendung durch Sharding mit elastische ist nicht möglich mit **LocalDb**. Beachten Sie, dass Entwicklung noch **LocalDb**. 

* Änderungen an die Anwendung, die Datenbank geändert implizieren müssen EF Migrationen auf alle Splitter durchlaufen. Der Beispielcode für dieses Dokument ist nicht dazu veranschaulicht. Sollten Sie Datenbank aktualisieren mit ConnectionString-Parameter durchlaufen alle Splitter; T-SQL-Skript für die anstehende Migration Update-Datenbank extrahieren – Skript option und das T-SQL-Skript für die Splitter.  

* Wenn eine Anforderung, davon aus, dass alle Datenbank verarbeitet in einen Splitter enthalten ist, von der Anforderung bereitgestellten shardingschlüssel. Allerdings ist diese Annahme nicht immer gelten. Z. B. wenn es nicht möglich ein shardingschlüssel verfügbar ist. Um dieses Problem anzugehen, bietet die Clientbibliothek **MultiShardQuery** -Klasse, die eine Verbindung Abstraktion für Abfragen über mehrere Splitter implementiert. Verwenden Sie **MultiShardQuery** mit EF ist nicht Gegenstand dieses Dokuments

## <a name="conclusion"></a>Abschluss

In diesem Dokument beschriebenen Schritte können EF Applikationen elastische Datenbank-Clientbibliothek Funktion datenabhängiges routing durch Umgestalten Konstruktoren **DbContext** Unterklassen in der EF-Anwendung verwendet. Dies beschränkt die Änderungen zu den Orten, wo **DbContext** Klassen vorhanden. Darüber hinaus können EF Applikationen weiterhin nutzen Automatische Schema Bereitstellung kombiniert die Schritte, die notwendigen EF Migrationen mit der Registrierung des neuen Splitter und Mappings in der shardzuordnung aufrufen. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
 