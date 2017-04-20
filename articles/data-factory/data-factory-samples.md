<properties     
    pageTitle="Azure Data Factory - Beispiele" 
    description="Enthält Details zu liefern Beispiele mit dem Azure Data Factory-Dienst." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---samples"></a>Azure Data Factory - Beispiele

## <a name="samples-on-github"></a>Beispiele für GitHub
[GitHub Azure-DataFactory Repository](https://github.com/azure/azure-datafactory) enthält mehrere Beispiele, mit denen Sie schnell abheben mit Azure Data Factory (oder) ändern Sie die Skripts und in eigenen Anwendung verwenden. Der Ordner Samples\JSON enthält JSON Ausschnitte für Szenarien.

| Beispiel | Beschreibung |
| :----- | :---------- | 
| [ADF Exemplarische Vorgehensweise](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) | Dieses Beispiel stellt eine End-to-End Exemplarische Vorgehensweise für die Verarbeitung mit Azure Data Factory Daten aus Protokolldateien Erkenntnisse zu aktivieren. <br/><br/>Data Factory-Pipeline in dieser exemplarischen Vorgehensweise sammelt Beispiel anmeldet, Prozesse und Daten aus Protokollen mit Referenzdaten bereichert und transformiert die Daten, um die Wirksamkeit einer Marketingkampagne, die zuletzt gestartet wurde. |
| [JSON-Beispiele](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) | Dieses Beispiel stellt JSON Beispiele für Szenarien. | 
| [HTTP-Daten Downloader-Beispiel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) | Dieses Beispiel zeigt Herunterladen von Daten von einem HTTP-Endpunkt auf Azure BLOB-Speicher mit benutzerdefinierten .NET Aktivität. |
| [Cross-AppDomain Dot Net Aktivität Beispiel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) | In diesem Beispiel können Sie eine benutzerdefinierte Aktivität .NET erstellen, die keine Assemblyversionen der ADF Launcher (z. B. WindowsAzure.Storage v4.3.0 Newtonsoft.Json v6.0.x usw.) beschränkt ist. |
| [Skript R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |  Dieses Beispiel enthält die benutzerdefinierte Aktivität Data Factory, die verwendet werden kann, um RScript.exe aufzurufen. Dieses Beispiel funktioniert nur mit eigenen (nicht auf Anforderung) HDInsight, die bereits R installiert ist. |
| [Spark-Aufträge HDInsight Hadoop Cluster aufrufen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) | In diesem Beispiel veranschaulicht, wie mit MapReduce Aktivität Spark-Programm aufzurufen. Spark-Programm kopiert nur Daten aus einem Azure BLOB-Container in einen anderen. |
| [Azure Machine Learning Bewertung Aktivität mit Twitter](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) | Dieses Beispiel veranschaulicht, wie mit AzureMLBatchScoringActivity ein Azure Machine Learning-Modell aufrufen, die Twitter stimmungsanalyse, Bewertung, Vorhersagen usw. führt. |
| [Benutzerdefinierte Aktivität mit Twitter](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |  Dieses Beispiel veranschaulicht, wie mit einer benutzerdefinierten Aktivität .NET Azure Machine Learning-Modell aufrufen, die Twitter stimmungsanalyse, Bewertung, Vorhersagen usw. führt. |
| [Parametrisierte Pipelines für Azure maschinelles lernen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) | Enthält einen End-to-End C# Code N Pipelines bewerten und mit anderen Region Parameter, die Liste der Regionen eine parameters.txt stammt von diesem Beispiel, Umschulung bereitstellen. | 
| [Referenz für Azure Stream Analytics stellen Daten aktualisieren](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |  Dieses Beispiel veranschaulicht, wie mit Azure Data Factory und Azure Stream Analytics zusammen mit Daten Abfragen und aktualisieren für Verweis auf einen Zeitplan einrichten. |
| [Hybrid-Pipeline mit lokalen Hortonworks Hadoop](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) | Das Beispiel verwendet einen lokale Hadoop Cluster als Ziel Compute wie andere Ziele Compute Sie fügen wie ein HDInsight Hadoop Cluster Cloud Aufträge im Data Factory ausgeführt. |
| [JSON-Konvertierungstool](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) | Dieses Tool ermöglicht JSONs Version 2015-07-01-Vorschau zu neuesten 2015-07-01-Vorschau (Standard) konvertieren. |  
| [U-SQL-Beispiel einer Eingabedatei](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |  Diese Datei ist eine Beispieldatei U-SQL-Aktivität verwendet. | 

## <a name="azure-resource-manager-templates"></a>Azure Ressourcenmanager Vorlagen
Die folgenden Vorlagen Azure-Ressourcen-Manager finden Sie auf Github für Daten. 

| Vorlage | Beschreibung |
| -------- | ----------- | 
| [Kopieren von Azure BLOB-Speicher in Azure SQL-Datenbank](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) | Bereitstellen von dieser Vorlage erstellt eine Azure Data Factory mit eine Pipeline, die Daten aus dem angegebenen Azure blobspeicher mit SQL Azure-Datenbank kopiert |    
| [Kopieren von Salesforce in Azure BLOB-Speicher](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) | Bereitstellen von dieser Vorlage erstellt eine Azure Data Factory mit eine Pipeline, die Daten aus Salesforce-Kontoname in Azure BLOB-Speicher kopiert. |    
| [Transformieren von Daten mit Hive-Skript in einem Cluster Azure HDInsight](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) | Bereitstellen von dieser Vorlage erstellt eine Azure Data Factory mit eine Pipeline, die Daten transformiert das Beispielskript Struktur auf Azure HDInsight Hadoop-Cluster ausgeführt. | 


## <a name="samples-in-azure-portal"></a>Muster in der Azure-portal
Auf der Homepage Ihrer Data Factory können Kachel **Beispiel Pipelines** Sie Beispiel Pipelines und die zugeordneten Entitäten (Datasets und verknüpften Diensten) im Werk Daten bereitstellen. 

1. Erstellen Sie Data Factory oder öffnen Sie eine vorhandene Data Factory. Finden Sie unter [Erste Schritte mit Azure Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#CreateDataFactory) Schritte Data Factory erstellt.
2. Blatt **DATA FACTORY** für die Data Factory klicken Sie **Beispiel Rohrleitungen** .

    ![Pipelines Beispielkachel](./media/data-factory-samples/SamplePipelinesTile.png)

2. Klicken Sie im **Beispiel Pipelines** Blade **Beispiel** , das Sie bereitstellen möchten. 
    
    ![Beispiel-Pipelines blade](./media/data-factory-samples/SampleTile.png)

3. Festlegen Sie Konfigurationen für das Beispiel. Beispielsweise Ihre Azure-Konto und Speicherschlüssel Azure SQL-Servername, Datenbank, Benutzer-ID und Kennwort usw. 

    ![Beispiel-blade](./media/data-factory-samples/SampleBlade.png)

4. Danach werden mit Konfigurationsinformationen angeben, klicken Sie auf **Erstellen** erstellen/Beispiel Pipelines und verknüpften Dienste/Tabellen durch die Rohrleitungen bereitstellen.
5. Den Status der Bereitstellung auf bereits auf die **Probe Rohrleitungen** angeklickte Beispielkachel angezeigt.

    ![Bereitstellungsstatus](./media/data-factory-samples/DeploymentStatus.png)

6. Schließen Sie die **Bereitstellung war erfolgreich** auf der Kachel das Beispiel Meldung **Beispiel Pipelines** Blade.  
5. Blatt **DATA FACTORY** sehen Sie verknüpfte Dienste, Daten und Pipelines Werk Daten hinzugefügt werden.  

    ![Data Factory blade](./media/data-factory-samples/DataFactoryBladeAfter.png)
   
## <a name="samples-in-visual-studio"></a>Beispiele in Visual Studio

### <a name="prerequisites"></a>Erforderliche Komponenten

Sie müssen Folgendes auf Ihrem Computer installiert: 

- Visual Studio 2013 oder Visual Studio 2015
- Azure SDK für Visual Studio 2013 oder Visual Studio 2015 herunterladen Wechseln Sie [Azure-Downloadseite](https://azure.microsoft.com/downloads/) und auf **VS 2013** oder **VS 2015** im Abschnitt **.NET** .
- Die neuesten Azure Data Factory-Plug-In für Visual Studio herunterladen: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) oder [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Wenn Sie Visual Studio 2013 verwenden, Sie können auch aktualisieren Plug-in folgendermaßen: Klicken Sie im Menü auf **Extras** -> **Extensions und Updates** -> **Online** -> **Visual Studio Gallery** -> **Microsoft Azure Data Factory-Tools für Visual Studio** -> **Aktualisieren**.

### <a name="use-data-factory-templates"></a>Factory Datenvorlagen verwenden

1. Klicken Sie im Menü auf **Datei** , zeigen Sie auf **neu**und klicken Sie auf **Projekt**. 
2. Führen Sie im Dialogfeld **Neues Projekt** die folgenden Schritte aus: 
    1. **DataFactory** unter **Vorlagen**auswählen 
    2. Wählen Sie **Factory Datenvorlagen** im rechten Fensterbereich. 
    3. Geben Sie einen **Namen** für das Projekt. 
    4. Wählen Sie einen **Speicherort** für das Projekt. 
    5. Klicken Sie auf **OK**. 

    ![Dialogfeld Neues Projekt](./media/data-factory-samples/vs-new-project-adf-templates.png)
6. Klicken Sie im Dialogfeld **Datenvorlagen Factory** Abschnitt **Anwendungsfall Vorlagen** wählen Sie die Vorlage aus und auf **Weiter**. Die folgenden Schritte gehen mit der Vorlage **Customer Profiling** . Die Beispiele beschriebenen Schritte. 

    ![Dialogfeld Factory Vorlagen](./media/data-factory-samples/vs-data-factory-templates-dialog.png) 
7. Klicken Sie im Dialogfeld **Konfiguration Daten** auf der Seite **Data Factory Grundlagen** auf **Weiter** .
8. Gehen Sie auf der Seite **Konfigurieren "Data Factory"** folgendermaßen vor: 
    1. Wählen Sie **neue Data Factory erstellen**. Sie können auch **vorhandene Daten Factory verwenden**.
    2. Geben Sie einen **Namen** für die Daten-Factory.
    3. Wählen Sie den **Azure-Abonnement** Data Factory erstellt werden soll. 
    4. Wählen Sie die **Ressourcengruppe** für Daten-Factory.
    5. **Westen der USA**, **USA Osten**oder **Nordeuropa** **Region**auswählen
    6. Klicken Sie auf **Weiter**. 
9. Geben Sie auf der Seite **Konfigurieren Datenspeicher** vorhandenen **Azure SQL-Datenbank** und **Azure Storage-Konto an** (oder) erstellen Sie Datenbank-Speicherung und auf Weiter. 
10. Wählen Sie auf der Seite **Konfigurieren berechnen** Standardwerte aus und auf **Weiter**. 
11. Überprüfen Sie auf der Seite **Zusammenfassung** alle Einstellungen, und klicken Sie auf **Weiter**. 
12. Warten Sie auf der Seite **Bereitstellung** , bis die Bereitstellung abgeschlossen ist, und klicken Sie auf **Fertig stellen**.
13. Klicken Sie Projekt im Projektmappen-Explorer, und klicken Sie auf **Veröffentlichen**. 
19. Dialogfeld **Melden Sie sich bei Ihrem Microsoft-Konto** anzuzeigen, geben Sie Ihre Anmeldeinformationen für das Konto Azure-Abonnement und klicken Sie auf **Anmelden**.
20. Das Dialogfeld sollte angezeigt werden:

    ![Veröffentlichen (Dialogfeld)](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Klicken Sie auf **Configure Data Factory** folgendermaßen Sie vor: 
    1. Bestätigen Sie die Option **mit vorhandenen Daten Factory** .
    2. Wählen Sie **"Data Factory"** , wählen Sie hatte, als die Vorlage. 
    6. Klicken Sie auf **nächste** Seite **Veröffentlichen Elemente** wechseln. (Drücken Sie **Registerkarte** aus dem Namen zu verschieben, wenn die Schaltfläche **Weiter** deaktiviert.) 
23. Seite **Veröffentlichen Elemente** sicher, dass alle Daten-Factorys Elemente ausgewählt sind, und klicken Sie auf **Weiter** auf der Seite **Zusammenfassung** zu wechseln.     
24. Überprüfen Sie die Zusammenfassung, und klicken Sie auf **Weiter** um den Bereitstellungsprozess zu starten und den **Bereitstellungsstatus**anzeigen.
25. Auf der Seite **Bereitstellung** erhalten Sie den Status des Bereitstellungsprozesses. Klicken Sie auf Fertig stellen, nachdem die Bereitstellung abgeschlossen ist. 

Details zur Verwendung von Visual Studio Data Factory Entitäten erstellen und Azure veröffentlichen finden Sie unter [erstellen Ihre erste "Data Factory" (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) .          