< Eigenschaften
    
    pageTitle="Upgrade to the latest elastic database client library | Microsoft Azure" 
    description="Upgrade apps and libraries using Nuget" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>Aktualisieren einer Anwendung die neueste elastische Datenbank-Clientbibliothek verwenden

Neue Versionen der [Clientbibliothek elastische Datenbank](sql-database-elastic-database-client-library.md) sind durch NuGetand der NuGetPackage Manager-Schnittstelle in Visual Studio verfügbar. Updates enthalten Fehlerbehebungen und neue Funktionen der Clientbibliothek.

**Für die neueste Version:** Gehen Sie zu [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Erneutes Erstellen der Anwendung mit der neuen Bibliothek sowie Ändern der vorhandenen Splitter Karte Metadaten in SQL Azure Datenbanken zur Unterstützung neuer Features gespeichert.

Diese Schritte in der Reihenfolge wird sichergestellt, dass alte Versionen der Clientbibliothek nicht mehr in Ihrer Umgebung beim Aktualisieren von Metadatenobjekten, sodass alte Version Metadatenobjekte nach Aktualisierung nicht erstellt werden.   

## <a name="upgrade-steps"></a>Schritte

**1. aktualisieren Sie Ihre Anwendung.** In Visual Studio herunterladen und in alle Ihre Entwicklungsprojekte, die die Bibliothek nutzen die neueste Version des Client-Bibliothek verweisen. neu erstellen und bereitstellen. 

 * Wählen Sie in der Visual Studio-Projektmappe **Extras** --> **NuGet Paket-Manager** -->  **NuGet-Pakete verwalten, Lösung**. 
 * (Visual Studio 2013) Wählen Sie im linken Bereich **Updates aus**, und wählen Sie die Schaltfläche **Update** -Paket **Azure SQL Datenbank elastische Skalierung Clientbibliothek** , die im Fenster angezeigt wird.
 * (Visual Studio 2015) Setzen Sie das Filter aktualisieren **zur Verfügung**. Wählen Sie das Paket aktualisieren, und klicken Sie auf die Schaltfläche **Aktualisieren** .
    
 
 * Erstellen und bereitstellen. 

**2. aktualisieren Sie Ihre Skripts.** Wenn Sie Splitter [herunterladen Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) verwalten und in das Verzeichnis, aus dem Sie Skripten, **PowerShell** -Skripts verwenden. 

**3. aktualisieren Sie Ihren Service Teilen zusammenführen.** Verwenden Sie das elastische Datenbank Split-Dienstprogramm Sharding Daten [herunterzuladen und die neueste Version des Tools](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)neu organisieren. Detaillierte Schritte für der Service finden Sie [hier](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. Shardzuordnungs-Manager Datenbanken aktualisieren**. Aktualisieren der Metadaten Splitter Karten in Azure SQL-Datenbank unterstützen.  Es gibt zwei Arten Sie dies ausführen können PowerShell oder C#. Beide Optionen sind unten aufgeführt.

***Option 1: Aktualisieren von Metadaten mithilfe von PowerShell***

1. Herunterladen Sie das neuesten Befehlszeilenprogramm für NuGet [hier](http://nuget.org/nuget.exe) und in einem Ordner speichern. 

2. Öffnen Sie ein Eingabeaufforderungsfenster, navigieren Sie zu dem Ordner und den Befehl:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`

3. Navigieren Sie zum Unterordner, enthält der neuen Client-DLL-Version, die Sie gerade, die zum Beispiel heruntergeladen haben:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`

4. Herunterladen Sie elastische Datenbank Client Upgrade Scriptlet im [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)und speichern Sie sie in denselben Ordner, der die DLL enthält.

5. Aus dem Ordner "PowerShell.\upgrade.ps1" von der Befehlszeile aus ausführen und befolgen.
 
***Option 2: Aktualisieren Sie Metadaten mit C#***

Alternativ erstellen Sie eine Visual Studio-Anwendung, die die ShardMapManager öffnet, durchläuft alle Splitter und führt die Aktualisierung der Metadaten durch Aufrufen der Methoden [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) und [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) wie in diesem Beispiel: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 
    
    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Diese Techniken für Metadaten-Upgrades können ohne Schaden mehrmals angewendet werden. Beispielsweise wenn eine ältere Clientversion einen versehentlich nach bereits aktualisiert haben, führen Sie Aktualisierung erneut über alle Splitter um sicherzustellen, dass die neueste Version der Metadaten in der gesamten Infrastruktur vorhanden ist. 

**Hinweis:**  Neue Versionen der Clientbibliothek veröffentlicht bis Datum weiterhin in früheren Versionen der Shardzuordnungs-Manager Metadaten Azure SQL DB und umgekehrt.   Aber einige der neuen Features in der neuesten Client nutzen, Metadaten aktualisiert werden muss.   Beachten Sie, dass Benutzer Daten oder anwendungsspezifische Daten nur Objekte erstellt und von der Shardzuordnungs-Manager verwendet Metadaten Upgrades nicht beeinträchtigt werden.  Und Applikationen weiterhin durch die oben beschriebenen Upgradesequenz. 

## <a name="elastic-database-client-version-history"></a>Elastische Datenbank Client Versionsverlauf 

Versionsverlauf finden Sie unter [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  


<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png
 