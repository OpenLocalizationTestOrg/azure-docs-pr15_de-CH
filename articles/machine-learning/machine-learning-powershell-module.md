<properties
    pageTitle="PowerShell-Module für maschinelles lernen | Microsoft Azure"
    description="PowerShell-Module für Azure Machine Learning ist im public Preview-Version verfügbar. Mithilfe von PowerShell erstellen und Verwalten von Arbeitsbereichen, Experimente und Webdienste."
    keywords="Experimentieren Sie lineare Regression Algorithmen für maschinelles lernen Computer lernen Lernprogramm, Vorhersagemodellen Verfahren Daten wissenschaftliches experiment"
    services="machine-learning"
    documentationCenter=""
    authors="hning86"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="garye;haining"/>

# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>PowerShell-Modul für Microsoft Azure maschinelles lernen

PowerShell-Module für Azure Machine Learning ist ein leistungsfähiges Tool, das Sie mit Windows PowerShell Arbeitsbereiche, Versuche, Datasets, Web Service und verwalten kann.

Lesen Sie die Dokumentation, und downloaden Sie das Modul mit der vollständige Quellcode unter [https://aka.ms/amlps](https://aka.ms/amlps). 

## <a name="what-is-the-machine-learning-powershell-module"></a>Was ist Computer lernen PowerShell-Modul?

Computer lernen PowerShell-Modul ist ein. NET-basierten DLL-Modul, das vollständig Azure Machine Learning Arbeitsbereiche Versuche, Datasets, Webdienste und Webdienst-Endpunkte von Windows PowerShell verwalten kann. Mit dem Modul können Sie den vollständigen Quellcode sauber getrennte [C#-API-Ebene](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs)umfasst. Damit diese DLL aus eigener verweisen und Azure Machine Learning .NET Code verwalten. Darüber hinaus hängt die DLL zugrunde liegenden REST-APIs direkt aus Ihren bevorzugten Client nutzen können.

## <a name="what-can-i-do-with-the-powershell-module"></a>Was kann ich mit dem PowerShell-Modul?

Hier sind einige der Aufgaben mit diesem Modul PowerShell ausführen. Überprüfen Sie die [vollständige Dokumentation](https://aka.ms/amlps) für diese und viele weitere Funktionen.

- Bereitstellen eines neuen Arbeitsbereichs Verwaltungszertifikat ([New AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
- Exportieren und Importieren einer JSON-Datei, die ein Diagramm Experiment ([AmlExperimentGraph Export](https://github.com/hning86/azuremlps#export-amlexperimentgraph) und [Import-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
- Führen Sie ein Experiment ([AmlExperiment Start](https://github.com/hning86/azuremlps#start-amlexperiment))
- Erstellen Sie einen Webdienst aus einer vorhersageexperiment ([New AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
- Erstellen Sie einen neuen veröffentlichten Webdienst ([AmlWebServiceEndpoint hinzufügen](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
- Aufrufen eines Ressourceneinträge oder BES Webdienst-Endpunkts ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) und [AmlWebServicBESEndpoint aufrufen](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Hier ist ein Beispiel mit PowerShell vorhandenen Test ausführen:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Einen ausführlicheren Anwendungsfall finden Sie in diesem Artikel sehr häufig angeforderte Aufgabe automatisieren, mit PowerShell-Modul: [viele Computerlernen Modelle und Web aus einem Experiment mit PowerShell-Endpunkte erstellen](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Wie fange ich an?

Computer lernen PowerShell, GitHub herunterladen [Paket freigeben](https://github.com/hning86/azuremlps/releases) und folgen der [Anleitung](https://github.com/hning86/azuremlps/blob/master/README.md). Sie müssen die DLL heruntergeladen/entpackt und importieren sie dann in der PowerShell-Umgebung. Die meisten Cmdlets müssen die Arbeitsplatz-ID, Arbeitsbereich Autorisierungstoken und Arbeitsbereich ist Azure-Region angeben. Am einfachsten stellen wird über eine standardmäßige config.json, die die Installationshinweise ausführlich behandelt. Natürlich können auch Klonen die Git Struktur und ändern/kompilieren Sie den Code lokal mit Visual Studio.

## <a name="next-steps"></a>Nächste Schritte

PowerShell-Modul weiter verbessert und während dieses Vorschauzeitraums erweitert. Ein Auge auf den [Cortana und Machine Learning Blog](https://blogs.technet.microsoft.com/machinelearning/) für weitere Neuigkeiten und Informationen.
