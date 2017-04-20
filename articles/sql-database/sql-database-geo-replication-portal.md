<properties 
    pageTitle="Geo-Replikation für Azure SQL-Datenbank mit der Azure-Portal konfigurieren | Microsoft Azure" 
    description="Konfigurieren Sie Geo-Replikation für Azure SQL Azure-portal" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/18/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-the-azure-portal"></a>Konfigurieren Sie Geo-Replikation für Azure SQL-Datenbank mit der Azure-portal


> [AZURE.SELECTOR]
- [Übersicht](sql-database-geo-replication-overview.md)
- [Azure-portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

Dieser Artikel veranschaulicht die aktive Geo-Replikation für Datenbank SQL [Azure-Portal](http://portal.azure.com)zu konfigurieren.

Um Failover mit der Azure-Portal zu initiieren, finden Sie unter [Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit der Azure-Portal](sql-database-geo-replication-failover-portal.md).

>[AZURE.NOTE] Aktive Geo-Replikation (lesbare sekundäre) steht jetzt für alle Datenbanken in alle Dienstebenen. Im April 2017 nicht lesbaren sekundären Typs zurückgezogen und Datenbanken nicht lesbar, lesbare sekundäre werden automatisch aktualisiert.

Konfigurieren Sie Geo-Replikation mithilfe von Azure-Portal benötigen Sie den folgenden Ressourcen:

- Eine SQL Azure-Datenbank - der primären Datenbank auf einer anderen geographischen Region repliziert.

## <a name="add-secondary-database"></a>Sekundäre Datenbank hinzufügen

Die folgenden Schritte erstellen eine neue sekundäre Datenbank Geo Replikationspartnerschaft  

Fügen Sie einen sekundären müssen Sie Ihrem Besitzer oder Eigentümer sein. 

Die sekundäre Datenbank haben denselben Namen wie die primäre Datenbank und standardmäßig haben die gleichen Service-Level. Die sekundäre Datenbank kann eine einzelne Datenbank oder elastische Datenbank sein. Weitere Informationen finden Sie unter [Dienstebenen](sql-database-service-tiers.md).
Nach der sekundären erstellt und ausgesät, beginnen die Daten aus der primären Datenbank in die neue sekundäre Datenbank replizieren. 

> [AZURE.NOTE] Wenn Partner-Datenbank vorhanden (-aufgrund eine frühere Geo-Replikation Beziehung beendet) schlägt der Befehl fehl.

### <a name="add-secondary"></a>Sekundäre hinzufügen

1. Suchen Sie in [Azure-Portal](http://portal.azure.com)die Datenbank für Geo-Replikation.
2. Wählen Sie auf der Seite SQL Datenbank **Geo-Replikation**, und wählen Sie den Bereich auf die sekundäre Datenbank erstellen. Wählen Sie eine Region als Bereich für die primäre Datenbank wird, [paarweise Region](../best-practices-availability-paired-regions.md) jedoch empfohlen.

    ![Geo-Replikation](./media/sql-database-geo-replication-portal/configure-geo-replication.png)


4. Wählen oder Server und Tarif für die sekundäre Datenbank konfigurieren.

    ![Sekundäre konfigurieren](./media/sql-database-geo-replication-portal/create-secondary.png)

5. Optional können Sie einen Datenbankpool elastische eine sekundäre Datenbank hinzufügen:

 Zum Erstellen der sekundären Datenbank in einem Pool **elastische Datenbankpool** auf und wählen Sie einen Pool auf dem Zielserver. Ein Pool muss bereits auf dem Zielserver vorhanden sein, diesen Workflow keinen Pool erstellen.

6. Klicken Sie auf **Erstellen** , um die sekundäre hinzufügen.
 
6. Die sekundäre Datenbank erstellt und seeding beginnt. 
 
    ![Sekundäre konfigurieren](./media/sql-database-geo-replication-portal/seeding0.png)

7. Nach Abschluss der Aussaat zeigt die sekundäre Datenbank den Status.

    ![das Seeding abgeschlossen](./media/sql-database-geo-replication-portal/seeding-complete.png)


## <a name="remove-secondary-database"></a>Sekundäre Datenbank entfernen

Der Vorgang beendet die Replikation auf die sekundäre Datenbank dauerhaft und ändert die Rolle des sekundären in eine reguläre Datenbank schreibgeschützt. Wenn die Verbindung mit der sekundären Datenbank beschädigt ist, der Befehl erfolgreich ausgeführt wird, jedoch die sekundäre wird kein Schreibzugriff erst nach Verbindung wiederhergestellt.  

1. Wechseln Sie in [Azure-Portal](http://portal.azure.com)mit der primären Datenbank partnerschaftlich Geo-Replikation.
2. Wählen Sie auf der Seite SQL Datenbank **Geo-Replikation**.
3. Wählen Sie in der Liste **sekundäre** die Datenbank Geo Replikationspartnerschaft entfernen möchten.
4. Klicken Sie auf **Replikation beenden**.

    ![Sekundäre entfernen](./media/sql-database-geo-replication-portal/remove-secondary.png)

5. Durch Klicken auf **Stop Replikation** wird ein Bestätigungsfenster klicken **Ja** Partnerschaft Geo-Replikation die Datenbank aufgehoben (mit einer schreibgeschützten Datenbank nicht Teil der Replikation festgelegt).


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über aktive Geo-Replikation finden Sie unter - [Active Geo-Replikation](sql-database-geo-replication-overview.md)
- Eine Übersicht über Business Continuity und Szenarien finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md)

