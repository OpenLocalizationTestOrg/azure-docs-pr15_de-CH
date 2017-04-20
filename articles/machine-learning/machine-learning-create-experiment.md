<properties
    pageTitle="Ein einfaches Experiment Machine Learning Studio | Microsoft Azure"
    description="Dieser Computer lernen Lernprogramm führt Sie durch eine einfache wissenschaftliches Experiment. Wir werden den Preis für ein Auto mit einer regressionsalgorithmus vorherzusagen."
    keywords="Experimentieren Sie lineare Regression Algorithmen für maschinelles lernen Computer lernen Lernprogramm, Vorhersagemodellen Verfahren Daten wissenschaftliches experiment"
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
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="garye"/>

# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Computer lernen Tutorial: Erstellen Sie Ihre erste Daten wissenschaftliches Experiment in Azure Machine Learning Studio

Dieser Computer lernen Lernprogramm führt Sie durch eine einfache wissenschaftliches Experiment. Wir erstellen eine lineare Regressionsmodell, das den Preis eines Autos basierend auf verschiedenen Faktoren wie prognostiziert und technischen Spezifikationen. Dazu verwenden wir Azure Machine Learning Studio zu durchlaufen eines einfachen predictive Analytics experimentieren.

*Vorhersageanalytik* ist eine Art von datenwissenschaft, die aktuelle Daten verwendet werden, um zukünftige Ergebnisse vorherzusagen. Sehen Sie ein sehr einfaches Beispiel Vorhersageanalysen Datenwissenschaft für Anfänger video 4: [eine Antwort mit einem einfachen Modell vorhergesagt](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) (Laufzeit: 7:42).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Wie hilft Machine Learning Studio?

Machine Learning Studio erleichtert es einem Experiment mit Drag & Drop-Modulen mit Vorhersagemodellen vorprogrammiert einrichten. Führen Sie das Experiment Antwort Vorhersagen und verwenden Sie Machine Learning Studio *Modell erstellen*, *Trainieren des Modells*und *Bewertung und Test des Modells*.

Geben Sie Machine Learning Studio: [https://studio.azureml.net](https://studio.azureml.net). Machine Learning Studio vor angemeldet haben, klicken Sie auf **hier anmelden**. Andernfalls klicken Sie auf **Anmelden** und kostenlosen und kostenpflichtigen Optionen entscheiden.

Weitere allgemeine Informationen zu Computer Learning Studio finden Sie unter [Neuigkeiten Machine Learning Studio?](machine-learning-what-is-ml-studio.md)

## <a name="five-steps-to-create-an-experiment"></a>Fünf Schritte zum Erstellen von Hypothesen

In diesem Lernprogramm lernen Computer folgen Sie fünf grundlegenden Schritte zum Erstellen von Hypothesen Machine Learning Studio erstellen, Trainieren und bewerten Ihr Modell:

- Modell erstellen
    - [Schritt 1: Get data]
    - [Schritt 2: Daten vorverarbeiten]
    - [Schritt 3: Definieren von Funktionen]
- Trainieren Sie das Modell
    - [Schritt 4: Auswählen und Anwenden eines Learning-Algorithmus]
- Bewerten und Testen des Modells
    - [Schritt 5: Neue Autos Preise Vorhersagen]

[Schritt 1: Get data]: #step-1-get-data
[Schritt 2: Daten vorverarbeiten]: #step-2-preprocess-data
[Schritt 3: Definieren von Funktionen]: #step-3-define-features
[Schritt 4: Auswählen und Anwenden eines Learning-Algorithmus]: #step-4-choose-and-apply-a-learning-algorithm
[Schritt 5: Neue Autos Preise Vorhersagen]: #step-5-predict-new-automobile-prices


## <a name="step-1-get-data"></a>Schritt 1: Get data

Gibt eine Anzahl von Beispiel-Datasets enthalten mit Computer lernen, die Sie auswählen können, und Importieren von Daten aus vielen Quellen. In diesem Beispiel verwenden wir die enthalten Stichprobendataset **Automobile Daten (Raw)**.
Dieses Dataset enthält Einträge für eine Anzahl von Automobile, einschließlich Informationen wie Marke, Modell, technische Daten und Preis.

1. Durch Klicken auf **+ neu** am unteren Fensterrand Machine Learning Studio einen neuen Test starten, wählen Sie **EXPERIMENTIEREN**und dann **Leere experimentieren**. Wählen Sie Experiment Standardnamen am oberen Rand der Leinwand und benennen Sie ihn in einen aussagekräftigeren Namen, z. B. **Automobile preisvorhersage**.

2. Links des Experiments ist Leinwand eine Palette von Datasets und Module. Typ **Auto** in das Suchfeld am oberen Rand dieser Palette zu Dataset Bezeichnung **Automobile Daten (Raw)**.

    ![Palette suchen][screen1a]

3. Ziehen Sie das Dataset Experiment Leinwand.

    ![DataSet][screen1]

Sieht diese Daten finden auf der Ausgangs-Port am unteren Rand des Autos Datasets und wählen Sie **Visualisierung**.

![Modul-Ausgang][screen1c]

Die Variablen im Dataset als Spalten angezeigt, und jeweils ein Auto als angezeigt. Die rechten Spalte (Spalte 26 und mit dem Titel "Price") ist die Zielvariable werden wir versuchen vorherzusagen.

![DataSet-Visualisierung][screen1b]

Schließen Sie das Fenster Visualization durch Klicken auf das "**X**" in der oberen rechten Ecke.

## <a name="step-2-preprocess-data"></a>Schritt 2: Daten vorverarbeiten

Ein Dataset ist einige preprocessing vor analysiert werden kann. Sie können die fehlenden Werte in den Spalten der verschiedenen Zeilen bemerkt. Diese fehlende Werte müssen bereinigt werden, damit das Modell ordnungsgemäß Daten analysieren kann. In diesem Fall entfernen wir alle Zeilen, die fehlende Werte aufweisen. Auch hat die **normalisiert Verluste** Spalte Teil fehlende Werte daher diese Spalte aus dem Modell vollständig ausgeschlossen werden.

> [AZURE.TIP] Fehlende Werte aus den Eingabedaten Reinigung ist Voraussetzung für die meisten Module verwenden.

Zunächst werden wir **normalisiert Verluste** Spalte entfernen und dann entfernen wir jede Zeile, die Daten fehlen.

1. Geben Sie in das Suchfeld am oberen Rand der Modul-Palette zu [Spalten im Dataset auswählen] **Wählen Sie Spalten** [ select-columns] Modul und Ziehen des Experiments Leinwand und Ausgang des Datasets **Automobile Daten (Raw)** herstellen. Dieses Modul ermöglicht die Datenspalten wir einschließen oder ausschließen möchten im Modell auswählen.

2. Wählen Sie die [Spalten im Dataset auswählen] [ select-columns] Modul und klicken Sie im **Eigenschaftenbereich** auf **Spaltenauswahl starten** .

    - Klicken Sie auf der linken Seite **mit Regeln**
    - Klicken Sie unter **Mit** **allen Spalten**aus. Dies weist [Spalten im Dataset auswählen] [ select-columns] durch die Spalten (außer wir ausschließen).
    - Dropdown-Liste Wählen Sie **ausschließen** und **Spaltennamen**und klicken Sie dann auf das Textfeld. Eine Liste der Spalten angezeigt. Wählen Sie **normalisiert Verluste**und das Textfeld hinzugefügt.
    - Klicken Sie auf Häkchen (OK), um die Spaltenauswahl schließen.

    ![Spalten auswählen][screen3]

    Eigenschaftenbereich **Wählen Sie Spalten im Dataset** gibt an, dass sie alle Spalten aus dem Dataset außer **normalisiert Verluste**verläuft.

    ![Wählen Sie Spalten im Dataset-Eigenschaften][screen4]

    > [AZURE.TIP] Durch Doppelklicken auf das Modul und Eingeben von Text können Sie einen Kommentar zu einem Modul hinzufügen. Dadurch können Sie einen Blick in das Experiment geht das Modul. Doppelklicken Sie in diesem Fall auf die [Spalten im Dataset auswählen] [ select-columns] Modul, und geben Sie den Kommentar "Exclude normalisiert Verluste."

3. Ziehen Sie die [Saubere fehlen Daten] [ clean-missing-data] Modul des Experiments Zeichnungsbereich und [Wählen Sie Spalten im Dataset] an[ select-columns] Modul. Wählen Sie im Bereich **Eigenschaften** **Reinigung Modus** bereinigen die Daten durch Entfernen von Zeilen, die Werte fehlen **ganze Zeile entfernen** . Doppelklicken Sie auf das Modul, und geben Sie den Kommentar "Fehlende Zeilen entfernen".

    ![Saubere Fehlende Eigenschaften][screen4a]

4. Ausführen des Experiments durch **klicken unter experimentcanvas** .

Wenn das Experiment abgeschlossen ist, haben alle Module ein grünes Häkchen an, dass sie erfolgreich beendet. Beachten Sie auch den Status **Fertig ausgeführt** , in der oberen rechten Ecke.

![Ersten Test ausführen][screen5]

Im Experiment dazu lediglich die Daten bereinigen. Soll bereinigte Dataset anzuzeigen, klicken Sie auf linken Ausgang der [Bereinigten Daten fehlen] [ clean-missing-data] -Modul ("gesäubert" Dataset) und wählen Sie **visualisieren**. Beachten Sie, dass die Spalte **normalisiert Verluste** nicht mehr enthalten ist und keine fehlenden Werte.

Jetzt bereinigen Daten können wir Was geben wir das Vorhersagemodell verwenden wollen.

## <a name="step-3-define-features"></a>Schritt 3: Definieren von Funktionen

Machine learning sind *Features* einzelne messbare Eigenschaften etwas, das Sie interessiert. Unser Dataset jede Zeile stellt ein Auto, und jeder Spalte ist ein Feature, Auto.

Finden eines guten erfordert Funktionen zum Erstellen eines Vorhersagemodells experimentieren und wissen über das Problem lösen möchten. Einige Features sind für das Ziel als andere vorherzusagen. Außerdem haben einige Features einen starken Zusammenhang mit anderen Features (z. B. Stadt mpg oder Autobahn mpg), sie werden das Modell nicht viele neue Informationen hinzufügen und können entfernt werden.

Erstellen Sie ein Modell, das eine Teilmenge der Features in unser Dataset verwendet. Sie können zurückkehren und wählen Sie andere Features Experiments erneut ausführen und sehen, wenn Sie bessere Ergebnisse erzielen. Aber starten, versuchen Sie Folgendes:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Ziehen Sie weitere [Spalten im Dataset auswählen] [ select-columns] des Experiments Leinwand und linken Ausgang der [Sauberen fehlen Daten] verbinden[ clean-missing-data] Modul. Doppelklicken Sie auf das Modul, und geben Sie "Select Features für die Vorhersage".

2. Klicken Sie im **Eigenschaftenbereich** auf **Spaltenauswahl starten** .

3. Klicken Sie auf **Regeln**.

4. Klicken Sie **keine Spalten**unter **Mit** und wählen Sie dann in der Filterzeile **einschließen** und **Spaltennamen** . Geben Sie unsere Liste der Spaltennamen. Dies weist das Modul nur Spalten durchlaufen, die wir angeben.

    > [AZURE.TIP] Ausführen des Experiments, haben wir sichergestellt, dass [Bereinigten Daten fehlen] Spaltendefinitionen für die Daten aus dem Dataset durchlaufen[ clean-missing-data] Modul. Dies bedeutet, dass andere Module die Verbindung auch Informationen aus dem Datensatz.

5. Klicken Sie auf das Häkchen (OK).

![Spalten auswählen][screen6]

Dies erzeugt das Dataset in der Learning-Algorithmus in den nächsten Schritten verwendet werden. Sie können zurück und versuchen Sie es erneut mit anderen Features.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>Schritt 4: Auswählen und Anwenden eines Learning-Algorithmus

Nun, dass die Daten bereit, besteht ein Vorhersagemodell erstellen Training und testen. Verwenden wir unsere Daten zu trainieren Sie das Modell Testen des Modells zu sehen, wie sie Preise Vorhersagen. Jetzt Gedanken Sie über wozu wir benötigen trainieren und Testen eines Modells.

*Klassifizierung* und *Regression* sind zwei Typen von überwachten Computer Techniken. Klassifizierung prognostiziert eine Antwort von einem definierten Satz von Kategorien, wie eine Farbe (Rot, Grün oder Blau). Regression wird verwendet, um eine Zahl vorherzusagen.

Um Preis, vorherzusagen, die eine Zahl ist, verwenden wir ein Regressionsmodell. Für dieses Beispiel trainieren wir eine einfache *lineare Regression* Modell und im nächsten Schritt werden wir sie testen.

1. Daten verwendet für Schulung und Testen von Teilen in separaten und legt testen. Markieren und ziehen Sie die [Aufgeteilten Daten] [ split] des Experiments Leinwand und verbinden Sie ihn mit der Ausgabe des letzten [Spalten im Dataset auswählen] [ select-columns] Modul. **Bruchteil der Zeilen in der ersten ausgabedataset** auf 0,75 festgelegt. Auf diese Weise wir 75 Prozent der Daten zum Trainieren des Modells verwenden und 25 Prozent testen halten.

    > [AZURE.TIP] **Random Seed** -Parameter ändern, können Sie unterschiedliche Stichproben für Training und Tests erstellen. Dieser Parameter steuert das seeding der Pseudo-Zufallszahlen-Generators.

2. Ausführen des Experiments. Dadurch können die [Spalten im Dataset auswählen] [ select-columns] und [Aufgeteilten Daten] [ split] Module Spaltendefinitionen an Module übergeben wir als Nächstes hinzufügen werden.  

3. Wählen Sie lernalgorithmus erweitern Sie Kategorie **Computer lernen** in der Modul-Palette zum linken Rand der Leinwand und dann **Modell initialisieren**. Mehrere Kategorien mit Algorithmen für maschinelles lernen initialisieren Module angezeigt.

    Wählen Sie für dieses Experiment [Lineare Regression] [ linear-regression] Modul Kategorie **Regression** (Sie finden auch das Modul "linearen Regression" in das Suchfeld Palette eingeben), und ziehen Sie es auf die Leinwand experimentieren.

4. Und ziehen Sie das [Zug-Modell] [ train-model] Modul zur Leinwand experimentieren. Verbinden den linken Eingang in die Ausgabe der [Linearen Regression] [ linear-regression] Modul. Verbinden Sie den richtigen Eingang mit Training-Datenausgabe (linke Port) [Aufteilen] [ split] Modul.

5. Wählen Sie das [Modell Zug] [ train-model] Modul klicken Sie im **Eigenschaftenbereich** auf **Spaltenauswahl starten** und dann die Spalte **Price** . Dies ist der Wert, den unser Modell vorhergesagt wird.

    ![Wählen Sie die Spalte "Price"][screen7]

6. Ausführen des Experiments.

Das Ergebnis ist ausgebildeten Regressionsmodell, mit der neue Beispiele Vorhersagen zu bewerten.

![Anwenden des Algorithmus für maschinelles lernen][screen8]

## <a name="step-5-predict-new-automobile-prices"></a>Schritt 5: Neue Autos Preise Vorhersagen

Da wir mit 75 Prozent unserer Daten trainiert haben, wir können andere 25 Prozent der Daten wie auch Ergebnis unserer Modellfunktionen.

1. Und ziehen Sie die [Bewertung Modell] [ score-model] Modul des Experiments Leinwand und die Ausgabe des [Zuges Modell] der linken Eingang an[ train-model] Modul. Verbinden den richtige Eingang der Test Data Ausgabe (rechts Port) [Aufteilen] [ split] Modul.  

    ![Bewertung Modell Modul][screen8a]

2. Experiments und zeigen die Ausgabe aus dem [Faktor Modell] [ score-model] Modul auf der Ausgangs-Port und wählen Sie **visualisieren**. Die Ausgabe zeigt die vorhergesagten Werte für Preis und den bekannten Werten von Testdaten.  

3. Schließlich testen Sie die Qualität der Ergebnisse wählen Sie Drag & Drop [Modell auswerten] [ evaluate-model] Modul des Experiments Leinwand und die Ausgabe der [Bewertung Modell] der linken Eingang an[ score-model] Modul. (Es sind zwei Eingänge [Modell auswerten] [ evaluate-model] Modul können zwei Modelle vergleichen.)

4. Ausführen des Experiments.

Zum Anzeigen der Ausgabe im [Modell ausgewertet] [ evaluate-model] Modul auf der Ausgangs-Port und wählen Sie **visualisieren**. Für unser Modell werden die folgenden Statistiken angezeigt:

- **Meine Absolute Fehler** (MAE): der Durchschnitt der absoluten Fehler ( *Fehler* ist die Differenz zwischen der vorhergesagte Wert und der tatsächliche Wert).
- **Mittlere quadratische Fehler Stamm** (RMSE): die Quadratwurzel der Durchschnitt der quadratischen Fehler Prognosen für das testdataset.
- **Relativen Absolute Fehler**: der Durchschnitt der absoluten Fehler relativ die absolute Differenz zwischen den tatsächlichen Werten und dem Mittelwert aller aktuellen Werte.
- **Relative quadrierten Fehler**: Mittelwert der quadratischen Fehler relativ das Quadrat der Differenz zwischen den Istwerten und den Mittelwert aller aktuellen Werte.
- **Koeffizienten der Bestimmung**: Alias **R-squared Wert**, dies ist eine statistische Kennzahl, die angibt, wie ein Modell Daten passt.

Für jeden Fehler ist kleiner Statistiken besser. Ein kleinerer Wert gibt an, dass die Vorhersagen genauer die tatsächlichen Werten entsprechen. **Koeffizienten der Bestimmung**, je näher der Wert ist auf eins (1,0) besseren Vorhersagen.

![Ergebnisse][screen9]

Der letzte Versuch sollte wie folgt aussehen:

![Computer lernen Tutorial: Führen Sie lineare Regression Experiment, das Vorhersagemodellen Methoden verwendet.][screen10]

## <a name="next-steps"></a>Nächste Schritte

Eine erste Computer lernen Lernprogramm abgeschlossen haben und das Experiment eingerichtet haben können Sie durchlaufen, um das Modell zu verbessern. Beispielsweise können Sie die Funktionen ändern Ihre Vorhersage verwenden. Oder Sie können die Eigenschaften der [Linearen Regression] [ linear-regression] Algorithmus oder einen anderen Algorithmus insgesamt. Sie können auch mehrere Computer gleichzeitig lernen Algorithmen Test hinzufügen und zwei mit dem [Modell auswerten] [ evaluate-model] Modul.

> [AZURE.TIP] Die Schaltfläche **Speichern unter** unter der Leinwand Experiment Iteration des Experiments kopieren. Sie können alle Iterationen des Tests einsehen **Aufrufen ausführen** unter der Leinwand. Siehe [Verwalten experimentieren Iterationen in Azure Machine Learning Studio] [ runhistory] Weitere Informationen.

[runhistory]: machine-learning-manage-experiment-iterations.md

Wenn Sie mit Ihrem Modell zufrieden sind, können Sie es als ein Webdienst Automobile Preise mit neuen Daten vorhergesagt bereitstellen. Finden Sie unter [Bereitstellen einer Azure Machine Learning-Webdienst] [ publish] Weitere Informationen.

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Eine umfassende und detaillierte Anleitung Techniken zum Erstellen von Vorhersagemodellen Schulung, bewerten und Bereitstellen eines Modells finden Sie unter [Entwickeln einer vorhersehbaren Lösung mithilfe von Azure Machine Learning][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
