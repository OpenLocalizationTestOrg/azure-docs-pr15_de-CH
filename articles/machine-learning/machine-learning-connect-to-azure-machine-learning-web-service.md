<properties
    pageTitle="Verbinden mit einem Computer Learning-Webdienst | Microsoft Azure"
    description="Verbinden Sie mit C# oder Python einen Azure Machine Learning-Webdienst mit Autorisierungsschlüssel."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016" 
    ms.author="garye" />


# <a name="connect-to-an-azure-machine-learning-web-service"></a>Verbinden Sie mit einer Azure Machine Learning-Webdienst

Azure Machine Learning ist Entwickler eine Webdienst-API von Daten in Echtzeit oder im Batch-Modus Vorhersagen. Verwenden Sie Azure Machine Learning Studio Vorhersagen erstellen und Bereitstellen eines Webdiensts Azure Computer lernen.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

So erhalten Informationen zum Erstellen und Bereitstellen eines Computers Learning-Webdiensts mit Machine Learning Studio:

- Ein Lernprogramm zum Erstellen von Hypothesen Machine Learning Studio finden Sie unter [erstellen Ihre ersten Test](machine-learning-create-experiment.md).
- Informationen zur Bereitstellung eines Webdienstes finden Sie unter [Bereitstellen einer Machine Learning-Webdienst](machine-learning-publish-a-machine-learning-web-service.md).
- Weitere Informationen über Computerlernen im Allgemeinen finden Sie im [Arbeitsplatz Learning-Dokumentation](https://azure.microsoft.com/documentation/services/machine-learning/).

## <a name="azure-machine-learning-web-service"></a>Azure Machine Learning-Webdienst ##

Mit dem Azure Machine Learning-Webdienst kommuniziert eine externe Anwendung mit einem Computerlernen Bewertungsmodell Workflow in Echtzeit. Ein Computer Learning Web Service-Aufruf gibt Vorhersageergebnisse an eine externe Anwendung. Um einen Computer lernen Webdienstaufruf, übergeben Sie einen API-Schlüssel, der beim Bereitstellen einer Vorhersage erstellt. Machine Learning-Webdienst basiert auf weiteren Auswahl Architektur für Web programming Projekte.

Azure Machine Learning hat zwei Dienste:

- Anforderung-Antwort-Dienst (RRS) – eine niedrige Latenz, hochgradig skalierbaren Dienst eine Schnittstelle für die statusfreie Modelle erstellt und bereitgestellt von Machine Learning Studio.
- Batch Execution (BES) – eine asynchrone service dieser Faktoren eine Stapelverarbeitung für Datensätze.

Weitere Informationen über Computer lernen Webdienste finden Sie unter [Bereitstellen einer Machine Learning-Webdienst](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Autorisierungsschlüssel Azure Machine Learning erhalten ##

Wenn das Experiment bereitstellen, werden API-Schlüssel für den Webdienst generiert. Sie können die Schlüssel aus mehreren Speicherorten abrufen.

## <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Microsoft Azure Machine Learning Web Services-Portal

Mit [Microsoft Azure Machine Learning Web Services](https://services.azureml.net) -Portal anmelden.

API-Schlüssel für einen neuen Computer Learning-Webdienst abrufen:

1. Klicken Sie im Portal Azure Machine Learning Webdienste auf **Webdienste** im oberen Menü.
2. Klicken Sie auf den Webdienst den Schlüssel abgerufen werden soll.
3. Klicken Sie im oberen Menü auf **verbrauchen**.
4. Kopieren Sie und speichern Sie der **Primärschlüssel**.


API-Schlüssel für eine klassische Computer Learning-Webdienst abrufen:

1. Klicken Sie auf **Klassische Webdienste** im oberen Menü im Portal Azure Machine Learning Web Services.
2. Klicken Sie auf den Webdienst, mit dem Sie arbeiten.
3. Klicken Sie auf den Endpunkt den Schlüssel abgerufen werden soll.
3. Klicken Sie im oberen Menü auf **verbrauchen**.
4. Kopieren Sie und speichern Sie der **Primärschlüssel**.

## <a name="classic-web-service"></a>Klassische Webdienst ##

 Sie können auch einen Schlüssel für einen klassischen Webdienst Machine Learning Studio oder Azure-Portal abrufen.

### <a name="machine-learning-studio"></a>Machine Learning Studio ###

1. Machine Learning Studio klicken Sie links auf **Webdienste** .
2. Klicken Sie auf einen Webdienst. Der **API-Schlüssel** ist auf der **DASHBOARD** -Registerkarte.

### <a name="azure-portal"></a>Azure-portal ###

1. Klicken Sie auf der linken Seite auf **COMPUTERLERNEN** .
2. Klicken Sie auf den Arbeitsbereich, in dem sich der Webdienst befindet.
3. Klicken Sie auf **Webdienste**.
4. Klicken Sie auf einen Webdienst.
5. Klicken Sie auf einen Endpunkt. "API-Schlüssels" ist in der rechten unteren Ecke.

## <a id="connect"></a>Verbinden Sie mit einem Computer Learning-Webdienst

Sie können auf einen Computer Learning-Webdienst mit jeder Programmiersprache, die HTTP-Anforderung und Antwort unterstützt. Sie können Beispiele in C#, Python und R von einem Computer Learning-Webdienst-Hilfeseite anzeigen.

**Computer lernen API-Hilfe** Computer lernen API-Hilfe wird erstellt, wenn Sie einen Webdienst bereitstellen. Finden Sie unter [Exemplarische Vorgehensweise Azure Machine Learning - Webdienst bereitstellen](machine-learning-walkthrough-5-publish-web-service.md).
Computer lernen API-Hilfe enthält Details zu einer Vorhersage Webdienst.

2. Klicken Sie auf den Webdienst, mit dem Sie arbeiten.
3. Klicken Sie auf den Endpunkt für den API-Hilfeseite angezeigt werden soll.
3. Klicken Sie im oberen Menü auf **verbrauchen**.
3. Klicken Sie unter Anforderungsantwort oder Batchausführung Endpunkte **API-Hilfeseite** .

**Für einen neuen Webdienst auf Computer Learning-API unterstützen**

Im Web Services Lernportal Azure Computer:

1. Klicken Sie im oberen Menü auf **Webdienste** .
2. Klicken Sie auf den Webdienst den Schlüssel abgerufen werden soll.

Klicken Sie auf **verbrauchen** zu die URIs für die Anforderung Reposonse, Batch Execution Services und Beispielcode in C#, R und Python.

Klicken Sie auf **Swagger API-** basierte stolz Dokumentation für die APIs der angegebenen URIs aufgerufen.

### <a name="c-sample"></a>C#-Beispiel ###

Verwenden Sie zum Verbinden mit einem Computer Learning-Webdienst eine **HttpClient** ScoreData übergeben. ScoreData enthält eine FeatureVector, eine n-dimensionale Vektor numerische Funktionen, die die ScoreData darstellt. Sie authentifizieren sich Machine Learning-Dienst mit einem API-Schlüssel.

Verbindung zu einem Computer Learning-Webdienst muss **Microsoft.AspNet.WebApi.Client** NuGet-Paket installiert sein.

**Installieren von Microsoft.AspNet.WebApi.Client NuGet in Visual Studio**

1. Veröffentlichen von UCI Dataset Download: Erwachsene 2 Klasse Dataset Webdienst.
2. Klicken Sie auf **Extras** > **NuGet Paket-Manager** > **Paket-Manager-Konsole**.
2. Wählen Sie **Install-Package Microsoft.AspNet.WebApi.Client**.

**Um das Codebeispiel auszuführen**

1. Veröffentlichen "Beispiel 1: UCI Dataset herunterladen: Erwachsene 2 Klasse Dataset" Experiment, Teil der Computerlernen Muster.
2. Weisen Sie ApiKey mit dem Schlüssel in einem Webdienst. Siehe oben **Autorisierungsschlüssel Azure Machine Learning erhalten** .
3. ServiceUri Request-URI zuordnen


### <a name="python-sample"></a>Python-Beispiel ###

Verbindung zu einem Computer Learning-Webdienst übergeben ScoreData **urllib2** -Bibliothek verwenden. ScoreData enthält eine FeatureVector, eine n-dimensionale Vektor numerische Funktionen, die die ScoreData darstellt. Sie authentifizieren sich Machine Learning-Dienst mit einem API-Schlüssel.


**Um das Codebeispiel auszuführen**

1. Bereitstellen "Beispiel 1: UCI Dataset herunterladen: Erwachsene 2 Klasse Dataset" Experiment, Teil der Computerlernen Muster.
2. Weisen Sie ApiKey mit dem Schlüssel in einem Webdienst. Abschnitt **Autorisierungsschlüssel Azure Machine Learning erhalten** am Anfang dieses Artikels.
3. ServiceUri Request-URI zuordnen
