<properties 
    pageTitle="Abfragen mit mehreren Splitter | Microsoft Azure" 
    description="Splitter mit der Clientbibliothek elastische Datenbank verlaufen Sie Abfragen." 
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
    ms.date="04/12/2016" 
    ms.author="torsteng"/>

# <a name="multi-shard-querying"></a>Abfragen mit mehreren Splitter

## <a name="overview"></a>Übersicht

[Elastische Datenbanktools](sql-database-elastic-scale-introduction.md)können Sie Projektmappen Sharding Datenbank erstellen. **Abfragen mit mehreren Splitter** wird für Vorgänge wie Data Collection/verwendet, Ausführen einer Abfrage, die sich über mehrere Splitter erfordern. (Vergleichen Sie dies mit [datenabhängiges routing](sql-database-elastic-scale-data-dependent-routing.md), das alle auf einen Splitter.) 

## <a name="overview"></a>Übersicht

1. Abgerufen Sie ein [**RangeShardMap**](https://msdn.microsoft.com/library/azure/dn807318.aspx) oder [**ListShardMap**](https://msdn.microsoft.com/library/azure/dn807370.aspx) mit [**TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), [**TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)oder [**GetShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) -Methode werden. [**Erstellen einer ShardMapManager**](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) und [**einem RangeShardMap oder ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap)anzeigen
2. Erstellen eines **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)** -Objekts.
2. Erstellen einer **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
3. Legen Sie die **[CommandText-Eigenschaft](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)** eine T-SQL-Befehl.
3. Führen Sie den Befehl durch Aufruf der **[ExecuteReader-Methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
4. Zeigen Sie die Ergebnisse mithilfe der **[MultiShardDataReader-Klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Beispiel

Der folgende Code veranschaulicht die Verwendung von Multi-Splitter Abfragen mit einem bestimmten **ShardMap** namens *MyShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                } 
           } 
    } 

 
Ein Hauptunterschied besteht der Erstellung von Multi-Splitter Verbindungen. Wo **SqlConnection** auf eine einzelne Datenbank arbeitet, wird **MultiShardConnection** als Eingabe eine ***Auflistung Splitter*** . Füllen Sie diese Sammlung Splitter Splitter-Karte. Die Abfrage erfolgt dann auf Splitter mit **UNION ALL** Semantik einer einzelnen Gesamtergebnis zusammenstellen. Optional kann die Ausgabe mithilfe der **ExecutionOptions** -Eigenschaft Befehl Name, in dem die Zeile stammt, Splitter hinzugefügt werden. 

Beachten Sie den Aufruf von **myShardMap.GetShards()**. Diese Methode ruft alle Splitter aus der shardzuordnung und bietet eine einfache Möglichkeit zum Ausführen einer Abfrage über alle relevanten Datenbanken. Die zurückgegebene Auflistung von Splitter für eine Abfrage mit mehreren Splitter verfeinert weiter durch Ausführen einer LINQ-Abfrage in der Auflistung vom Aufruf von **myShardMap.GetShards()**. In Kombination mit der Teilergebnisse aktuelle Fähigkeit Multi-Splitter Abfragen soll für Dutzende bis Hunderte von Splitter geeignet.

Eine Einschränkung mit mehreren Splitter Abfragen fehlt derzeit Validierung Splitter und Shardlets abgefragt werden. Beim datenabhängigen routing überprüft, ob ein angegebene Splitter Teil der shardzuordnung bei Abfragen, führen mehrere Splitter Abfragen nicht dazu. Dies kann zu Multi-Splitter Abfragen für Datenbanken, die von der shardzuordnung entfernt wurden.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Abfragen mit mehreren Splitter und Teilen Zusammenführungsoperationen

Multi-Splitter Abfragen überprüfen nicht, ob laufenden Teilen Zusammenführungsoperationen Shardlets der abgefragten Datenbank beteiligt sind. (Siehe [Skalieren elastische Dienstprogramm zum Teilen zusammenführen verwenden](sql-database-elastic-scale-overview-split-and-merge.md).) Dies kann zu Inkonsistenzen führen, Zeilen aus der gleichen Shardlet für mehrere Datenbanken in einer Multi-Splitter Abfrage anzeigen. Diese Einschränkung berücksichtigen und Ausgleichsmodus laufender Teilen zusammenführen und Änderung der shardzuordnung bei mehreren Splitter Abfragen berücksichtigen.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Siehe auch
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)** Klassen und Methoden.


Verwalten Sie Splitter mit der [Clientbibliothek elastische Datenbank](sql-database-elastic-database-client-library.md). Enthält einen Namespace mit dem Namen [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) , die die Möglichkeit bietet, mehrere Splitter mit einer einzigen Abfrage Ergebnis abzufragen. Es stellt eine Abfrage Abstraktion Auflistung Splitter. Darüber hinaus alternative Ausführungsrichtlinien, insbesondere Teilergebnisse zu Fehlern beim Abfragen über viele Splitter.  

 