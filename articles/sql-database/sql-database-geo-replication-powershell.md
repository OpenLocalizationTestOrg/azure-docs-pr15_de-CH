<properties 
    pageTitle="Aktive Geo-Replikation für Azure SQL-Datenbank mit PowerShell konfigurieren | Microsoft Azure" 
    description="Konfigurieren Sie aktive Geo-Replikation für Azure SQL-Datenbank mit PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
   ms.workload="NA"
    ms.date="07/14/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-powershell"></a>Konfigurieren Sie Geo-Replikation für SQL Azure-Datenbank mit PowerShell

> [AZURE.SELECTOR]
- [Übersicht](sql-database-geo-replication-overview.md)
- [Azure-Portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

Dieser Artikel veranschaulicht die aktive Geo-Replikation für SQL-Datenbank mit PowerShell konfigurieren.

Um Failover mit PowerShell starten finden Sie unter [Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit PowerShell](sql-database-geo-replication-failover-powershell.md).

>[AZURE.NOTE] Aktive Geo-Replikation (lesbare sekundäre) steht jetzt für alle Datenbanken in alle Dienstebenen. Im April 2017 nicht lesbaren sekundären Typs zurückgezogen und Datenbanken nicht lesbar, lesbare sekundäre werden automatisch aktualisiert.



Aktive Geo-Replikation konfigurieren PowerShell verwenden, benötigen Sie:

- Ein Azure-Abonnement. 
- Eine SQL Azure-Datenbank - die primäre Datenbank, die Sie replizieren möchten.
- Azure PowerShell 1.0 oder höher. Sie können herunterladen und Installieren von Azure PowerShell-Module folgenden [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Ihre Anmeldeinformationen konfigurieren und Ihr Abonnement auswählen

Zunächst müssen Sie Zugriff auf Ihre Azure-Konto so PowerShell starten und führen Sie das folgende Cmdlet. Geben Sie im Anmeldebildschirm derselben e-Mail und Kennwort für die Anmeldung bei Azure-Portal.


    Login-AzureRmAccount

Nach der erfolgreichen Anmeldung sehen Sie einige Informationen auf dem Bildschirm, die die Id Sie angemeldet und Azure-Abonnements haben Sie Zugriff auf.


### <a name="select-your-azure-subscription"></a>Wählen Sie Ihre Azure-Abonnement

Wählen Sie das Abonnement benötigen Sie Ihre Abonnement-ID Sie können Informationen aus dem vorherigen Schritt die Abonnement-Id kopieren oder wenn Sie mehrere Abonnements und Weitere Informationen benötigen, kann das Cmdlet " **Get-AzureRmSubscription** " ausführen und kopieren die gewünschten Abonnementinformationen aus dem Resultset. Das folgende Cmdlet verwendet die Abonnement-Id das aktuelle Abonnement festgelegt:

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Nach erfolgreich ausgeführt **AzureRmSubscription auswählen** werden Sie die PowerShell Prompt zurückgegeben.


## <a name="add-secondary-database"></a>Sekundäre Datenbank hinzufügen


Die folgenden Schritte erstellen eine neue sekundäre Datenbank Geo Replikationspartnerschaft  
  
Um eine sekundäre aktivieren müssen Sie Ihrem Besitzer oder Eigentümer sein. 

**Neu-AzureRmSqlDatabaseSecondary** -Cmdlet können Sie hinzufügen, eine sekundäre Datenbank auf einem Partnerserver in einer lokalen Datenbank auf dem Server die (primäre Datenbank) verbunden. 

Dieses Cmdlet ersetzt Parameters **– IsContinuous** **Start AzureSqlDatabaseCopy** .  Es gibt ein **AzureRmSqlDatabaseSecondary** -Objekt, die von anderen Cmdlets verwendet werden, eine spezielle Replikations-Verknüpfung identifizieren. Dieses Cmdlet wird zurückgegeben, wenn die sekundäre Datenbank erstellt und vollständigem Seeding. Je nach Größe der Datenbank kann es Stunden Minuten dauern.

Die replizierte Datenbank auf dem sekundären Server haben denselben Namen wie die Datenbank auf dem primären Server und standardmäßig haben die gleichen Service-Level. Die sekundäre Datenbank gelesen oder nicht gelesen werden kann und kann eine einzelne Datenbank oder elastische Datenbank. Weitere Informationen finden Sie unter [Neue AzureRMSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx) und [Dienstebenen](sql-database-service-tiers.md).
Nach der sekundären erstellt und ausgesät, beginnen die Daten aus der primären Datenbank in die neue sekundäre Datenbank replizieren. Schritte beschrieben, wie diese Aufgabe mithilfe von PowerShell nicht lesbar und lesbar sekundäre mit einer einzelnen Datenbank oder einer elastischen Datenbank erstellen.

Partner-Datenbank bereits (-aufgrund eine frühere Geo-Replikation Beziehung beendet) schlägt der Befehl fehl.



### <a name="add-a-non-readable-secondary-single-database"></a>Fügen Sie eine nicht lesbare sekundäre (einzelne Datenbank)

Der folgende Befehl erstellt eine nicht lesbare sekundäre Datenbank "Mydb" der Server "srv2" in Ressource Gruppe "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "No"



### <a name="add-readable-secondary-single-database"></a>Lesbare sekundäre (einzelne Datenbank) hinzufügen

Der folgende Befehl erstellt eine lesbare sekundäre Datenbank "Mydb" der Server "srv2" in Ressource Gruppe "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "All"




### <a name="add-a-non-readable-secondary-elastic-database"></a>Fügen Sie eine nicht lesbare sekundäre (elastische Datenbank)

Der folgende Befehl erstellt eine nicht lesbare sekundäre Datenbank "Mydb" im Datenbankpool elastische mit dem Namen "ElasticPool1" der Server "srv2" in Ressource Gruppe "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "No"


### <a name="add-a-readable-secondary-elastic-database"></a>Fügen Sie eine lesbare sekundäre (elastische Datenbank)

Der folgende Befehl erstellt eine lesbare sekundäre Datenbank "Mydb" im Datenbankpool elastische mit dem Namen "ElasticPool1" der Server "srv2" in Ressource Gruppe "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "All"





## <a name="remove-secondary-database"></a>Sekundäre Datenbank entfernen

Verwenden Sie das Cmdlet **Entfernen AzureRmSqlDatabaseSecondary** Replikationspartnerschaft zwischen einer sekundären Datenbank und den primären endgültig beendet. Die sekundäre Datenbank wird nach Beendigung Beziehung einer schreibgeschützten Datenbank. Wenn die Verbindung mit der sekundären Datenbank beschädigt ist wird der Befehl erfolgreich ausgeführt wird aber der sekundären Lese-/ Schreibzugriff nach Konnektivität wiederhergestellt ist. Weitere Informationen finden Sie unter [Entfernen AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603457.aspx) und [Dienstebenen](sql-database-service-tiers.md).

Dieses Cmdlet ersetzt Stop-AzureSqlDatabaseCopy für die Replikation. 

Das Entfernen wird die erzwungene Beendigung, die Replikationslink entfernt und die ehemalige sekundäre als eigenständige Datenbank vor Beendigung nicht vollständig repliziert wird. Alle Link-Daten werden auf die ehemalige primäre und sekundäre, ehemalige bereinigt Wenn oder verfügbar sind. Dieses Cmdlet wird zurückgegeben, wenn Replikationslink entfernt wird. 


Um sekundäre entfernen, müssen Benutzer Schreibzugriff auf primären und sekundären Datenbanken RBAC. Rollenbasierte Zugriffskontrolle für Details anzeigen

Folgender entfernt Replikationslink der Datenbank mit dem Namen "Mydb" Server "srv2" von der Ressource "rg2". 

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –SecondaryResourceGroup "rg2" –PartnerServerName "srv2"
    $secondaryLink | Remove-AzureRmSqlDatabaseSecondary 


## <a name="monitor-geo-replication-configuration-and-health"></a>Geo-Replikationskonfiguration und Gesundheit überwachen

Überwachungsaufgaben sind Überwachung des Geo-Replikationskonfiguration und Überwachung Daten-Replikation.  

[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx) kann verwendet werden, zum Abrufen der Informationen vor Replikation Hyperlinks in sys.geo_replication_links Katalog angezeigt.

Der folgende Befehl ruft Verbindungsstatus Replikation zwischen der primären Datenbank "Mydb" und den sekundären Server "srv2" von der Ressource "rg2".

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –PartnerResourceGroup "rg2” –PartnerServerName "srv2”


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über aktive Geo-Replikation finden Sie unter - [Active Geo-Replikation](sql-database-geo-replication-overview.md)
- Eine Übersicht über Business Continuity und Szenarien finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md)

