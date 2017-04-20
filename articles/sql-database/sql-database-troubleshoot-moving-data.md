<properties
    pageTitle="Verschieben von Datenbanken zwischen Servern, Abonnements und in Azure."
    description="Schritte zum Kopieren, verlagern und Migrieren von Daten und Datenbanken in Azure SQL-Datenbank."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="move-databases-between-servers-between-subscriptions-and-in-and-out-of-azure"></a>Verschieben von Datenbanken zwischen Servern, Abonnements und in Azure

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]
##<a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Verschieben eine Datenbank auf einem anderen Server in der gleichen Anmeldung
- Klicken Sie in [Azure-Portal](https://portal.azure.com)auf **SQL-Datenbanken**, wählen Sie eine Datenbank aus der Liste und klicken Sie auf **Kopieren**. Weitere Details finden Sie unter [kopieren eine Azure SQL-Datenbank](sql-database-copy.md) .

## <a name="to-move-a-database-between-subscriptions"></a>Verschieben eine Datenbank von Abonnements
- In [Azure-Portal](https://portal.azure.com)auf **SQL Server** und wählen Sie die Server, der die Datenbank aus der Liste gespeichert. Klicken Sie auf **Verschieben**, und wählen Sie die Ressourcen verschieben und das Abonnement zu verschieben.

## <a name="to-migrate-a-sql-database-into-azure"></a>In Azure SQL-Datenbank migrieren
- Datenbank-Kompatibilitätsgrad bestimmen und die richtigen Migrationsmethode Ihren Bedürfnissen entsprechend wählen. Befolgen Sie die Richtlinien und Optionen bei der [Migration einer SQL Server-Datenbank](sql-database-cloud-migrate.md).

## <a name="to-create-a-copy-of-a-database-for-use-outside-of-azure"></a>Erstellen Sie eine Kopie einer Datenbank für die Verwendung von Azure
- [Exportieren Sie eine BACPAC-Datei.](sql-database-export.md)
