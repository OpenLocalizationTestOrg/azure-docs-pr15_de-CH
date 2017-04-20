<properties 
   pageTitle="Erste Schritte mit Azure Data Lake Analytics über Azure-Befehlszeilenschnittstelle | Microsoft Azure" 
   description="Informationen Sie zum Datenspeicher See Konto erstellen, erstellen Sie einen See Datenanalyse Druckauftrag U SQL Azure-Befehlszeilenschnittstelle verwenden und senden Sie den Auftrag. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Lernprogramm: Erste Schritte mit Azure Data Lake Analytics mit Azure-Befehlszeilenschnittstelle (CLI)

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Informationen Sie zum Verwenden von Azure CLI Azure Data Lake Analytics Konten, [U-SQL](data-lake-analytics-u-sql-get-started.md)Data Lake Analytics Aufträge definieren und Aufträge auf See Datenanalyse. Weitere Informationen über See Datenanalyse Übersicht [Azure Data Lake Analytics](data-lake-analytics-overview.md).

In diesem Lernprogramm entwickeln Sie einen Auftrag, der liest eine Wertedatei (TSV) getrennt und konvertiert ihn in eine Datei mit kommagetrennten Werten (CSV). Gleiche Lernprogramm mit unterstützten Tools klicken Sie, die Registerkarten oben in diesem Abschnitt.

##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).
- **Azure CLI**. Siehe [Installieren und Konfigurieren von Azure CLI](../xplat-cli-install.md).
    - Downloaden Sie und installieren Sie der **Vorabversion** [Azure CLI-Tools](https://github.com/MicrosoftBigData/AzureDataLake/releases) Abschließen dieser Demo.
- **Authentifizierung**mit dem folgenden Befehl:

        azure login
    Weitere Informationen zu Authentifizierung mit einem Arbeits- oder Schulcomputer Konto finden Sie unter [mit Azure-Abonnement von Azure-CLI](../xplat-cli-connect.md).
- **Wechsel zum Azure-Ressourcen-Manager-Modus**, mit dem folgenden Befehl:

        azure config mode arm
        
## <a name="create-data-lake-analytics-account"></a>Datenanalyse See-Konto erstellen

Datenanalyse See Konto müssen vor dem Ausführen alle Aufträge verwendet werden. Zum Erstellen eines Kontos Datenanalyse See müssen Sie Folgendes angeben:

- **Azure-Ressourcengruppe**: See Datenanalyse ein Konto in einer Azure-Ressourcengruppe erstellt werden muss. [Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md) können Sie die Ressourcen in der Anwendung als Gruppe arbeiten. Bereitstellen, aktualisieren, oder löschen alle Ressourcen für die Anwendung in einer einzigen koordinierten Operation.  

    Ressourcengruppen in Ihrem Abonnement aufgelistet werden:
    
        azure group list 
    
    So erstellen Sie eine neue Ressourcengruppe

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Data Lake Analytics-Kontoname**
- **Lage**: eine Azure Rechenzentren, die Datenanalyse See unterstützt.
- **Standardmäßig Daten See Konto**: jedes Konto See Datenanalyse ist ein Standardkonto Daten See.

    Liste Daten See-Konto:
    
        azure datalake store account list

    So erstellen Sie ein neues Konto Daten See

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] Daten See Kontoname darf nur Kleinbuchstaben und Zahlen enthalten.



**Erstellen eines Kontos See Datenanalyse**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![Datenanalyse See anzeigen Konto](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)

> [AZURE.NOTE] Datenanalyse See Kontoname darf nur Kleinbuchstaben und Zahlen enthalten.


## <a name="upload-data-to-data-lake-store"></a>Upload von Daten auf See Datenspeicher

In diesem Lernprogramm werden Sie einige Protokolle Suche verarbeitet.  Suchprotokoll kann Daten See Informationsspeicher oder Azure BLOB-Speicher gespeichert werden. 

Azure-Portal bietet eine Benutzeroberfläche für das Kopieren von einigen Beispieldateien in das Standardkonto Daten See, einschließlich eine Protokolldatei suchen. Siehe [Vorbereiten der Quelldaten](data-lake-analytics-get-started-portal.md#prepare-source-data) Standardkonto See Datenspeicher die Daten hochladen.

Zum Hochladen von Dateien über die Befehlszeilenschnittstelle verwenden Sie den folgenden Befehl ein:

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

Datenanalyse See können auch Azure BLOB-Speicher zugreifen.  Upload von Daten in Azure BLOB-Speicher, finden Sie unter [Verwendung der Azure-CLI mit Azure-Speicher](../storage/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Datenanalyse See Aufträge

Datenanalyse See Aufträge werden in der U-SQL-Sprache geschrieben. Über U-SQL finden Sie unter [Erste Schritte mit U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md) und [U-SQL Referenzhandbuch](http://go.microsoft.com/fwlink/?LinkId=691348).

**Erstellen eines Skripts See Datenanalyse Auftrag**

- Eine Datei mit folgenden U-SQL-Skript erstellen und Speichern der Datei auf Ihrer Arbeitsstation:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Dieses U-SQL-Skript liest die Quelldatei mit **Extractors.Tsv()**und erstellt eine CSV-Datei mit **Outputters.Csv()**. 
    
    Ändern Sie nicht die beiden Pfade, sofern Sie die Quelldatei in einen anderen Speicherort kopieren.  Datenanalyse See erstellt den Ausgabeordner existiert nicht.
    
    Es ist einfacher, relative Pfade für Dateien im See Konten Daten. Sie können auch absolute Pfade.  Zum Beispiel 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Zugriff auf Dateien im verknüpften Speicherkonten müssen Sie absolute Pfade verwenden.  Die Syntax für Dateien verknüpfte Azure Storage-Konto lautet:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure BLOB-Container mit öffentlichen Blobs oder öffentlichen Container Berechtigungen werden derzeit nicht unterstützt.      

    
**Den Auftrag senden**


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"
    
    
Die folgenden Befehle können Aufträge auflisten, Auftragsdetails und Aufträge abbrechen verwendet werden:

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

Nachdem der Auftrag abgeschlossen ist, können die folgenden Cmdlets verwenden, um die Datei und Laden Sie die Datei:
    
    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## <a name="see-also"></a>Siehe auch

- Klicken Sie das gleiche Lernprogramm mit anderen Tools Registerkarte Selektoren oben auf der Seite.
- Eine komplexe Abfrage finden Sie unter [Analysieren Website Protokolle mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Finden Sie zunächst Anwendungsentwicklung U-SQL [entwickeln U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL finden Sie unter [Einstieg in Azure Data Lake Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md).
- Management-Aufgaben finden Sie unter [Verwalten von Azure See Datenanalyse mithilfe von Azure-Portal](data-lake-analytics-manage-use-portal.md).
- Um einen Überblick der Datenanalyse See Übersicht [Azure Data Lake Analytics](data-lake-analytics-overview.md).

