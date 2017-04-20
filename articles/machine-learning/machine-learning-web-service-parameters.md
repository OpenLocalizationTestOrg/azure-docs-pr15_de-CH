<properties 
    pageTitle="Azure Computer lernen Webdienstparameter | Microsoft Azure" 
    description="Wie Sie Azure Machine Learning Web Service Parameter das Verhalten Ihres Modells ändern, wenn der Webdienst zugegriffen wird." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="raymondl;garye"/>

#<a name="use-azure-machine-learning-web-service-parameters"></a>Azure Computer lernen Webdienstparametern

Ein Azure Machine Learning-Webdienst erstellt veröffentlichen Hypothesen, die Module mit konfigurierbaren Parameter enthält. Möglicherweise möchten Modul Änderung während der Webdienst ausgeführt wird. *Web Service-Parameter* können Sie für diese Aufgabe. 

Ein gängiges Beispiel Einrichten der [Importdaten] [ reader] Modul, damit der Benutzer den veröffentlichten Webdienst beim Zugriff auf der Webdienst eine andere Datenquelle angeben kann. Oder [Daten exportieren] [ writer] Modul, um ein anderes Ziel angegeben werden kann. Andere Beispiele für das [Feature Hashing] ändern, die Anzahl der Bits[ feature-hashing] Modul oder die Anzahl der gewünschten Funktionen für die [Featureauswahl filterbasierte] [ filter-based-feature-selection] Modul. 

Web Service Parameter und ordnen sie mindestens Modulparameter im Experiment und Sie können angeben, ob diese erforderlich oder optional sind. Der Benutzer des Webdienstes kann geben Werte für diese Parameter beim Aufruf des Webdiensts. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


##<a name="how-to-set-and-use-web-service-parameters"></a>Wie können mit Webdienstparametern

Sie definieren einen Webdienstparameter auf das Symbol neben dem Parameter für ein Modul und "Als Web Service Parameter festlegen" auswählen. Erstellt ein neues Web Service Parameter und Modulparameter Verbindung. Wenn der Webdienst zugegriffen wird, kann der Benutzer einen Wert für den Webdienstparameter angeben und Modul-Parameter wird.

Nachdem einen Webdienstparameter definiert steht anderen Modul-Parameter im Experiment. Definieren einer Web Service-Parameter einen Parameter für ein Modul zugeordnet können gleichen Webdienstparameter für andere Modul Sie als der Parameter die gleichen Werttyp erwartet. Beispielsweise ist die Webdienstparameter einen numerischen Wert, kann dann es nur für Parameter für das Modul verwendet werden, die einen numerischen Wert erwarten. Wenn der Benutzer einen Wert für den Webdienstparameter festlegt, wird es auf alle Modul angewendet werden.

Sie können entscheiden, ob einen Standardwert für Parameter für den Webdienst angeben. Andernfalls ist der Parameter optional für die Benutzer des Webdienstes. Wenn Sie einen Standardwert angeben, muss der Benutzer einen Wert eingeben, wenn der Webdienst zugegriffen wird.

Die API-Dokumentation für den Webdienst enthält Informationen für Web Service-Benutzer können Sie Parameter für den Webdienst beim Zugriff auf den Webdienst programmgesteuert angeben.

>[AZURE.NOTE] Die API-Dokumentation für klassische Webdienst wird über den Link **Hilfe-API** im Webdienst **DASHBOARD** in Machine Learning Studio bereitgestellt. Die API-Dokumentation für einen neuen Webdienst erfolgt über das Portal [Azure Machine Learning Web Services](https://services.azureml.net/Quickstart) **verbrauchen** und **Swagger API** -Seiten für den Webdienst.


##<a name="example"></a>Beispiel

Nehmen wir beispielsweise an haben wir mit einem [Daten exportieren] [ writer] Modul, das Informationen zu Azure BLOB-Speicher sendet. Definieren wir einen Web Service Parameter namens "BLOB-Pfad", die Web Service Benutzer beim Zugriff auf den Dienst der BLOB-Speicher ändern können.

1.  Machine Learning Studio klicken Sie auf [Daten exportieren] [ writer] -Modul, um es auszuwählen. Die Eigenschaften werden rechts von der Leinwand Experiment im Eigenschaftenbereich angezeigt.

2.  Geben Sie den Speicher:

    - Wählen Sie unter **Bitte Datenziel angeben**"Azure BLOB-Speicher".
    - Wählen Sie unter **Authentifizierung geben Sie bitte**"Konto".
    - Geben Sie die Kontoinformationen für Azure BLOB-Speicher. 
    <p />

3.  Klicken Sie rechts neben den **Pfad mit Containerparameter BLOB**. Es sieht folgendermaßen aus:

    ![Symbol für Web Service-Parameter][icon]

    Wählen Sie "als Web Service Parameter".

    Ein Eintrag wird unter **Webdienstparameter** unteren Bereich Eigenschaften mit dem Namen "Pfad mit Container BLOB" hinzugefügt. Dies ist Webdienstparameter, die jetzt zugeordnete [Daten exportieren] [ writer] Module-Parameter.

4.  Parameter für den Webdienst umbenennen klicken Geben Sie ein "BLOB-Pfad" und drücken Sie **die EINGABETASTE** . 
 
5.  Um einen Standardwert für den Parameter für den Webdienst bereitstellen, klicken Sie rechts neben den Namen "Standardwert bereitstellen" wählen, geben einen Wert (z. B. "container1/output1.csv") und drücken **die EINGABETASTE** .

    ![Webdienstparameter][parameter]

6.  Klicken Sie auf **Ausführen**. 

7.  Klicken Sie auf **Webdienst bereitstellen** und wählen Sie **Webdienst bereitstellen [Classic]** oder **[New] Webdienst bereitstellen** den Web Service bereitstellen.

Der Benutzer des Webdienstes können jetzt ein neues Ziel für das [Exportieren von Daten] [ writer] Modul beim Zugriff auf den Webdienst.

##<a name="more-information"></a>Weitere Informationen

Ein ausführlicheres Beispiel finden Sie unter [Webdienstparameter](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) Eintrag im [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Weitere Informationen zum Zugriff auf einen Webdienst Machine Learning finden Sie unter [einem veröffentlichten Webdienst lernen Computer nutzen](machine-learning-consume-web-services.md).



<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
 
