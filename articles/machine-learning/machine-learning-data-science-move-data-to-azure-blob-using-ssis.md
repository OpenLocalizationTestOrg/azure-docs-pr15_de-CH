<properties
    pageTitle="Verschieben von Daten zu oder von Azure BLOB-Speicher mithilfe von SSIS-Connectors | Microsoft Azure"
    description="Verschieben Sie Daten an oder von Azure BLOB-Speicher mithilfe von SSIS-Connectors."
    services="machine-learning,storage"
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

# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Verschieben von Daten zu oder von Azure BLOB-Speicher mithilfe von SSIS-connectors

[SQL Server Integration Services Feature Pack für Azure](https://msdn.microsoft.com/library/mt146770.aspx) stellt Komponenten Verbindung zu Azure Datenübertragung zwischen Azure und lokalen Datenquellen und Prozessdaten in Azure gespeichert.

Hinweise auf Daten und/oder von Azure BLOB-Speicher verschoben werden hier verknüpft:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


Nach Kunden für lokale Daten in der Cloud verschoben haben, können sie über alle Azure Service nutzen die volle Leistungsfähigkeit der Azure-Technologien-Suite zugreifen. Er kann, z. B. in Azure maschinelles lernen oder einen HDInsight-Cluster verwendet werden.

Werden in der Regel der erste Schritt für [SQL](machine-learning-data-science-process-sql-walkthrough.md) und [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) Exemplarische Vorgehensweisen.

Kanonische Szenarien, mit denen SSIS geschäftlichen Bedürfnisse häufig in hybridszenarien-Integration, siehe [bei SQL Server Integration Services Feature Pack für Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) -Blog.

> [AZURE.NOTE] Eine vollständige Einführung in Azure BLOB-Speicher finden Sie in [Azure Blob Grundlagen](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure BLOB-Dienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Artikel beschriebenen Aufgaben, benötigen Sie ein Azure-Abonnement und ein Azure Storage-Konto eingerichtet. Sie benötigen den Azure-Speicher Konto und Key upload oder download von Daten.

- Einrichten einer **Azure-Abonnement**finden Sie [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/).

- Anleitung zum Erstellen ein **Speicherkonto** und Konto und wichtige Informationen anzeigen Sie [zum Azure-Speicherkonten](../storage/storage-create-storage-account.md)


Um die **SSIS-Connectors**verwenden, müssen Sie Folgendes herunterladen:

- **SQL Server 2014 oder 2016 Standard (oder höher)**: Installation beinhaltet SQL Server Integration Services.
- **Microsoft SQL Server 2014 oder 2016 Integration Services Feature Pack für Azure**: diese heruntergeladen werden, bzw. von [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) und [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) -Seiten.

> [AZURE.NOTE] SSIS ist mit SQL Server installiert, jedoch nicht in der Express-Version enthalten. Informationen über welche Anwendung in verschiedenen Editionen von SQL Server enthalten sind finden Sie unter [SQL Server-Editionen](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)

Schulungsmaterial für SSIS finden Sie unter [Hände auf Schulung für SSIS](http://www.microsoft.com/download/details.aspx?id=20766)

Mithilfe von Informationen wie oben ausgeführt SSI zu extrahieren, Transformieren und laden (ETL) Pakete Siehe einfache [SSIS-Lernprogramm: Erstellen eines einfachen Pakets ETL-](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>NYC Taxi Dataset herunterladen  
Beispiel hier öffentlich Dataset - [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) Dataset verwenden. Das Dataset enthält ca. 173 Millionen taxifahrten in New York im Jahr 2013. Es gibt zwei Arten von Daten: Reise werden Daten und Preis. Gibt eine Datei für jeden Monat, haben wir 24 Dateien in allen, die jeweils ca. 2 GB nicht komprimiert ist.


## <a name="upload-data-to-azure-blob-storage"></a>Upload von Daten in Azure BLOB-Speicher
Um Daten mithilfe von SSIS Feature Pack von lokalen Azure BLOB-Speicher zu verschieben, verwenden Sie eine Instanz der [**Azure Blob hochladen Aufgabe**](https://msdn.microsoft.com/library/mt146776.aspx)hier:

![Konfigurieren-Daten-Science-vm](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)


Die Parameter, die vom Task verwendet werden hier beschrieben:


Feld|Beschreibung|
----------------------|----------------|
**AzureStorageConnection**|Gibt einen vorhandenen Azure Storage Verbindungs-Manager oder eine neue erstellt, die auf ein Konto Azure-Speicher, der auf dem BLOB-Dateien befinden.|
**BlobContainer**|Gibt den Namen der blobcontainer, der die hochgeladenen Dateien als Blobs.|
**BlobDirectory**|Gibt das BLOB-Verzeichnis als ein die hochgeladene Datei Speicherort. Das BLOB-Verzeichnis ist eine virtuelle hierarchische Struktur. Wenn das Blob bereits vorhanden ist, ia ersetzte.|
**LocalDirectory**|Gibt das lokale Verzeichnis mit den Dateien hochgeladen werden.|
**Dateiname**|Gibt einen Namensfilter zur Auswahl der Dateien mit dem angegebenen Namen. Z. B. My Sheet\*xls\* enthält Dateien wie MySheet001.xls und MySheetABC.xlsx|
**TimeRangeFrom-TimeRangeTo**|Gibt einen Bereichsfilter Zeit. Dateien nach *TimeRangeFrom* geändert und vor *TimeRangeTo* einbezogen werden.|


> [AZURE.NOTE] **AzureStorageConnection** -Anmeldeinformationen müssen korrekt sein und **BlobContainer** muss vorhanden sein, bevor die Übertragung versucht wird.

## <a name="download-data-from-azure-blob-storage"></a>Herunterladen von Daten von Azure BLOB-Speicher

Download von Daten von Azure BLOB-Speicher mit SSIS lokale verwenden Sie eine Instanz der [Azure Blob hochladen Aufgabe](https://msdn.microsoft.com/library/mt146779.aspx).

##<a name="more-advanced-ssis-azure-scenarios"></a>Erweiterte Azure SSIS-Szenarios
Wir Beachten Sie, dass SSIS-Feature Pack für komplexere Abläufe von verpackungsaufgaben zusammen verarbeitet werden können. Beispielsweise können BLOB-Daten direkt in einen HDInsight-Cluster feed, deren Ausgabe in ein Blob und anschließend lokale Speicher heruntergeladen werden konnte. SSIS können Struktur und Schwein Aufträge auf ein HDInsight Cluster mit zusätzlichen SSIS-Connectors ausführen:

- Gehen Sie zum Ausführen eines Skripts Struktur auf eine Azure HDInsight Cluster mit SSIS [Azure HDInsight Struktur beschrieben](https://msdn.microsoft.com/library/mt146771.aspx).
- Führen Sie ein Schwein Skript in einem Cluster Azure HDInsight mit SSIS mit [Azure HDInsight Schwein Aufgabe](https://msdn.microsoft.com/library/mt146781.aspx).
