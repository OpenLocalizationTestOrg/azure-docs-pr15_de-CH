<properties
    pageTitle="Schritt 4: Schulen und Auswerten der analytischen Vorhersagemodelle | Microsoft Azure"
    description="Schritt 4 entwickeln eine vorbeugende Lösung Exemplarische Vorgehensweise: Zug, bewerten und mehrere Modelle in Azure Machine Learning Studio auswerten."
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


# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Schritt 4 der exemplarischen Vorgehensweise: Schulen und analytische Vorhersagemodelle auswerten

Dieses Thema enthält im vierten Schritt der exemplarischen Vorgehensweise [entwickeln eine Lösung Vorhersageanalytik Azure maschinelles lernen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Erstellen eines Arbeitsbereichs maschinelles lernen](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Vorhandene Daten](machine-learning-walkthrough-2-upload-data.md)
3.  [Erstellen Sie einen neuen Test](machine-learning-walkthrough-3-create-new-experiment.md)
4.  **Schulen und Auswerten der Modelle**
5.  [Den Web Service bereitstellen](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Zugriff auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

----------

Die Vorteile von Azure Machine Learning Studio zum Erstellen von Learning Modelle gehört die Möglichkeit, Testen mehr als ein Modell gleichzeitig in einem Experiment und vergleichen. Dieses Experiment können Sie die beste Lösung für Ihr Problem zu finden.

Im Experiment, das wir in dieser exemplarischen Vorgehensweise entwickeln wir zwei verschiedene Typen von Modellen erstellen und vergleichen Sie die Bewertungsergebnisse Algorithmus entscheiden wir in unserer letzten Experiment verwenden möchten.  

Es gibt verschiedene Modelle wählen wir konnte. Um die Modelle anzuzeigen, erweitern Sie in der Palette Modul **Maschinelles lernen** und dann **Modell initialisieren** und darunterliegenden Knoten. Für die Zwecke dieses Experiment wählen wir Support Vector Machine (SVM) und zwei Klassen erhöht Entscheidungsstrukturen Module.    

> [AZURE.TIP] Entscheidungshilfe maschinelles lernen Algorithmus am besten passt das Problem, das Sie lösen möchten, finden Sie [Algorithmen für Microsoft Azure Machine Learning auswählen](machine-learning-algorithm-choice.md).

##<a name="train-the-models"></a>Trainieren der Modelle
Zunächst richten Sie stärkere Entscheidungsbaummodell:  

1.  [Zwei Klassen erhöht Entscheidungsstruktur] suchen[ two-class-boosted-decision-tree] Modul in der Modul-Palette und die Leinwand ziehen.
2.  Der [Zug Modell] [ train-model] Modul auf die Leinwand ziehen, und verbinden Sie die Ausgabe des Moduls Struktur stärkere Entscheidung mit linken Eingangs-Port ("ungeschulten Modell") des [Zuges Modell] [ train-model] Modul.
    
    [Zwei Klassen erhöht Entscheidungsstruktur] [ two-class-boosted-decision-tree] Modul initialisiert generischen Modell und [Schulen Modell] [ train-model] Daten zum Trainieren des Modells verwendet. 
     
3.  Verbinden die linke Ausgabe ("Resultdataset") des linken [R-Skript ausführen] [ execute-r-script] Modul rechts input Port ("Dataset") des [Zuges Modell] [ train-model] Modul.

    > [AZURE.TIP] Wir brauchen nicht zwei Eingaben, Ausgaben des [R-Skript ausführen] [ execute-r-script] Modul für dieses Experiment, damit wir verlassen. 

4.  Wählen Sie das [Modell Zug] [ train-model] Modul. Im Bereich **Eigenschaften** auf **Spaltenauswahl zu starten**, wählen Sie **Alle Typen** in der Dropdownliste unten unter **Verfügbare Spalten** und geben Sie ein "Kreditrisiko" in das Textfeld. **Alle Typen** in der Dropdownliste unter **Ausgewählte Spalten**auswählen Wählen Sie "Kreditrisiko" und klicken Sie auf hervorgehobenen Pfeil, um **Ausgewählte Spalten**nach. 
5.  Klicken Sie auf **Speichern**.


Dieser Teil des Experiments sieht jetzt folgendermaßen aus:  

![Ein trainiert][1]

Richten wir SVM-Modell.  

Zunächst ein wenig Erläuterung SVM. Stärkere Entscheidungsstrukturen arbeiten mit Features eines beliebigen Typs. Allerdings seit SVM-Modul eine lineare Klassifizierung generiert, das Modell generiert den besten Fehler bei allen numerische Funktionen denselben Maßstab. Um alle numerischen Funktionen denselben Maßstab zu konvertieren, verwenden wir eine Transformation "Tanh" (mit [Daten normalisieren] [ normalize-data] Modul.) Dies wird [0,1] Bereich Zahlen verwandelt. String-Funktionen werden SVM-Modul kategorische Features und dann binäre 0-1-Funktionen müssen wir nicht manuell Zeichenfolge Features transformieren umgewandelt. Außerdem wollen wir die Kreditrisiko Spalte (21) transformieren - ist numerisch, aber Wert wir das Modell vorhergesagt, müssen wir es allein Ausbildung.  

Um das Modell SVM einzurichten, führen Sie folgende Schritte aus:

1.  Die [Zwei Klassen Vektor Maschine] [ two-class-support-vector-machine] Modul in der Modul-Palette und die Leinwand ziehen.
2.  Maustaste [Zug Modell] [ train-model] Modul wählen Sie **Kopie**die Leinwand Maustaste und wählen Sie **Einfügen**. Die Kopie des [Zuges Modell] [ train-model] Modul hat dieselbe Spaltenauswahl wie das Original.
3.  Verbinden die Ausgabe des Moduls SVM mit linken Eingangs-Port ("ungeschulten Modell") des zweiten [Zug Modell] [ train-model] Modul.
4.  Suchen nach [Daten normalisieren] [ normalize-data] Modul und die Leinwand ziehen.
5.  Verbinden die Eingabe dieses Moduls der linken Ausgabe des linken [R-Skript ausführen] [ execute-r-script] Modul (Hinweis Ausgang eines Moduls mit mehr als einem Modul verbunden sein kann).
6.  Verbinden der linken Ausgabeanschluss ("transformiert Dataset") [Daten normalisieren] [ normalize-data] Modul rechts input Port ("Dataset") des zweiten [Zug Modell] [ train-model] Modul.
7.  Im Bereich **Eigenschaften** die [Daten normalisieren] [ normalize-data] Modul wählen **Tanh** für **Transformation** Methodenparameter.
8.  Klicken Sie auf **Starten Spaltenauswahl**, für **Beginnen mit**"Keine Spalten" auswählen, **Wählen Sie in der ersten Dropdownliste** , **Spalte** in der zweiten Dropdownliste wählen und wählen Sie in der dritten Dropdownliste **numerische** Gibt an, dass alle numerischen Spalten (und nur numerische) transformiert werden.
9.  Klicken Sie auf das Pluszeichen (+) neben dieser Zeile – Dadurch kann eine Reihe von Dropdownmenüs. Wählen Sie in der ersten Dropdownliste **ausschließen** , und wählen Sie **Spaltennamen** in der zweiten Dropdownliste klicken Sie auf das Textfeld "Kreditrisiko" aus der Liste der Spalten. Dies gibt an, dass die Kreditrisiko Spalte ignoriert werden soll (wir müssen, da diese Spalte numerische und andernfalls umgewandelt würde).
10. Klicken Sie auf **OK**.  


[Daten normalisieren] [ normalize-data] Modul soll nun alle numerischen Spalten außer der Spalte Kreditrisiko Tanh Umwandlung durchführen.  

Dieser Teil des Experiment sollte jetzt folgendermaßen aussehen:  

![Die zweite trainiert][2]  

##<a name="score-and-evaluate-the-models"></a>Bewerten und Auswerten der Modelle

Wir verwenden die Daten, die durch die [Aufgeteilten Daten] getrennt wurde[ split] Modul unsere ausgebildeten Modelle erzielen. Dann Vergleichen wir die Ergebnisse der beiden Modelle finden Sie unter die bessere Ergebnisse generiert.  

1.  Die [Bewertung Modell] [ score-model] Modul und die Leinwand ziehen.
2.  [Zug Modell] verbinden[ train-model] Modul [Zwei Klassen erhöht Entscheidungsstruktur] an[ two-class-boosted-decision-tree] Modul links input Port [Bewertung Modell] [ score-model] Modul.
3.  Rechts Eingabeanschluss [Bewertung Modell] verbinden[ score-model] linken Ausgabe des rechten [Execute R Script] -Modul[ execute-r-script] Modul.

    [Bewertung Modell] [ score-model] Modul kann jetzt die Gutschrift Informationen aus den Daten durchlaufen das Modell und die Vorhersagen des Modells mit der tatsächlichen Risiken Spalte in den Daten generiert vergleichen.

4.  Kopieren und Einfügen der [Bewertung Modell] [ score-model] Modul eine zweite Kopie erstellen oder ein neues Modul auf die Leinwand ziehen.
5.  Linken Eingang dieses Modell SVM verbinden (d. h. Ausgang des [Zuges Modell] verbinden[ train-model] Modul [Zwei Klassen Vektor Maschine] an[ two-class-support-vector-machine] Modul).
6.  SVM-Modell müssen dieselbe Transformation auf die Testdaten werden wie die Trainingsdaten. So kopieren und Einfügen der [Daten normalisieren] [ normalize-data] Modul erstellen eine zweite Kopie und linken Ausgabe des rechten [Ausführen R Skript] an[ execute-r-script] Modul.
7.  Rechts Eingabeanschluss [Bewertung Modell] verbinden[ score-model] Modul linken Ausgabe der [Daten normalisieren] [ normalize-data] Modul.  

Um zwei Bewertungsergebnisse auszuwerten, verwenden wir ein [Modell auswerten] [ evaluate-model] Modul.  

1.  Das [Modell auswerten] [ evaluate-model] Modul und die Leinwand ziehen.
2.  Verbinden den linken Eingang Ausgang [Bewertung Modell] [ score-model] gesteigert Entscheidungsbaummodell zugeordnet.
3.  Verbinden den richtige Eingang zum anderen [Faktor Modell] [ score-model] Modul.  

Zum Ausführen des Experiments auf **Ausführen** klicken Schaltfläche unterhalb der Leinwand. Es kann einige Minuten dauern. Eine rotierende Indikator für jedes Modul zeigt ausgeführt wird und ein grünes Häkchen nach Abschluss des Moduls. Wenn alle Module aktiviert haben, hat das Experiment ausgeführt.

Der Versuch sollte jetzt wie folgt aussehen:  

![Beide Modelle auswerten][3]

Die Ergebnisse klicken Sie auf Ausgang des [Modells auswerten] [ evaluate-model] Modul und wählen **visualisieren**.  

Das [Modell auswerten] [ evaluate-model] Modul erzeugt ein Paar und Metriken, mit denen Sie die Ergebnisse der beiden Modelle erzielte vergleichen. Sie können die Ergebnisse als Empfänger Operator Merkmal (ROC) Kurven, Precision-Rückruf Kurven oder heben Sie Kurven anzeigen. Zusätzliche Daten angezeigt enthält eine Verwirrung Matrix kumulative Werte für die Fläche unter der Kurve (AUC) und andere. Sie können den Schwellenwert mit dem Schieberegler nach links oder rechts ändern und Einfluss der festgelegten Kriterien.  

Klicken Sie rechts neben dem Diagramm **erzielte Dataset** oder **erzielte Dataset vergleichen** zugehörige Kurve markieren und die Metrik unten. In der Legende für die Kurven "Erzielte Dataset" entspricht der linken Eingang [Modell ausgewertet] [ evaluate-model] Modul – in unserem Fall ist dies die stärkere Entscheidungsbaummodell. "Erzielte Dataset vergleichen" entspricht den richtigen Eingang - SVM-Modell in unserem Fall. Beim Anklicken eines dieser Kurve für dieses Modell wird hervorgehoben und die entsprechenden Metriken wie in der folgenden Abbildung dargestellt angezeigt.  

![ROC Kurven für Modelle][4]

Anhand dieser Werte können Sie entscheiden, welches Modell liegt, geben Sie die Ergebnisse, die Sie suchen. Sie können zurückgehen und das Experiment durch Ändern der Werte in den verschiedenen Modellen durchlaufen. 

> [AZURE.TIP] Jedem Datensatz dieser Iteration des Experiments ausführen bleibt im Verlauf ausführen. Sie können diese Iterationen anzeigen und zurück zu, der Leinwand **Aufrufen führen Sie** unten. Sie können auch im **Eigenschaftenbereich unmittelbar vorhergehende Iteration wieder haben Sie öffnen** klicken **Vor Ausführen** .
> 
Eine Iteration des Experiments können durch Klicken auf **Speichern unter** unter der Leinwand. Verwenden des Experiments **Zusammenfassung** und **Beschreibung** Eigenschaften zum Speichern der Iterationen Experiment versucht.

>  Weitere Informationen finden Sie unter [Verwalten Iterationen in Azure Machine Learning Studio experimentieren](machine-learning-manage-experiment-iterations.md).  


----------

**Weiter: [den Web Service bereitstellen](machine-learning-walkthrough-5-publish-web-service.md)**

[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train1.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train2.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train3.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train4.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
