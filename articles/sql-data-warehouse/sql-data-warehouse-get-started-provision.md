<properties
   pageTitle="Erstellen Sie ein SQL Data Warehouse in Azure-Portal | Microsoft Azure"
   description="Erstellen Sie ein Azure SQL Data Warehouse in Azure-portal"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="barbkess;lodipalm;sonyama"/>

# <a name="create-an-azure-sql-data-warehouse"></a>Erstellen einer Azure SQL Datawarehouse

> [AZURE.SELECTOR]
- [Azure-portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Diese praktische Einführung verwendet eine SQL Data Warehouse erstellen, eine Beispieldatenbank für AdventureWorksDW enthält, Azure-Portal.


## <a name="prerequisites"></a>Erforderliche Komponenten

Zunächst müssen wie folgt vor:

- **Ein Azure-Konto**: Besuchen Sie [Azure-Testversion][] oder [MSDN Azure][] ein Konto erstellen.
- **SQL Azure-Server**: Weitere Einzelheiten finden Sie unter [Erstellen eines logisches Servers Azure SQL-Datenbank mit Azure-Portal][] .

> [AZURE.NOTE] Erstellen ein SQL Data Warehouse möglicherweise neue fakturierbaren Service.  Weitere Informationen finden Sie unter [SQL Data Warehouse Preise][] .

## <a name="create-a-sql-data-warehouse"></a>Erstellen Sie ein SQL Datawarehouse

1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.

2. Klicken Sie auf **+ Neuer** > **Daten + Speicher** > **SQL Datawarehouse**.

    ![Erstellen](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. **SQL Data Warehouse** -Blatt geben Sie die Informationen benötigt, und drücken Sie 'Create' erstellen.

    ![Datenbank erstellen](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Server**: Wählen Sie zuerst den Server empfohlen.  

    - **Datenbankname**: der Name, der auf SQL Data Warehouse verwendet wird.  Sie müssen auf dem Server eindeutig sein.
    
    - **Leistung**: Wir empfehlen 400 [DWUs][DWU]. Sie können den Schieberegler nach links oder rechts die Leistung des Datawarehouse oder Skalierung nach oben oder unten nach der Erstellung.  Weitere Informationen über DWUs finden Sie unter Dokumentation [Skalieren](./sql-data-warehouse-manage-compute-overview.md) oder unserer [Preisseite][SQL Data Warehouse Preise]. 

    - **Abonnement**: [Abonnements] , die diese SQL Data Warehouse Rechnung.

    - **Ressourcengruppe**: [Ressourcengruppen] [ Resource group] sind eine Sammlung von Azure Ressourcen verwalten soll. Erfahren Sie mehr über [Ressourcengruppen](../azure-resource-manager/resource-group-overview.md).

    - **Quelle auswählen**: Klicken Sie auf **Quelle auswählen** > **Beispiel**. Azure füllt automatisch die Option **Stichprobe** AdventureWorksDW.

> [AZURE.NOTE] Die Standardsortierreihenfolge für ein SQL Data Warehouse ist SQL_Latin1_General_CP1_CI_AS. Bei Bedarf eine andere Sortierung kann [T-SQL][] verwendet werden, die Datenbank mit einer anderen Sortierung erstellen.

4. Klicken Sie auf **Erstellen** , um Ihre SQL Data Warehouse erstellen.

5. Warten Sie einige Minuten. Wenn das Datawarehouse bereit ist, sollte Sie [Azure-Portal](https://portal.azure.com)zurück. Finden Sie SQL Data Warehouse auf dem Dashboard unter SQL-Datenbanken oder in der Ressourcengruppe verwendet zu erstellen. 

    ![Portal anzeigen](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## <a name="next-steps"></a>Nächste Schritte

Erstellung einer SQL Data Warehouse können [Verbinden](./sql-data-warehouse-connect-overview.md) und Abfragen beginnen.

Zum Laden von Daten in SQL Data Warehouse finden Sie unter [Übersicht über laden](./sql-data-warehouse-overview-load.md).

Wenn Sie eine bestehende Datenbank SQL Data Warehouse verschieben möchten, finden Sie unter [Übersicht über die Migration](./sql-data-warehouse-overview-migrate.md) oder [Migration Utility](./sql-data-warehouse-migrate-migration-utility.md).

Firewall-Regeln können auch mit Transact-SQL konfiguriert werden. Weitere Informationen finden Sie unter [Sp_set_firewall_rule][] und [Sp_set_database_firewall_rule][].

Es ist auch eine gute Idee sich [Best Practices][].

<!--Article references-->
[Erstellen Sie einen logischen Server Azure SQL-Datenbank mit Azure-portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Create an Azure SQL Database logical server with PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[resource groups]: ../resource-group-template-deploy-portal.md
[Bewährte Methoden]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Abonnement]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md
 
<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse Preise]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure-Testversion]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure-Gutschriften]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

