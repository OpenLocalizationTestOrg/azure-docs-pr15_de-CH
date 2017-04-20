<properties
    pageTitle="Erstellen Ihrer erste Data Factory (Azure Portal) | Microsoft Azure"
    description="In diesem Lernprogramm erstellen Sie eine Beispiel Azure Data Factory-Pipeline mit Factory im Azure-Portal."
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
    ms.date="09/14/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Lernprogramm: Erstellen Sie Ihrer erste Azure Data Factory mit Azure-portal
> [AZURE.SELECTOR]
- [Übersicht und Komponenten](data-factory-build-your-first-pipeline.md)
- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Ressourcen-Manager-Vorlage](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

In diesem Artikel erfahren Sie, wie Sie [Azure-Portal](https://portal.azure.com/) erstellen Sie Ihre erste Azure Data Factory. 

## <a name="prerequisites"></a>Erforderliche Komponenten        
1. [Lernprogramm](data-factory-build-your-first-pipeline.md) Artikel lesen und die **erforderliche** Schritte.
2. Dieser Artikel bietet keine Übersicht Azure Data Factory-Dienst. [Einführung in Azure Data Factory](data-factory-introduction.md) Artikel eine detaillierte Übersicht des Dienstes zu empfohlen.  

## <a name="create-data-factory"></a>Data Factory erstellen
Eine Factory Daten können eine oder mehrere Rohrleitungen. Eine Pipeline kann eine oder mehrere Aktivitäten enthalten. Z. B. eine Aktivität kopieren Kopieren von Daten aus einer Quelle zu einem Ziel-Datenspeicher und eine HDInsight Struktur Aktivität Hive-Skript zum Transformieren von Eingabedaten Produkt-Ausgabedaten. Beginnen wir mit die Data Factory in diesem Schritt erstellen. 

1.  Auf der [Azure-Portal](https://portal.azure.com/)anmelden.
2.  Klicken Sie im linken Menü auf **neu** und anschließend auf **Daten + Analytik**, **Data Factory**.
        
    ![Blade erstellen](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)

2.  Geben Sie in das **neue Data Factory** -Blade **GetStartedDF** ein.

    ![Neue Data Factory blade](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

    > [AZURE.IMPORTANT] 
    > Der Name der Azure Data Factory muss **global eindeutig**sein. Wenn Sie die Fehlermeldung: **Data Factory Name "GetStartedDF" ist nicht verfügbar**. Ändern Sie den Namen der Data Factory (z. B. YournameGetStartedDF), und versuchen Sie es erneut erstellen. Benennungskonventionen für Artefakte Data Factory finden Sie [Data Factory - Namenskonventionen](data-factory-naming-rules.md) .
    > 
    > Der Name der Factory Daten möglicherweise als einen **DNS-** Namen werden in der Zukunft und damit öffentlich sichtbar registriert werden.

3.  Wählen Sie den **Azure-Abonnement** Data Factory erstellt werden soll. 
4.  Wählen Sie vorhandene **Ressourcengruppe** oder erstellen Sie eine Ressourcengruppe. Erstellen Sie für das Lernprogramm für eine Ressourcengruppe mit dem Namen: **ADFGetStartedRG**. 
5.  Klicken Sie auf das **neue Data Factory** auf **Erstellen** .

    > [AZURE.IMPORTANT] Um Data Factory-Instanzen erstellen, muss ein Mitglied der [Data Factory](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) Beitragendenrolle auf Abonnement-Ressource sein. 
6.  Sehen Sie die "Data Factory" im **Startmenü** der Azure-Portal erstellt werden:   

    ![Auslieferungszustand Daten erstellen](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
7. Herzlichen Glückwunsch! Erstellt die erste Data factory Nachdem die Data Factory erfolgreich erstellt wurde, anzeigen die Factory Datenseite der Inhalt der Data factory   

    ![Data Factory blade](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Vor dem Erstellen einer Pipeline in der Data Factory, müssen Sie zunächst einige Data Factory Entitäten erstellen. Zuerst erstellen Sie verknüpfte Dienste verknüpfen Daten Speicher/berechnet dem Datenspeicher, definieren Input und output Datasets Eingabe/Ausgabe Daten in verknüpften Datenspeichern und dann die Pipeline mit einer Aktivität, die diese Datasets verwendet. 

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten
In diesem Schritt verknüpfen Sie Ihr Konto Azure-Speicher und einer bedarfsgesteuerten Azure HDInsight Cluster mit Daten Werk. Azure Storage-Konto enthält die Eingabe- und Daten für die Pipeline in diesem Beispiel. Der Dienst HDInsight verknüpft zur Skript Struktur in der Aktivität der Pipeline in diesem Beispiel angegeben. Identifizieren, welche [Datenspeicher](data-factory-data-movement-activities.md)/[Services](data-factory-compute-linked-services.md) in Ihrem Szenario verwendet und verknüpfen diese Dienste mit Data Factory von verknüpften Diensten erstellen.  

### <a name="create-azure-storage-linked-service"></a>Erstellen von verknüpften Azure Storage-Diensts
In diesem Schritt verknüpfen Sie Ihre Azure Storage-Konto mit Daten Werk. In diesem Lernprogramm verwenden Sie dasselbe Azure Storage-Konto Eingabe/Ausgabe von Daten und die Skriptdatei HQL speichern. 

1.  Klicken Sie auf **Autor und** auf dem Blatt **DATA FACTORY** für **GetStartedDF**. Die Factory-Editor sollte angezeigt werden. 
     
    ![Erstellen und Bereitstellen der Kachel](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2.  Klicken Sie auf **neue Daten zu speichern** , und wählen Sie **Azure-Speicher**.

    ![Neuer Datenspeicher - Azure Storage - Menü](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)

3.  JSON-Skript zum Erstellen einer verknüpften Azure Storage-Diensts im Editor sollte angezeigt werden. 
    
    ![Azure verknüpft Speicherdienst](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
     
4. Ersetzen Sie **Kontoname** mit dem Namen der Azure-Speicher und **Konto-Taste** und der Zugriffstaste des Kontos Azure-Speicher. Wie man die Zugriffstaste Speicher finden Sie unter [anzeigen, kopieren und Zugriffstasten Regenerieren Speicher](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
5. Klicken Sie auf der Befehlsleiste verknüpften Dienst bereitstellen **Bereitstellen** .

    ![Schaltfläche bereitstellen](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Nach der verknüpfte Dienst erfolgreich bereitgestellt, Fenster **Draft 1** sollte verschwinden, und finden Sie unter **AzureStorageLinkedService** in der Strukturansicht auf der linken Seite. 
    ![Verknüpfte Speicherdienst im Menü](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)   

 
### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight verknüpfte Dienst erstellen
In diesem Schritt verknüpfen Sie einen HDInsight-Cluster auf Anforderung mit Daten Werk. HDInsight Cluster automatisch zur Laufzeit erstellt und danach Verarbeitung und im Leerlauf für die angegebene Zeitspanne wird gelöscht. 

1. Klicken Sie im **Editor für Factory** **... Weitere**auf **neu berechnen**, und wählen Sie **bei Bedarf HDInsight-Cluster**.

    ![Neu berechnen](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Kopieren Sie und fügen Sie die folgenden Codeausschnitt zum Fenster **Draft 1 ein** . JSON-Ausschnitt beschreibt die Eigenschaften, mit denen HDInsight Cluster auf Anforderung erstellen. 

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService"
            }
          }
        }
    
    Die folgende Tabelle enthält eine Beschreibung der JSON-Eigenschaften im Ausschnitt verwendet:
    
  	| Eigenschaft | Beschreibung |
  	| :------- | :---------- |
  	| Version | Gibt an, dass die Version der HDInsight 3.2 erstellt. | 
  	| ClusterSize | Gibt die Größe des HDInsight-Clusters. | 
  	| TimeToLive | Gibt an, dass die Leerlaufzeit für den HDInsight-Cluster, bevor es gelöscht wird. |
  	| Nameverknüpfterdienst | Gibt das Speicherkonto, mit der von HDInsight generierten Protokolle speichern. |

    Beachten Sie folgende Punkte: 
    
    - Der erstellt eines **Windows-basierten** HDInsight Clusters mit JSON. Sie können auch einen **Linux-basierten** HDInsight Cluster erstellen lassen. Einzelheiten finden Sie [Bei Bedarf HDInsight verknüpften Serviceartikel](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
    - **HDInsight-Cluster** können anstelle eines bedarfsgesteuerten HDInsight Clusters. Details finden Sie in der [Verknüpften HDInsight-Dienst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
    - HDInsight-Cluster erstellt einen **Standardcontainer** im BLOB-Speicher, in JSON (**Nameverknüpfterdienst angegeben**). HDInsight löscht keine Container, wenn Cluster gelöscht wird. Dieses Verhalten ist entwurfsbedingt. Auf Anforderung verknüpft HDInsight Service wird HDInsight Cluster ein Slice verarbeitet, es sei denn ein vorhandener live Cluster (**TimeToLive**) erstellt. Der Cluster wird automatisch gelöscht wird.
    
        Mehrere Segmente verarbeitet werden, sehen Sie viele Container im Azure BLOB-Speicher. Benötigen Sie nicht diese für die Problembehandlung der Aufträge, möchten Sie löschen, um die Speicherkosten zu senken. Die Namen dieser Container ein Muster folgen: "Adf**Yourdatafactoryname**-**nameverknüpfterdienst**- datumuhrzeitstempel". Verwenden Sie Tools wie [Microsoft Storage Explorer](http://storageexplorer.com/) Container im Azure BLOB-Speicher löschen.

    Einzelheiten finden Sie [Bei Bedarf HDInsight verknüpften Serviceartikel](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) .
3. Klicken Sie auf der Befehlsleiste verknüpften Dienst bereitstellen **Bereitstellen** . 

    ![Auf Anforderung verknüpft HDInsight Service bereitstellen](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)

4. Vergewissern Sie, dass **AzureStorageLinkedService** und **HDInsightOnDemandLinkedService** in der Struktur auf der linken Seite angezeigt.

    ![Strukturansicht mit verknüpften Diensten](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Datasets erstellen
In diesem Schritt erstellen Sie Datasets Eingabe und Ausgabedaten für die Verarbeitung der Struktur. Diese Datasets finden Sie in der **AzureStorageLinkedService** , die Sie zuvor in diesem Lernprogramm erstellt haben. Verknüpfte Dienstpunkte Azure Storage-Konto als Datasets angeben im Speicher, die Eingabe enthält Container, Ordner, Dateiname und Ausgabedaten.   

### <a name="create-input-dataset"></a>Eingabe-Dataset erstellen

1. Klicken Sie im **Editor für Factory** **... Weitere** auf der Befehlsleiste klicken Sie auf **Neues Dataset**und **Azure BLOB-Speicher**.

    ![Neues dataset](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Kopieren und Einfügen im folgenden Codeausschnitt zum Fenster Draft 1. Im Ausschnitt JSON erstellen Sie ein Dataset namens **AzureBlobInput** , die Eingabedaten für eine Aktivität in der Pipeline darstellt. Darüber hinaus geben Sie, dass die eingegebenen Daten im BLOB-Container namens **Adfgetstarted** und Ordner **Inputdata**befindet.
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
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
  	| Nameverknüpfterdienst | bezieht sich auf die AzureStorageLinkedService, die Sie zuvor erstellt haben. |
  	| Dateiname | Diese Eigenschaft ist optional. Wenn diese Eigenschaft nicht angeben, werden alle Dateien aus dem FolderPath entnommen. In diesem Fall wird die input.log verarbeitet. |
  	| Typ | Die Protokolldateien werden im Textformat TextFormat verwendet. | 
  	| columnDelimiter | Spalten in den Protokolldateien werden durch Komma (,) getrennt. |
  	| / Intervall | Frequenz Monat und Intervall ist 1 bedeutet, dass die Eingabe Segmente monatlich. | 
  	| externe | Diese Eigenschaft wird festgelegt auf true, wenn die eingegebenen Daten nicht vom Data Factory-Dienst generiert werden. | 
        
3. Klicken Sie auf der Befehlsleiste auf das neu erstellte Dataset bereitstellen **Bereitstellen** . Sie sollten das Dataset in der Strukturansicht auf der linken Seite sehen. 


### <a name="create-output-dataset"></a>Ausgabedataset erstellen
Erstellen Sie jetzt das ausgabedataset stellen die Ausgabedaten in Azure BLOB-Speicher gespeichert. 

1. Klicken Sie im **Editor für Factory** **... Weitere** auf der Befehlsleiste klicken Sie auf **Neues Dataset**und **Azure BLOB-Speicher**.  
2. Kopieren und Einfügen im folgenden Codeausschnitt zum Fenster Draft 1. Im Ausschnitt JSON erstellen Sie ein Dataset namens **AzureBlobOutput**und die Struktur der Daten, die vom Skript Struktur erzeugt angeben. Darüber hinaus geben Sie die Ergebnisse in der BLOB-Container namens **Adfgetstarted** und Ordner mit dem Namen **Partitioneddata**gespeichert. Abschnitt **Verfügbarkeit** gibt an, dass das ausgabedataset monatlich erstellt wird.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
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
3. Klicken Sie auf der Befehlsleiste auf das neu erstellte Dataset bereitstellen **Bereitstellen** .
4. Stellen Sie sicher, dass das Dataset erstellt wird.

    ![Strukturansicht mit verknüpften Diensten](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a>Pipeline erstellen
In diesem Schritt erstellen Sie Ihre erste Pipeline mit einer **HDInsightHive** . Eingang Scheibe steht monatlich (Häufigkeit: Monat, Intervall: 1) ausgabeslice monatlich erstellt und Planer-Eigenschaft der Aktivität auch monatlich festgelegt. Das ausgabedataset und der Planer Aktivität müssen übereinstimmen. Derzeit treibt ausgabedataset planen, müssen Sie eine ausgabedataset erstellen, wenn die Aktivität keine Ausgabe erzeugt. Wenn die Aktivität Eingaben nicht, überspringen Sie Eingabedatasets erstellen. Am Ende dieses Abschnitts werden die Eigenschaften der folgenden JSON erläutert. 

1. Klicken Sie im **Data Factory-Editor**auf **Auslassungszeichen (...) Weitere Befehle** und klicken Sie dann auf **neue**.
    
    ![Pipeline-Schaltfläche](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Kopieren und Einfügen im folgenden Codeausschnitt zum Fenster Draft 1.

    > [AZURE.IMPORTANT] Ersetzen Sie **Storageaccountname** durch den Namen Ihres Speicherkontos in JSON.
        
        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService",
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
    
    Struktur-Skriptdatei **partitionweblogs.hql**ist in Azure Storage Konto (angegeben durch ScriptLinkedService als **AzureStorageLinkedService**bezeichnet) und im **Skript** -Ordner im Container **Adfgetstarted**gespeichert.

    Abschnitt **definiert** wird verwendet, um die Laufzeit Einstellungen, die Hive-Skript als Struktur Werte übergeben werden (z. B. ${Hiveconf: inputtable}, ${Hiveconf:partitionedtable}).

    Die Eigenschaften **start** und **Ende** der Pipeline gibt die Laufzeit der Pipeline.

    In JSON Aktivität geben Sie dass Hive-Skript auf die Berechnung gemäß der **Nameverknüpfterdienst** - **HDInsightOnDemandLinkedService**ausgeführt wird.

    > [AZURE.NOTE] Details im Beispiel verwendeten JSON-Eigenschaften finden Sie unter [Anatomie einer Rohrleitung](data-factory-create-pipelines.md#anatomy-of-a-pipeline) . 

3. Überprüfen Sie Folgendes: 
    1. **Input.log** -Datei im Ordner **Inputdata** **Adfgetstarted** Container in Azure BLOB-Speicher vorhanden ist
    2. **partitionweblogs.hql** Datei im Ordner **Skripts** **Adfgetstarted** Container in Azure BLOB-Speicher vorhanden ist. Schließen Sie Schritte Voraussetzung im [Lernprogramm (Übersicht)](data-factory-build-your-first-pipeline.md) Wenn Sie diese Dateien nicht angezeigt. 
    3. Bestätigen Sie, dass Sie **Storageaccountname** durch den Namen Ihres Speicherkontos in der Pipeline JSON ersetzt. 
2. Klicken Sie auf der Befehlsleiste zum Bereitstellen der Pipeline **Bereitstellen** . Da ** **start** und** Endzeiten in der Vergangenheit und **IsPaused** auf False festgelegt, führt die Pipeline (Aktivität in der Pipeline) unmittelbar nach der Bereitstellung. 
4. Bestätigen Sie, dass die Rohrleitung in der Strukturansicht angezeigt.

    ![Strukturansicht mit](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
5. Herzlichen Glückwunsch, Sie haben Ihre erste Pipeline erstellt!

## <a name="monitor-pipeline"></a>Monitor-pipeline

### <a name="monitor-pipeline-using-diagram-view"></a>Monitor-Rohrleitung mit der Diagrammansicht

6. Klicken Sie auf **X** -Blades Data Factory-Editor schließen und navigieren zu dem Blatt Data Factory, und klicken Sie auf **Diagramm**.
  
    ![Diagramm-Kachel](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
7. In der Diagrammansicht sehen Sie eine Übersicht der Rohrleitungen und Datasets in diesem Lernprogramm verwendet.
    
    ![Diagramm anzeigen](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png) 
8. Zum Anzeigen aller Aktivitäten in der Pipeline Rohrleitung im Diagramm und offene Pipeline. 

    ![Offene Pipeline-Menü](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
9. Bestätigen Sie, dass die HDInsightHive Aktivität in der Pipeline finden Sie unter. 
  
    ![Ansicht öffnen](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    Um zur vorherigen Ansicht zu wechseln, klicken Sie auf **Data Factory** Breadcrumb-Menü oben. 
10. Doppelklicken Sie im **Diagramm**das Dataset **AzureBlobInput**. Bestätigen Sie, dass das Segment **bereit** . Es dauert ein paar Minuten für das Segment bereit angezeigt. Wenn es nicht geschieht, nachdem Sie einmal warten, finden Sie die Datei (input.log) in den richtigen Container (Adfgetstarted) und den Ordner (Inputdata) platziert.

    ![Eingang Scheibe bereit](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
11. Klicken Sie auf **X** -Blade **AzureBlobInput** schließen. 
12. Doppelklicken Sie im **Diagramm**das Dataset **AzureBlobOutput**. Sie sehen, dass das Segment, das derzeit verarbeitet wird.

    ![DataSet](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
9. Nach Abschluss der Verarbeitung sehen Sie das Segment **bereit** .
    
>[AZURE.IMPORTANT] Erstellung eines Clusters HDInsight auf Anforderung dauert einige Zeit (ca. 20 Minuten). Pipeline **30 Minuten** Segment zu erwarten.    

    ![Dataset](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png) 
    
10. Wenn der Speicherbereich **bereit** ist, überprüfen Sie den Ordner **Partitioneddata** im Container **Adfgetstarted** im BLOB-Speicher für die Ausgabe von Daten.  
 
    ![Ausgabedaten](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
11. Klicken Sie auf die Informationen in einem Blade **Datenslice** darauf.

    ![Segment Datendetails](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
12. Klicken Sie auf eine Aktivität ausführen in der **Liste der Aktivitäten ausgeführt** , um Details über eine Aktivität ausführen (Hive-Aktivität in diesem Szenario) in einem Fenster **Aktivität ausführen Details** anzuzeigen.   

    ![Aktivität ausführen details](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)  
    
    Aus den Protokolldateien finden Sie unter Hive-Abfrage ausgeführt wurde und Statusinformationen. Diese Protokolle sind für die Behandlung von Problemen nützlich.
Siehe [Überwachen und Verwalten von Pipelines mit Azure Portal](data-factory-monitor-manage-pipelines.md) Artikel. 

> [AZURE.IMPORTANT] Die Eingabedatei wird gelöscht, wenn das Segment verarbeitet. Daher ggf. erneut das Segment oder das Lernprogramm erneut hochladen der Eingabedatei (input.log) in den Ordner Inputdata Adfgetstarted Container.

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Monitor-Pipeline mit Monitor & App verwalten
Sie können auch Monitor verwenden und Anwendung zum Überwachen von Pipelines verwalten. Ausführliche Informationen zur Verwendung dieser Anwendung finden Sie unter [Überwachen und Verwalten von Azure Data Factory Rohrleitungen überwachen und Anwendung](data-factory-monitor-manage-app.md).

1. Klicken Sie auf **Überwachen und verwalten** nebeneinander auf der Homepage Ihrer Data Factory.

    ![Überwachen und Verwalten der Kachel](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png) 
2. **Überwachen und verwalten Anwendung**sollte angezeigt werden. Ändern Sie die **Startzeit** und **Endzeit** entsprechend Start (01 04 2016 12:00 Uhr) und Endzeit (04 02 2016 12:00 Uhr) von der Pipeline **Klicken**.

    ![Überwachen und Verwalten der App](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png) 
3. Wählen Sie ein aktivitätenfenster in der Liste **Aktivität** Details anzeigen aus 
    ![Details der Fenster](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)


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

  

