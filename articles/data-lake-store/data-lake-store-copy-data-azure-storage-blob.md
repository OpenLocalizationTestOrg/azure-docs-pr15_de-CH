<properties
   pageTitle="Daten aus Azure Storage Blobs in Lake Datenspeicher | Microsoft Azure"
   description="Verwenden von AdlCopy Tool zum Kopieren von Daten von Azure Storage Blobs See Datenspeicher"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Daten von Azure Storage Blobs See Datenspeicher

> [AZURE.SELECTOR]
- [DistCp verwenden](data-lake-store-copy-data-wasb-distcp.md)
- [AdlCopy verwenden](data-lake-store-copy-data-azure-storage-blob.md)

Azure See Datenspeicher bietet ein Befehlszeilentool [AdlCopy](http://aka.ms/downloadadlcopy)Kopieren von Daten aus den folgenden Quellen:

* Von Azure-Speicher Blobs in Lake Datenspeicher. AdlCopy können Daten aus See Datenspeicher in Azure Storage Blobs kopieren.

* Zwischen zwei Konten Azure See Datenspeicher. 

Außerdem können Sie das Tool AdlCopy in zwei verschiedenen Modi:

* **Eigenständige**, wo mit dem Datenspeicher See Ressourcen für die Aufgabe verwendet.

* **Datenanalyse See Konto**, wo Ihr Benutzerkonto See Datenanalyse Einheiten zum Ausführen des Kopiervorgangs verwendet werden. Sie sollten diese Option verwenden, wenn Sie die Kopie in einer vorhersagbaren Weise Aufgaben suchen.

##<a name="prerequisites"></a>Erforderliche Komponenten

Vor diesem Artikel benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

- **Azure Storage Blobs** Container mit Daten.

- **Azure Data Lake Analytics ein (optional)** - Siehe [Einstieg in Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) Anleitung See Datenspeicher-Konto erstellen.

- **AdlCopy-Tool**. Installieren Sie das Tool AdlCopy von [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>Die Syntax des Tools AdlCopy

Verwenden Sie folgende Syntax funktioniert mit dem AdlCopy-Werkzeug

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern 

Die Parameter in der Syntax werden nachfolgend beschrieben:

| Option    | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------|------------|
| Quelle    | Gibt den Speicherort der Quelldaten im Blob Azure-Speicher. Die Quelle kann BLOB-Container ein Blob oder einem anderen Datenspeicher See Konto sein.                                                                                                                                                                                                                                                                                                 |
| Ziel      | Gibt das Ziel See Datenspeicher zu kopieren.                                                                                                                                                                                                                                                                                                                                                                |
| SourceKey | Gibt Speicher Zugriffstaste Quell-Blob Azure-Speicher. Dies ist erforderlich, wenn die Quelle ein BLOB-Container oder Blob.                                                                                                                                                                                                                                                                                                                                                 |
| Konto   | **Optional**. Verwenden Sie diese Option, wenn auf Azure Data Lake Analytics der Kopiervorgang ausgeführt werden soll. Wenn Sie die Option AccountType in der Syntax jedoch See Datenanalyse Konto nicht angeben, wird AdlCopy ein Standardkonto zum Ausführen des Auftrags verwendet. Auch wenn Sie diese Option verwenden, müssen Sie (Azure Storage Blob) Quelle und Ziel (Azure Datenspeicher See) als Datenquellen für Ihr Konto See Datenanalyse hinzufügen.  |
| Einheiten     |  Gibt die Anzahl der Datenanalyse See Einheiten für den Auftrag zum Kopieren verwendet werden. Diese Option ist erforderlich, wenn Sie die **AccountType** Option See Datenanalyse Konto angeben.
| Muster   | Gibt eine Regex-Muster, die die Blobs kopieren angibt. AdlCopy wird Groß-/Kleinschreibung entsprechen. Das Standardmuster verwendet, wenn kein Muster angegeben werden alle Elemente kopiert. Angeben mehrerer Dateimuster wird nicht unterstützt.                                                                                                                                                                                                                                                                                                                                               



## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>Kopieren von Daten aus einem Blob Azure Storage verwenden Sie AdlCopy (als Standalone)

1. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zum Verzeichnis, in dem AdlCopy, normalerweise installiert ist `%HOMEPATH%\Documents\adlcopy`.

2. Führen Sie den folgenden Befehl einen bestimmten Blob aus dem Quellcontainer in einem Datenspeicher See kopieren:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Zum Beispiel:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    
    >[AZURE.NOTE] Die obigen Syntax gibt die Datei in einen Ordner im Datenspeicher See Konto kopiert werden. AdlCopy Tool erstellt einen Ordner, wenn der angegebene Ordner nicht vorhanden ist.

    Sie werden aufgefordert die Anmeldeinformationen für den Azure-Abonnement unter dem Datenspeicher See-Konto haben. Sie sehen eine Ausgabe ähnlich der folgenden:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Sie können auch alle Blobs aus einem Container auf See Datenspeicher Konto mithilfe des folgenden Befehls kopieren:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Zum Beispiel:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>Verwenden Sie AdlCopy (als eigenständige) Kopieren von Daten aus einem anderen Datenspeicher See Konto

AdlCopy können Sie Daten zwischen zwei See Datenspeicher Konten kopieren.

1. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zum Verzeichnis, in dem AdlCopy, normalerweise installiert ist `%HOMEPATH%\Documents\adlcopy`.

2. Führen Sie den folgenden Befehl auf eine bestimmte Datei von einem Datenspeicher See Konto in eine andere kopieren.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Zum Beispiel:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

    >[AZURE.NOTE] Die obigen Syntax gibt die Datei in einen Ordner im Datenspeicher See Konto Ziel kopiert werden. AdlCopy Tool erstellt einen Ordner, wenn der angegebene Ordner nicht vorhanden ist.

    Sie werden aufgefordert die Anmeldeinformationen für den Azure-Abonnement unter dem Datenspeicher See-Konto haben. Sie sehen eine Ausgabe ähnlich der folgenden:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Der folgende Befehl kopiert alle Dateien aus einem bestimmten Ordner in der Quelle See Datenspeicher Konto in einen Ordner im Ziel See Datenspeicher Konto.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>Mit der AdlCopy (See Datenanalyse Berücksichtigung) Daten kopieren

Ihr Konto See Datenanalyse können zum Ausführen des Auftrags AdlCopy, um Daten von Azure-Speicher Blobs See Datenspeicher zu kopieren. Sie normalerweise verwenden diese Option, wenn die Daten verschoben werden im Bereich Gigabyte und Terabyte und bessere und vorhersagbare Performance-Durchsatz soll.

Um Ihr Konto See Datenanalyse AdlCopy verwenden eine Azure Storage BLOB kopiert, muss die Quelle (Azure Storage Blob) als Datenquelle für Ihr Konto See Datenanalyse hinzugefügt werden. Informationen Ihres Kontos See Datenanalyse zusätzliche Datenquellen hinzugefügt finden Sie unter [Konto verwalten See Datenanalyse Datenquellen](..//data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

>[AZURE.NOTE] Kopieren von einem Azure See Datenspeicher als Quelle ein See Datenanalyse Konto müssen nicht See Datenanalyse Konto Konto See Datenspeicher zugeordnet werden. Die Anforderung Quellspeicher See Datenanalyse-Konto zugeordnet ist, nur, wenn die Quelle ein Azure Storage-Konto ist.

Führen Sie den folgenden Befehl aus einem Blob Azure Storage See Datenspeicher-Konto See Datenanalyse Konto kopieren:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Zum Beispiel:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2


Ebenso führen Sie den folgenden Befehl aus einem Blob Azure Storage See Datenspeicher-Konto See Datenanalyse Konto kopieren:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Verwenden Sie AdlCopy Kopieren von Daten mit Mustervergleich

In diesem Abschnitt lernen Sie Verwendung von AdlCopy zum Kopieren von Daten aus einer Quelle (in unserem Beispiel verwenden wir Azure Storage Blob) auf ein Konto mit Mustervergleich See Datenspeicher. Folgendermaßen können Sie alle Dateien mit der Erweiterung CSV aus dem Blob Quelle in das Ziel kopiert.

1. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zum Verzeichnis, in dem AdlCopy, normalerweise installiert ist `%HOMEPATH%\Documents\adlcopy`.

2. Führen Sie den folgenden Befehl zum Kopieren aller Dateien mit der Erweiterung *.csv aus bestimmten Blob aus dem Quellcontainer in einem Datenspeicher See:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Zum Beispiel:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

    
## <a name="billing"></a>Abrechnung

* Verwenden Sie das Tool AdlCopy als eigenständige werden Sie Ausgang Kosten Daten, abgerechnet ist die Quelle Azure Storage-Konto nicht in derselben Region als Datenspeicher See.

* Verwenden Sie das Tool AdlCopy mit Ihrem Konto Datenanalyse See gelten standard [Verrechnungssätze See Datenanalyse](https://azure.microsoft.com/pricing/details/data-lake-analytics/) .

## <a name="considerations-for-using-adlcopy"></a>Aspekte der Verwendung von AdlCopy

* AdlCopy (für Version 1.0.5) unterstützt Daten aus Quellen, die gemeinsam mehr als tausend Dateien und Ordner. Wenn ein großes Dataset kopieren Probleme können Sie jedoch Dateien und Ordner in verschiedenen Unterverzeichnissen verteilen und stattdessen den Pfad auf die Unterordner als Quelle.

## <a name="next-steps"></a>Nächste Schritte

- [Sichere Daten im Datenspeicher See](data-lake-store-secure-data.md)
- [Verwenden Sie Azure Data Lake Analytics See Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
