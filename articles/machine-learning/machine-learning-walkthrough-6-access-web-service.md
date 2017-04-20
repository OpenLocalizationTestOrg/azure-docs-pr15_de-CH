<properties
    pageTitle="Schritt 6: Zugriff auf den Webdienst maschinelles lernen | Microsoft Azure"
    description="Schritt 6 entwickeln eine vorbeugende Lösung Exemplarische Vorgehensweise: Zugreifen auf einen aktiven Azure Machine Learning-Webdienst."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Exemplarische Vorgehensweise Schritt 6: Zugriff auf den Webdienst Azure maschinelles lernen

Dies ist der letzte Schritt der exemplarischen Vorgehensweise [entwickeln eine Lösung Vorhersageanalytik Azure maschinelles lernen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Erstellen eines Arbeitsbereichs maschinelles lernen](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Vorhandene Daten](machine-learning-walkthrough-2-upload-data.md)
3.  [Erstellen Sie einen neuen Test](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Schulen und Auswerten der Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Den Web Service bereitstellen](machine-learning-walkthrough-5-publish-web-service.md)
6.  **Zugriff auf den Webdienst**

----------

Im vorherigen Schritt in dieser exemplarischen Vorgehensweise bereitgestellt wir einen Webdienst, der unsere einer Risiko Vorhersagemodell verwendet. Benutzer müssen nun in der Lage, Daten zu senden und Empfangen von Ergebnissen. 

Der Webdienst ist ein Webdienst Azure, der empfangen und Daten mithilfe von REST-APIs auf zwei Arten:  

-   **Anforderung-Antwort** - Benutzer sendet eine oder mehrere Datenzeilen Gutschrift für den Dienst ein HTTP-Protokoll und der Webdienst sendet einen oder mehrere Sätze von Ergebnissen.
-   **Batch Execution** - Benutzer eine oder mehrere Datenzeilen Gutschrift in Azure Blob gespeichert und BLOB-Position an den Dienst sendet. Der Dienst Punktzahlen für alle Datenzeilen im input-Blob, speichert die Ergebnisse in einem anderen Blob und gibt die URL des Containers.  

Über das Web App Vorlagen in [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/)ist die schnellste und einfachste Möglichkeit zum Zugriff auf den Webdienst.
Diese app Webvorlagen können benutzerdefinierte Web app erstellen, die weiß des Webdiensts Eingabedaten und was er zurückgibt. Müssen Sie lediglich Ihren Webdienst und Daten zugreifen, und die Vorlage erledigt den Rest.

Weitere Informationen über die app Webvorlagen finden Sie [verbrauchen einen Azure Machine Learning-Webdienst mit einer Webvorlage app](machine-learning-consume-web-service-with-web-app-template.md).

Sie können auch eine benutzerdefinierte Anwendung auf den Webdienst mit Starter stehen im R C# und Python Programmiersprachen entwickeln.
Einzelheiten finden Sie unter [ein Azure Machine Learning Webdienst, der von einem Computer lernen Experiment veröffentlicht wurde](machine-learning-consume-web-services.md).
