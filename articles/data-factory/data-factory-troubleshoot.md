<properties 
    pageTitle="Problembehandlung bei Azure Data Factory" 
    description="Informationen Sie zu Problemen mit Azure Data Factory." 
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
    ms.topic="article" 
    ms.date="08/31/2016" 
    ms.author="spelluru"/>

# <a name="troubleshoot-data-factory-issues"></a>Problembehandlung bei Data Factory
Dieser Artikel enthält Tipps zur Fehlerbehebung für Probleme bei Verwendung von Azure Data Factory. Dieser Artikel listet nicht alle möglichen Probleme bei Verwendung des Diensts, aber einige Probleme und Tipps zur Fehlerbehebung behandelt.   

## <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung

### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Fehler: Das Abonnement ist nicht registriert Namespace 'Microsoft.DataFactory' verwenden
Wenn Sie diese Fehlermeldung erhalten, wurde der Ressourcenanbieter Azure Data Factory nicht auf Ihrem Computer registriert. Folgendermaßen Sie vor: 

1. Starten Sie Azure PowerShell. 
2. Melden Sie sich bei Ihrem Azure-Konto mit dem folgenden Befehl.
        Login-AzureRmAccount 
3. Führen Sie den folgenden Befehl Azure Data Factory-Anbieter registrieren.
        Register-AzureRmResourceProvider - ProviderNamespace Microsoft.DataFactory

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Problem: Nicht autorisierte Fehler beim Data Factory-Cmdlet ausgeführt
Sie verwenden wahrscheinlich nicht direkt ein Azure-Konto oder Abonnement Azure PowerShell. Mithilfe der folgenden Cmdlets rechts Azure-Konto und mit PowerShell Azure-Abonnement aus. 

1. Login-AzureRmAccount - mit richtigen Benutzer-ID und Kennwort
2. Get-AzureRmSubscription - alle Abonnements für das Konto anzeigen. 
3. Wählen Sie AzureRmSubscription &lt;Namen&gt; -wählen Sie das richtige Abonnement. Verwenden Sie dasselbe Data Factory Azure-Portal erstellen verwenden.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Problem: Nicht von Azure-Portal Data Management Gateway Express Setup starten
Express-Setup für das Daten-Management Gateway erfordert Internet Explorer oder einem kompatiblen Webbrowser Microsoft ClickOnce. Wenn die Express-Installation nicht starten, führen Sie eine der folgenden: 

- Verwenden Sie Internet Explorer oder einem kompatiblen Webbrowser Microsoft ClickOnce.

    Bei Verwendung von Chrome wechseln Sie [Chrome Webstore](https://chrome.google.com/webstore/)"ClickOnce" Schlüsselwort suchen und wählen Sie eine ClickOnce-Erweiterung installieren 
    
    Führen Sie dasselbe für Firefox (Installation hinzufügen-in). Klicken Sie Menü öffnen auf der Symbolleiste (drei horizontale Linien oben rechts), klicken Sie auf Add-Ons "ClickOnce" Schlüsselwort suchen Sie, wählen Sie eine ClickOnce-Erweiterung und installieren. 

- Verwenden Sie den **Manuelle Installation** Link auf demselben Blatt im Portal. Mithilfe dieses Ansatzes Installationsdatei herunterzuladen und manuell ausführen. Wenn die Installation abgeschlossen ist, wird das Dialogfeld Data Management Gateway-Konfiguration angezeigt. Kopieren Sie den **Schlüssel** vom Portal Bildschirm und im Konfigurations-Manager verwenden Sie, um das Gateway Service manuell registrieren.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Problem: Bei lokalen SQL Server-Verbindung herstellen 
Starten Sie **Data Management Gateway Configuration Manager** auf dem Gatewaycomputer und verwenden Sie die Registerkarte **Problembehandlung** zum Testen der Verbindung zu SQL Server auf dem Gatewaycomputer. Tipps zur Behebung von Verbindungs-Gateways Siehe [Problembehandlung Gateway stellt](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Fragen.   
 

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Problem: Eingabe Slices sind immer auf Status

Segmente möglicherweise **Warten** Zustand aus verschiedenen Gründen. Einer der Gründe ist **externe** Eigenschaft nicht auf **true**festgelegt ist. Jeder Anwendungsbereich Azure Data Factory erzeugte Dataset sollte mit **externen** Eigenschaft gekennzeichnet werden. Diese Eigenschaft gibt an, dass die Daten externer und von Pipelines im Werk Daten nicht unterstützt. Die Datensegmente werden **als** markiert, sobald Daten in den entsprechenden Speicher verfügbar ist. 

Im folgende Beispiel für die Verwendung der **externen** Eigenschaft anzeigen Sie können optional **ExternalData*** externe auf True festlegen.

Weitere Informationen zu dieser Eigenschaft finden Sie unter [Datasets](data-factory-create-datasets.md) Artikel.
    
    {
      "name": "CustomerTable",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "MyLinkedService",
        "typeProperties": {
          "folderPath": "MyContainer/MySubFolder/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          }
        }
      }
    }

Zum Beheben des Fehlers JSON-Definition der Eingabetabelle **externe** Eigenschaft und optionale **ExternalData** Abschnitt hinzu, und erstellen Sie die Tabelle. 

### <a name="problem-hybrid-copy-operation-fails"></a>Problem: Hybrid-Kopiervorgang schlägt fehl
Anzeigen Sie Schritte zur Problembehandlung beim Kopieren in einen lokalen Datenspeicher Data Management Gateway [Gateway Problembehandlung](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) 

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Problem: Bei Bedarf HDInsight Bereitstellung fehlschlägt
Bei verknüpften Dienst vom Typ HDInsightOnDemand müssen Sie ein Nameverknüpfterdienst angeben, die auf Azure BLOB-Speicher. Data verwendet Factory diesen Speicher zum Speichern von Protokollen und Hilfsdateien für Ihren Bedarf HDInsight.  Manchmal Bereitstellung eines Clusters HDInsight auf Anforderung schlägt fehl mit Fehler:

        Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

Dieser Fehler gibt normalerweise an, dass die Position in der Nameverknüpfterdienst angegebenen Speicherkonto nicht an der gleichen Data Center geschieht, HDInsight provisioning. Beispiel: Wenn Daten Werk im Westen der USA und der Azure-Speicher im südostasiatischen US ist, schlägt die Bereitstellung nach Bedarf im Westen der USA.

Außerdem ist eine zweite JSON-Eigenschaft AdditionalLinkedServiceNames, zusätzliche Speicherkonten in HDInsight-Anforderung angegeben werden können. Diese zusätzliche verknüpfte Speicherkonten sollten an HDInsight-Cluster oder derselbe Fehler fehl.

### <a name="problem-custom-net-activity-fails"></a>Problem: Benutzerdefinierte .NET Aktivität fehlschlägt
Einzelheiten finden Sie unter [Debuggen eine Rohrleitung mit benutzerdefinierten Aktivität](data-factory-use-custom-activities.md#debug-the-pipeline) . 

## <a name="use-azure-portal-to-troubleshoot"></a>Mit der Problembehandlung bei Azure-portal 

### <a name="using-portal-blades"></a>Mithilfe von Portal-blades
Schritte finden Sie unter [Monitor Pipeline](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) . 

### <a name="using-monitor-and-manage-app"></a>Mithilfe von Systemmonitor und App verwalten
Finden Sie [Überwachen und Verwalten von Daten Factory Rohrleitungen überwachen und Verwalten von App](data-factory-monitor-manage-app.md) Informationen. 

## <a name="use-azure-powershell-to-troubleshoot"></a>Mithilfe von Azure PowerShell beheben

### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Problembehandlung mithilfe von Azure PowerShell  
Einzelheiten finden Sie unter [Monitor Data Factory Rohrleitungen Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) . 


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 