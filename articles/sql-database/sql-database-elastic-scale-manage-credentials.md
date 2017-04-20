<properties 
    pageTitle="Verwalten von Anmeldeinformationen in der Clientbibliothek elastische Datenbank | Microsoft Azure" 
    description="Wie Sie die richtige Anmeldeinformationen Admin Apps elastische Datenbank schreibgeschützt festlegen" 
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

# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Die Clientbibliothek elastische Datenbank Zugriff auf Anmeldeinformationen

Die [Clientbibliothek elastische Datenbank](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) verwendet [shardzuordnungs-Manager](sql-database-elastic-scale-shard-map-management.md)Zugriff auf drei verschiedene Arten von Anmeldeinformationen. Verwenden Sie je nach Bedarf die Anmeldeinformationen mit den niedrigsten Zugriffsstufe.

* **Verwaltung von Anmeldeinformationen**: Erstellen oder Bearbeiten eines shardzuordnungs-Managers. (Siehe [Glossar](sql-database-elastic-scale-glossary.md)) 
* **Anmeldeinformationen**: auf eine vorhandene shardzuordnungs-Manager Informationen über Splitter.
* **Anmeldeinformationen**: Verbindung zum Splitter. 

Siehe auch [Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md). 
 
## <a name="about-management-credentials"></a>Verwaltung von Anmeldeinformationen

Management Anmeldeinformationen wird ein [**ShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) -Objekt für Clientanwendungen erstellen, die Splitter Karten ändern. (Siehe beispielsweise [einen Splitter mit elastische Datenbanktools hinzufügen](sql-database-elastic-scale-add-a-shard.md) und [datenabhängiges routing](sql-database-elastic-scale-data-dependent-routing.md)) Der Benutzer der Clientbibliothek elastische Skalierung erstellt die SQL-Benutzer und SQL-Benutzernamen und stellt sicher, dass jede Datenbank der globalen Splitter und alle Splitter Datenbanken Lese-/Schreibberechtigung gewährt wird. Diese Anmeldeinformationen werden zum Verwalten der globalen shardzuordnung und lokale Splitter wird bei Änderung der shardzuordnung. Beispielsweise mit der Management-Anmeldeinformationen Splitter Map-Manager-Objekt erstellt (mit [**GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

Die Variable **SmmAdminConnectionString** wird eine Verbindungszeichenfolge, die Management-Anmeldeinformationen enthält. Die Benutzer-ID und Kennwort ermöglicht Schreibzugriff Splitter Datenbank und einzelne Splitter. Die Verbindungszeichenfolge Management auch den Servernamen und den Datenbanknamen die globalen Splitter Datenbank identifizieren. Hier ist eine normale Verbindungszeichenfolge dazu:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Verwenden Sie keine Werte in Form von "username@server"—instead den Wert "Username" verwenden.  Dies ist da Anmeldeinformationen für die Splitter-Manager-Datenbank und einzelne Splitter auf verschiedenen Servern arbeiten müssen.

## <a name="access-credentials"></a>Anmeldeinformationen für Zugriff
  
Beim einen Schema-Manager in einer Anwendung erstellen, die keine Splitter Karten verwalten verwenden Sie Anmeldeinformationen der schreibgeschützte Berechtigungen auf der globalen shardzuordnung. Die Daten aus der globalen shardzuordnung diese Anmeldeinformationen werden verwendet für [datenabhängiges routing](sql-database-elastic-scale-data-dependent-routing.md) und Splitter Karte Cache auf dem Client auffüllen. Die Anmeldeinformationen werden mit demselben Aufruf Muster **GetSqlShardMapManager** bereitgestellt wie oben gezeigt: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Beachten Sie die Verwendung von **SmmReadOnlyConnectionString** , die Verwendung der verschiedenen Anmeldeinformationen für diesen Zugriff für Benutzer **ohne Administratorrechte** : Diese Anmeldeinformationen soll keine Schreibberechtigungen auf der globalen shardzuordnung. 

## <a name="connection-credentials"></a>Anmeldeinformationen 

Die [**OpenConnectionForKey**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) -Methode auf einen Splitter ein shardingschlüssel zugeordnet sind zusätzliche Berechtigungen erforderlich. Diese Anmeldeinformationen müssen Berechtigungen für schreibgeschützten Zugriff auf die lokale Splitter Karte Tabellen auf den Splitter. Dies ist die Validierung für datenabhängiges routing auf den Splitter Verbindung erforderlich. Dieser Codeausschnitt ermöglicht Zugriff auf Daten im Kontext des datenabhängiges routing: 
 
    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

In diesem Beispiel enthält **SmmUserConnectionString** die Verbindungszeichenfolge für die Anmeldeinformationen des Benutzers. Für Azure SQL-Datenbank ist eine normale Verbindungszeichenfolge Anmeldeinformationen hier: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Mit den Administratoranmeldeinformationen keine Werte aus "username@server". Verwenden Sie stattdessen einfach "Benutzername".  Beachten Sie, dass die Verbindungszeichenfolge keinem Servernamen und Datenbanknamen enthalten. Deshalb **OpenConnectionForKey** Aufruf automatisch die Verbindung zum richtigen Splitter anhand des Schlüssels führt. Daher sind die Datenbanknamen und Servernamen nicht bereitgestellt. 

## <a name="see-also"></a>Siehe auch
[Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md)

[Die SQL-Datenbank sichern](sql-database-security.md)

[Erste Schritte mit elastische Datenbank Aufträge](sql-database-elastic-jobs-getting-started.md)

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 