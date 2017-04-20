<properties
    pageTitle="Leistungsindikatoren für shardzuordnungs-manager"
    description="ShardMapManager-Klasse und Daten von Leistungsindikatoren"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="SilviaDoomra"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="SilviaDoomra"/>

# <a name="performance-counters-for-shard-map-manager"></a>Leistungsindikatoren für shardzuordnungs-manager

Die Leistung des [shardzuordnungs-Manager](sql-database-elastic-scale-shard-map-management.md)kann insbesondere bei [datenabhängiges routing](sql-database-elastic-scale-data-dependent-routing.md)verwendet werden. Leistungsindikatoren werden mit Methoden der Microsoft.Azure.SqlDatabase.ElasticScale.Client-Klasse erstellt.  

Leistungsindikatoren werden zum [datenabhängiges routing](sql-database-elastic-scale-data-dependent-routing.md) -Prozesse überwachen. Diese Leistungsindikatoren sind im Systemmonitor unter der Kategorie "Elastische Datenbank: Splitter Management".

**Für die neueste Version:** Gehen Sie zu [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Siehe auch [Aktualisieren eine Anwendung die neueste elastische Datenbank-Clientbibliothek verwenden](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

* Um Leistungskategorie und Leistungsindikatoren erstellen, muss der Benutzer Mitglied **der lokalen Administratorengruppe auf dem Computer mit der Anwendung** sein.  

* Eine Instanz erstellen und Aktualisieren der Leistungsindikatoren, muss der Benutzer Mitglied der Gruppe **Administratoren** oder **Systemmonitorbenutzer** sein. 

## <a name="create-performance-category-and-counters"></a>Die Leistungskategorie und Leistungsindikatoren erstellen 

Rufen Sie zum Erstellen der Leistungsindikatoren CreatePeformanceCategoryAndCounters-Methode der [ShardMapManagmentFactory-Klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Nur Administratoren kann die Methode ausführen: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Auch können [diese](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) PowerShell-Skript Sie die Methode ausgeführt. Die Methode erstellt die folgenden Leistungsindikatoren:  

* **Cache-Zuordnung**: Anzahl der Mappings für die Zuordnung Splitter zwischengespeichert.
*  **DDR-Operationen/Sek.**: bei Daten von Arbeitsgängen der shardzuordnung. Dieser Zähler wird bei Aufruf eine Verbindung zum Ziel-Splitter auf [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) aktualisiert. 
*  **Zuordnen von Lookup Cachetreffer/Sek.**: Rate der erfolgreichen Cache Suchvorgänge für Zuordnungen Splitter zuordnen. 
*  **Zuordnung Suche: Cachefehlschläge/Sek.**: bei fehlerhaften Cache Suchvorgänge für Zuordnungen Splitter zuordnen.
*  **Zuordnung hinzugefügt oder aktualisiert/s**: bei der Zuordnung hinzugefügt werden oder aktualisierten Cache für die shardzuordnung. 
*  **Zuordnung entfernt/s**: Rate an der Zuordnung werden Cache für die shardzuordnung entfernt. 

Leistungsindikatoren werden für jedes zwischengespeicherte shardzuordnung pro Prozess erstellt.  


## <a name="notes"></a>Notizen
Die folgenden Ereignisse des Erstellen der Leistungsindikatoren:  

* Initialisierung des [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) mit [unverzüglichem Laden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), wenn die ShardMapManager Splitter enthält. Dazu gehören [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) und [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) -Methoden.
* Erfolgreiche Suche einen Splitter-Karte (mit [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) oder [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 

* Erfolgreiche Erstellung der Splitter Karte CreateShardMap().

Die Leistungsindikatoren werden von allen Cache-Operationen auf shardzuordnung und Mappings aktualisiert. Erfolgreiche Entfernen der Splitter Karte mit DeleteShardMap () Reults Instanz der Leistungsindikatoren löschen.  

## <a name="best-practices"></a>Bewährte Methoden

* Erstellung der Leistungskategorie und Indikatoren sollte nur einmal durchgeführt werden, bevor ShardMapManager Objekt. Jede Ausführung des Befehls CreatePerformanceCategoryAndCounters() Löscht vorherigen Leistungsindikatoren (verlieren Daten von allen Instanzen) und neue erstellt.  

* Leistungsindikatorinstanzen werden pro Prozess erstellt. Absturz oder Entfernen einer Zuordnung Splitter aus dem Cache führt Löschen von Instanzen Leistungsindikatoren Leistung.  

### <a name="see-also"></a>Siehe auch

[Elastische Datenbank Funktionen im Überblick](sql-database-elastic-scale-introduction.md)  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

