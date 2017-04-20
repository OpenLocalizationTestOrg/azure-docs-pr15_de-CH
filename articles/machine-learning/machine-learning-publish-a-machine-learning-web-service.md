<properties
    pageTitle="Bereitstellen eines Webdiensts maschinelles lernen | Microsoft Azure"
    description="Zum Konvertieren eines trainingsexperiments vorhersehbaren Experiment, Bereitstellung vorbereiten und als Azure Machine Learning Web Service bereitstellen."
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

# <a name="deploy-an-azure-machine-learning-web-service"></a>Bereitstellen eines Webdiensts Azure maschinelles lernen

Azure Machine Learning ermöglicht das Erstellen, testen und Bereitstellen von predictive Analytics-Lösungen.

Aus einer allgemeinen der Sicht geschieht dies in drei Schritten:

- **[Erstellen eines trainingsexperiments]** - Azure Machine Learning Studio ist eine gemeinsame visuelle, die Sie verwenden und Testen eines Vorhersageanalytik Modells mit Daten, die Sie angeben.
- **[Konvertieren eines vorhersehbaren Experiments]** - einmal Modell mit vorhandenen Daten ausgebildet und möchten sie verwenden, um neue Daten, Vorbereiten und optimieren das Experiment Vorhersagen.
- **Bereitstellen als Webdienst** - vorhersehbaren Test als [neuen] oder [klassische] Azure Web Service bereitstellen. Benutzer können dem Modell senden und Empfangen von Vorhersagen des Modells.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Erstellen eines Experiments Schulung

Zum Trainieren eines Modells Vorhersageanalysen verwenden Sie Azure Machine Learning Studio trainingsexperiment erstellen, können Sie verschiedene Module Daten laden, die Daten nach Bedarf, Machine Learning-Algorithmen anwenden und die Ergebnisse auswerten. Sie können einem Experiment durchlaufen und versuchen abweichender Learning Algorithmen zu vergleichen und die Ergebnisse auswerten.

Prozess erstellen und Verwalten von Schulung Versuche an anderer Stelle genauer behandelt. Weitere Informationen finden Sie in folgenden Artikeln:

- [Erstellen Sie einen einfachen Test in Azure Machine Learning Studio](machine-learning-create-experiment.md)
- [Eine vorausschauende Lösung mit Azure maschinelles lernen](machine-learning-walkthrough-develop-predictive-solution.md)
- [Importieren von Trainingsdaten in Azure Machine Learning Studio](machine-learning-data-science-import-data.md)
- [Experiment Iterationen in Azure Machine Learning Studio verwalten](machine-learning-manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Ein vorhersageexperiment trainingsexperiment konvertieren

Nachdem das Modell trainiert haben, können Sie das Experiment Schulung in vorhersageexperiment auf neue Daten konvertieren.

In einen vorhersageexperiment konvertieren, bekommen ausgebildete Modell als Punktzahl Webdienst bereitgestellt Sie. Benutzer des Webdiensts können Daten senden, Modell und Modell wird die Vorhersageergebnisse zurücksenden. Konvertieren einer vorhersehbaren Experiment dabei wie das Modell von anderen Benutzern verwendet werden soll.

Prädiktive Experiment Training Test umwandeln, **Führen Sie** am unteren Rand der Leinwand Experiment, **Web-Service**Klicken Wählen Sie **Vorbeugende Webdienst**.

![Bewertungsexperiment konvertieren](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

Weitere Informationen für diese Konvertierung finden Sie unter [Konvertieren ein Experiments Machine Learning Training vorhersehbaren Experiment](machine-learning-convert-training-experiment-to-scoring-experiment.md).

Die folgenden Schritte beschreiben ein vorhersageexperiment als neuen Webdienst bereitstellen. Sie können auch das Experiment als Standardansicht WebService bereitstellen.

## <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>Bereitstellen Sie vorhersageexperiment als einen neuen Webdienst

Vorhersageexperiment erstellt wurde, können Sie es als Azure Web Service bereitstellen. Mithilfe des Webdiensts, Benutzer können Daten senden, Modell und Modell Prognosen zurück.

Zum Bereitstellen der vorhersageexperiment klicken Sie am unteren Rand der Leinwand Experiment **Ausführen** . Nach der Ausführung des Experiments auf **Webdienst bereitstellen** und **Web Deploy-Dienst** [neu].  Die Bereitstellung der Computer lernen Web Service Portal geöffnet.

### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Computer lernen Web Service Portal bereitstellen Experiment Seite

Geben Sie einen Namen für den Webdienst auf der Seite Bereitstellung experimentieren.
Wählen Sie einen Zahlungsplan aus. Haben Sie eine vorhandene Zahlungsplan können Sie auswählen, müssen andernfalls Sie einen neuen Preisplan für den Dienst erstellen.

1.  **Preisplan** Dropdown-Liste, wählen Sie einen vorhandenen Plan oder die Option **neuen Plan auswählen** .
2.  Geben Sie in **Name für**ein Name, den Plan auf Ihrer Rechnung identifizieren.
3.  Wählen Sie eine **Monatliche Planung Ebenen**. Die Plan Ebenen standardmäßig die Pläne für die Standardregion und den Webdienst wird dieser Region bereitgestellt.

Klicken Sie auf **Bereitstellen** und die Seite " **Schnellstart** " für Ihren Web Service zu öffnen.

Der Schnellstart-Webdienstseite gibt Hinweise und Zugriff auf die Aufgaben, die Sie nach dem Erstellen eines Webdiensts ausführen. Von hier aus können Sie problemlos die Testseite und verbrauchen Seite zugreifen.

<!-- ![Deploy the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-web-service"></a>Testen Sie den Webdienst

Um Ihren neuen Webdienst zu testen, klicken Sie unter Allgemeine Aufgaben auf **Webdienst testen** . Auf der Testseite können Sie den Webdienst als eine Anforderung-Antwort-Dienst (RRS) oder Batchausführung Service (BES) testen.

RSS-Testseite zeigt die Eingaben, Ausgaben und globale Parameter, die für das Experiment definiert haben. Zum Testen des Webdienstes können manuell Geben Sie geeignete Werte für die Eingaben oder geben Sie eine durch Trennzeichen getrennte Wert (CSV) formatierte Datei, die die Werte enthält.

Der Modus der Listenansicht Ressourceneinträge, aus, geben entsprechende Werte für die Eingaben und **Anforderungsantwort testen**klicken. Die Vorhersageergebnisse anzeigen in der Ausgabespalte links.

![Den Web Service bereitstellen](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

Klicken Sie zum Testen der BES **Batch**. Testseite Stapel klicken Sie unter Ihre Eingabe, und wählen Sie eine CSV-Datei mit entsprechenden Werten. Wenn eine CSV-Datei müssen und erstellt vorhersehbaren Test Machine Learning Studio verwenden, können Sie DataSet für die vorhersageexperiment herunterladen und verwenden.

Öffnen Sie zum Herunterladen der Daten Machine Learning Studio. Öffnen Sie prädiktiven Test und klicken Sie mit der rechten Maustaste die Eingabe für den Test. Wählen Sie aus dem Kontextmenü **Dataset** und wählen Sie dann **herunterladen**.

![Den Web Service bereitstellen](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

Klicken Sie auf **Test**. Der Status des Auftrags Batchausführung zeigt rechts unter **Stapelverarbeitungen Test**.

![Den Web Service bereitstellen](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

Auf der Seite **Konfiguration** Sie ändern Sie die Beschreibung, Titel, Speicherschlüssel Konto aktualisieren und Beispieldaten für Ihren Web Service aktivieren.

![Webdienst konfigurieren](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Wenn Sie den Webdienst bereitgestellt haben, können Sie:

- **Zugriff** über Web Service-API.
- **Verwalten** über Azure Machine Learning Services Webportal oder klassischen Azure-Portal.
- **Update** Wenn das Modell ändert.

### <a name="access-the-web-service"></a>Zugriff auf den Webdienst

Nachdem Sie den Webdienst aus Machine Learning Studio bereitstellen, können Sie Daten an den Dienst senden und Antworten programmgesteuert erhalten.

Die **verbrauchen** Seite enthält alle Informationen, die auf den Webdienst zugreifen müssen. Beispielsweise wird der API-Schlüssel autorisierten Zugriff auf den Dienst bereitgestellt.

Weitere Informationen über den Zugriff auf einen Computer Learning-Webdienst finden Sie unter [bereitgestellten Azure Machine Learning-Webdienst verwenden](machine-learning-consume-web-services.md).

### <a name="manage-your-new-web-service"></a>Verwalten Sie Ihren neuen Webdienst

Sie können klassische Services Machine Learning-Webdiensten Webportal verwalten. Klicken Sie auf **Webdienste** [Portal Hauptseite](https://services.azureml-test.net/). Auf der Web Services können gelöscht oder einen Dienst. Um einen bestimmten Dienst zu überwachen, klicken Sie auf den Dienst, und klicken Sie auf **Dashboard**. Stapelverarbeitungen Webdienst zugeordnet zu überwachen, klicken Sie auf **Batch Anforderungsprotokoll**.

## <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>Bereitstellen Sie vorhersageexperiment als Standardansicht Web service

Vorhersageexperiment ausreichend vorbereitet wurde, können Sie es als Azure Web Service bereitstellen. Mithilfe des Webdiensts, Benutzer können Daten senden, Modell und Modell Prognosen zurück.

Zum Bereitstellen der vorhersageexperiment klicken Sie auf am unteren Rand der Leinwand Experiment **Ausführen** und dann auf **Webdienst bereitstellen**. Der Webdienst einrichten und Sie werden im Web Service-Dashboard.

![Den Web Service bereitstellen](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

Sie können den Webdienst im Computer Learning Web Services Portal oder Machine Learning Studio testen.

Zum Testen des Webdienstes Antwort anfordern klicken Sie **Testen** im Web Service-Dashboard. Ein Dialogfeld erscheint, fordern die Eingabedaten für den Dienst. Dies sind die Spalten Punktzahl Experiment erwartet. Geben Sie Daten, und klicken Sie dann auf **OK**. Die vom Webdienst generierten Ergebnisse werden unten im Schaltpult angezeigt.

Klicken Sie auf den Link, um Ihren Dienst im Azure Machine Learning Web-Portal testen, wie zuvor im Bereich Service Web **Test** Vorschau.

Batch Execution Service zu testen, klicken Sie auf **Test** Vorschau. Testseite Stapel klicken Sie unter Ihre Eingabe, und wählen Sie eine CSV-Datei mit entsprechenden Werten. Wenn eine CSV-Datei müssen und erstellt vorhersehbaren Test Machine Learning Studio verwenden, können Sie DataSet für die vorhersageexperiment herunterladen und verwenden.

![Testen des Webdienstes](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

Auf der Seite **Konfiguration** können Sie den Anzeigenamen des Dienstes ändern und eine Beschreibung dafür. Der Name und die Beschreibung wird im [klassischen Azure-Portal](http://manage.windowsazure.com/) verwalten Sie Ihre Webdienste.

Eine Beschreibung können für Ihre Daten, Daten und Webdienstparametern Sie durch Eingabe einer Zeichenfolge für jede Spalte unter **INPUT SCHEMA** **AUSGABESCHEMA**und **WEBDIENSTPARAMETER**. Diese Beschreibung wird in der Dokumentation Sample Code für den Webdienst.

Sie können die Protokollierung Fehler diagnostizieren, die Sie beim Zugriff auf der Webdienst sehen. Weitere Informationen finden Sie unter [Aktivieren der Protokollierung für maschinelles lernen Webdienste](machine-learning-web-services-logging.md).

![Webdienst konfigurieren](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

Sie können auch die Endpunkte für den Webdienst im Azure Machine Learning Web Portal ähnlich gezeigt im Bereich Service Web konfigurieren. Die Optionen sind unterschiedlich, hinzufügen oder Ändern der Beschreibung Protokollierung aktivieren und Beispieldaten für Tests aktivieren.

### <a name="access-the-web-service"></a>Zugriff auf den Webdienst

Nachdem Sie den Webdienst aus Machine Learning Studio bereitstellen, können Sie Daten an den Dienst senden und Antworten programmgesteuert erhalten.

Das Dashboard bietet alle Informationen, die auf den Webdienst zugreifen müssen. Z. B. API-Schlüssel wird bereitgestellt, um autorisierten Zugriff auf den Dienst und API-Hilfeseiten dienen zu fangen Sie Code schreiben.

Weitere Informationen über den Zugriff auf einen Computer Learning-Webdienst finden Sie unter [bereitgestellten Azure Machine Learning-Webdienst verwenden](machine-learning-consume-web-services.md).

### <a name="manage-the-web-service"></a>Verwalten Sie den Webdienst

Es gibt verschiedene Aktionen auf einen Webdienst zu überwachen. Sie können aktualisieren und löschen. Sie können auch zusätzliche Endpunkte an einen Webdienst Classic neben der Standardendpunkt hinzufügen, die bei der Bereitstellung erstellt wird.

Weitere Informationen finden Sie unter [Verwalten eines Webdienstes mit Azure Machine Learning Web Services-Portal](machine-learning-manage-new-webservice.md)und [Verwalten einer Azure Machine Learning-Arbeitsbereich](machine-learning-manage-workspace.md) .

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>Aktualisieren Sie den Webdienst

Änderungen an den Webdienst wie Aktualisieren des Modells mit Schulung und Bereitstellen wieder den ursprünglichen Webdienst überschreiben.

Um den Webdienst zu aktualisieren, öffnen Sie das ursprüngliche vorhersageexperiment verwendet den Web Service bereitstellen und eine bearbeitbare Kopie durch Klicken auf **Speichern unter**. Ändern Sie und dann auf **Webdienst bereitstellen**.

Da Sie dieses Experiment vor bereitgestellt haben, werden Sie gefragt, ob Sie überschreiben (klassische Webdienst) oder (neue Webdienst) vorhandenen Dienst aktualisieren möchten. Auf **Ja** oder **Update** beendet den vorhandenen Webdienst und stellt die neue vorhersageexperiment wird an seiner Stelle bereitgestellt.

> [AZURE.NOTE] Wenn Sie Konfiguration im ursprünglichen Webdienst geändert, z. B. müssen einen neuen Anzeigenamen oder Beschreibung eingeben, Sie diese Werte erneut eingeben.

Eine Option zum Aktualisieren Ihres Web Service ist das Modell programmgesteuert umschulen. Weitere Informationen finden Sie unter [Computer Learning umschulen Modelle programmgesteuert](machine-learning-retrain-models-programmatically.md).


<!-- internal links -->
[Erstellen eines Experiments Schulung]: #create-a-training-experiment
[Ein vorhersageexperiment umwandeln]: #convert-the-training-experiment-to-a-predictive-experiment
[Neu]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Classic]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
