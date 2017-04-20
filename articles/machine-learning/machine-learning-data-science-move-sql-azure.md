<properties 
    pageTitle="Verschieben von Daten mit einer Azure SQL-Datenbank für Azure Machine Learning | Azure" 
    description="Erstellen Sie SQL-Tabelle und Daten laden SQL-Tabelle" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016"
    ms.author="bradsev" /> 

# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Verschieben von Daten mit einer Azure SQL-Datenbank für Azure maschinelles lernen

In diesem Thema werden die Optionen zum Verschieben von Daten aus Flatfiles (CSV- oder TSV-Format) oder aus einer lokalen SQL Server mit einer Azure SQL-Datenbank gespeicherten Daten. Diese Vorgänge zum Verschieben von Daten in der Cloud gehören Daten Wissenschaft Team.

Ein Thema, das die Optionen zum Verschieben von Daten in einer lokalen SQL Server-Computer weitere beschreibt, finden Sie unter [Verschieben von Daten in SQL Server auf einem virtuellen Computer Azure](machine-learning-data-science-move-sql-server-virtual-machine.md).

Das folgende **Menü** enthält Links zu Themen, die beschreiben, wie Daten in zielumgebungen aufnehmen, wo die Daten gespeichert und verarbeitet während Team Daten Wissenschaft Prozess (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Die folgenden Tabelle werden die Optionen zum Verschieben von Daten in einer SQL Azure-Datenbank.

<b>QUELLE</b> |<b>Ziel: Azure SQL-Datenbank</b> |
-------------- |--------------------------------|
<b>Flatfiles (CSV- oder TSV-Format)</b> |<a href="#bulk-insert-sql-query">Bulk Insert SQL-Abfrage |
<b>Lokale SQL Server</b> | 1 <a href="#export-flat-file">exportieren in Flatfile<br> 2. <a href="#insert-tables-bcp">SQL Datenbank-Migrationsassistenten<br> 3. <a href="#db-migration">Datenbank zurück und Wiederherstellen<br> 4. <a href="#adf">Azure Data Factory |


## <a name="prereqs"></a>Erforderliche Komponenten
Die hier beschriebenen Verfahren benötigen Sie:

* Ein **Azure-Abonnement**. Wenn Sie nicht über ein Abonnement verfügen, können Sie eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)anmelden.
* Ein **Azure-Speicherkonto**. Zum Speichern der Daten in diesem Lernprogramm verwenden Sie ein Konto Azure-Speicher. Wenn ein Azure Storage-Konto haben, finden Sie im Artikel [erstellen ein Speicherkonto](storage-create-storage-account.md#create-a-storage-account) . Nachdem Sie das Speicherkonto erstellt haben, müssen Sie Zugriff auf den Speicher verwendet Konto abrufen. [Anzeigen, kopieren und Zugriffstasten Regenerieren Speicher](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)anzeigen
* Zugriff auf eine **SQL Azure-Datenbank**. Wenn Sie eine SQL Azure-Datenbank einrichten müssen, enthält [Erste Schritte mit Microsoft Azure SQL-Datenbank](../sql-database/sql-database-get-started.md) Informationen eine neue Instanz einer Azure SQL-Datenbank bereitstellen.
* Installierte und konfigurierte **Azure PowerShell** lokal. Eine Anleitung hierzu finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

**Daten**: die Migrationsprozesse sind gezeigt [NYC Taxi Dataset](http://chriswhong.com/open-data/foil_nyc_taxi/). NYC Taxi Dataset enthält Informationen über Daten und messen und steht, wie dieser Beitrag auf Azure BLOB-Speicher: [NYC Taxi Daten](http://www.andresmh.com/nyctaxitrips/). Ein Beispiel und Beschreibung dieser Dateien sind in [NYC Taxi Trips Datasetbeschreibung](machine-learning-data-science-process-sql-walkthrough.md#dataset)bereitgestellt.
 
Eigene Daten hier beschriebenen Verfahren anpassen oder gehen Sie entsprechend des NYC Taxi Datasets. Um NYC Taxi Dataset in der lokalen SQL Server-Datenbank hochzuladen, folgen Sie dem Verfahren in [Massendaten in SQL Server-Datenbank importieren](machine-learning-data-science-process-sql-walkthrough.md#dbload). Diese Anleitung gilt für SQL Server ein Azure Virtual Machine, aber das Verfahren zum Hochladen auf lokale SQL Server entspricht.


## <a name="file-to-azure-sql-database"></a>Daten aus einer Flatfile mit einer Azure SQL-Datenbank

Mit einer SQL Azure-Datenbank mit einer Masse einfügen SQL Abfrage können Daten in Flatfiles (CSV- oder TSV formatiert) verschoben werden.

### <a name="bulk-insert-sql-query"></a>Bulk Insert SQL-Abfrage

Die Schritte für die Prozedur mithilfe der Bulk einfügen SQL-Abfrage ähneln, die in den Abschnitten zum Verschieben von Daten aus einer Flatfile-Quelle zu SQL Server auf eine Azure-VM. Details finden Sie unter [Einfügen SQL Massenabfrage](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery).


##<a name="sql-on-prem-to-sazure-sql-database"></a>Verschieben von Daten von lokalen SQL Server mit einer Azure SQL-Datenbank

Wenn die Daten in einer lokalen SQL Server gespeichert ist, werden verschiedene Verfahren zum Verschieben von Daten in einer SQL Azure-Datenbank:

1. [Exportieren in Flatfile](#export-flat-file) 
2. [SQL Datenbank-Migrationsassistenten](#insert-tables-bcp)
3. [Datenbank zurück und Wiederherstellen](#db-migration)
4. [Azure Data Factory](#adf)

Für die ersten drei Schritte sind ähnlich wie diese Abschnitte bei [Verschieben von Daten in SQL Server auf einem virtuellen Computer Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) mit demselben Verfahren. Im folgenden sind Links zu den entsprechenden Abschnitten in diesem Thema bereitgestellt.

###<a name="export-flat-file"></a>Exportieren in Flatfile

Die Schritte zum Exportieren in eine Flatfile ähneln die in [Flatfile exportieren](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file).

###<a name="insert-tables-bcp"></a>SQL Datenbank-Migrationsassistenten

Die Schritte zur Verwendung der SQL-Datenbank-Migrationsassistent ähneln in [SQL Datenbank-Migrationsassistenten](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration)behandelt.

###<a name="db-migration"></a>Datenbank zurück und Wiederherstellen

Die Schritte zur Verwendung der Datenbank sichern und Wiederherstellen ähneln die Datenbank [Sichern und Wiederherstellen](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup).

###<a name="adf"></a>Azure Data Factory

Verfahren zum Verschieben von Daten in eine SQL Azure-Datenbank mit Azure Data Factory (ADF) wird im Thema [Verschieben von Daten von einem lokalen SQL Server SQL Azure mit Azure Daten](machine-learning-data-science-move-sql-azure-adf.md)bereitgestellt. In diesem Thema veranschaulicht wie Daten aus einer lokalen SQL Server-Datenbank mit einer Azure SQL-Datenbank über Azure BLOB-Speicher mit ADF. 

Erwägen Sie ADF, wenn Daten ständig im Hybrid-Szenario migriert werden, die lokale und Cloud-Ressourcen zugreift und die Daten Transaktionen oder geändert oder hinzugefügt, wenn migriert Geschäftslogik. ADF kann für die Planung und Überwachung von mit einfachen JSON-Skripts, die zur Verwaltung von der Verlagerung von Daten in regelmäßigen Abständen. ADF hat auch andere Funktionen wie Unterstützung für komplexe Vorgänge.




