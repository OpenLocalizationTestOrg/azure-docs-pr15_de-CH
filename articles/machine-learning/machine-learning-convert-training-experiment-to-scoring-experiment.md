<properties
    pageTitle="Konvertieren ein Experiments Machine Learning Training vorhersehbaren Experiment | Microsoft Azure"
    description="So konvertieren Sie eine Computer Learning trainingsexperiment zur Schulung Modell Vorhersageanalysen vorhersehbaren Experiment als Webdienst eingesetzt."
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
    ms.date="08/19/2016"
    ms.author="garye"/>

# <a name="convert-a-machine-learning-training-experiment-to-a-predictive-experiment"></a>Konvertieren eines Experiments Machine Learning Training vorhersehbaren Experiment

Azure Machine Learning ermöglicht das Erstellen, testen und Bereitstellen von Vorhersageanalysen Solutions.

Nachdem Sie erstellt und auf einer *Schulung experimentieren* Vorhersageanalysen Modell trainieren durchlaufen haben und sie verwenden, um neue Daten möchten müssen Sie vorbereiten und optimieren das Experiment zur Bewertung. Dann können Sie dieses *vorhersageexperiment* als Azure Webdienst bereitstellen, damit Benutzer Modell senden und des Modells Vorhersagen empfangen können.

In einen vorhersageexperiment konvertieren, bekommen geschulte Modell als Webdienst bereitgestellt Sie. Benutzer des Webdiensts senden Eingabedaten Modell und Modell wird die Vorhersageergebnisse zurücksenden. So wie ein vorhersageexperiment konvertieren soll zu beachten wie das Modell von anderen Benutzern verwendet werden soll.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Der Prozess der Konvertierung eines trainingsexperiments vorhersehbaren Experiment umfasst drei Schritte:

1.  Speichern Sie Machine Learning-Modell, das Sie gelernt haben, und Ersetzen Sie Computer lernen Algorithmus und [Zug Modell] [ train-model] mit gespeicherten ausgebildeten Modell.
2.  Schneiden Sie das Experiment nur die Module, die für die Bewertung erforderlich sind. Schulung Experiment umfasst eine Reihe von Modulen, die für Ausbildung, aber sind nicht erforderlich, wenn das Modell vorbereitet und für die Bewertung verwendet wird.
3.  Definieren Sie, wo im Experiment Sie Daten Web Service akzeptiert und welche Daten zurückgegeben werden.

## <a name="set-up-web-service-button"></a>Web-Service-Schaltfläche

Nach dem Ausführen des Experiments (Schaltfläche am unteren Rand der Leinwand Experiment**Ausführen** ) führt Schaltfläche **Web-Service** (die Option **Vorhersehbaren Webdienst** ) Sie die drei Schritte Training Test in eine vorhersageexperiment konvertieren:

1.  Geschulte Modell wie ein Modul im Abschnitt **Ausgebildeten Modelle** der Modul-Palette (nach links Experiment Leinwand) ersetzt den Computer lernen Algorithmus und [Zug Modell] gespeichert[ train-model] mit gespeicherten ausgebildeten Modell.
2.  Module, die eindeutig nicht benötigt werden entfernt. In unserem Beispiel umfasst dies die [Aufgeteilten Daten][split], 2. [Bewertung Modell][score-model], und das [Modell auswerten] [ evaluate-model] Module.
3.  Erstellt Web Service Input und output Module und in Standardverzeichnissen Test hinzugefügt.

Folgende Experiment Züge z. B. eine Klasse zwei stärkere Entscheidungsbaummodell Erhebung Beispieldaten verwenden:

![Trainingsexperiment][figure1]

Die Module dieses Experiment Funktionen im Grunde vier verschiedene:

![Funktionen][figure2]

Beim Konvertieren dieses trainingsexperiments vorhersehbaren Experiment diese Module nicht mehr benötigt oder sie einen anderen Zweck:

- **Daten** - Daten in dieses Dataset Beispiel wird kein während Punktwertung - Benutzer des Webdiensts angeben der Daten erzielt werden. Jedoch ist dieses Dataset wie Datentypen mit Metadaten von ausgebildeten Modell verwendet. So müssen Sie das Dataset im vorhersageexperiment, damit diese Metadaten bereitstellen kann.

- Diese Module **Prep** - je nach den Daten, die zur Bewertung, übermittelt werden können oder möglicherweise nicht zum Verarbeiten der eingehenden Daten erforderlich.

    Beispielsweise in diesem Beispiel müssen die Stichprobendataset fehlende Werte und enthält Spalten, die zum Trainieren des Modells nicht benötigt werden. So [Säubern fehlende Daten] [ clean-missing-data] fehlende Werte, und [Wählen Sie Spalten im Dataset] enthalten war[ select-columns] ausgeschlossen werden die zusätzlichen Spalten Daten enthalten war. Wenn Sie wissen, dass die Daten, die zur Bewertung durch den Webdienst übermittelt werden fehlende Werte nicht haben, dann [Sauber fehlende Daten] entfernen[ clean-missing-data] Modul. Da die [Spalten im Dataset auswählen] [ select-columns] Modul unterstützt erzielt werden Funktionen definieren, Modul bleiben muss.

- **Zug** - nach Modell trainierte, speichern Sie es als einheitliches ausgebildeten Modul. Sie ersetzen dann diese einzelne Module mit gespeicherten ausgebildeten Modell.

- In diesem Modul Split dient **Bewertung** - Datenstream in einen Satz von Daten und Daten unterteilen. Im vorhersageexperiment dies nicht benötigt und kann entfernt werden. Entsprechend der 2. [Faktor Modell] [ score-model] Modul und das [Modell auswerten] [ evaluate-model] Modul Vergleichen von Testdaten, damit diese Module im vorhersageexperiment auch nicht benötigt werden. Verbleibende [Bewertung Modell] [ score-model] Modul jedoch muss ein Wert über den Webdienst Ergebnis.

So sieht Beispiel nach dem Klicken auf **Web-Service**:

![Konvertiert vorhersageexperiment][figure3]

Dies kann das Experiment als Webdienst bereitgestellt werden vorbereitet ausreichen. Sie möchten jedoch einige zusätzliche Arbeit für den Test.

### <a name="adjust-input-and-output-modules"></a>Anpassen der ein-und Ausgabe

In das trainingsexperiment verwendet einen Satz von Daten und hat eine Verarbeitung die Daten in einem Formular abruft, die der computerlernen-Algorithmus. Wenn die Daten durch den Webdienst empfangen erwarten diese Verarbeitung nicht benötigen, können Sie **Modul Web Service input** im Experiment zu einem anderen Knoten verschieben.

Standardmäßig stellt z. B. **Web-Service** **Web Service Input** -Modul am oberen Rand des Datenflusses in der obigen Abbildung. Wenn die Eingabedaten Verarbeitung nicht benötigen, können dann Sie jedoch manuell **Web Service Eingabe** über die Verarbeitungsmodule platzieren:

![Verschieben von Web Service-Eingabe][figure4]

Über den Webdienst bereitgestellten Eingabedaten werden nun direkt in Modul Score Model Vorverarbeitungsschritten werden.

Standardmäßig stellt **Web-Service** entsprechend das Web Service-Ausgabemodul am unteren Rand des Datenflusses. In diesem Beispiel der Webdienst wird an den Benutzer zurückgegeben Ausgabe [Bewertung Modell] [ score-model] Modul umfasst vollständige Eingabedaten Vektor sowie die Bewertung ergibt.

Allerdings möchten Sie wieder etwas anderes - Beispiel, nur die Bewertungsergebnisse und nicht den gesamten Vektor der eingegebenen Daten -können [Spalten im Dataset auswählen] [ select-columns] Modul alle Spalten mit Ausnahme der Bewertungsergebnisse. Dann verschieben **Web Service** Ausgabemodul in die Ausgabe der [Spalten im Dataset auswählen] [ select-columns] Modul:

![Verschieben von Web Service-Ausgabe][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Fügen Sie hinzu oder entfernen Sie zusätzlicher Verarbeitung

Gibt weitere Module in das Experiment wissen während Punktwertung nicht benötigt werden, können diese entfernt. Beispielsweise weil wir nach Modulen Datenverarbeitung Modul **Web Service Input** verschoben haben, wir können entfernen [Bereinigen fehlen Daten] [ clean-missing-data] Modul vorhersageexperiment.

Unser vorhersageexperiment sieht nun folgendermaßen aus:

![Zusätzliche Module entfernen][figure6]

### <a name="add-optional-web-service-parameters"></a>Fügen Sie Optionaler Webdienstparametern hinzu

In einigen Fällen möchten Sie Benutzer des Webdienstes sollen ändern das Verhalten der Module beim Zugriff auf den Dienst. *Webdienstparameter* können dazu.

Ein gängiges Beispiel Einrichten der [Importdaten] [ import-data] Modul, damit der Benutzer des bereitgestellten Webdiensts beim Zugriff auf der Webdienst eine andere Datenquelle angeben kann. Oder [Daten exportieren] [ export-data] Modul, um ein anderes Ziel angegeben werden kann.

Web Serviceparameter definieren und mindestens Modulparameter zuordnen und Sie können angeben, ob diese erforderlich oder optional sind. Der Benutzer des Web Service kann dann Werte für diese Parameter bereitstellen, beim Zugriff auf den Dienst und die Aktionen Modul entsprechend geändert.

Weitere Informationen über Web Service finden Sie [Mithilfe von Azure Machine Learning-Webdienstparameter ] [ webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Bereitstellen von vorhersageexperiment als Webdienst

Vorhersageexperiment ausreichend vorbereitet wurde, können Sie es als Azure Web Service bereitstellen. Mithilfe des Webdiensts, Benutzer können Daten senden, Modell und Modell Prognosen zurück.

Weitere vollständige Verfahren finden Sie unter [Bereitstellen einer Azure Machine Learning-Webdienst][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
