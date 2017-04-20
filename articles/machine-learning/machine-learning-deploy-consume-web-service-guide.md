<properties
    pageTitle="Azure Machine Learning Web Services: Bereitstellung und Verbrauch | Microsoft Azure"
    description="Ressourcen zum Bereitstellen und Verwenden von Webdiensten."
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
    ms.date="10/12/2016"
    ms.author="v-donglo"/>

# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure Machine Learning Web Services: Bereitstellung und Verbrauch

Azure Machine Learning können lernfähige Workflows und Modelle als Webdienste. Diese Webdienste können dann Aufruf lernfähige Modelle aus Programmen über das Internet dazu Vorhersagen in Echtzeit oder im Batch-Modus verwendet werden. Da Webdienste RESTful sind, können Sie diese von verschiedenen Programmiersprachen und Plattformen wie .NET und Java, und aus Programmen wie Excel aufrufen.

Die nächsten Abschnitte enthalten Links zu exemplarischen Vorgehensweisen, Code und Dokumentation, die Ihnen helfen.

## <a name="deploy-a-web-service"></a>Bereitstellen eines Webdiensts

### <a name="with-azure-machine-learning-studio"></a>Mit Azure maschinelles lernen

Machine Learning Studio und die Microsoft Azure Machine Learning Web Services Portal bereitstellen und Verwalten von einem Webdienst ohne Code zu schreiben.

Die folgenden Links bieten allgemeine Informationen über einen neuen Webdienst bereitstellen:

* Überblick über einen neuen Webdienst bereitstellen, der basiert auf Azure-Ressourcen-Manager finden Sie unter [Bereitstellen einer neuen Webdienst](machine-learning-webservice-deploy-a-web-service.md).
* Eine Anleitung dazu, wie Sie einen Webdienst finden Sie unter [Bereitstellen einer Azure Machine Learning-Webdienst](machine-learning-publish-a-machine-learning-web-service.md).
* Eine Anleitung zum Erstellen und Bereitstellen eines Webdiensts finden Sie unter [Exemplarische Vorgehensweise Schritt 1: Erstellen eines Arbeitsbereichs maschinelles lernen](machine-learning-walkthrough-1-create-ml-workspace.md).
* Beispiele, die einen Webdienst bereitstellen, finden Sie unter:

    * [Exemplarische Vorgehensweise Schritt 5: Bereitstellen von Azure Machine Learning-Webdienst](machine-learning-walkthrough-5-publish-web-service.md)
    * [Einen Webdienst in mehrere Regionen bereitstellen](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Mit Web Resource APIs (Azure-Ressourcen-Manager-APIs)

Azure Machine Learning Ressourcenanbieter für Webdienste ermöglicht Bereitstellung und Verwaltung von Webdiensten mithilfe von REST-API-Aufrufe. Weitere Informationen finden Sie auf der MSDN-Referenz [Computer Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) .

### <a name="with-powershell-cmdlets"></a>Mit PowerShell-cmdlets

Azure Machine Learning Ressourcenanbieter für Webdienste ermöglicht Bereitstellung und Verwaltung von Webdiensten mithilfe von PowerShell-Cmdlets.

Um die Cmdlets verwenden, müssen Sie zuerst Ihre Azure-Konto in der PowerShell-Umgebung anmelden mithilfe des [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) -Cmdlets. Wenn mit PowerShell-Befehle aufrufen, die basieren auf Ressourcen-Manager nicht vertraut sind, finden Sie [Mithilfe von Azure PowerShell mit Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md#login-to-your-azure-account).

Verwenden Sie zum Exportieren der vorhersageexperiment [dieser Beispielcode](https://github.com/ritwik20/AzureML-WebServices). Nachdem Sie die .exe-Datei aus dem Code erstellen, können Sie Folgendes eingeben:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Die Anwendung erstellt einen Web Service JSON. Um die Vorlage verwenden, um einen Webdienst bereitstellen, müssen Sie die folgende Informationen hinzufügen:

* Speicher-Kontonamen und Schlüssel

    [Azure-Portal](https://portal.azure.com/) oder das [klassische Azure-Portal](http://manage.windowsazure.com/)können Sie Storage-Kontonamen und Schlüssel abrufen.
* Engagement Plan-ID

    Die Plan-ID erhalten aus dem [Azure Machine Learning Web Services](https://services.azureml.net) Portal durch anmelden und ein Plan auf.

Hinzufügen der JSON-Vorlage als untergeordnete Knoten *Eigenschaften* auf der gleichen Ebene wie der *MachineLearningWorkspace* -Knoten.

Hier ist ein Beispiel:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Finden Sie in den folgenden Artikeln und Beispielcode für weitere Details:

* [Azure Machine Learning Cmdlets]( https://msdn.microsoft.com/library/azure/mt767952.aspx) -Referenz auf MSDN
* Beispiel [Exemplarische Vorgehensweise](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) auf GitHub

## <a name="consume-the-web-services"></a>Webdienste nutzen

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Von Azure Machine Learning-Webdiensten UI (Test)

Testen Sie den Webdienst in Azure Machine Learning Web Services-Portal. Dies beinhaltet das Testen des Anforderung / Antwort-Dienstes (RRS) und Batchausführung Service (BES)-Schnittstellen.

* [Einen neuen Webdienst bereitstellen](machine-learning-webservice-deploy-a-web-service.md)
* [Bereitstellen eines Webdiensts Azure maschinelles lernen](machine-learning-publish-a-machine-learning-web-service.md)
* [Exemplarische Vorgehensweise Schritt 5: Bereitstellen von Azure Machine Learning-Webdienst](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Aus Excel

Sie können eine Excel-Vorlage, die den Webdienst verwendet:

* [Verwenden einen Webdienst Azure maschinelles lernen von Excel](machine-learning-consuming-from-excel.md)
* [Excel-add-in für Azure Machine Learning-Webdienste](machine-learning-excel-add-in-for-web-services.md)


### <a name="from-a-rest-based-client"></a>REST-basierte Clients

Azure Machine Learning-Webdienste sind RESTful-APIs. Sie können diese APIs von verschiedenen Plattformen wie .NET, Python, R, Java usw. nutzen. **Verbrauchen** Seite für Ihren Web Service in [Microsoft Azure Machine Learning Web Services Portal](https://services.azureml.net) enthält Beispielcode, die Ihnen beim Einstieg helfen. Weitere Informationen finden Sie unter [ein Azure Machine Learning Webdienst, der von einem Computer lernen Experiment bereitgestellt wurde](machine-learning-consume-web-services.md).

