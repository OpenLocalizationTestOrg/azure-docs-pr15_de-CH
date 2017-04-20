<properties
    pageTitle="Einen neuen Webdienst mit Computer Learning Management PowerShell-Cmdlets umschulen | Microsoft Azure"
    description="Informationen Sie zum programmgesteuerten Trainieren eines Modells und aktualisieren den Webdienst neu ausgebildete Modell in Azure Machine Learning verwenden Computer Learning Management PowerShell-Cmdlets verwenden."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-new-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Umschulung einen neuen Webdienst mit Computer Learning Management PowerShell-cmdlets

Wenn Umschulung einen neuen Webdienst aktualisieren vorhersehbaren Webdienstdefinition neue ausgebildete Modell verweisen.  

## <a name="prerequisites"></a>Erforderliche Komponenten

Sie müssen ein trainingsexperiment und ein vorhersageexperiment eingerichtet haben Siehe [umschulen maschinelles lernen Modelle programmgesteuert](machine-learning-retrain-models-programmatically.md). 

>[AZURE.IMPORTANT] Vorhersageexperiment muss ein Webdienst lernen Azure Ressourcenmanager (neue) Computer bereitgestellt werden. 
 
Zusatzinformationen zum Bereitstellen von Webdiensten finden Sie unter [Bereitstellen einer Azure Machine Learning-Webdienst](machine-learning-publish-a-machine-learning-web-service.md).

Dieser Prozess erfordert Azure Machine Learning Cmdlets installiert haben. Finden Sie weitere Informationen zur Installation von Cmdlets maschinelles lernen im [Azure Machine Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) auf MSDN.

Die folgende Informationen kopiert der Umschulung Ausgabe:

* BaseLocation
* RelativeLocation

Die Schritte sind:

1.  Mit der Azure-Ressourcen-Manager-Konto anmelden.
2.  Abrufen der Webdienstdefinition
3.  Exportieren der Webdienstdefinition als JSON
4.  Aktualisieren Sie auf Ilearner Blob in JSON.
5.  Web Service Definition JSON importieren
6.  Aktualisierung mit neuen Web Service Definition

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Der Azure-Ressourcen-Manager-Konto anmelden

Sie müssen zuerst Ihre Azure-Konto in der PowerShell-Umgebung mithilfe des [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) -Cmdlet anmelden.

## <a name="get-the-web-service-definition"></a>Abrufen der Webdienstdefinition

Als Nächstes rufen Sie den Webdienst durch das Cmdlet " [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) " aufrufen. Der Webdienstdefinition ist eine interne Darstellung des qualifizierten Modell des Webdiensts und kann nicht direkt geändert werden. Stellen Sie sicher, dass Sie die Webdienstdefinition vorhersehbaren Test und Schulung Test nicht abrufen.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Um festzustellen Ressourcengruppenname eines vorhandenen Webdiensts führen Sie das Cmdlet Get-AzureRmMlWebService ohne Parameter Webdienste in Ihrem Abonnement angezeigt. Suchen Sie den Webdienst, und betrachten Sie die Web Service-ID Der Name der Ressourcengruppe ist das vierte Element die ID nach *ResourceGroups* -Element. Im folgenden Beispiel wird der Gruppe Ressourcenname Standard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternativ ermitteln Sie den Ressourcennamen Gruppe einen vorhandenen Webdienst melden Sie Portal Microsoft Azure Machine Learning Web Services an. Wählen Sie den Webdienst. Der Ressourcengruppenname ist das fünfte Element der URL des Web Service nach *ResourceGroups* -Element. Im folgenden Beispiel wird der Gruppe Ressourcenname Standard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Exportieren der Webdienstdefinition als JSON

Die Definition der qualifizierten Modell neu ausgebildeten ändern Modell müssen Sie zuerst verwenden das [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) -Cmdlet in eine JSON-Format exportieren.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>Aktualisieren Sie auf Ilearner Blob in JSON.

Suchen Sie in den Anlagen [ausgebildete Modell], aktualisieren Sie *Uri* -Wert im *LocationInfo* -Knoten mit dem URI des BLOBs Ilearner. Der URI wird durch Kombination von *BaseLocation* und *RelativeLocation* aus der Ausgabe von BES Umschulung Aufruf generiert.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
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

## <a name="import-the-json-into-a-web-service-definition"></a>Web Service Definition JSON importieren

[Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) -Cmdlet verwenden JSON-modifiziertes in Web Service Definition konvertiert, mit dem Sie experimentieren Prädikativen aktualisieren.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Aktualisierung mit neuen Web Service Definition

Schließlich verwenden Sie [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) -Cmdlet vorhersageexperiment aktualisieren.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Zusammenfassung

Computer lernen PowerShell Management Cmdlets können Sie geschulte Modell eines vorhersehbaren Webdienstes Szenarien wie aktualisieren:

* Periodische Modell mit neuen Daten Umschulung.
* Verteilung eines Modells für Kunden mit dem Ziel mit ihren eigenen Daten umschulen lassen.
