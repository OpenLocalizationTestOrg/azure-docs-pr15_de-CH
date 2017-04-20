<properties
    pageTitle="Übersicht: Verwaltungstools für SQL-Datenbank | Microsoft Azure"
    description="Tools und Optionen zum Verwalten von Azure SQL-Datenbank vergleicht"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="sstein"/>

# <a name="overview-management-tools-for-sql-database"></a>Übersicht: Verwaltungstools für SQL-Datenbank

In diesem Thema untersucht und vergleicht Tools und Optionen für SQL Azure-Datenbanken verwalten, damit Sie das richtige Tool für die Arbeit und Ihr Unternehmen erhalten. Auswählen des richtigen Tools wie viele Datenbanken hängt verwalten Sie, Aufgabe und wie oft ein Task ausgeführt wird.

## <a name="azure-portal"></a>Azure-portal

[Azure-Portal](https://portal.azure.com) ist eine webbasierte Anwendung, können erstellen, aktualisieren und Löschen von Datenbanken und logische Server und Datenbankaktivität überwacht. Dieses Tool eignet sich, wenn Sie sind gerade erst mit Azure einige Datenbanken verwalten oder etwas zu schnell.

Weitere Informationen über das Portal finden Sie in der [SQL-Datenbanken verwalten mithilfe des Azure-Portals](sql-database-manage-portal.md).

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio und SQL Server-Tools in Visual Studio

SQL Server Management Studio (SSMS) und SQL Server Data Tools (SSDT) sind Clienttools, die auf dem Computer für die Verwaltung und die Datenbank in der Cloud ausgeführt. Wenn Sie eine Anwendungsentwickler mit Visual Studio oder integrated Development Environments (IDEs) [SSDT in Visual Studio verwenden](https://msdn.microsoft.com/library/mt204009.aspx). Viele Datenbankadministratoren kennen SSMS, die mit SQL Azure-Datenbanken verwendet werden können. [Herunterladen der neuesten Version von SSMS](https://msdn.microsoft.com/library/mt238290) und beim Arbeiten mit Azure SQL-Datenbank immer die neueste Version verwenden. Weitere Informationen zum Verwalten von Datenbanken SQL Azure mit SSMS finden Sie unter [SQL-Datenbanken verwalten SSMS verwenden](sql-database-manage-azure-ssms.md).

> [AZURE.IMPORTANT] Verwenden Sie immer die neueste Version von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) und [SQL Server-Tools](https://msdn.microsoft.com/library/mt204009.aspx) mit Updates für Microsoft Azure und SQL Datenbank synchronisiert bleiben.


## <a name="powershell"></a>PowerShell

Sie können PowerShell Verwaltung von Datenbanken und elastische datenbankpools und Azure Ressource Bereitstellung automatisieren. Microsoft empfiehlt, dieses Tool für die Verwaltung einer großen Anzahl von Datenbanken und Automatisieren der Bereitstellung und Ressource ändert sich in einer.

Weitere Informationen finden Sie in der [SQL-Datenbank mit PowerShell verwalten](sql-database-manage-powershell.md)

## <a name="elastic-database-tools"></a>Elastische Datenbanktools
Aktionen wie verwenden Sie elastische Datenbank 

* T-SQL-Skript anhand einer Reihe von Datenbanken mit einer [elastischen Job](sql-database-elastic-jobs-overview.md) ausführen
* Verschieben von modelldatenbanken Multi-Tenant-zu einem einzigen Mieter Modell mit [Split - Dienstprogramm](sql-database-elastic-scale-overview-split-and-merge.md)
* Verwalten von Datenbanken in einem einzelnen Mieter Modell oder ein Multi-Tenant-Modell die [Clientbibliothek elastische skalieren](sql-database-elastic-database-client-library.md).
 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Azure Ressourcenmanager](https://azure.microsoft.com/features/resource-manager/)
- [Azure-Automatisierung](https://azure.microsoft.com/documentation/services/automation/)
- [Azure Scheduler](https://azure.microsoft.com/documentation/services/scheduler/)