<properties 
    pageTitle="Lernprogramm: Erstellen eine Pipeline mit Kopie mit Visual Studio | Microsoft Azure" 
    description="In diesem Lernprogramm erstellen Sie mit einer Kopie eine Azure Data Factory-Pipeline mit Visual Studio." 
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
    ms.date="10/17/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Lernprogramm: Erstellen einer Pipeline mit Kopie mit Visual Studio
> [AZURE.SELECTOR]
- [Übersicht und Komponenten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Assistent zum Kopieren von](data-factory-copy-data-wizard-tutorial.md)
- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure-Ressourcen-Manager-Vorlage](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


In diesem Lernprogramm wird das Erstellen und Überwachen einer Azure Data Factory mit Visual Studio veranschaulicht. Pipeline in der Data Factory verwendet eine Aktivität kopieren Daten von Azure BLOB-Speicher in Azure SQL-Datenbank kopiert.

Hier sind die Schritte, die Sie als Teil dieses Lernprogramms ausführen:

1. Erstellen Sie zwei verknüpften Dienste: **AzureStorageLinkedService1** und **AzureSqlinkedService1**. 

    Die AzureStorageLinkedService1 links Azure-Speicher und AzureSqlLinkedService1 links eine SQL Azure-Datenbank Daten Fabrik: **ADFTutorialDataFactoryVS**. Die Eingabedaten für die Pipeline befindet sich in einem BLOB-Container Azure BLOB-Speicher und Daten in einer Tabelle in SQL Azure-Datenbank gespeichert. Daher hinzugefügt diese zwei Datenspeicher als verknüpfte Dienste Data Factory.
2. Erstellen Sie zwei Datasets: **InputDataset** und **OutputDataset**, die die Eingabe/Ausgabe-Daten, die im Datenspeicher gespeichert. 

    Geben Sie die InputDataset den blobcontainer, der ein mit den Quelldaten BLOB. Die OutputDataset Geben Sie die SQL-Tabelle, die Ausgabedaten gespeichert. Sie geben auch andere Eigenschaften wie Struktur, Verfügbarkeit und Richtlinien.
3. Erstellen Sie eine Rohrleitung mit dem Namen **ADFTutorialPipeline** in der ADFTutorialDataFactoryVS. 

    Die Pipeline hat eine **Aktivität kopieren** , die Eingabe kopiert Daten aus dem Azure, SQL Azure Ausgabetabelle blob. Der Kopie Aktivität Datenübertragungen in Azure Data Factory. Die Aktivität wird durch global verfügbaren Service betrieben, die Daten zwischen verschiedenen Datenspeichern eine sichere, zuverlässige und skalierbare Weise kopieren können. Siehe [Datenaktivitäten](data-factory-data-movement-activities.md) Weitere Details zur Aktivität kopieren. 
4. Erstellen Sie eine **VSTutorialFactory**namens "Data Factory". Die Factory Daten und alle Daten Factory Entitäten (verknüpften Diensten, Tabellen und der Pipeline) bereitstellen.    

## <a name="prerequisites"></a>Erforderliche Komponenten

1. [Lernprogramm](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) Artikel lesen und die **erforderliche** Schritte. 
2. Ein **Administrator des Azure-Abonnements** muss Data Factory Entitäten in Azure Data Factory veröffentlichen können.  
3. Sie müssen Folgendes auf Ihrem Computer installiert: 
    - Visual Studio 2013 oder Visual Studio 2015
    - Azure SDK für Visual Studio 2013 oder Visual Studio 2015 herunterladen Wechseln Sie [Azure-Downloadseite](https://azure.microsoft.com/downloads/) und auf **VS 2013** oder **VS 2015** im Abschnitt **.NET** .
    - Die neuesten Azure Data Factory-Plug-In für Visual Studio herunterladen: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) oder [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Sie können auch Plug-in aktualisieren, führen Sie die folgenden Schritte aus: Klicken Sie im Menü auf **Extras** -> **Extensions und Updates** -> **Online** -> **Visual Studio Gallery** -> **Microsoft Azure Data Factory-Tools für Visual Studio** -> **Aktualisieren**.

## <a name="create-visual-studio-project"></a>Visual Studio-Projekt erstellen 
1. Starten Sie **Visual Studio 2013**. Klicken Sie auf **Datei**, zeigen Sie auf **neu**, und klicken Sie auf **Projekt**. Das Dialogfeld **Neues Projekt** sollte angezeigt werden.  
2. Wählen Sie im Dialogfeld **Neues Projekt** **DataFactory** Vorlage aus und auf **Leere Data Factory-Projekt**. Wenn DataFactory-Vorlage nicht angezeigt wird, schließen Sie Visual Studio installieren Azure SDK für Visual Studio 2013 und öffnen Sie Visual Studio erneut.  

    ![Dialogfeld Neues Projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)

3. Geben Sie einen **Namen** für das Projekt, **Speicherort**und einen Namen für die **Projektmappe**, und klicken Sie auf **OK**.

    ![Projektmappen-Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png) 

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten
Verknüpften Dienste verknüpfen Datenspeicher oder Dienste einer Azure Data Factory zu berechnen. Siehe [unterstützte Daten](data-factory-data-movement-activities.md##supported-data-stores-and-formats) für alle Datenquellen und Datensenken unterstützt durch die Kopie. Finden Sie unter Liste der Compute-Dienste von Data Factory unterstützt [verknüpfte Dienste berechnet](data-factory-compute-linked-services.md) . In diesem Lernprogramm verwenden Sie keine Computing-Service. 

In diesem Schritt erstellen Sie zwei verknüpften Dienste: **AzureStorageLinkedService1** und **AzureSqlLinkedService1**. AzureStorageLinkedService1 Service-Links verknüpft ein Speicherkonto Azure und AzureSqlLinkedService eine Azure SQL-Datenbank mit Data Factory verknüpft: **ADFTutorialDataFactory**. 

### <a name="create-the-azure-storage-linked-service"></a>Azure Storage verknüpfte Dienst erstellen

4. Maustaste **Verknüpften Diensten** im Projektmappen-Explorer zeigen Sie auf **Hinzufügen**, und klicken Sie auf **Neues Element**.      
5. Klicken Sie im Dialogfeld **Neues Element hinzufügen** wählen Sie **Azure Storage verknüpfte Service aus** und auf **Hinzufügen**. 

    ![Neue verknüpfte Dienst](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
 
3. Ersetzen Sie `<accountname>` und `<accountkey>`* mit dem Namen der Azure-Speicher und den Schlüssel. 

    ![Azure verknüpfter Dienst](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)

4. Speichern Sie die Datei **AzureStorageLinkedService1.json** .

> Details über JSON-Eigenschaften finden Sie unter [Verschieben von Daten vom und zum Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) .

### <a name="create-the-azure-sql-linked-service"></a>Erstellen des verknüpften Azure SQL-Dienstes

5. Maustaste auf **Verknüpfte** Knoten im **Projektmappen-Explorer** erneut, zeigen Sie auf **Hinzufügen**, und klicken Sie auf **Neues Element**. 
6. Dieses Mal wählen Sie **Azure SQL verknüpften Serviceartikel**und klicken Sie auf **Hinzufügen**. 
7. Ersetzen Sie in der **Datei AzureSqlLinkedService1.json** `<servername>`, `<databasename>`, `<username@servername>`, und `<password>` mit Namen der Azure SQL Server, Datenbank-Benutzerkonto und Kennwort.    
8.  Speichern Sie die Datei **AzureSqlLinkedService1.json** . 

> [AZURE.NOTE]
> Details über JSON-Eigenschaften finden Sie unter [Verschieben von Daten aus und in Azure SQL-Datenbank](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .

## <a name="create-datasets"></a>Datasets erstellen
Im vorherigen Schritt erstellten verknüpfte Diensten Data Factory ein Speicherkonto Azure und SQL Azure-Datenbank verknüpfen, **AzureStorageLinkedService1** und **AzureSqlLinkedService1** : **ADFTutorialDataFactory**. In diesem Schritt definieren Sie zwei Datasets - **InputDataset** und **OutputDataset** -, die e/a-Daten darstellen, die durch AzureStorageLinkedService1 und AzureSqlLinkedService1 verwiesen Datenspeichern gespeichert ist. InputDataset Geben Sie den blobcontainer, der ein mit den Quelldaten BLOB. OutputDataset Geben Sie die SQL-Tabelle, die Ausgabedaten gespeichert.

### <a name="create-input-dataset"></a>Eingabe-Dataset erstellen
In diesem Schritt erstellen Sie ein Dataset mit dem Namen **InputDataset** auf BLOB-Container in den Azure-Speicher als Service **AzureStorageLinkedService1** verknüpft. Eine Tabelle ist eine rechteckige Dataset und ist der einzige Dataset unterstützt jetzt. 

9. **Tabellen** im **Projektmappen-Explorer**klicken Sie, zeigen Sie auf **Hinzufügen**und klicken Sie auf **Neues Element**.
10. Klicken Sie im Dialogfeld **Neues Element hinzufügen** wählen Sie **Azure Blob aus**und auf **Hinzufügen**.   
10. Ersetzen von JSON-Text durch folgenden Text, und speichern Sie die Datei **AzureBlobLocation1.json** . 

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
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adftutorial/",
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

> [AZURE.NOTE]
> Details über JSON-Eigenschaften finden Sie unter [Verschieben von Daten vom und zum Azure Blob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) .

### <a name="create-output-dataset"></a>Ausgabedataset erstellen
In diesem Schritt erstellen Sie eine ausgabedataset mit dem Namen **OutputDataset**. Dieses Dataset verweist auf eine SQL-Tabelle in der SQL Azure-Datenbank durch **AzureSqlLinkedService1**dargestellt. 

11. Mit der rechten Maustaste erneut auf **Tabellen** im **Projektmappen-Explorer** , zeigen Sie auf **Hinzufügen**, und klicken Sie auf **Neues Element**.
12. Klicken Sie im Dialogfeld **Neues Element hinzufügen** wählen Sie **SQL Azure**und klicken Sie auf **Hinzufügen**. 
13. Ersetzen von JSON-Text durch folgenden JSON, und speichern Sie die Datei **AzureSqlTableLocation1.json** .

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
            "linkedServiceName": "AzureSqlLinkedService1",
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

> [AZURE.NOTE]
> Details über JSON-Eigenschaften finden Sie unter [Verschieben von Daten aus und in Azure SQL-Datenbank](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .

## <a name="create-pipeline"></a>Pipeline erstellen 
Verknüpfte Eingabe-/Ausgabedienste und Tabellen haben bisher erstellt werden. Jetzt erstellen Sie eine Pipeline mit einer **Aktivität kopieren** kopiert Daten aus dem Azure blob in Azure SQL-Datenbank. 


1. **Rohrleitungen** im **Projektmappen-Explorer**klicken Sie, zeigen Sie auf **Hinzufügen**und klicken Sie auf **Neues Element**.  
15. Wählen Sie **Datenpipeline kopieren** im Dialogfeld **Neues Element hinzufügen** , und klicken Sie auf **Hinzufügen**. 
16. Die folgenden JSON ersetzen Sie JSON und speichern Sie die Datei **CopyActivity1.json** .
            
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
                  "style": "StartOfInterval",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z",
            "isPaused": false
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

## <a name="publishdeploy-data-factory-entities"></a>Data Factory Entitäten veröffentlichen/Bereitstellung
In diesem Schritt Data Factory Entitäten (verknüpften Diensten, Datasets und Pipeline) veröffentlichen Sie zuvor erstellt haben. Sie geben Sie den Namen des neuen Data Factory erstellt diese Entitäten.  

18. Klicken Sie Projekt im Projektmappen-Explorer, und klicken Sie auf **Veröffentlichen**. 
19. Dialogfeld **Melden Sie sich bei Ihrem Microsoft-Konto** anzuzeigen, geben Sie Ihre Anmeldeinformationen für das Konto Azure-Abonnement und klicken Sie auf **Anmelden**.
20. Das Dialogfeld sollte angezeigt werden:

    ![Veröffentlichen (Dialogfeld)](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
21. Datenzugriffsseite Factory konfigurieren gehen Sie folgendermaßen vor: 
    1. **Erstellen neuer Data Factory** die Option.
    2. Geben Sie **VSTutorialFactory** **ein**.  
    
        > [AZURE.IMPORTANT]  
        > Der Name der Azure Data Factory muss eindeutig sein. Wenn namens "Data Factory" beim Veröffentlichen Fehlermeldung, ändern Sie den Namen der Data Factory (z. B. YournameVSTutorialFactory) und versuchen Sie erneut zu veröffentlichen. Benennungskonventionen für Artefakte Data Factory finden Sie [Data Factory - Namenskonventionen](data-factory-naming-rules.md) .     
    3. Wählen Sie Ihre Azure-Abonnement für das **Abonnement** -Feld.
     
        > [AZURE.IMPORTANT]Abonnement nicht angezeigt wird, sicher, dass Sie über ein Konto, das ein Administrator oder co-Administrator des Abonnements angemeldet.  
    4. Wählen Sie die **Ressourcengruppe** für Data Factory erstellt werden. 5. Wählen Sie die **Region** für die Data Factory. Nur Bereiche vom Dienst Data Factory unterstützt werden in der Dropdown-Liste angezeigt.
6. Klicken Sie auf **nächste** Seite **Veröffentlichen Elemente** wechseln.
    
        ![Konfigurieren der Datenseite factory](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
23. Seite **Veröffentlichen Elemente** sicher, dass alle Daten-Factorys Elemente ausgewählt sind, und klicken Sie auf **Weiter** auf der Seite **Zusammenfassung** zu wechseln.
    
    ![Elemente der Veröffentlichungsseite](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
24. Überprüfen Sie die Zusammenfassung, und klicken Sie auf **Weiter** um den Bereitstellungsprozess zu starten und den **Bereitstellungsstatus**anzeigen.

    ![Zusammenfassung veröffentlichen](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
25. Auf der Seite **Bereitstellung** erhalten Sie den Status des Bereitstellungsprozesses. Klicken Sie auf Fertig stellen, nachdem die Bereitstellung abgeschlossen ist. 
    ![Bereitstellung Statusseite](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png) Beachten Sie folgende Punkte: 

- Wenn Sie die Fehlermeldung: "**dieses Abonnement ist nicht registriert Microsoft.DataFactory-Namespace verwenden**," die folgenden und versuchen Sie erneut veröffentlichen: 

    - In Azure PowerShell, führen Sie folgenden Befehl Data Factory-Anbieter registrieren. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Dem folgenden Befehl zum bestätigen kann der ausführen registrieren. 
    
            Get-AzureRmResourceProvider
    - Mit der Azure-Abonnement in [Azure-Portal](https://portal.azure.com) anmelden und navigieren Sie zu einem Blatt Data Factory (oder) eine Factory Daten in Azure-Portal erstellen. Dadurch wird automatisch den Provider für Sie registriert.
-   Der Name der Factory Daten möglicherweise als DNS-Name in der Zukunft und damit sichtbar öffentlich registriert werden.

> [AZURE.IMPORTANT] Um Data Factory-Instanzen erstellen, müssen Sie ein Admin/co-Admin Azure-Abonnement

## <a name="summary"></a>Zusammenfassung
In diesem Lernprogramm erstellt eine Azure Data Factory kopiert Daten aus einem Azure blob mit einer Azure SQL-Datenbank. Mithilfe von Visual Studio Daten Factory, verknüpften Diensten, Datasets und einer Pipeline erstellen. Hier werden die allgemeinen Schritte in diesem Lernprogramm durchgeführt:  

1.  Ein Azure **Data Factory**erstellt.
2.  **Verknüpfte Dienste**erstellt:
    1. Ein **Azure-Speicher** verknüpft Dienst Azure Storage-Konto verknüpfen, die Daten enthält.    
    2. Ein **Azure SQL** -verknüpfte Dienst Azure SQL-Datenbank zu verknüpfen, die die Ausgabedaten enthält. 
3.  **Datasets**erstellt, die Daten und Ausgabedaten für Pipelines beschrieben.
4.  Eine **Rohrleitung** erstellt mit einer **Kopieraktivität** mit **BlobSource** als Quell- und **SqlSink** als Empfänger. 


## <a name="use-server-explorer-to-view-data-factories"></a>Verwenden Sie Server-Explorer Daten Factorys anzeigen

1. Klicken Sie in **Visual Studio**im Menü auf **Ansicht** und klicken Sie auf **Server-Explorer**.
2. Klicken Sie im Server-Explorer erweitern Sie **Azure** und **Data Factory**. Siehst du **Visual Studio anmelden**, geben Sie das **Konto** der Azure-Abonnement zugeordnet und auf **Weiter**. Geben Sie **Kennwort ein**und klicken Sie auf **Anmelden**. Visual Studio versucht, Informationen über alle Azure Data Factorys in Ihrem Abonnement. Der Status dieses Vorgangs im Fenster **Daten Factory-Aufgabenliste** angezeigt.
    ![Server-Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. Sie können mit der rechten Maustaste auf eine Daten und wählen neues Projekt, erstellen Sie ein Visual Studio-Projekt basierend auf einer vorhandenen Daten Data Factory exportieren.
    !["Data Factory" in einem Projekt exportieren](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Aktualisieren von Data Factory-Tools für Visual Studio
Aktualisieren von Azure Data Factory-Tools für Visual Studio gehen Sie folgendermaßen vor:

1. Klicken Sie im Menü auf **Extras** , und wählen Sie **Erweiterungen und Updates**. 
2. Wählen Sie **Updates** im linken Fensterbereich und dann **Visual Studio Gallery**.
4. Wählen Sie **Azure Data Factory-Tools für Visual Studio** , und klicken Sie auf **Aktualisieren**. Wenn dieser Eintrag nicht angezeigt wird, verfügen Sie bereits über die neueste Version der Tools. 

Finden Sie [Monitor Datasets und Pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) Anleitung mit der Azure-Portal Überwachen der Pipeline und des Datasets in diesem Lernprogramm erstellt haben.

## <a name="see-also"></a>Siehe auch
| Thema | Beschreibung |
| :---- | :---- |
| [Daten-Bewegung](data-factory-data-movement-activities.md) | Dieser Artikel enthält detaillierte Informationen zur Aktivität kopieren im Lernprogramm verwendet. |
| [Planung und Ausführung](data-factory-scheduling-and-execution.md) | Dieser Artikel beschreibt die Planung und Ausführung Aspekte der Azure Data Factory-Anwendungsmodell. |
| [Rohrleitungen](data-factory-create-pipelines.md) | Dieser Artikel hilft Ihnen Pipelines und Aktivitäten in Azure Data Factory |
| [Datasets](data-factory-create-datasets.md) | Dieser Artikel hilft Ihnen zu Datasets in Azure Data Factory.
| [Überwachen und Verwalten von Rohrleitungen App überwachen](data-factory-monitor-manage-app.md) | Dieser Artikel beschreibt das Überwachen, verwalten und Rohrleitungen überwachen und Anwendung debuggen. 
