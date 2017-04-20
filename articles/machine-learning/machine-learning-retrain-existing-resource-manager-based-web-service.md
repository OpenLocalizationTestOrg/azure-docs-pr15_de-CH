<properties
    pageTitle="Umschulung einen vorhersehbaren Webdienst | Microsoft Azure"
    description="Informationen Sie zum Trainieren eines Modells und aktualisieren Sie den Webdienst neu ausgebildete Modell in Azure Machine Learning verwenden."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="v-donglo"/>


# <a name="retrain-an-existing-predictive-web-service"></a>Umschulung einen vorhersehbaren Webdienst

Dieses Dokument beschreibt die Umschulung folgendes Szenario:

* Sie haben Training experimentieren und eine vorausschauende Experiment als standardisierten Webdienst bereitgestellt haben.
* Neue Daten, vorbeugende Webdiensts zum Durchführen der Bewertung soll.

Beginnend mit der vorhandenen Webdienst und Versuche, müssen die folgenden Schritte:

1. Aktualisieren Sie das Modell.
    1. Ändern Sie das trainingsexperiment für Web Service Eingaben und Ausgaben.
    2. Bereitstellen Sie Schulung Experiment als Umschulung Web Service.
    3. Verwenden Sie Schulung Experiment Batch Execution Service (BES) zum Trainieren des Modells.
2. Verwenden Sie Azure Machine Learning PowerShell-Cmdlets zum Aktualisieren von vorhersageexperiment.
    1.  Mit der Azure-Ressourcen-Manager-Konto anmelden.
    2.  Abrufen der Webdienstdefinition.
    3.  Exportieren der Webdienstdefinition als JSON.
    4.  Aktualisieren Sie auf Ilearner Blob in JSON.
    5.  Web Service Definition importieren Sie JSON.
    6.  Aktualisieren Sie den Webdienst mit neuen Web Service Definition.

## <a name="deploy-the-training-experiment"></a>Bereitstellen des Experiments Schulung

Zum Bereitstellen des Experiments Schulung als Umschulung Webdienst müssen Sie das Modell Web Service Eingaben und Ausgaben hinzufügen. Durch des Experiments ein *Web Service* -Ausgabemodul mit * [Zug Modell] [ train-model] * Modul aktivieren trainingsexperiment zu in der vorhersageexperiment Verwendung ein neues ausgebildeten Modell. Haben Sie ein Modul *Modell evaluieren* können Sie Ausgabe zu Ergebnisse als Ausgabe anfügen.

So aktualisieren Sie das Experiment Schulung

1. Ein *Web Service* -Eingabemodul an Ihre Dateneingabe (z. B. ein Modul *Clean Missing Data* ) anschließen Normalerweise werden Sie sicherstellen, dass Eingabedaten auf gleiche Weise wie die ursprünglichen Daten verarbeitet werden.
2. Verbinden Sie ein *Web Service* -Ausgabemodul in die Ausgabe des Moduls *Zug Modell* .
3. Wenn Sie ein Modul *Modell ausgewertet* und die Ergebnisse ausgegeben werden sollen, verbinden Sie ein *Web Service* -Ausgabemodul in die Ausgabe des Moduls *Modell ausgewertet* .

Führen Sie den Test.

Anschließend müssen Sie trainingsexperiment als Webdienst bereitstellen, die geschulten Model und Modellergebnisse erzeugt.  

Am unteren Rand der Leinwand Experiment auf **Webdienst,**und wählen Sie **Webdienst bereitstellen [New]**. Das Azure Machine Learning Web Portal öffnet Seite **Webdienst bereitstellen** . Geben Sie einen Namen für den Webdienst, wählen Sie einen Zahlungsplan und klicken Sie auf **Bereitstellen**. Die Batchausführung-Methode können nur für geschulte Modelle erstellen.


## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Das Modell mit neuen Daten mit BES Umschulung

Für dieses Beispiel wird C# verwendet Umschulung Anwendung erstellen. Python oder R Beispielcode können Sie diese Aufgabe.

Umschulung APIs aufrufen:

1. Erstellen Sie eine C# in Visual Studio (**New** > **Projekt** > **Windows-Desktop** > **Console Application**).
2.  Mit Computer Learning Web Services-Portal anmelden.
3.  Klicken Sie auf den Webdienst mit dem Sie arbeiten.
2.  Klicken Sie auf **verwenden**.
3.  Klicken Sie am unteren Rand der Seite **verbrauchen** im Abschnitt **Beispielcode** auf **Stapel**.
5.  C# Beispielcode für die Batchausführung kopieren und Einfügen der Datei Program.cs. Stellen Sie sicher, dass der Namespace erhalten bleibt.

NuGet-Paket Microsoft.AspNet.WebApi.Client, wie die Kommentare hinzufügen. Fügen Sie den Verweis auf Microsoft.WindowsAzure.Storage.dll müssen Sie zuerst die [Clientbibliothek für Azure Storage Services](https://www.nuget.org/packages/WindowsAzure.Storage)installieren.

Der folgende Screenshot zeigt die Seite **verbrauchen** im Azure Machine Learning Web-Portal.

![Verwenden Sie die Seite][1]

### <a name="update-the-apikey-declaration"></a>Aktualisiert die Apikey-Deklaration

Suchen Sie die **Apikey** -Deklaration:

    const string apiKey = "abc123"; // Replace this with the API key for the web service

Suchen Sie im **grundlegenden Verbrauch** Abschnitt der Seite **verbrauchen** den Primärschlüssel und die **Apikey** Deklaration.

### <a name="update-the-azure-storage-information"></a>Azure-Speicher aktualisieren

Der Beispielcode BES eine Datei von einem lokalen Laufwerk (z. B. "C:\temp\CensusIpnput.csv") in den Azure-Speicher, verarbeitet und schreibt die Ergebnisse in Azure-Speicher zurück.  

Um den Azure-Speicher zu aktualisieren, müssen Sie speicherkontonamen, Schlüssel und Containerinformationen für das Speicherkonto von Azure-Verwaltungsportal abrufen sollte, und dann Update verlangten nach dem Test, der resultierende Workflow ausführen ähnlich der folgenden:

![Resultierende Workflow nach][4]NG Werte im Code.

1. Mit klassischen Azure-Portal anmelden.
1. Klicken Sie in der Spalte links auf **Speicher**.
1. Die Liste der Speicherkonten wählen Sie trainierte Modell speichern.
1. Klicken Sie am unteren Rand der Seite auf **Zugriffstasten verwalten**.
1. Kopieren speichern Sie der **Primärschlüssel Zugriff** und schließen Sie das Dialogfeld.
1. Klicken Sie am oberen Rand der Seite auf **Container**.
1. Wählen Sie einen vorhandenen Container oder erstellen Sie einen neuen und speichern Sie den Namen.

Suchen Sie die *StorageAccountName*, *StorageAccountKey*und *StorageContainerName* -Deklarationen und Werte, die von dem Verwaltungsportal gespeichert.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Sie müssen auch sicherstellen, dass die Eingabedatei an, die an den Code verfügbar ist.

### <a name="specify-the-output-location"></a>Geben Sie den Ausgabespeicherort

Bei Angabe den Ausgabespeicherort in der Nutzlast anfordern muss die Erweiterung der Datei, die im *RelativeLocation* angegebenen als angegeben `ilearner`. Siehe folgendes Beispiel:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Im folgenden ist ein Beispiel der Umschulung Ausgabe: ![Umschulung Ausgabe][6]

## <a name="evaluate-the-retraining-results"></a>Die Umschulung Ergebnisse auswerten

Beim Ausführen der Anwendungdes enthält die Ausgabe URL und freigegebenen Signaturen Zugriffstoken, die auf Ergebnisse erforderlich sind.

Die Leistungsergebnisse trainierte Modell sehen durch Kombination von *BaseLocation*, *RelativeLocation*und *SasBlobToken* aus *ausgang2* Ausgabe (siehe vorherigen Umschulung Bild) und Einfügen vollständige URL in der Adressleiste des Browsers.  

Untersuchen Sie die Ergebnisse, um festzustellen, ob das neu ausgebildete Modell gut zum Ersetzen der vorhandenen ist.

Kopieren Sie *BaseLocation*, *RelativeLocation*und *SasBlobToken* aus der Ergebnisse

## <a name="retrain-the-web-service"></a>Trainieren Sie den Webdienst

Wenn Umschulung einen neuen Webdienst aktualisieren vorhersehbaren Webdienstdefinition neue ausgebildete Modell verweisen. Die Webdienstdefinition ist eine interne Darstellung des qualifizierten Modell des Webdiensts und kann nicht direkt geändert werden. Stellen Sie sicher, dass Sie die Webdienstdefinition vorhersehbaren Test und Schulung Test nicht abrufen.

## <a name="sign-in-to-azure-resource-manager"></a>In Azure Ressource-Manager anmelden

Sie müssen zuerst Ihre Azure-Konto in der PowerShell-Umgebung anmelden mithilfe des [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) -Cmdlets.

## <a name="get-the-web-service-definition-object"></a>Der Webdienstdefinition Objekt

Als Nächstes rufen Sie Webdienstdefinition Objekt durch das Cmdlet " [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) " aufrufen.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Um festzustellen Ressourcengruppenname eines vorhandenen Webdiensts führen Sie das Cmdlet Get-AzureRmMlWebService ohne Parameter Webdienste in Ihrem Abonnement angezeigt. Suchen Sie den Webdienst, und betrachten Sie die Web Service-ID Der Name der Ressourcengruppe ist das vierte Element die ID nach *ResourceGroups* -Element. Im folgenden Beispiel wird der Gruppe Ressourcenname Standard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternativ ermitteln Sie den Ressourcennamen Gruppe einen vorhandenen Webdienst melden Sie Portal Azure Machine Learning Web Services an. Wählen Sie den Webdienst. Der Ressourcengruppenname ist das fünfte Element der URL des Web Service nach *ResourceGroups* -Element. Im folgenden Beispiel wird der Gruppe Ressourcenname Standard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Exportieren des Webdienstdefinition Objekts als JSON

Um die Definition eines ausgebildeten Modell der neu ausgebildeten Modell ändern müssen Sie zum Exportieren in eine Datei im JSON-Format [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) -Cmdlet verwenden.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>Aktualisieren Sie den Verweis auf Ilearner-blob

Suchen Sie in den Anlagen [ausgebildete Modell], aktualisieren Sie *Uri* -Wert im *LocationInfo* -Knoten mit dem URI des BLOBs Ilearner. Der URI wird durch Kombination von *BaseLocation* und *RelativeLocation* aus der Ausgabe von BES Umschulung Aufruf generiert.

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition-object"></a>Ein Web Service-Definitionsobjekt importieren Sie JSON

[Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) -Cmdlet verwenden die geänderte JSON-Datei in ein Web Service Definition-Objekt konvertieren, mit denen Sie prädikative Experiment aktualisieren.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>Aktualisieren Sie den Webdienst

Schließlich dem Cmdlet [Update AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) vorhersageexperiment aktualisieren.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
