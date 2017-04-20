<properties 
    pageTitle="SQL Azure elastische Skalierung FAQ | Microsoft Azure" 
    description="Häufig gestellte Fragen zur Azure SQL-Datenbank elastische skalieren." 
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
    ms.date="05/03/2016" 
    ms.author="ddove"/>

# <a name="elastic-database-tools-faq"></a>Elastische Datenbanktools FAQ 

#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Wie füllen ich shardingschlüssel für Schemainformationen, habe einen einzelnen Mandanten Splitter und keine shardingschlüssel?

Das Schemaobjekt Info dient nur Zusammenführungsszenarien aufgeteilt. Ist eine Anwendung grundsätzlich einzelne Mieter erfordert keine Zusammenführung von Teilen und somit entfällt das Schemaobjekt Informationen aufgefüllt.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Bereitgestellt ich habe eine Datenbank und habe eine Shardzuordnungs-Manager Wie registriere ich die neue Datenbank als einen?

Finden Sie **[einen Splitter eine Anwendung die Clientbibliothek elastische Datenbank hinzufügen](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Wie Kosten elastische Datenbanktools?

Mit der Clientbibliothek elastische Datenbank entstehen keine Kosten. Kosten entstehen nur für SQL Azure-Datenbanken, die Bruchstücke und Splitter Karte Manager verwenden sowie Web-Worker-Rollen, die für das Zusammenführen von Teilen bereitstellen.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Warum funktionieren meine Anmeldeinformationen nicht einen von einem anderen Server hinzufügen?
Verwenden Sie keine Anmeldeinformationen in Form von "Benutzer ID=username@servername”, einfach verwenden" Benutzer-ID = Benutzername ".  Auch sicher, dass die Anmeldung "Username" Berechtigungen auf den Splitter.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Muss ich eine Shardzuordnungs-Manager erstellen und Auffüllen Splitter, jedem Start Meine Programme?

Nein – der Shardzuordnungs-Manager (z. B. **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) ist einmalig.  Die Anwendung sollte den Aufruf **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** beim Start der Anwendung verwenden.  Es sollte nur ein Aufruf pro Anwendungsdomäne.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Habe ich Fragen mit elastische Datenbanktools, Anreise beantwortet? 

Wenden Sie sich an uns im [Forum Azure SQL-Datenbank](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Wenn man eine Verbindung mit einem shardingschlüssel kann ich noch Daten für andere Sharding-Schlüssel auf derselben Splitter.  Ist dies beabsichtigt?

Elastische Skalierung APIs können Sie eine Verbindung mit der richtigen Datenbank für die shardingschlüssel jedoch nicht Sharding Schlüssel Filterung bereitzustellen.  Fügen Sie **WHERE** -Klausel der Abfrage einschränken, die bereitgestellten shardingschlüssel bei Bedarf hinzu

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Kann ich eine andere Azure Database Edition für jeden Splitter in meinem Set Splitter verwenden?

Ja, einen ist eine einzelne Datenbank und somit einen Splitter konnte sein Premium Edition einen anderen Standard Edition. Außerdem kann die Ausgabe des einen nach oben oder unten mehrmals während der Lebensdauer der Splitter skaliert werden.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Eine Datenbank während einer Operation Teilen oder Verbinden verfügt Teilung Merge Tool bereitstellen (oder löschen)? 

Nein. Für Vorgänge **Teilen** Zieldatenbank muss mit dem entsprechenden Schema vorhanden und der Shardzuordnungs-Manager registriert werden.  Für **Zusammenführungsoperationen** müssen Sie den Splitter aus der shardzuordnungs-Manager löschen und löschen Sie die Datenbank.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 