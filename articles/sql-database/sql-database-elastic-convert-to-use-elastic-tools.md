<properties
   pageTitle="Migrieren bestehende Datenbanken nach Skalierung | Microsoft Azure"
   description="Konvertieren von Sharding Datenbanken um elastische Datenbanktools erstellen einen Schema-Manager verwenden"
   services="sql-database"
   documentationCenter=""
   authors="ddove"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/24/2016"
   ms.author="ddove"/>

# <a name="migrate-existing-databases-to-scale-out"></a>Migrieren Sie bestehende Datenbanken nach Dezentrales Skalieren

Verwalten Sie Ihre skalierte Sharding Datenbanken mit Azure SQL-Datenbank Database Tools (wie die [Clientbibliothek elastische Datenbank](sql-database-elastic-database-client-library.md)). Sie müssen zunächst einen vorhandenen Satz von Datenbanken mit dem [Splitter Landkarten-Manager](sql-database-elastic-scale-shard-map-management.md)konvertieren. 

## <a name="overview"></a>Übersicht
So migrieren Sie vorhandene Sharding Datenbank 

1. Vorbereiten der [Splitter-Manager-Datenbank](sql-database-elastic-scale-shard-map-management.md).
2. Erstellen Sie Splitter-Zuordnung.
3. Vorbereiten der einzelnen Splitter.  
2. Splitter-Zuordnung hinzufügen.

Diese Verfahren können mit [.NET Framework Client Library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)oder am [Azure SQL DB - Tools elastische Datenbankskripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)PowerShell-Skripts implementiert werden. Die Beispiele verwenden die PowerShell-Skripts.

Weitere Informationen zu den ShardMapManager finden Sie unter [Splitter Management zuordnen](sql-database-elastic-scale-shard-map-management.md). Eine Übersicht über die elastische Datenbanktools Übersicht [elastische Datenbank Funktionen](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>Vorbereiten der Splitter Map-Manager-Datenbank

Der shardzuordnungs-Manager ist eine spezielle Datenbank, die skalierte Datenbanken verwalten Daten enthält. Sie können eine vorhandene Datenbank verwenden oder eine neue Datenbank erstellen. Beachten Sie, dass eine Datenbank als shardzuordnungs-Manager einen derselben Datenbank nicht. Beachten Sie, dass PowerShell-Skript nicht die Datenbank für Sie erstellen. 

## <a name="step-1-create-a-shard-map-manager"></a>Schritt 1: Erstellen Sie einen Map-manager

    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>Abrufen den shardzuordnungs-manager

Nach dem erstellen können Sie den shardzuordnungs-Manager mit diesem Cmdlet abrufen. Dieser Schritt ist erforderlich, jedes Mal Sie das ShardMapManager-Objekt verwenden müssen.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 

  
## <a name="step-2-create-the-shard-map"></a>Schritt 2: Erstellen der shardzuordnung

Wählen Sie den Typ der shardzuordnung erstellen. Die Auswahl hängt von der Architektur: 

1. Einzelnen Mandanten pro Datenbank (Begriffe Siehe [Glossar](sql-database-elastic-scale-glossary.md).) 
2. Mehrere Mandanten pro Datenbank (zwei Typen):
    3. Liste Zuordnung
    4. Bereich zuordnen
 

Erstellen Sie für ein Modell einzelne Mieter eine **Liste Zuordnung** Splitter Zuordnung. Single-Tenant-Modells weist eine Datenbank pro Mandant. Dies entspricht effektives Modell für Entwickler SaaS Management vereinfacht.

![Liste Zuordnung][1]

Multi-Tenant-Modells eine einzelne Datenbank mehrere Mandanten zugewiesen und Gruppen der Mandanten auf mehrere Datenbanken verteilen. Verwenden Sie dieses Modell jeden Mandanten zu kleinen Daten erwartet. In diesem Modell einer Datenbank **Bereich**an Mieter zugewiesen. 
 

![Bereich zuordnen][2]

Oder Sie Multi-Tenant-Datenbankmodell mit einer *Liste Zuordnung* einer einzelnen Datenbank mehrere Mandanten zuordnen. Beispielsweise DB1 wird zum Speichern von Informationen über den Mandanten-Id 1 und 5 und DB2 Daten für Mieter 7 und 10 Mieter gespeichert. 

![Verschachteln Mieter einzelne DB][3] 

**Basierend auf Ihrer Auswahl, wählen Sie eine der folgenden Optionen:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Option 1: Erstellen einer shardzuordnung Liste Zuordnung
Erstellen einer shardzuordnung mit ShardMapManager-Objekt. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 
 
 
### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Option 2: Erstellen einer Zuordnung Splitter zwischen Bereich

Beachten Sie, dass diese Zuordnung Muster, Mandanten-Id nutzen Werte fortlaufende Bereiche werden und wird in den Bereichen haben Bereich einfach überspringen, wenn Sie Datenbanken erstellen.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Option 3: Liste Zuordnung in einer Datenbank
Dieses Muster einrichten muss auch Erstellung einer Liste Zuordnung wie in Schritt 2 die Option 1.

## <a name="step-3-prepare-individual-shards"></a>Schritt 3: Bereiten Sie einzelne Splitter vor

Der shardzuordnungs-Manager jeden Splitter (Datenbank) hinzugefügt. Zum Speichern von Informationen über die Zuordnung vorbereitet die einzelnen Datenbanken. Führen Sie diese Methode auf jeden Splitter.
     
    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.
 

## <a name="step-4-add-mappings"></a>Schritt 4: Hinzufügen

Das Hinzufügen der Zuordnung hängt von der Art Splitter-Karte, die Sie erstellt haben. Erstellt eine Liste-Karte können Sie Liste Zuordnung hinzufügen. Erstellt eine Zuordnung Bereich fügen Sie Bereich Mappings.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Option 1: Ordnen Sie die Daten für die Zuordnung einer Liste

Ordnen Sie die Daten für jeden Mandanten eine Liste Zuordnung hinzufügen.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Option 2: Ordnen Sie die Daten für eine bereichszuordnung

Hinzufügen der Bereich für alle Mandanten-Id Bereich – Datenbank Assoziationen:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Schritt 4-Option 3: Ordnen Sie die Daten für mehrere Mandanten in einer Datenbank

Führen Sie für jeden Mandanten hinzufügen-ListMapping (option 1 oben). 


## <a name="checking-the-mappings"></a>Überprüfen der Zuordnung

Informationen zu vorhandenen Splitter und Mappings zugeordnet abgefragt werden mit folgenden Befehlen:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Zusammenfassung

Nach Abschluss von Setup können Sie beginnen die Clientbibliothek elastische Datenbank verwenden. Sie können auch [datenabhängiges routing](sql-database-elastic-scale-data-dependent-routing.md) und [Multi-Splitter Abfrage](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Nächste Schritte


Erhalten Sie [Azure SQL DB elastische Database Tools Textes](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)PowerShell-Skripts.

Die Tools sind auch auf GitHub: [Azure/elastisch-Db-Tools](https://github.com/Azure/elastic-db-tools).

Verwenden Sie Split-Dienstprogramm Daten in oder aus Multi-Tenant-Modells eine einzelne Modell verschieben. [Split-Dienstprogramm](sql-database-elastic-scale-get-started.md)anzeigen

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Finden Sie Informationen auf gemeinsame Daten Architektur Multi-Tenant-Software als Service (SaaS) ergeben [Entwurfsmuster für Multi-Tenant SaaS mit Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Feature- Anfragen

Bei Fragen wenden Sie sich an uns [SQL Datenbank Forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) und Wünsche, fügen sie das [Bewertungsportal SQL-Datenbank](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png
 
