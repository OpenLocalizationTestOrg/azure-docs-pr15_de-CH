<properties
    pageTitle="Schritt 3: Erstellen Sie einen neuen Computerlernen Test | Microsoft Azure"
    description="Schritt 3 entwickeln eine vorbeugende Lösung Exemplarische Vorgehensweise: erstellen ein neuen trainingsexperiments in Azure Machine Learning Studio."
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
    ms.date="10/05/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Exemplarische Vorgehensweise Schritt 3: Erstellen eines neuen Azure Machine Learning Experiments

Dies ist der dritte Schritt der exemplarischen Vorgehensweise [entwickeln eine Lösung Vorhersageanalytik Azure maschinelles lernen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Erstellen eines Arbeitsbereichs maschinelles lernen](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Vorhandene Daten](machine-learning-walkthrough-2-upload-data.md)
3.  **Erstellen Sie einen neuen Test**
4.  [Schulen und Auswerten der Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Den Web Service bereitstellen](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Zugriff auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

----------

Im nächsten Schritt dieser exemplarischen Vorgehensweise ist ein Experiment in Machine Learning Studio zu erstellen, die hochgeladen Dataset verwendet.  

1.  Studio klicken Sie auf **+ neu** am unteren Rand des Fensters.
2.  Wählen Sie **EXPERIMENTIEREN**und dann "Blank Experiment". Wählen Sie Experiment Standardnamen am oberen Rand der Leinwand und etwas Sinnvolles umbenennen

    > [AZURE.TIP] In diesem Zusammenhang empfiehlt es sich, **Zusammenfassung** und **Beschreibung** für das Experiment im **Eigenschaftenbereich** ausfüllen. Diese Eigenschaften geben die Möglichkeit des Experiments dokumentieren, wer es später Ihre Ziele und Methodik verstehen.

3.  In der Unterrichtseinheit Palette links des Experiments Leinwand, erweitern Sie **Datasets gespeichert**.
4.  Suchen unter **Eigene Datasets** erstellten Dataset und die Leinwand ziehen. Das Dataset finden durch Eingabe des Namens in **das Suchfeld oben der Palette** .  

##<a name="prepare-the-data"></a>Vorbereiten der Daten
Sie können die ersten 100 Zeilen der Daten und einige statistische Informationen für das gesamte Dataset anzeigen, indem Ausgang des Datasets (dem kleinen Kreis unten) und **visualisieren**.  

Da die Datei mit Spaltenüberschriften kommen, hat Studio generische Überschriften *(Col1, Col2 usw.)*. Gute werden nicht für ein Modell erstellen, aber sie erleichtern die Daten im Experiment. Auch wenn wir schließlich dieses Modell in einen Webdienst veröffentlichen, werden die Überschriften identifizieren die Spalten für den Benutzer des Dienstes.  

Wir können die Spaltenüberschriften mit den [Metadaten bearbeiten] [ edit-metadata] Modul.
Anhand der [Metadaten bearbeiten] [ edit-metadata] Modul Metadaten Dataset ändern. In diesem Fall können sie weitere angezeigten Spaltenüberschriften enthalten. 

[Metadaten bearbeiten]mit[edit-metadata], Sie zunächst angeben, welche Spalten ändern (in diesem Fall alle.) Geben Sie anschließend die Aktion für diese Spalten (in diesem Fall Spaltenüberschriften ändern.)

1.  Geben Sie in der Palette Modul "Metadaten" im **Suchfeld** . Sehen Sie [Metadaten bearbeiten] [ edit-metadata] in der Liste der Module angezeigt.
2.  Klicken und ziehen Sie die [Metadaten bearbeiten] [ edit-metadata] Modul auf der Leinwand und das Dataset bereits hinzugefügte unterschritten.
3.  Verbinden Sie das Dataset mit [Metadaten bearbeiten][edit-metadata]: Klicken Sie auf den Ausgang des Datasets (dem kleinen Kreis unten Dataset.) Ziehen Sie als Nächstes auf den Eingang der [Metadaten bearbeiten] [ edit-metadata] (den kleinen Kreis am Anfang des Moduls), lassen Sie die Maustaste los. Dataset und Modul bleiben verbunden, wenn Sie entweder auf der Leinwand verschieben.

    Der Versuch sollte jetzt wie folgt aussehen:  

    ![Hinzufügen von Metadaten bearbeiten][2]
    
    Das rote Ausrufezeichen zeigt an, dass wir noch die Eigenschaften für dieses Modul eingerichtet haben. Wir tun, weiter.
    
    > [AZURE.TIP] Durch Doppelklicken auf das Modul und Eingeben von Text können Sie einen Kommentar zu einem Modul hinzufügen. Dadurch können Sie einen Blick in das Experiment geht das Modul. Doppelklicken Sie in diesem Fall auf die [Metadaten bearbeiten] [ edit-metadata] Modul und geben Sie den Kommentar "Spaltenüberschriften hinzufügen". Klicken Sie auf der Leinwand um das Textfeld zu schließen. Um den Kommentar anzuzeigen, klicken Sie auf den Pfeil für das Modul.

4.  Wählen Sie [Metadaten bearbeiten][edit-metadata], klicken Sie im **Eigenschaften** rechts von der Leinwand auf **Spaltenauswahl starten**.
5.  Im Dialogfeld **Spalten auswählen** wählen Sie alle Zeilen in **Verfügbare Spalten** , und klicken Sie auf > **Ausgewählte Spalten**zu verschieben.
Das Dialogfeld sollte wie folgt aussehen: ![Spaltenmarkierer alle Spalten ausgewählt][4]
7.  Klicken Sie auf das Häkchen **OK** .
8.  Suchen Sie im Bereich **Eigenschaften** für den **neuen Spaltennamen** Parameter. Geben Sie in diesem Feld eine Liste von Namen für die Spalten im Dataset, getrennt durch Kommas und Spalte 21. Erhalten Sie die Spaltennamen aus der Dokumentation Dataset UCI-Website oder zur Vereinfachung können Sie kopieren und Einfügen in der folgenden Liste:  

          Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  

    Eigenschaftenbereich sieht folgendermaßen aus:

    ![Eigenschaften für Metadaten bearbeiten][1]

> [AZURE.TIP] Wenn Sie überprüfen, Spaltenüberschriften möchten, Ausführen des Experiments (klicken Sie unter experimentbereich **Ausführen** ). Beim Abschluss ausgeführt (Metadaten [Bearbeiten]wird ein grünes Häkchen angezeigt[edit-metadata]), klicken Sie auf den Ausgang der [Metadaten bearbeiten] [ edit-metadata] Modul und wählen **visualisieren**. Sie können die Ausgabe eines beliebigen Moduls auf die gleiche Weise den Status der Daten des Experiments anzeigen anzeigen.

##<a name="create-training-and-test-datasets"></a>Ausbildung erstellen und Testen von datasets

Der nächste Schritt des Experiments soll separate Datasets, die wir zum Trainieren und Testen unser Modell verwenden.

Dazu verwenden wir die [Aufgeteilten Daten] [ split] Modul.  

1.  Die [Aufgeteilten Daten] [ split] Modul auf die Leinwand ziehen und die letzten [Metadaten bearbeiten] mit[ edit-metadata] Modul.
2.  Standardmäßig das Teilungsverhältnis 0,5 und der Parameter **Randomized aufgeteilt** . Dies bedeutet, dass eine zufällige Daten Ausgabe über einen Anschluss der [Aufgeteilten Daten] ist[ split] Modul und durch die andere Hälfte. Diese sowie Parameter **Random Seed** Aufteilung zwischen Trainings- und Testdaten ändern können. Für dieses Beispiel lassen wir sie-ist.
    
    > [AZURE.TIP] Die Eigenschaft **Teil Zeilen im ersten ausgabedataset** bestimmt wie viel Daten Ausgabe durch den linken Ausgang. Beispielsweise ist das Verhältnis auf 0,7 festgelegt, 70 % der Daten Ausgabe über den linken Port und 30 % über den richtigen Anschluss.  
    
3. Doppelklicken Sie auf die [Aufgeteilten Daten] [ split] Modul und geben Sie den Kommentar, "Training und Tests Daten split 50 %". 

Wir können die Ausgaben [Aufteilen] [ split] Modul wir wie jedoch wählen wir linke Ausgabe verwenden Daten und rechts als Daten ausgegeben.  

Wie auf der Website UCI kostet ein hohes Kreditrisiko niedrigen sicherzustellen fünfmal größer als die Kosten für ein geringes Kreditrisiko hoch sicherzustellen. Um dies zu berücksichtigen, erstellen wir ein neues Dataset, das diese Kostenfunktion wiedergibt. Im neuen Dataset wird jede hohes Risiko wird fünf Mal repliziert, während jeder niedriges Risiko wird nicht repliziert.   

Können wir diese Replikation R mit:  

1.  Und ziehen Sie das [R-Skript ausführen] [ execute-r-script] auf das Experiment Leinwand und Verbinden der linken Ausgabeanschluss [Aufgeteilten Daten] [ split] ersten Eingang ("Dataset1") des [Execute R Script] -Modul[ execute-r-script] Modul.
2. Doppelklicken Sie auf [R-Skript ausführen] [ execute-r-script] Modul und geben Sie den Kommentar "Set Kosten Anpassung".
2.  Löschen Sie im Bereich **Eigenschaften** den Standardtext im **R** Skriptparameter, und geben Sie dieses Skript:

          dataset1 <- maml.mapInputPort(1)
          data.set<-dataset1[dataset1[,21]==1,]
          pos<-dataset1[dataset1[,21]==2,]
          for (i in 1:5) data.set<-rbind(data.set,pos)
          maml.mapOutputPort("data.set")


Wir diese Replikationsvorgang für jede Ausgabe [Aufteilen] müssen[ split] Modul so, dass die Trainings- und Daten dieselbe Einst.

1.  Mit der rechten Maustaste [Ausführen R Skript] [ execute-r-script] Modul und wählen Sie **Kopieren**.
2.  Die Leinwand Experiment Maustaste und wählen Sie **Einfügen**.
3.  Verbinden Sie den ersten Eingang dieses [R-Skript ausführen] [ execute-r-script] Modul rechts-Ausgang [Aufgeteilten Daten] [ split] Modul. 
4.  Klicken Sie am unteren Rand des Bereichs auf **Ausführen**. 

> [AZURE.TIP] Das Skriptmodul ausführen R Kopie dasselbe Skripts wie das Originalmodul. Beim Kopieren und ein Modul auf der Leinwand einfügen behält die Kopie alle Eigenschaften des Originals.  

Experiment sieht nun folgendermaßen aus:

![Split-Modul und R Skripts hinzufügen][3]

Weitere Informationen zur Verwendung von R-Skripts in Ihre Experimente finden Sie unter [erweitern das Experiment mit](machine-learning-extend-your-experiment-with-r.md).

**Weiter: [Zug und Auswerten der Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
