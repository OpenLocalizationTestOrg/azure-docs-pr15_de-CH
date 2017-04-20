<properties 
    pageTitle="Lernprogramm: Erstellen eine Pipeline mit Azure-Portal mit Kopieren | Microsoft Azure" 
    description="In diesem Lernprogramm erstellen Sie eine Azure Data Factory-Pipeline mit einer Kopie von mit dem Werk in Azure-Portal." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/16/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-portal"></a>Lernprogramm: Erstellen einer Pipeline mit Kopie mit Azure-portal
> [AZURE.SELECTOR]
- [Übersicht und Komponenten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Assistent zum Kopieren von](data-factory-copy-data-wizard-tutorial.md)
- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure-Ressourcen-Manager-Vorlage](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)



In diesem Lernprogramm wird das Erstellen und Überwachen einer Azure Data Factory mithilfe des Azure-Portals veranschaulicht. Pipeline in der Data Factory verwendet eine Aktivität kopieren Daten von Azure BLOB-Speicher in Azure SQL-Datenbank kopiert.

Hier sind die Schritte, die Sie als Teil dieses Lernprogramms ausführen:

Schritt | Beschreibung
-----| -----------
[Erstellen einer Azure Data Factory](#create-data-factory) | In diesem Schritt erstellen Sie eine **ADFTutorialDataFactory**namens Azure Data Factory.  
[Erstellen von verknüpften Diensten](#create-linked-services) | In diesem Schritt erstellen Sie zwei verknüpften Dienste: **AzureStorageLinkedService** und **AzureSqlLinkedService**. <br/><br/>Der AzureStorageLinkedService verbindet den Azure-Speicher und AzureSqlLinkedService SQL Azure-Datenbank mit der ADFTutorialDataFactory verknüpft. Die Eingabedaten werden für die Pipeline in einem BLOB-Container in den Azure BLOB-Speicher und Ausgabe befindet gespeichert in einer Tabelle in SQL Azure-Datenbank. Daher hinzugefügt diese zwei Datenspeicher als verknüpfte Dienste Data Factory.      
[Erstellen von Eingabe- und Ausgabe-datasets](#create-datasets) | Im vorherigen Schritt erstellt Sie verknüpfte Diensten für Datenspeicher verweisen, die Eingabe/Ausgabe-Daten enthalten. In diesem Schritt definieren Sie zwei Datasets - **InputDataset** und **OutputDataset** -, die e/a-Daten darstellen, die im Datenspeicher gespeichert. <br/><br/>Für die InputDataset geben den blobcontainer, der ein mit den Quelldaten und der OutputDataset BLOB, die SQL-Tabelle der Ausgabe Daten angegeben werden. Sie geben auch andere Eigenschaften wie Struktur, Verfügbarkeit und Richtlinien. 
[Erstellen Sie eine pipeline](#create-pipeline) | In diesem Schritt erstellen Sie eine Rohrleitung mit dem Namen **ADFTutorialPipeline** in der ADFTutorialDataFactory. <br/><br/>Die Eingabe kopiert Daten aus dem Azure, SQL Azure Ausgabetabelle blob Pipeline hinzugefügt **Kopieraktivität** . Der Kopie Aktivität Datenübertragungen in Azure Data Factory. Es versorgt von einem global zur Verfügung, die Daten zwischen verschiedenen Datenspeichern eine sichere, zuverlässige und skalierbare Weise kopieren können. Siehe [Datenaktivitäten](data-factory-data-movement-activities.md) Weitere Details zur Aktivität kopieren. 
[Monitor-pipeline](#monitor-pipeline) | In diesem Schritt werden die Segmente der Eingabe- und Tabellen mithilfe Azure-Portal überwachen.

## <a name="prerequisites"></a>Erforderliche Komponenten 
In dem [Lernprogramm](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) Artikel vor diesem Lernprogramm aufgeführten erforderlichen Komponenten abgeschlossen.

## <a name="create-data-factory"></a>Data Factory erstellen
In diesem Schritt verwenden Sie Azure-Portal eine Azure Data Factory namens **ADFTutorialDataFactory**erstellt.

1.  Nach dem [Azure-Portal](https://portal.azure.com/)Anmelden klicken Sie auf **neu**, wählen Sie **Intelligenz und Analyse**und **Data Factory**auf. 

    ![Neu -> DataFactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)  

6. Im **neuen Data Factory** -Blade:
    1. Geben Sie den **Namen** **ADFTutorialDataFactory** . 
    
        ![Neue Data Factory blade](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)

        Der Name der Azure Data Factory muss **global eindeutig**sein. Wenn Sie die Fehlermeldung erhalten, ändern Sie den Namen der Data Factory (z. B. YournameADFTutorialDataFactory) und erstellen erneut. Benennungskonventionen für Artefakte Data Factory finden Sie [Data Factory - Namenskonventionen](data-factory-naming-rules.md) .
    
            Data factory name “ADFTutorialDataFactory” is not available  
     
        ![Data Factory Name nicht verfügbar](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
    2. Wählen Sie Ihre Azure- **Abonnement**.
    3. Gehen Sie für die Ressourcengruppe folgendermaßen vor:
        1. Wählen Sie **vorhandene**und eine vorhandene Ressourcengruppe aus der Dropdown-Liste. 
        2. Wählen Sie **neu erstellen**, und geben Sie den Namen einer Ressourcengruppe.   
    
            Einige der Schritte in diesem Lernprogramm wird davon ausgegangen, dass Sie den Namen: **ADFTutorialResourceGroup** für die Ressourcengruppe. Zu Ressourcengruppen finden Sie unter [Using Ressourcengruppen Azure Ressourcen verwalten](../azure-resource-manager/resource-group-overview.md).  
    4. **Speicherort** der Data Factory auswählen. Nur Bereiche vom Dienst Data Factory unterstützt werden in der Dropdown-Liste angezeigt.
    5. Wählen Sie **an Startmenü anheften**.     
    6. Klicken Sie auf **Erstellen**.

        > [AZURE.IMPORTANT] Um Data Factory-Instanzen erstellen, muss ein Mitglied der [Data Factory](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) Beitragendenrolle auf Abonnement-Ressource sein.
        >  
        >  Der Name der Factory Daten möglicherweise als DNS-Name in der Zukunft und damit sichtbar öffentlich registriert werden.              
9.  Klicken Sie Status/Benachrichtigung auf der Symbolleiste auf das Glockensymbol. 

    ![Benachrichtigung](./media/data-factory-copy-activity-tutorial-using-azure-portal/Notifications.png) 
10. Nach Abschluss die Erstellung Siehe Blatt **Data Factory** , wie in der Abbildung dargestellt.

    ![Data Factory-Homepage](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten
Verknüpften Dienste verknüpfen Datenspeicher oder Dienste einer Azure Data Factory zu berechnen. Siehe [unterstützte Daten](data-factory-data-movement-activities.md##supported-data-stores-and-formats) für alle Datenquellen und Datensenken unterstützt durch die Kopie. Finden Sie unter Liste der Compute-Dienste von Data Factory unterstützt [verknüpfte Dienste berechnet](data-factory-compute-linked-services.md) . In diesem Lernprogramm verwenden Sie keine Computing-Service. 

In diesem Schritt erstellen Sie zwei verknüpften Dienste: **AzureStorageLinkedService** und **AzureSqlLinkedService**. AzureStorageLinkedService Service-Links ein Azure Storage-Konto verknüpft und AzureSqlLinkedService eine Azure SQL-Datenbank mit der **ADFTutorialDataFactory**verknüpft. Erstellen eine Pipeline später in diesem Lernprogramm, das Daten von einem BLOB-Container in AzureStorageLinkedService mit einer SQL-Tabelle in AzureSqlLinkedService kopiert.

### <a name="create-a-linked-service-for-the-azure-storage-account"></a>Erstellen Sie verknüpften Dienst für Azure Storage-Konto
1.  Klicken Sie auf Blatt **Data Factory** **Autor und** Kachel einzuführen **Editor** für Data Factory.

    ![Erstellen und Bereitstellen der Kachel](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
5. Klicken Sie im **Editor**auf die Schaltfläche **Speichern neuer Daten** und wählen Sie **Azure-Speicher** aus dem Dropdown Menü aus. JSON-Vorlage zum Erstellen von Azure-Speicher verknüpft-Dienst im rechten Fensterbereich sollte angezeigt werden. 

    ![Editor neue Shop Schaltfläche](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
6. Ersetzen Sie `<accountname>` und `<accountkey>` mit dem Kontonamen Kontowerte für das Konto Azure-Speicher. 

    ![Editor-BLOB-Speicher JSON](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png) 
6. Klicken Sie auf der Symbolleiste **Bereitstellen** . Die bereitgestellte **AzureStorageLinkedService** in der Strukturansicht sollte jetzt angezeigt werden. 

    ![Editor-BLOB-Speicher bereitstellen](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

> [AZURE.NOTE]
> Details über JSON-Eigenschaften finden Sie unter [Verschieben von Daten vom und zum Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) .

### <a name="create-a-linked-service-for-the-azure-sql-database"></a>Erstellen Sie einen verknüpften Dienst für Azure SQL-Datenbank
1. **Data Factory-Editor**klicken Sie auf die Schaltfläche **Speichern neuer Daten** und wählen Sie aus dem Dropdown Menü **Azure SQL-Datenbank** . JSON-Vorlage zum Erstellen des verknüpften Azure SQL-Dienstes im rechten Fensterbereich sollte angezeigt werden.
2. Ersetzen Sie `<servername>`, `<databasename>`, `<username>@<servername>`, und `<password>` mit Namen der Azure SQL Server, Datenbank-Benutzerkonto und Kennwort. 
3. Klicken Sie auf der Symbolleiste erstellen und Bereitstellen der **AzureSqlLinkedService** **Bereitstellen** .
4. Bestätigen Sie, dass **AzureSqlLinkedService** in der Strukturansicht angezeigt. 

> [AZURE.NOTE]
> Details über JSON-Eigenschaften finden Sie unter [Verschieben von Daten aus und in Azure SQL-Datenbank](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .

## <a name="create-datasets"></a>Datasets erstellen
Im vorherigen Schritt erstellten verknüpfte Diensten Data Factory ein Speicherkonto Azure und SQL Azure-Datenbank verknüpfen, **AzureStorageLinkedService** und **AzureSqlLinkedService** : **ADFTutorialDataFactory**. In diesem Schritt definieren Sie zwei Datasets - **InputDataset** und **OutputDataset** -, die e/a-Daten darstellen, die durch AzureStorageLinkedService und AzureSqlLinkedService verwiesen Datenspeichern gespeichert ist. Für InputDataset Festlegen des Blob-Containers, der ein mit den Quelldaten und OutputDataset BLOB, SQL-Tabelle der Ausgabe Daten angegeben werden. 

### <a name="create-input-dataset"></a>Eingabe-Dataset erstellen 
In diesem Schritt erstellen Sie ein Dataset mit dem Namen **InputDataset** auf BLOB-Container in den Azure-Speicher als Service **AzureStorageLinkedService** verknüpft.

1. Klicken Sie im **Editor** für die Daten auf **... Weitere**und anschließend auf **Neues Dataset**, **Azure BLOB-Speicher** aus dem Dropdown Menü. 

    ![Neues Dataset-Menü](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. Der folgende Codeausschnitt JSON ersetzen Sie JSON im rechten: 

        {
          "name": "InputDataset",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "fileName": "emp.txt",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }
        
     Beachten Sie folgende Punkte: 
    
    - DataSet- **Typ** wird auf **AzureBlob**festgelegt.
    - **Nameverknüpfterdienst** wird auf **AzureStorageLinkedService**festgelegt. Dieser verknüpften Dienst erstellt in Schritt2.
    - **Adftutorial** Container **FolderPath** fest. Sie können auch den Namen eines Blob innerhalb des Ordners mit der **FileName** -Eigenschaft. Da Sie nicht den Namen des BLOBs angeben, gilt die Daten alle Blobs im Container als eingegebenen Daten.  
    - **TextFormat** ist **Typ** fest.
    - Es gibt zwei Felder in der Textdatei – **Vorname** und **Nachname** , getrennt durch ein Komma (**ColumnDelimiter**) 
    - **Verfügbarkeit** soll **stündlich** (** **Stunde** festgelegt ist** und **Intervall** auf **1**festgelegt ist). Daher sucht Data Factory Eingabedaten stündlich im Stammordner der BLOB-Container (**Adftutorial**) angegebenen. 
    
    Wenn Sie einen **Dateinamen** für **Eingabedatasets** angeben, gelten alle Dateien/Blobs aus dem Eingabeordner (**FolderPath**) als Eingaben. Bei Angabe ein Dateinamens in JSON gilt nur angegebene Datei/Blob Asn-Eingabe.
 
    Wenn Sie einen **Dateinamen** für die **Ausgabetabelle**nicht angeben, werden die generierten Dateien im **Ordnerpfad** im folgenden Format benannt: Daten. &lt;Guid\&Gt;. TXT (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    Um **Ordnerpfad** und **Dateiname** dynamisch anhand des **SliceStart** festzulegen, verwenden Sie die **PartitionedBy** -Eigenschaft. Im folgenden Beispiel FolderPath verwendet, Jahr, Monat und Tag SliceStart (Startzeit des Segments verarbeitet) und Dateiname verwendet Stunde aus der SliceStart. Beispielsweise ein Segment für 2016 entsteht-09-20T08:00:00, Wikidatagateway/Wikisampledataout/2016/09/20 Ordnername festgelegt ist und der Dateiname wird auf 08.csv festgelegt. 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
            ],
2. Klicken Sie auf der Symbolleiste erstellen und Bereitstellen von **InputDataset** Dataset **Bereitstellen** . Bestätigen Sie, dass die **InputDataset** in der Strukturansicht angezeigt.

> [AZURE.NOTE]
> Details über JSON-Eigenschaften finden Sie unter [Verschieben von Daten vom und zum Azure Blob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) .

### <a name="create-output-dataset"></a>Ausgabedataset erstellen
In diesem Teil der Schritt erstellen Sie eine ausgabedataset mit dem Namen **OutputDataset**. Dieses Dataset verweist auf eine SQL-Tabelle in der SQL Azure-Datenbank durch **AzureSqlLinkedService**dargestellt. 

1. Klicken Sie im **Editor** für die Daten auf **... Weitere**und anschließend auf **Neues Dataset**, **SQL Azure** aus dem Dropdown Menü. 
2. Der folgende Codeausschnitt JSON ersetzen Sie JSON im rechten:

        {
          "name": "OutputDataset",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }
        
     Beachten Sie folgende Punkte: 
    
    - DataSet- **Typ** wird auf **AzureSQLTable**festgelegt.
    - **Nameverknüpfterdienst** wird auf **AzureSqlLinkedService** gesetzt (Sie diesen verknüpften Dienst in Schritt2 erstellten).
    - **emp** **TableName** fest.
    - Gibt es drei Spalten- **ID**, **Vorname**und **Nachname** – der emp-Tabelle in der Datenbank. ID ist eine Identitätsspalte, müssen Sie hier nur **Vorname** und **Nachname** angeben.
    - Die **Verfügbarkeit** wird **stündlich** (**Häufigkeit** **Stunde** und **Intervall** auf **1**festgelegt) fest.  Data Factory-Dienst generiert eine Ausgabe Datenslice stündlich in der Tabelle **emp** Azure SQL-Datenbank.

3. Klicken Sie auf der Symbolleiste erstellen und Bereitstellen von **OutputDataset** Dataset **Bereitstellen** . Bestätigen Sie, dass die **OutputDataset** in der Strukturansicht angezeigt. 

> [AZURE.NOTE]
> Details über JSON-Eigenschaften finden Sie unter [Verschieben von Daten aus und in Azure SQL-Datenbank](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .

## <a name="create-pipeline"></a>Pipeline erstellen
In diesem Schritt erstellen Sie eine Rohrleitung mit einer **Kopieraktivität** , die **InputDataset** als Eingabe verwendet und **OutputDataset** ausgegeben.

1. Klicken Sie im **Editor** für die Daten auf **... Weitere**, und klicken Sie auf **neue**. Alternativ können Sie **Rohrleitungen** in der Strukturansicht klicken und klicken Sie auf **neue**.
2. Der folgende Codeausschnitt JSON ersetzen Sie JSON im rechten: 
        
        {
          "name": "ADFTutorialPipeline",
          "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
              {
                "name": "CopyFromBlobToSQL",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "InputDataset"
                  }
                ],
                "outputs": [
                  {
                    "name": "OutputDataset"
                  }
                ],
                "typeProperties": {
                  "source": {
                    "type": "BlobSource"
                  },
                  "sink": {
                    "type": "SqlSink",
                    "writeBatchSize": 10000,
                    "writeBatchTimeout": "60:00:00"
                  }
                },
                "Policy": {
                  "concurrency": 1,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2016-07-12T00:00:00Z",
            "end": "2016-07-13T00:00:00Z"
          }
        } 

    Beachten Sie folgende Punkte:

    - Im Abschnitt Aktivitäten ist nur eine Aktivität vom **Typ** **Kopieren**festgelegt ist.
    - Eingabe für die Aktivität, **InputDataset** und Ausgabe für die Aktivität ist auf **OutputDataset**festgelegt.
    - In Abschnitt **TypeProperties** **BlobSource** als Quelltyp angegeben und **SqlSink** Senke Dateityp angegeben.

    Ersetzen Sie den Wert der **start** -Eigenschaft mit dem aktuellen Tag und **Ende** mit am nächsten Tag. Sie geben nur den Datumsteil und Zeitteil der Zeitpunkt überspringen. Beispielsweise "2016-02-03" entspricht "2016-02-03T00:00:00Z"
    
    Start und Ende Datumswerte müssen im [ISO-Format](http://en.wikipedia.org/wiki/ISO_8601). Beispiel: 2016-10-14T16:32:41Z. **Die Endzeit** ist optional, aber wir in diesem Lernprogramm verwenden. 
    
    Wenn kein Wert für die **End** -Eigenschaft angeben, wird es als "**Start + 48 Stunden**" berechnet. Führen Sie die Pipeline unbegrenzt Geben Sie **9999-09-09** als Wert für die **End** -Eigenschaft.
    
    Im vorherigen Beispiel gibt 24 Datenslices jedes Datenslice stündlich erzeugt wird.
    
4. Klicken Sie auf der Symbolleiste erstellen und Bereitstellen der **ADFTutorialPipeline** **Bereitstellen** . Bestätigen Sie, dass die Rohrleitung in der Strukturansicht angezeigt. 
5. Schließen Sie durch Klicken auf **X**-Blade **Editor** jetzt. Klicken Sie auf **X** erneut aus, um **Data Factory** -Homepage finden in **ADFTutorialDataFactory**.

**Herzlichen Glückwunsch!** Erfolgreich erstellt eine Azure Data Factory, verknüpften Diensten, Tabellen und einer Pipeline und die Pipeline geplant.   
 
### <a name="view-the-data-factory-in-a-diagram-view"></a>Factory Daten in einer Ansicht anzeigen 
1. Klicken Sie im Blatt **Data Factory** auf **Diagramm**.

    ![Data Factory Blade - Diagramm nebeneinander](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. Das Diagramm ähnlich dem folgenden Bild sollte angezeigt werden: 

    ![Diagramm anzeigen](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)

    Sie können vergrößern, verkleinern, zoom auf 100 % Zoom anpassen, Rohrleitungen und Tabellen positionieren und Datenherkunftsinformationen anzeigen (highlights Upstream- und downstream-Elemente der ausgewählten Elemente).  Doppelklicken Sie auf ein Objekt (Eingabe/Ausgabe-Tabelle oder Pipeline) Eigenschaften sehen. 
3. **ADFTutorialPipeline** in der Diagrammansicht und **offene Pipeline**. 

    ![Offene Pipeline](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenPipeline.png)
4. Die Aktivitäten in der Pipeline mit Eingabe- und Datasets für Aktivitäten sollte angezeigt werden. In diesem Lernprogramm haben Sie nur eine Aktivität in der Pipeline (Kopieraktivität) mit InputDataset als Eingabe-Dataset und OutputDataset als ausgabedataset.   

    ![Ansicht öffnen](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenedPipeline.png)
5. Klicken Sie auf **Data Factory** im Pfad oben links zu der Ansicht zurückzukehren. Die Ansicht zeigt alle Rohrleitungen. In diesem Beispiel haben Sie nur eine Pipeline erstellt.   
 

## <a name="monitor-pipeline"></a>Monitor-pipeline
In diesem Schritt verwenden Sie Azure-Portal überwachen, was in einer Azure Daten. 

### <a name="monitor-pipeline-using-diagram-view"></a>Monitor-Rohrleitung mit der Diagrammansicht

1. Klicken Sie auf **X** , um **die Ansicht um die Homepage "Data Factory" Data Factory finden Sie unter** schließen. Wenn Sie den Web-Browser geschlossen haben, gehen Sie folgendermaßen vor: 
    2. Navigieren Sie zum [Azure-Portal](https://portal.azure.com/). 
    2. Doppelklicken Sie auf **ADFTutorialDataFactory** auf dem **Startmenü** (oder) klicken Sie im linken Menü auf **Daten Factorys** und ADFTutorialDataFactory suchen. 
3. Anzahl und Namen der Tabellen und auf diese erstellt Pipeline sollte angezeigt werden.

    ![Homepage mit Namen](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactory-home-page-pipeline-tables.png)
4. Klicken Sie nun auf **Datasets** nebeneinander.
5. **Datasets** Blatt klicken Sie auf **InputDataset**. Dieses Dataset wird Eingabedatasets für **ADFTutorialPipeline**.

    ![Datasets mit InputDataset ausgewählt](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. Klicken Sie auf **... (Auslassungspunkte)** Filtert die Daten anzeigen.

    ![Alle Eingaben Datenslices](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  

    Beachten Sie, dass die Datensegmente bis zum aktuellen Zeitpunkt **bereit** sind, da die **emp.txt** immer im BLOB-Container existiert: **Adftutorial\input**. Bestätigen Sie, dass keine Segmente im **zuletzt fehlerhafte Segmente** unten angezeigt.

    Die **letzte AKTUALISIERUNGSZEIT**Listen **zuletzt aktualisiert Segmente** und **Segmente einer vor kurzem fehlschlug** sortiert. 
    
    Klicken Sie auf der Symbolleiste auf die Segmente Filtern auf **Filter** .  
    
    ![Eingabeslices Filter](./media/data-factory-copy-activity-tutorial-using-azure-portal/filter-input-slices.png)
6. Schließen Sie die Blades bis Blade **Datasets** finden Sie unter. Klicken Sie auf **OutputDataset**. Dieses Dataset wird das ausgabedataset für **ADFTutorialPipeline**.

    ![Daten-blade](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datasets-blade.png)
6. **OutputDataset** Blade sollte angezeigt werden, wie in der folgenden Abbildung dargestellt:

    ![Tabelle blade](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-table-blade.png) 
7. Beachten Sie, dass Datenslices bis zum aktuellen Zeitpunkt bereits produziert wurden, und sie sind **bereit**. Keine Segmente werden in **Segmente Problem** unten angezeigt.
8. Klicken Sie auf **... (Auslassungspunkte)** alle Segmente anzeigen.

    ![Daten Segmente blade](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png)
9. Klicken Sie in der Liste auf alle Daten und Blade **Datenslice** sollte angezeigt werden.

    ![Daten Slice-blade](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
  
    Wenn das Segment nicht im Zustand **bereit** ist, sehen Sie upstream Segmente, die noch nicht blockieren das aktuelle Segment ausführen aus **Upstream Segmente, die nicht bereit sind** .
11. Blatt **DATENSLICE** sollte angezeigt werden, dass alle Aktivitäten in der Liste unten ausgeführt wird. Klicken Sie auf eine **Aktivität ausführen** um Blade **Aktivität ausführen Details** anzuzeigen. 

    ![Aktivität ausführen Details](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)
12. Klicken Sie auf **X** , um alle Blades schließen, bis Sie wieder zu Hause Blade für **ADFTutorialDataFactory**.
14. (optional) Klicken Sie auf der Homepage **Rohrleitungen** **ADFTutorialDataFactory**und Drillthrough input (**verbraucht**) oder Ausgabetabellen (**Produktion**) auf **ADFTutorialPipeline** **Rohrleitungen** Blatt.
15. Starten Sie **SQL Server Management Studio**, Azure SQL-Datenbank herstellen Sie und überprüfen Sie, ob die Zeilen der Tabelle **emp** in der Datenbank eingefügt werden.

    ![die Ergebnisse der SQL-Abfrage](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Monitor-Pipeline mit Monitor & App verwalten
Sie können auch Monitor verwenden und Anwendung zum Überwachen von Pipelines verwalten. Ausführliche Informationen zur Verwendung dieser Anwendung finden Sie unter [Überwachen und Verwalten von Azure Data Factory Rohrleitungen überwachen und Anwendung](data-factory-monitor-manage-app.md).

1. Klicken Sie auf **Überwachen und verwalten** nebeneinander auf der Homepage Ihrer Data Factory.

    ![Überwachen und Verwalten der Kachel](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. **Überwachen und verwalten Anwendung**sollte angezeigt werden. Ändern Sie die **Startzeit** und **Endzeit** enthalten Beginn (2016-07-12) und Endzeit (2016-07-13) der Pipeline klicken Sie auf **Übernehmen**. 

    ![Überwachen und Verwalten der App](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png) 
3. Wählen Sie ein aktivitätenfenster in der Liste **Aktivität** Details anzeigen aus 
    ![Details der Fenster](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

## <a name="summary"></a>Zusammenfassung 
In diesem Lernprogramm erstellt eine Azure Data Factory kopiert Daten aus einem Azure blob mit einer Azure SQL-Datenbank. Mithilfe von Azure-Portal Data Factory, verknüpften Diensten, Datasets und einer Pipeline erstellen. Hier werden die allgemeinen Schritte in diesem Lernprogramm durchgeführt:  

1.  Ein Azure **Data Factory**erstellt.
2.  **Verknüpfte Dienste**erstellt:
    1. Ein **Azure-Speicher** verknüpft Dienst Azure Storage-Konto verknüpfen, die Daten enthält.    
    2. Ein **Azure SQL** -verknüpfte Dienst Azure SQL-Datenbank zu verknüpfen, die die Ausgabedaten enthält. 
3.  **Datasets** , die Daten und Ausgabedaten für Pipelines beschrieben erstellt.
4.  Eine **Rohrleitung** erstellt mit einer **Kopieraktivität** mit **BlobSource** als Quell- und **SqlSink** als Empfänger.  


## <a name="see-also"></a>Siehe auch
| Thema | Beschreibung |
| :---- | :---- |
| [Daten-Bewegung](data-factory-data-movement-activities.md) | Dieser Artikel enthält detaillierte Informationen zur Aktivität kopieren im Lernprogramm verwendet. |
| [Planung und Ausführung](data-factory-scheduling-and-execution.md) | Dieser Artikel beschreibt die Planung und Ausführung Aspekte der Azure Data Factory-Anwendungsmodell. |
| [Rohrleitungen](data-factory-create-pipelines.md) | Dieser Artikel hilft Ihnen Pipelines und Aktivitäten in Azure Data Factory. |
| [Datasets](data-factory-create-datasets.md) | Dieser Artikel hilft Ihnen zu Datasets in Azure Data Factory.
| [Überwachen und Verwalten von Rohrleitungen App überwachen](data-factory-monitor-manage-app.md) | Dieser Artikel beschreibt das Überwachen, verwalten und Rohrleitungen überwachen und Anwendung debuggen. 


