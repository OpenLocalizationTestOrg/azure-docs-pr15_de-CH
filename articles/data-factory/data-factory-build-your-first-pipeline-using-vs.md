<properties
    pageTitle="Erstellen Sie Ihre erste "Data Factory" (Visual Studio) | Microsoft Azure"
    description="In diesem Lernprogramm erstellen Sie eine Probe Azure Data Factory-Pipeline mit Visual Studio."
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
    ms.topic="hero-article" 
    ms.date="10/17/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-azure-first-data-factory-using-microsoft-visual-studio"></a>Lernprogramm: Erstellen Sie Ihre erste Azure Data Factory mit Microsoft Visual Studio
> [AZURE.SELECTOR]
- [Übersicht und Komponenten](data-factory-build-your-first-pipeline.md)
- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Ressourcen-Manager-Vorlage](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

In diesem Artikel verwenden Sie Microsoft Visual Studio zum Erstellen Ihrer ersten Azure Data Factory.

## <a name="prerequisites"></a>Erforderliche Komponenten
1. [Lernprogramm](data-factory-build-your-first-pipeline.md) Artikel lesen und die **erforderliche** Schritte.
2. Ein **Administrator des Azure-Abonnements** muss Data Factory Entitäten aus Visual Studio Azure Data Factory veröffentlichen können.
3. Sie müssen Folgendes auf Ihrem Computer installiert: 
    - Visual Studio 2013 oder Visual Studio 2015
    - Azure SDK für Visual Studio 2013 oder Visual Studio 2015 herunterladen Wechseln Sie [Azure-Downloadseite](https://azure.microsoft.com/downloads/) und auf **VS 2013** oder **VS 2015** im Abschnitt **.NET** .
    - Die neuesten Azure Data Factory-Plug-In für Visual Studio herunterladen: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) oder [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Sie können auch das Plugin aktualisieren, wie folgt: Klicken Sie im Menü auf **Extras** -> **Extensions und Updates** -> **Online** -> **Visual Studio Gallery** -> **Microsoft Azure Data Factory-Tools für Visual Studio** -> **Aktualisieren**. 
 
Jetzt verwenden wir Visual Studio zum Erstellen einer Azure Data Factory. 


## <a name="create-visual-studio-project"></a>Visual Studio-Projekt erstellen 
1. Starten Sie **Visual Studio 2013** oder **Visual Studio 2015**. Klicken Sie auf **Datei**, zeigen Sie auf **neu**, und klicken Sie auf **Projekt**. Das Dialogfeld **Neues Projekt** sollte angezeigt werden.  
2. Wählen Sie im Dialogfeld **Neues Projekt** **DataFactory** Vorlage aus und auf **Leere Data Factory-Projekt**.   

    ![Dialogfeld Neues Projekt](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)

3. Geben Sie einen **Namen** für das Projekt, **Speicherort**und einen Namen für die **Projektmappe**, und klicken Sie auf **OK**.

    ![Projektmappen-Explorer](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten
Eine Factory Daten können eine oder mehrere Rohrleitungen. Eine Pipeline kann eine oder mehrere Aktivitäten enthalten. Z. B. eine Kopieraktivität Daten aus einer Quelle in ein Ziel-Datenspeicher und eine HDInsight Struktur Aktivität Hive-Skript zum Transformieren von Eingabedaten kopiert. Siehe [unterstützte Daten](data-factory-data-movement-activities.md##supported-data-stores-and-formats) für alle Datenquellen und Datensenken unterstützt durch die Kopie. Finden Sie unter Liste der Compute-Dienste von Data Factory unterstützt [verknüpfte Dienste berechnet](data-factory-compute-linked-services.md) . 

In diesem Schritt verknüpfen Sie Ihr Konto Azure-Speicher und einer bedarfsgesteuerten Azure HDInsight Cluster mit Daten Werk. Azure Storage-Konto enthält die Eingabe- und Daten für die Pipeline in diesem Beispiel. Der Dienst HDInsight verknüpft zur Skript Struktur in der Aktivität der Pipeline in diesem Beispiel angegeben. Identifizieren Sie Daten Speicher/Compute-Dienste in Ihrem Szenario verwendet und verknüpfen diese Dienste mit Data Factory verknüpfte Diensten erstellen.  

Geben Sie Namen und Einstellungen für die Factory Daten später beim Veröffentlichen Ihrer Lösung Data Factory.

#### <a name="create-azure-storage-linked-service"></a>Erstellen von verknüpften Azure Storage-Diensts
In diesem Schritt verknüpfen Sie Ihre Azure Storage-Konto mit Daten Werk. In diesem Lernprogramm verwenden Sie dasselbe Azure Storage-Konto Eingabe/Ausgabe von Daten und die Skriptdatei HQL speichern. 

4. Maustaste **Verknüpften Diensten** im Projektmappen-Explorer zeigen Sie auf **Hinzufügen**, und klicken Sie auf **Neues Element**.      
5. Klicken Sie im Dialogfeld **Neues Element hinzufügen** wählen Sie **Azure Storage verknüpfte Service aus** und auf **Hinzufügen**. 
3. Ersetzen Sie **Kontoname** und **Accountkey** mit dem Namen der Azure-Speicher und den Schlüssel. Wie man die Zugriffstaste Speicher finden Sie unter [anzeigen, kopieren und Zugriffstasten Regenerieren Speicher](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)

    ![Azure verknüpfter Dienst](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)

4. Speichern Sie die Datei **AzureStorageLinkedService1.json** .

#### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight verknüpfte Dienst erstellen
In diesem Schritt verknüpfen Sie einen HDInsight-Cluster auf Anforderung mit Daten Werk. HDInsight Cluster automatisch zur Laufzeit erstellt und danach Verarbeitung und im Leerlauf für die angegebene Zeitspanne wird gelöscht. HDInsight-Cluster können anstelle eines bedarfsgesteuerten HDInsight Clusters. Details finden Sie in der [Verknüpften Dienste berechnet](data-factory-compute-linked-services.md) . 

1. Im **Projektmappen-Explorer**mit der rechten Maustaste **Verknüpften Diensten**zeigen Sie auf **Hinzufügen**und klicken Sie auf **Neues Element**.
2. Wählen Sie **HDInsight Anforderung verknüpfte Dienst aus**und auf **Hinzufügen**. 
3. Ersetzen Sie **JSON** durch Folgendes:

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService1"
            }
          }
        }
    
    Die folgende Tabelle enthält eine Beschreibung der JSON-Eigenschaften im Ausschnitt verwendet:
    
    Eigenschaft | Beschreibung
    -------- | -----------
    Version | Gibt an, dass die Version der HDInsight 3.2 erstellt. 
    ClusterSize | Gibt die Größe des HDInsight-Clusters. 
    TimeToLive | Gibt an, dass die Leerlaufzeit für den HDInsight-Cluster, bevor es gelöscht wird.
    Nameverknüpfterdienst | Gibt das Speicherkonto, mit der von HDInsight generierten Protokolle speichern

    Beachten Sie Folgendes: 
    
    - Der erstellt eines **Windows-basierten** HDInsight Clusters mit vorherigen JSON. Sie können auch einen **Linux-basierten** HDInsight Cluster erstellen lassen. Einzelheiten finden Sie [Bei Bedarf HDInsight verknüpften Serviceartikel](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
    - **HDInsight-Cluster** können anstelle eines bedarfsgesteuerten HDInsight Clusters. Details finden Sie in der [Verknüpften HDInsight-Dienst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
    - HDInsight-Cluster erstellt einen **Standardcontainer** im BLOB-Speicher, in JSON (**Nameverknüpfterdienst angegeben**). HDInsight löscht keine Container, wenn Cluster gelöscht wird. Dieses Verhalten ist entwurfsbedingt. Auf Anforderung verknüpft HDInsight Service wird HDInsight Cluster ein Slice verarbeitet, es sei denn ein vorhandener live Cluster (**TimeToLive**) erstellt. Der Cluster wird automatisch gelöscht wird.
    
        Mehrere Segmente verarbeitet werden, sehen Sie viele Container im Azure BLOB-Speicher. Benötigen Sie nicht diese für die Problembehandlung der Aufträge, möchten Sie löschen, um die Speicherkosten zu senken. Die Namen dieser Container ein Muster folgen: "Adf**Yourdatafactoryname**-**nameverknüpfterdienst**- datumuhrzeitstempel". Verwenden Sie Tools wie [Microsoft Storage Explorer](http://storageexplorer.com/) Container im Azure BLOB-Speicher löschen.

    Einzelheiten finden Sie [Bei Bedarf HDInsight verknüpften Serviceartikel](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
4. Speichern Sie die Datei **HDInsightOnDemandLinkedService1.json** .

## <a name="create-datasets"></a>Datasets erstellen
In diesem Schritt erstellen Sie Datasets Eingabe und Ausgabedaten für die Verarbeitung der Struktur. Diese Datasets finden Sie in der **AzureStorageLinkedService1** , die Sie zuvor in diesem Lernprogramm erstellt haben. Verknüpfte Dienstpunkte Azure Storage-Konto als Datasets angeben im Speicher, die Eingabe enthält Container, Ordner, Dateiname und Ausgabedaten.   

#### <a name="create-input-dataset"></a>Eingabe-Dataset erstellen

1. Im **Projektmappen-Explorer**Maustaste auf **Tabellen**, zeigen Sie auf **Hinzufügen**, und klicken Sie auf **Neues Element**. 
2. Wählen Sie **Azure Blob aus** , ändern Sie den Namen der Datei zu **InputDataSet.json**und klicken Sie auf **Hinzufügen**.
3. **JSON** im Editor durch den folgenden Code ersetzen: 

    Im Ausschnitt JSON erstellen Sie ein Dataset namens **AzureBlobInput** , die Eingabedaten für eine Aktivität in der Pipeline darstellt. Darüber hinaus angeben, dass die eingegebenen Daten im BLOB-Container namens **Adfgetstarted** und Ordner **Inputdata** befindet
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService1",
                "typeProperties": {
                    "fileName": "input.log",
                    "folderPath": "adfgetstarted/inputdata",
                    "format": {
                        "type": "TextFormat",
                        "columnDelimiter": ","
                    }
                },
                "availability": {
                    "frequency": "Month",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        } 

    Die folgende Tabelle enthält eine Beschreibung der JSON-Eigenschaften im Ausschnitt verwendet:

  	| Eigenschaft | Beschreibung |
  	| :------- | :---------- |
  	| Typ | Die Type-Eigenschaft wird auf AzureBlob festgelegt, da Daten in Azure BLOB-Speicher befindet. |  
  	| Nameverknüpfterdienst | bezieht sich auf die AzureStorageLinkedService1, die Sie zuvor erstellt haben. |
  	| Dateiname | Diese Eigenschaft ist optional. Wenn diese Eigenschaft nicht angeben, werden alle Dateien aus dem FolderPath entnommen. In diesem Fall wird die input.log verarbeitet. |
  	| Typ | Die Protokolldateien werden im Textformat TextFormat verwendet. | 
  	| columnDelimiter | Spalten in den Protokolldateien werden durch Komma (,) getrennt. |
  	| / Intervall | Frequenz Monat und Intervall ist 1 bedeutet, dass die Eingabe Segmente monatlich. | 
  	| externe | Diese Eigenschaft wird festgelegt auf true, wenn die eingegebenen Daten nicht vom Data Factory-Dienst generiert werden. | 
      
    
3. Speichern Sie die Datei **InputDataset.json** . 

 
#### <a name="create-output-dataset"></a>Ausgabedataset erstellen
Erstellen Sie jetzt das ausgabedataset stellen die Ausgabedaten in Azure BLOB-Speicher gespeichert. 

1. Im **Projektmappen-Explorer**Maustaste auf **Tabellen**, zeigen Sie auf **Hinzufügen**, und klicken Sie auf **Neues Element**. 
2. Wählen Sie **Azure Blob aus** , ändern Sie den Namen der Datei zu **OutputDataset.json**und klicken Sie auf **Hinzufügen**. 
3. **JSON** im Editor durch den folgenden Code ersetzen: 

    Im Ausschnitt JSON erstellen Sie ein Dataset namens **AzureBlobOutput**und die Struktur der Daten, die vom Skript Struktur erzeugt angeben. Darüber hinaus geben Sie die Ergebnisse in der BLOB-Container namens **Adfgetstarted** und Ordner mit dem Namen **Partitioneddata**gespeichert. Abschnitt **Verfügbarkeit** gibt an, dass das ausgabedataset monatlich erstellt wird.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adfgetstarted/partitioneddata",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Month",
              "interval": 1
            }
          }
        }

    **Erstellen des Eingabe-Dataset** siehe Beschreibung dieser Eigenschaften. Sie fest nicht als Dataset vom Dienst Data Factory erstellte externe Eigenschaft ein Ausgabe-DataSet.

4. Speichern Sie die Datei **OutputDataset.json** .


### <a name="create-pipeline"></a>Pipeline erstellen
In diesem Schritt erstellen Sie Ihre erste Pipeline mit einer **HDInsightHive** . Eingang Scheibe steht monatlich (Häufigkeit: Monat, Intervall: 1) ausgabeslice monatlich erstellt und Planer-Eigenschaft der Aktivität auch monatlich festgelegt. Das ausgabedataset und der Planer Aktivität müssen übereinstimmen. Derzeit treibt ausgabedataset planen, müssen Sie eine ausgabedataset erstellen, wenn die Aktivität keine Ausgabe erzeugt. Wenn die Aktivität Eingaben nicht, überspringen Sie Eingabedatasets erstellen. Am Ende dieses Abschnitts werden die Eigenschaften der folgenden JSON erläutert.

1. Im **Projektmappen-Explorer**mit der rechten Maustaste **Rohrleitungen**, zeigen Sie auf **Hinzufügen**und auf **Neues Element.** 
2. **Struktur Transformation Pipeline** wählen Sie aus und auf **Hinzufügen**. 
3. Der folgende Codeausschnitt **JSON** ersetzen.

    > [AZURE.IMPORTANT] Ersetzen Sie **Storageaccountname** durch den Namen Ihres Speicherkontos.

        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService1",
                            "defines": {
                                "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                                "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                            }
                        },
                        "inputs": [
                            {
                                "name": "AzureBlobInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "AzureBlobOutput"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "HDInsightOnDemandLinkedService"
                    }
                ],
                "start": "2016-04-01T00:00:00Z",
                "end": "2016-04-02T00:00:00Z",
                "isPaused": false
            }
        }

    Im Ausschnitt JSON erstellen Sie eine Rohrleitung mit einer einzelnen Aktivität, die Struktur Prozess Daten auf einen HDInsight-Cluster verwendet.
    
    Im Ausschnitt JSON erstellen Sie eine Rohrleitung mit einer einzelnen Aktivität, die Struktur Prozess Daten auf einen HDInsight-Cluster verwendet.
    
    Struktur-Skriptdatei **partitionweblogs.hql**ist in Azure Storage Konto (angegeben durch ScriptLinkedService als **AzureStorageLinkedService1**bezeichnet) und im **Skript** -Ordner im Container **Adfgetstarted**gespeichert.

    Abschnitt **definiert** wird verwendet, um die Laufzeit Einstellungen, die Hive-Skript als Struktur Werte übergeben werden (z. B. ${Hiveconf: inputtable}, ${Hiveconf:partitionedtable}).

    Die Eigenschaften **start** und **Ende** der Pipeline gibt die Laufzeit der Pipeline.

    In JSON Aktivität geben Sie dass Hive-Skript auf die Berechnung gemäß der **Nameverknüpfterdienst** - **HDInsightOnDemandLinkedService**ausgeführt wird.

    > [AZURE.NOTE] Details im Beispiel verwendeten JSON-Eigenschaften finden Sie unter [Anatomie einer Rohrleitung](data-factory-create-pipelines.md#anatomy-of-a-pipeline) . 
3. Speichern Sie die Datei **HiveActivity1.json** .

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a>Partitionweblogs.hql und input.log als Abhängigkeit hinzufügen 

1. Maustaste **Abhängigkeiten** im **Projektmappen** -Explorer zeigen Sie auf **Hinzufügen**, und klicken Sie auf **Vorhandenes Element**.  
2. Navigieren Sie zu **C:\ADFGettingStarted** , **partitionweblogs.hql**, **input.log** Dateien wählen Sie, und klicken Sie auf **Hinzufügen**. [Lernprogramm Übersicht](data-factory-build-your-first-pipeline.md)hatte diese beiden Dateien als Teil der erforderlichen Komponenten erstellt werden.

Beim Veröffentlichen der Lösung im nächsten Schritt wird die Datei **partitionweblogs.hql** in den Ordner "Scripts" im **Adfgetstarted** BLOB-Container hochgeladen.   

### <a name="publishdeploy-data-factory-entities"></a>Data Factory Entitäten veröffentlichen/Bereitstellung

18. Klicken Sie Projekt im Projektmappen-Explorer, und klicken Sie auf **Veröffentlichen**. 
19. Dialogfeld **Melden Sie sich bei Ihrem Microsoft-Konto** anzuzeigen, geben Sie Ihre Anmeldeinformationen für das Konto Azure-Abonnement und klicken Sie auf **Anmelden**.
20. Das Dialogfeld sollte angezeigt werden:

    ![Veröffentlichen (Dialogfeld)](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Datenseite Factory konfigurieren folgendermaßen Sie vor: 
    1. **Erstellen neuer Data Factory** die Option.
    2. Geben Sie einen eindeutigen **Namen** für die Daten-Factory. Beispiel: **FirstDataFactoryUsingVS09152016**. Der Name muss eindeutig sein.  
    
    
        > [AZURE.IMPORTANT] Wenn Sie beim Veröffentlichen die **Data Factory Name "FirstDataFactoryUsingVS" ist nicht verfügbar** Fehler erhalten, ändern Sie den Namen (z. B. YournameFirstDataFactoryUsingVS). Benennungskonventionen für Artefakte Data Factory finden Sie [Data Factory - Namenskonventionen](data-factory-naming-rules.md) .
3. Wählen Sie das richtige Abonnement für Feld **Abonnement** .
     
     
        > [AZURE.IMPORTANT] Abonnement nicht angezeigt wird, sicher, dass Sie über ein Konto, das ein Administrator oder co-Administrator des Abonnements angemeldet.  
        
    4. Wählen Sie die **Ressourcengruppe** für Data Factory erstellt werden. 
    5. Wählen Sie die **Region** für die Data Factory. 
    6. Klicken Sie auf **nächste** Seite **Veröffentlichen Elemente** wechseln. (Drücken Sie **Registerkarte** aus dem Namen zu verschieben, wenn die Schaltfläche **Weiter** deaktiviert.) 
23. Seite **Veröffentlichen Elemente** sicher, dass alle Daten-Factorys Elemente ausgewählt sind, und klicken Sie auf **Weiter** auf der Seite **Zusammenfassung** zu wechseln.     
24. Überprüfen Sie die Zusammenfassung, und klicken Sie auf **Weiter** um den Bereitstellungsprozess zu starten und den **Bereitstellungsstatus**anzeigen.
25. Auf der Seite **Bereitstellung** erhalten Sie den Status des Bereitstellungsprozesses. Klicken Sie auf Fertig stellen, nachdem die Bereitstellung abgeschlossen ist. 

 
Wichtige Punkte zu beachten: 

- Wenn Sie die Fehlermeldung: "**dieses Abonnement ist nicht registriert Microsoft.DataFactory-Namespace verwenden**," die folgenden und versuchen Sie erneut veröffentlichen: 

    - In Azure PowerShell, führen Sie folgenden Befehl Data Factory-Anbieter registrieren. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Dem folgenden Befehl zum bestätigen kann der ausführen registrieren. 
    
            Get-AzureRmResourceProvider
    - Mit der Azure-Abonnement in [Azure-Portal](https://portal.azure.com) anmelden und navigieren Sie zu einem Blatt Data Factory (oder) eine Factory Daten in Azure-Portal erstellen. Dadurch wird automatisch den Provider für Sie registriert.
-   Der Name der Factory Daten möglicherweise als DNS-Name in der Zukunft und damit sichtbar öffentlich registriert werden.
-   Um Data Factory-Instanzen erstellen, müssen Sie ein Administrator oder co-Administrator des Azure-Abonnements

 
## <a name="monitor-pipeline"></a>Monitor-pipeline

### <a name="monitor-pipeline-using-diagram-view"></a>Monitor-Rohrleitung mit der Diagrammansicht
6. [Azure-Portal](https://portal.azure.com/)anzumelden Sie, gehen Sie folgendermaßen vor:
    1. Klicken Sie auf **Weitere Dienste** und **Daten Fabriken**auf.
        ![Factorys Daten durchsuchen](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png) 
    2. Wählen Sie den Namen Ihrer Data Factory (z. B.: **FirstDataFactoryUsingVS09152016**) aus der Liste der Daten Factorys. 
        ![Wählen Sie die Data factory](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
7. Klicken Sie in der Homepage Ihrer Data Factory auf **Diagramm**.
  
    ![Diagramm-Kachel](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
7. In der Diagrammansicht sehen Sie eine Übersicht der Rohrleitungen und Datasets in diesem Lernprogramm verwendet.
    
    ![Diagramm anzeigen](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png) 
8. Zum Anzeigen aller Aktivitäten in der Pipeline Rohrleitung im Diagramm und offene Pipeline. 

    ![Offene Pipeline-Menü](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
9. Bestätigen Sie, dass die HDInsightHive Aktivität in der Pipeline finden Sie unter. 
  
    ![Ansicht öffnen](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    Um zur vorherigen Ansicht zu wechseln, klicken Sie auf **Data Factory** Breadcrumb-Menü oben. 
10. Doppelklicken Sie im **Diagramm**das Dataset **AzureBlobInput**. Bestätigen Sie, dass das Segment **bereit** . Es dauert ein paar Minuten für das Segment bereit angezeigt. Wenn es nicht geschieht, nachdem Sie einmal warten, finden Sie die Datei (input.log) in den richtigen Container (Adfgetstarted) und den Ordner (Inputdata) platziert.

    ![Eingang Scheibe bereit](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
11. Klicken Sie auf **X** -Blade **AzureBlobInput** schließen. 
12. Doppelklicken Sie im **Diagramm**das Dataset **AzureBlobOutput**. Sie sehen, dass das Segment, das derzeit verarbeitet wird.

    ![DataSet](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. Nach Abschluss der Verarbeitung sehen Sie das Segment **bereit** .

    > [AZURE.IMPORTANT] Erstellung eines Clusters HDInsight auf Anforderung dauert einige Zeit (ca. 20 Minuten). Pipeline **30 Minuten** Segment zu erwarten.  

    ![DataSet](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png) 
    
10. Wenn der Speicherbereich **bereit** ist, überprüfen Sie den Ordner **Partitioneddata** im Container **Adfgetstarted** im BLOB-Speicher für die Ausgabe von Daten.  
 
    ![Ausgabedaten](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. Klicken Sie auf die Informationen in einem Blade **Datenslice** darauf.

    ![Segment Datendetails](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. Klicken Sie auf eine Aktivität ausführen in der **Liste der Aktivitäten ausgeführt** , um Details über eine Aktivität ausführen (Hive-Aktivität in diesem Szenario) in einem Fenster **Aktivität ausführen Details** anzuzeigen.   
    ![Aktivität ausführen details](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)  
    
    Aus den Protokolldateien finden Sie unter Hive-Abfrage ausgeführt wurde und Statusinformationen. Diese Protokolle sind für die Behandlung von Problemen nützlich.  
 

Finden Sie [Monitor Datasets und Pipeline](data-factory-monitor-manage-pipelines.md) Anleitung mit der Azure-Portal Überwachen der Pipeline und des Datasets in diesem Lernprogramm erstellt haben.

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Monitor-Pipeline mit Monitor & App verwalten
Sie können auch Monitor verwenden und Anwendung zum Überwachen von Pipelines verwalten. Ausführliche Informationen zur Verwendung dieser Anwendung finden Sie unter [Überwachen und Verwalten von Azure Data Factory Rohrleitungen überwachen und Anwendung](data-factory-monitor-manage-app.md).

1. Klicken Sie auf Überwachung und Verwaltung nebeneinander.

    ![Überwachen und Verwalten der Kachel](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png) 
2. Sie sollten finden Sie unter Monitor & Anwendung verwalten. Ändern Sie die **Startzeit** und **Endzeit** entsprechend Start (01 04 2016 12:00 Uhr) und Endzeit (04 02 2016 12:00 Uhr) von der Pipeline **Klicken**.

    ![Überwachen und Verwalten der App](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png) 
3. Wählen Sie ein aktivitätenfenster in der Liste Aktivität Details anzeigen aus 
    ![Details der Fenster](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)


> [AZURE.IMPORTANT] Die Eingabedatei wird gelöscht, wenn das Segment verarbeitet. Daher ggf. erneut das Segment oder das Lernprogramm erneut hochladen der Eingabedatei (input.log) in den Ordner Inputdata Adfgetstarted Container.
 

## <a name="use-server-explorer-to-view-data-factories"></a>Verwenden Sie Server-Explorer Daten Factorys anzeigen

1. Klicken Sie in **Visual Studio**im Menü auf **Ansicht** und klicken Sie auf **Server-Explorer**.
2. Klicken Sie im Server-Explorer erweitern Sie **Azure** und **Data Factory**. Siehst du **Visual Studio anmelden**, geben Sie das **Konto** der Azure-Abonnement zugeordnet und auf **Weiter**. Geben Sie **Kennwort ein**und klicken Sie auf **Anmelden**. Visual Studio versucht, Informationen über alle Azure Data Factorys in Ihrem Abonnement. Der Status dieses Vorgangs im Fenster **Daten Factory-Aufgabenliste** angezeigt.

    ![Server-Explorer](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. Sie können eine Data Factory Maustaste und wählen **Exportieren "Data Factory" Neues Projekt** eine Visual Studio-Projekt basierend auf einer vorhandenen Daten zu erstellen.

    ![Exportieren von Daten factory](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a>Aktualisieren von Data Factory-Tools für Visual Studio

Aktualisieren von Azure Data Factory-Tools für Visual Studio folgendermaßen Sie vor:

1. Klicken Sie im Menü auf **Extras** , und wählen Sie **Erweiterungen und Updates**.
2. Wählen Sie **Updates** im linken Fensterbereich und dann **Visual Studio Gallery**.
3. Wählen Sie **Azure Data Factory-Tools für Visual Studio** , und klicken Sie auf **Aktualisieren**. Wenn dieser Eintrag nicht angezeigt wird, verfügen Sie bereits über die neueste Version der Tools. 

## <a name="use-configuration-files"></a>Konfigurationsdateien verwenden
Konfigurationsdateien in Visual Studio können Sie Eigenschaften für verknüpfte Services/Tabellen/Pipelines für jede Umgebung unterschiedlich konfigurieren. 

Betrachten Sie die folgende JSON-Definition für einen Dienst Azure Storage verknüpft. An **ConnectionString** mit unterschiedlichen Werten für Kontoname und Accountkey Umgebung (Test/Dev/Produktion), Data Factory Entitäten bereitstellen. Dies erreichen mit separaten Konfigurationsdatei für jede Umgebung. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "description": "",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    } 

### <a name="add-a-configuration-file"></a>Eine Konfigurationsdatei
Fügen Sie eine Konfigurationsdatei für jede Umgebung mit den folgenden Schritten:   

1. Maustaste Data Factory-Projekt in der Visual Studio-Projektmappe, auf **Hinzufügen**, und klicken Sie auf **Neues Element**.
2. Wählen Sie aus der Liste der installierten Vorlagen auf der linken Seite **Config** wählen Sie **Konfigurationsdatei aus**, geben Sie einen **Namen** für die Konfigurationsdatei und klicken Sie auf **Hinzufügen**.

    ![Konfigurationsdatei](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. Parameter und ihre Werte im folgenden Format hinzuzufügen.

        {
            "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
            "AzureStorageLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            ],
            "AzureSqlLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            ]
        }

    Das folgende Beispiel konfiguriert ConnectionString-Eigenschaft ein Azure Storage verknüpft und einer Azure SQL verknüpft. Beachten Sie, dass die Syntax zur Angabe von Namen [JsonPath](http://goessner.net/articles/JsonPath/).   

    Wenn JSON eine Eigenschaft hat, die ein Array von Werten, wie im folgenden Code dargestellt:  

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
    
    Konfigurieren Sie die Eigenschaften wie in der folgenden Konfigurationsdatei (mit nullbasierter Indizierung): 
        
        {
            "name": "$.properties.structure[0].name",
            "value": "FirstName"
        }
        {
            "name": "$.properties.structure[0].type",
            "value": "String"
        }
        {
            "name": "$.properties.structure[1].name",
            "value": "LastName"
        }
        {
            "name": "$.properties.structure[1].type",
            "value": "String"
        }

### <a name="property-names-with-spaces"></a>Namen mit Leerzeichen
Verwenden Sie ein Eigenschaftenname Leerzeichen enthält, Klammern wie im folgenden Beispiel (Datenbank-Servername): 

     {
         "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
         "value": "MyAsqlServer.database.windows.net"
     }


### <a name="deploy-solution-using-a-configuration"></a>Bereitstellen der Lösung mit einer Konfiguration
Beim Veröffentlichen von Azure Data Factory Entitäten in VS können Sie die Konfiguration, die Sie diesem Vorgang verwenden möchten. 

Elemente in einer Konfigurationsdatei verwenden Azure Data Factory Projekt veröffentlichen:   

1. Data Factory Projekt und **Veröffentlichen** , um das Dialogfeld **Elemente veröffentlichen** angezeigt. 
2. Wählen Sie eine vorhandene Data Factory oder geben Sie Werte für eine Data Factory auf **Configure Data Factory** erstellen und klicken Sie auf **Weiter**.   
3. Auf der Seite **Elemente veröffentlichen** : eine Dropdownliste mit verfügbaren Konfigurationen für das Konfigurationsfeld **Bereitstellung auswählen** angezeigt.

    ![Konfigurationsdatei auswählen](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)

4. Wählen Sie die **Konfigurationsdatei** , die Sie möchten, und klicken Sie auf **Weiter**. 
5. Bestätigen Sie, dass der Name des JSON-Datei auf der Seite **Zusammenfassung** , und klicken Sie auf **Weiter**. 
6. Klicken Sie auf **Fertig stellen** , nachdem die Bereitstellung abgeschlossen ist. 

Beim Bereitstellen, werden die Werte aus der Konfigurationsdatei verwendet Werte für Eigenschaften in JSON-Dateien für Data Factory Entitäten festgelegt, bevor die Entitäten Azure Data Factory-Dienst bereitgestellt werden.   

## <a name="summary"></a>Zusammenfassung 
In diesem Lernprogramm wird eine Azure Data Factory Prozessdaten mit Hive-Skript in einem Cluster HDInsight Hadoop erstellt. Die Factory-Editor wird in Azure-Portal, folgendermaßen verwendet:  

1.  Ein Azure **Data Factory**erstellt.
2.  Erstellt zwei **verknüpften Diensten**:
    1.  **Azure** verknüpfter Dienst Ihre Azure BLOB-Speicher zu verknüpfen, die Fabrik Daten Eingabe/Ausgabe-Dateien enthält.
    2.  **Azure HDInsight** auf Anforderung verknüpften Serviceartikel einen bedarfsgesteuerten HDInsight Hadoop Cluster Data Factory verknüpfen. Azure Data Factory erstellt einen HDInsight Hadoop Cluster just-in-Time Eingabedaten und Ausgabe Daten. 
3.  Erstellt zwei **Datasets**, die Eingabe- und Daten für HDInsight Struktur Aktivität in der Pipeline. 
4.  Erstellt eine **Rohrleitung** mit einer **HDInsight Struktur** .  


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie eine Rohrleitung mit einer Transformation (HDInsight-Aktivität) erstellt, die einen HDInsight-Cluster bei Bedarf Hive-Skript ausgeführt wird. Wie Sie eine Aktivität kopieren Daten auf Azure SQL Azure Blob kopieren finden Sie unter [Tutorial: Daten aus einer Azure, SQL Azure BLOB-](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
  
## <a name="see-also"></a>Siehe auch
| Thema | Beschreibung |
| :---- | :---- |
| [Aktivitäten für die Transformation von Daten](data-factory-data-transformation-activities.md) | Dieser Artikel enthält eine Liste der Data Transformationsaktivitäten (z. B. HDInsight Struktur Transformation in diesem Lernprogramm verwendeten) von Azure Data Factory unterstützt. | 
| [Planung und Ausführung](data-factory-scheduling-and-execution.md) | Dieser Artikel beschreibt die Planung und Ausführung Aspekte der Azure Data Factory-Anwendungsmodell. |
| [Rohrleitungen](data-factory-create-pipelines.md) | Dieser Artikel hilft Pipelines und Aktivitäten in Azure Data Factory und wie sie End-to-End datengesteuerten Workflows für Ihre Situation oder Unternehmen erstellen. |
| [Datasets](data-factory-create-datasets.md) | Dieser Artikel hilft Ihnen zu Datasets in Azure Data Factory.
| [Überwachen und Verwalten von Rohrleitungen App überwachen](data-factory-monitor-manage-app.md) | Dieser Artikel beschreibt das Überwachen, verwalten und Rohrleitungen überwachen und Anwendung debuggen. 
