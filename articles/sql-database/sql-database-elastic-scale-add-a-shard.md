<properties 
    pageTitle="Hinzufügen einen Splitter mit elastische Datenbanktools | Microsoft Azure" 
    description="Festlegen, wie elastische Skalierung APIs verwenden, um einen neuen Splitter hinzuzufügen." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="adding-a-shard-using-elastic-database-tools"></a>Einen Splitter mit elastische Database Tools hinzufügen

## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Splitter einen neuen Bereich oder Schlüssel hinzufügen  

Clientanwendungen müssen häufig fügen neue Splitter Daten behandeln, die möglicherweise neue Schlüssel oder Schlüsselbereiche für eine bereits shardzuordnung. Beispielsweise muss Mandanten-ID Sharding Antrag Bereitstellung neuen Splitter für einen neuen Mandanten oder Sharding monatlichen Daten möglicherweise einen neuen Splitter vor Monat bereitgestellt. 

Ist der neue Bereich der Schlüsselwerte nicht bereits Teil einer vorhandenen Zuordnung, ist sehr einfach neue Splitter hinzufügen und neue Schlüssel oder Bereich, Splitter. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Beispiel: einen und Hinzufügen des Bereichs eine vorhandene Zuordnung Splitter
Dieses Beispiel verwendet die [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx) [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0})) Methoden und erstellt eine Instanz der [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) -Klasse. Im folgenden Beispiel eine Datenbank namens **sample_shard_2** und alle erforderlichen Schemaobjekte verreisen erstellten Aufnahme Bereich [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Als Alternative können Sie Powershell ein neues Shardzuordnungs-Manager erstellen. Ein Beispiel steht [hier](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).
## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Splitter leere Teil eines vorhandenen Bereichs hinzufügen  

In einigen Fällen einen Bereich bereits einen zugeordnet und teilweise mit Daten gefüllt, aber jetzt anstehende Daten sollen an einen anderen Splitter. Angenommen, Sie Splitter von Tagen und einen bereits zugewiesenen 50 Tagen müssen jedoch am Tag 24 zukünftige Daten in verschiedenen Splitter landen möchten. Elastische Datenbank [Split - Dienstprogramm](sql-database-elastic-scale-overview-split-and-merge.md) können diesen Vorgang ausführen, aber wenn Daten nicht erforderlich (z. B. Daten für den Bereich von Tagen [25, 50), also ist Tag 25 einschließlich 50 exklusiv ist noch nicht vorhanden) können diese vollständig Splitter Karte Management APIs direkt verwenden.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Beispiel: einen Bereich Teilen und neu Splitter leeren Bereich zuweisen

Eine Datenbank namens "sample_shard_2" sowie alle erforderlichen Schemaobjekte verreisen erstellt wurden.  

 
    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
    
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Wichtig**: Verwenden Sie dieses Verfahren nur, wenn Sie sicher, dass der Bereich für sind die aktualisierte Zuordnung leer ist.  Die oben genannten Methoden aktivieren Daten für den Bereich verschoben wird, Sie auf Schecks im Code enthalten ist.  Im Bereich verschobenen Zeilen vorhanden sind, entspricht die Verteilung der Daten nicht aktualisierte shardzuordnung. Stattdessen führen Sie den Vorgang in diesen Fällen mithilfe des [Split - Dienstprogramm](sql-database-elastic-scale-overview-split-and-merge.md) .  


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
