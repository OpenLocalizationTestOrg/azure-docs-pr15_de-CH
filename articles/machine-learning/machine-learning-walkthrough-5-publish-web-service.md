<properties
    pageTitle="Schritt 5: Computer Learning-Webdienst bereitstellen | Microsoft Azure"
    description="Schritt 5 entwickeln eine vorbeugende Lösung Exemplarische Vorgehensweise: vorhersageexperiment in Machine Learning Studio als Web Service bereitstellen."
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


# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a>Exemplarische Vorgehensweise Schritt 5: Bereitstellen von Azure Machine Learning-Webdienst

Dies ist der fünfte Schritt der exemplarischen Vorgehensweise [entwickeln eine Lösung Vorhersageanalytik Azure maschinelles lernen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Erstellen eines Arbeitsbereichs maschinelles lernen](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Vorhandene Daten](machine-learning-walkthrough-2-upload-data.md)
3.  [Erstellen Sie einen neuen Test](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Schulen und Auswerten der Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  **Den Web Service bereitstellen**
6.  [Zugriff auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

----------

Um andere Möglichkeit das Vorhersagemodell verwenden, das wir in dieser exemplarischen Vorgehensweise erstellt haben, stellen wir ihn als Webdienst auf Azure.

Bisher haben wir Experimentieren mit unserem Modell. Aber bereitgestellte Dienst wird keine Erfassung - Vorhersagen mit der Eingabe des Benutzers basierend auf unser Modell generiert. Also wir vorbereitende Schritte dieses Experiment ein ***Training*** Experiment konvertiert werden einem ***vorhersehbaren*** experimentieren. 

Dies ist ein zweistufiger Prozess:  

1. Konvertieren der *Schulung experimentieren* wir erstellten *vorhersageexperiment*
2. Bereitstellen von vorhersageexperiment als Webdienst

Aber zunächst müssen wir dieses Experiment ein wenig zu optimieren. Wir haben zwei verschiedene Modelle des Experiments, aber wir wollen nur ein Modell Wenn wir dies als Webdienst bereitstellen.  

Angenommen, wir haben entschieden, dass stärkere Strukturmodell besser Modell verwendet wurde. Zunächst müssen also [Zwei Klassen Vektor Maschine] entfernen[ two-class-support-vector-machine] Modul und Module, die für das training verwendet wurden. Sie möchten das Experiment zuerst Kopieren mit dem Befehl **Speichern unter** am unteren Rand der Leinwand experimentieren.

Wir müssen folgende Module zu löschen:  

- [Zwei Klassen Vektor Maschine][two-class-support-vector-machine]
- [Schulen Modell] [ train-model] und [Bewertung Modell] [ score-model] Module verbunden waren
- [Normalisieren von Daten] [ normalize-data] (beide)
- [Auswerten des Modells][evaluate-model]

Wählen Sie das Modul und die ENTF-Taste drücken oder mit der rechten Maustaste des Moduls und wählen **Löschen**.

Jetzt sind wir dieses Modell [Zwei Klassen erhöht Entscheidungsstruktur]bereitstellen[two-class-boosted-decision-tree].

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Ein vorhersageexperiment trainingsexperiment konvertieren

Konvertieren in eine vorhersageexperiment umfasst drei Schritte:

1. Speichern Sie das Modell, das wir haben geschultes und unsere Module ersetzen
2. Zuschneiden des Experiments um Module zu entfernen, die nur für die Schulung benötigt wurden
3. Definieren von dem Webdienst Eingabe akzeptiert und wo die Ausgabe generiert

Glücklicherweise drei Schritte erfolgt durch **Festlegen von Webdienst** unten Experiment Leinwand (die Option **vorhersehbaren Webdienst** ) klicken.

Beim **Festlegen von Webdienst**klicken, werden verschiedene Aktionen durchgeführt:

- Geschulte Modell als ein Modul **Ausgebildeten Modell** in Modul Palette links von der Leinwand Experiment werden (Sie unter **Ausgebildeten**finden sie).
- Schulung verwendeten Module werden entfernt. Insbesondere:
  - [Zwei Klassen stärkere Entscheidungsstruktur][two-class-boosted-decision-tree]
  - [Zug-Modell][train-model]
  - [Aufteilen von Daten][split]
  - zweite [R-Skript ausführen] [ execute-r-script] für Testdaten verwendete Modul
- Gespeicherte ausgebildete Modell wird in das Experiment hinzugefügt.
- **Web Service ein-** und **Ausgabe von Web Service** -Module werden hinzugefügt.

> [AZURE.NOTE] Das Experiment wurde in zwei Teilen Registerkarten am oberen Rand der Leinwand Experiment hinzugefügt wurden gespeichert: ursprüngliche Schulung ist auf der Registerkarte **Training experimentieren**und neu erstellte vorhersehbaren ist unter **Predictive experimentieren**.

Wir müssen mit diesem bestimmten Experiment ein zusätzlicher Schritt erforderlich.
Wir haben zwei [R-Skript ausführen] [ execute-r-script] Module ermöglichen eine Gewichtung Funktion Daten zum Trainieren und testen. Wir müssen dies im Modell.

Machine Learning Studio entfernt ein [R-Skript ausführen] [ execute-r-script] Modul [Teilung] entfernen[ split] Modul. Jetzt können wir jetzt das andere entfernen und [Metadaten Editor] herstellen[ metadata-editor] direkt auf [Bewertung Modell][score-model].    

Experiment sollte jetzt folgendermaßen aussehen:  

![Geschulte Modell wird bewertet.][4]  

> [AZURE.NOTE] Sie Fragen sich vielleicht warum wir Dataset UCI Deutsch Daten predictive Experiment links. Der Dienst wird der Benutzer Daten nicht im ursprünglichen Dataset, also Warum im ursprüngliche Dataset im Modell?
>
>Es stimmt, dass der Dienst die Originaldaten Kreditkarte nicht. Aber das Schema für die Daten, einschließlich Informationen wie viele Spalten es gibt und welche Spalten numerisch. Diese Schemainformationen ist erforderlich, die Daten zu interpretieren. Wir lassen diese Komponenten verbunden, sodass Punktzahl Modul das Dataset-Schema hat, wenn der Dienst ausgeführt wird. Daten nicht verwendet, nur das Schema.  

Führen Sie dies einmal (klicken Sie auf **Ausführen**.) Möchten Sie überprüfen, ob das Modell noch funktioniert, klicken Sie auf die Ausgabe der [Bewertung Modell] [ score-model] Modul und wählen Sie **Ergebnisse anzeigen**. Sie sehen, dass die Originaldaten Wert für das Kreditrisiko ("erzielte Labels") und die Punktzahl Wahrscheinlichkeitswert ("erzielte Wahrscheinlichkeiten".) angezeigt wird 

## <a name="deploy-the-web-service"></a>Den Web Service bereitstellen

Sie können experimentieren oder eine klassische Webdienst basiert auf Azure-Ressourcen-Manager einen neuen Webdienst bereitstellen.

### <a name="deploy-as-a-classic-web-service"></a>Als klassische Web Service bereitstellen ###

Einen klassischen Webdienst abgeleitet Experiment Leinwand klicken Sie **Webdienst bereitstellen** und wählen Sie **Webdienst bereitstellen [Classic]**. Machine Learning Studio Experiment als Webdienst bereitstellt und Sie gelangen zum Dashboard für diesen Webdienst. Hier können Sie Experiment (**Anzeigen Schnappschuss** oder **neueste**) zurück und einem einfachen Test des Webdiensts (siehe **Test Webdienst** unten). Es gibt auch hier Informationen zum Erstellen von Programmen, die den Webdienst (mehr dazu im nächsten Schritt dieser exemplarischen Vorgehensweise) zugreifen können.

![Web Service-dashboard][6]

Sie können den Dienst konfigurieren, indem Sie auf die Registerkarte **Konfiguration** . Hier können Sie den Dienstnamen (es hat den Namen Experiment standardmäßig erteilt) und eine Beschreibung. Weitere benutzerfreundlichen Etiketten Geben Sie Eingabe- und Spalten.  

![Webdienst konfigurieren][5]  

### <a name="deploy-as-a-new-web-service"></a>Als neue Web Service bereitstellen

Um einen neuen Webdienst abgeleitet Experiment bereitzustellen, klicken Sie auf **Webdienst bereitstellen** Leinwand und **Webdienst bereitstellen [New]**. Machine Learning Studio überträgt in der Azure Machine Learning Services bereitstellen Experiment Webseite.

Geben Sie einen Namen für den Webdienst, und wählen Sie einen Zahlungsplan. Haben Sie eine vorhandene Zahlungsplan können Sie auswählen, müssen andernfalls Sie einen neuen Preisplan für den Dienst erstellen. 

1.  In der Dropdownliste **Preisplan für** einen vorhandenen Plan auswählen oder die Option **neuen Plan auswählen** .
2.  **Planname**Geben Sie einen Namen den Plan auf der Rechnung.
3.  Wählen Sie eine **Monatliche Planung Ebenen**. Die Plan Ebenen standardmäßig die Pläne für die Standardregion und den Webdienst wird dieser Region bereitgestellt.

Klicken Sie auf **Bereitstellen** und die Seite " **Schnellstart** " für Ihren Web Service zu öffnen.

Sie können den Dienst konfigurieren, durch Klicken auf die Option **Konfigurieren** . Hier können Sie den Dienstnamen (es hat den Namen Experiment standardmäßig erteilt) und eine Beschreibung. 

Klicken Sie zum Testen der Web Service auswählen die Option **Testen** (siehe unten **den Webdienst testen** ). Informationen zum Erstellen von Programmen, die auf den Webdienst zugreifen können, klicken Sie auf die Menüoption **verbrauchen** (mehr dazu im nächsten Schritt dieser exemplarischen Vorgehensweise).

> [AZURE.TIP] Nachdem Sie sie bereitgestellt haben, können Sie den Webdienst aktualisieren. Beispielsweise wenn Sie Ihr Modell ändern möchten, bearbeiten Sie trainingsexperiment, Optimieren Sie Modellparameter und klicken Sie auf **Webdienst bereitstellen**. Wählen Sie **Webdienst bereitstellen [Classic]** oder **[New] Webdienst bereitstellen**. Wenn das Experiment erneut bereitstellen ersetzt Webdienst das aktualisierte Modell verwendet.  

## <a name="test-the-web-service"></a>Testen des Webdienstes

### <a name="test-a-classic-web-service"></a>Klassischen Webdienst testen

Sie können den Dienst in Computer lernen oder im Azure Machine Learning Web-Portal testen. Im Azure Machine Learning Web-Portal Testen hat den Vorteil, dass Sie aktivieren 

**Machine Learning Studio testen**

Die Seite **DASHBOARD** klicken Sie auf **Test** unter **Standard-Endpunkt**. Ein Dialogfeld Popup und fordern Sie die Eingabedaten für den Dienst. Spalten, die im ursprünglichen Dataset der deutschen Gutschrift Risiko aufgetreten sind.  

Geben Sie Daten, und klicken Sie dann auf **OK**. 

**Testen Sie das Azure Machine Learning Web Portal**

Die Seite **DASHBOARD** klicken Sie **Test** Vorschau unter **Standard-Endpunkt**. Die Testseite im Azure Machine Learning Web-Portal für den Webdienst-Endpunkt wird geöffnet und fordert Sie für die Eingabedaten für den Dienst. Spalten, die im ursprünglichen Dataset der deutschen Gutschrift Risiko aufgetreten sind.

Klicken Sie auf **Anforderung / Antwort-Tests**. 

Im Web Service gibt die Daten über das Modul **Web Service Input** [Metadaten Editor] [ metadata-editor] Modul und [Bewertung Modell] [ score-model] Modul, bewertet. Die Ergebnisse werden dann vom Webdienst über **Web Service-Ausgabe**ausgegeben.

**Einen neuen Webdienst testen**

Azure Machine Learning Services Webportal klicken Sie auf **Test** am oberen Rand der Seite. **Die Testseite** wird geöffnet, und Sie können Eingabedaten für den Dienst. Die angezeigten Eingabefelder entsprechen den Spalten, die im ursprünglichen Dataset der deutschen Gutschrift Risiko aufgetreten. 

Geben Sie Daten und dann auf **Anforderung / Antwort-Tests**.

Die Testergebnisse werden auf der rechten Seite der Seite in der Ausgabespalte anzeigen. 

Beim Testen im Azure Machine Learning Web-Portal können Sie Daten, die mit den Anforderung / Antwort-Dienst testen. Wenn Sie den Webdienst in Machine Learning Studio erstellt, die Beispieldaten aus den Daten der mit Ihrem Modell stammt.

> [AZURE.TIP] Wie vorhersageexperiment konfiguriert, die gesamte [Punktzahl Modell] führt[ score-model] Modul zurückgegeben. Dies schließt alle eingegebenen Daten plus Wert für das Kreditrisiko und Wahrscheinlichkeit bewertet. Wollten Sie etwas anderes - zurück beispielsweise die Gutschrift Risiko Wert - und [Project-Spalten] einfügen könnte[ project-columns] Modul zwischen [Faktor] [ score-model] und **Web Service-Ausgabe** zu Spalten, nicht Webdienst gewünschte zurückgegeben. 

## <a name="manage-the-web-service"></a>Verwalten Sie den Webdienst

**Klassische Webdienst verwalten**

Nachdem Sie den klassischen Webdienst bereitgestellt haben, können Sie es aus dem [Azure-Verwaltungsportal](https://manage.windowsazure.com)verwalten.

1. Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.
2. Klicken Sie im Bereich Dienste Microsoft Azure **MASCHINELLES lernen**.
3. Klicken Sie auf den Arbeitsbereich.
4. Klicken Sie auf die Registerkarte **webServices** .
5. Klicken Sie auf den erstellten Webdienst.
6. Klicken Sie auf "Standard"-Endpunkt.

Sie können Aktionen wie überwachen wie der Webdienst ist hier und stellen Leistung Optimierungen wie viele gleichzeitige Anrufe des Dienstes ändern können.
Sie können auch den Webdienst in Azure Marketplace veröffentlichen.

Weitere Informationen finden Sie unter:

- [Endpunkte erstellen](machine-learning-create-endpoint.md)
- [Webdienst-Skalierung](machine-learning-scaling-webservice.md)
- [Azure Marketplace veröffentlichen Sie Azure Machine Learning-Webdienst](machine-learning-publish-web-service-to-azure-marketplace.md)

**Verwalten Sie einen Webdienst im Azure Machine Learning Web-portal**

Nachdem Sie den Webdienst Classic oder neu bereitgestellt haben können Sie sie in [Azure Machine Learning Services Webportal](https://servics.azureml.net)verwalten.

So überwachen Sie die Leistung des Webdienstes

1. Melden Sie sich bei [Azure Machine Learning Services Webportal](https://servics.azureml.net).
2. Klicken Sie auf **Webdienste**.
3. Klicken Sie auf den Webdienst.
4. Klicken Sie auf das **Dashboard**.

----------

**Weiter: [Zugriff auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)**

[1]: ./media/machine-learning-walkthrough-5-publish-web-service/publish1.png
[2]: ./media/machine-learning-walkthrough-5-publish-web-service/publish2.png
[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/