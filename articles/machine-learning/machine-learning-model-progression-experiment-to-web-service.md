<properties
    pageTitle="Wie Computer Lernmodell von Hypothesen auf einen standardisierten Webdienst wechselt | Microsoft Azure"
    description="Eine Übersicht über die Funktionsweise der standardisierten Webdienst wie Ihre Azure Machine Learning Modell verläuft von Entwicklung experimentieren."
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


# <a name="how-a-machine-learning-model-progresses-from-an-experiment-to-an-operationalized-web-service"></a>Wie Computer Lernmodell von Hypothesen auf einen standardisierten Webdienst wechselt

Azure Machine Learning Studio bietet eine interaktive Leinwand, mit dem Sie entwickeln, ausführen, testen und durchlaufen eine ***experimentieren*** , Vorhersageanalysen Modell darstellt. Es gibt eine Vielzahl von Modulen, die:

* Eingabedaten in das experiment
* Die Daten ändern
* Trainieren eines Modells mit Algorithmen für maschinelles lernen
* Bewerten des Modells
* Auswertung der Ergebnisse
* Endgültige Ausgabewerte

Sobald Sie das Experiment sind, können Sie es als ***klassische Azure Machine Learning Webdienst*** oder einen ***neuen Azure Machine Learning Webdienst*** bereitstellen, damit Benutzer neue Daten senden und wieder Ergebnisse erhalten können.

In diesem Artikel geben wir einen Überblick über die Funktionsweise der standardisierten Webdienst wie Ihr Computerlernen Modell verläuft von Entwicklung experimentieren.

>[AZURE.NOTE] Verschiedene Arten zu lernmodelle bereitstellen, aber dieser Artikel konzentriert sich wie Machine Learning Studio verwenden. Ausführliche Informationen zum Erstellen ein klassischen vorhersehbaren Webdienst r finden Sie im Blogbeitrag [Build und Bereitstellung vorhersehbaren Web Apps verwenden RStudio Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).

Während Azure Machine Learning Studio soll Ihnen bei der Entwicklung und Bereitstellung von *Vorhersageanalysen Modell*kann von Studio Experiment entwickeln, die ein Modell Vorhersageanalysen enthält. Beispielsweise kann ein Experiment nur Eingabedaten bearbeiten und die Ergebnisse. Wie ein Experiment Vorhersageanalysen dieses nicht vorhersehbaren Experiment als Webdienst bereitstellen, aber deshalb einfacher Experiments nicht Schulung oder Computer Lernmodell bewerten. Es ist, zwar nicht typisch Studio dazu verwenden werden wir sie in der Diskussion enthalten, damit wir eine vollständige Erläuterung der Funktionsweise von Studio geben.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Entwickeln und Bereitstellen eines vorhersehbaren Webdienstes

Hier werden die Phasen, die eine gebräuchliche Lösung entwickeln und Bereitstellen Machine Learning Studio folgt:

![Bereitstellung Fluss](media\machine-learning-model-progression-experiment-to-web-service\model-stages-from-experiment-to-web-service.png)

*Abbildung 1: Phasen eines normalen Vorhersageanalysen Modells*

### <a name="the-training-experiment"></a>Schulung-experiment

***Schulung experimentieren*** ist die Anfangsphase des Webdiensts in Machine Learning Studio entwickeln. Experiment Training soll Ihnen einen Platz entwickeln, testen, durchlaufen, und schließlich einen Computer Modell wird trainiert. Sie selbst können mehrere Modelle gleichzeitig suchen Sie die beste Lösung, da anschließend experimentieren Sie ein einzelnes geschult wählen Modell und den Rest aus dem Experiment zu beseitigen. Beispielsweise entwickeln prognostische Analyse experimentieren Sie, finden Sie unter [Entwickeln einer Lösung Vorhersageanalytik für Gutschrift Risikoanalyse in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="the-predictive-experiment"></a>Vorhersageexperiment

Haben Sie einen ausgebildeten Modell im Experiment Schulung auf **Web-Service** , und wählen Sie **Vorhersehbaren Webdienst** Machine Learning Studio Initiieren eines ***vorhersehbaren Experiment***Training Test umwandeln. Vorhersageexperiment dient mit qualifizierten Modell neue Daten, mit dem Ziel schließlich als Webdienst Azure operationalisiert werden.

Die Konvertierung wird durch die folgenden Schritte durchgeführt:

-   Modulen zur Schulung in einem einzigen Modul konvertiert und als geschulte Modell speichern

-   Vermeiden Sie überflüssige Module nicht im Zusammenhang mit der Bewertung

-   Hinzufügen von Eingabe- und Ports, die den tatsächliche Webdienst verwenden

Möglicherweise gibt es weitere Änderungen zu vorhersehbaren Test als Webdienst bereitgestellt werden soll. Wenn den Webdienst nur eine Teilmenge der Ergebnisse ausgeben möchten, können Sie filtern Modul vor der Ausgangs-Port hinzufügen.

In diesen Konvertierungsprozess trainingsexperiment nicht verworfen. Wenn der Prozess abgeschlossen ist, haben Sie zwei Registerkarten im Studio: Schulung experimentieren und vorhersageexperiment. Auf diese Weise können Sie Änderungen des Experiments Training Ihren Web Service bereitstellen und vorhersageexperiment neu erstellen. Oder speichern Sie eine Kopie des Experiments Schulung zu einer anderen Zeile experimentieren.

>[AZURE.NOTE] Wenn Sie **Vorhersehbaren Webdienst** Klicken starten einen automatischen Prozess Training Test vorhersehbaren Experiment konvertieren und dies funktioniert in den meisten Fällen. Wenn das Experiment Schulung ist (z. B. haben Sie mehrere Pfade für die Schulung, die Sie gemeinsam), kann diese Konvertierung manuell ausführen möchten. Weitere Informationen finden Sie unter [Konvertieren ein Experiments Machine Learning Training vorhersehbaren Experiment](machine-learning-convert-training-experiment-to-scoring-experiment.md).

### <a name="the-web-service"></a>Der Webdienst

Sobald Sie vorbeugende Experiment bereit ist, den Dienst als eine klassische Webdienst bereitstellen oder ein neuen Webdienst basiert auf Azure-Ressourcen-Manager. Um Ihr Modell Durchsetzen von als *klassische Machine Learning-Webdienst*bereitstellen, auf **Webdienst bereitstellen** und wählen Sie **Webdienst bereitstellen [Classic]**. Als *neue Computer Learning-Webdienst*bereitstellen auf **Webdienst bereitstellen** , und wählen Sie **Webdienst bereitstellen [New]**. Benutzer können jetzt mithilfe des Webdiensts REST API Modell senden und empfangen wieder die Ergebnisse. Weitere Informationen finden Sie unter [ein Azure Machine Learning Webdienst, der von einem Computer lernen Experiment bereitgestellt wurde](machine-learning-consume-web-services.md).

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>Nicht üblicherweise: nicht vorhersehbaren Webdienst erstellen

Wenn das Experiment trainieren nicht Vorhersageanalysen Modell dann brauchen Training experimentieren und ein bewertungsexperiment erstellen - gibt es nur ein Experiment und als Web Service bereitstellen. Machine Learning Studio erkennt, ob das Experiment ein Vorhersagemodell enthält anhand der verwendeten Module.

Nachdem Sie auf den Test durchlaufen haben und mit:

1.  Klicken Sie auf **Web-Dienst** und wählen Sie **Webdienst Umschulung** - Eingabe- und Knoten werden automatisch hinzugefügt

2.  Klicken Sie auf **Ausführen**

3. Auf **Webdienst bereitstellen** und wählen Sie **Webdienst bereitstellen [Classic]** oder **[New] Webdienst bereitstellen** je nach Umgebung bereitgestellt werden soll.

Der Webdienst jetzt bereitgestellt und zugreifen und wie einen vorhersehbaren Webdienst verwalten.

## <a name="updating-your-web-service"></a>Aktualisieren Ihren Web service

Bereitstellung von Test als Webdienst müssen wenn aktualisieren?

Das hängt Sie aktualisieren müssen:

**Die Eingabe oder Ausgabe ändern möchten oder ändern wie der Webdienst Daten bearbeitet werden sollen**

Wenn Sie das Modell nicht ändern, aber nur Behandlung von Daten durch der Webdienst ändern, können Sie das vorhersageexperiment bearbeiten klicken Sie auf **Webdienst bereitstellen** und erneut wählen Sie **Webdienst bereitstellen [Classic]** oder **[New] Webdienst bereitstellen** . Der Webdienst beendet, aktualisierte vorhersageexperiment bereitgestellt und der Webdienst neu gestartet.

Beispiel: Angenommen, vorhersehbare Test die gesamte Zeile der Eingabedaten mit geschätzten Ergebnis zurückgibt. Sie können entscheiden, dass den Webdienst nur das Ergebnis zurückgeben soll. So können Sie ein **Projekt Spalten** Modul im vorhersageexperiment vor der Ausgabeanschluss Spalten als Ergebnis hinzufügen. Wenn Sie auf **Webdienst bereitstellen** und erneut wählen Sie **Webdienst bereitstellen [Classic]** oder **[New] Webdienst bereitstellen** , wird der Webdienst aktualisiert.

**Das Modell mit neuen Daten trainieren möchten**

Computers Lernmodell bleiben sollen, jedoch mit neuen Daten trainieren möchten, haben Sie zwei Optionen:

1.  **Trainieren das Modell während der Webdienst ausgeführt wird** - möchten Sie das Modell während der vorhersehbaren umschulen Webdienst ausgeführt wird, dazu ein paar Änderungen des Experiments Training zu ***Umschulung Experiment***und als ** *Umschulung* Webdienst**bereitstellen. Informationen dazu finden Sie unter [umschulen maschinelles lernen Modelle programmgesteuert](machine-learning-retrain-models-programmatically.md).

2.  **Zurück zur ursprünglichen Training experimentieren und verschiedene Daten zu Ihrem Modell** - vorhersehbaren Test mit dem Webdienst verknüpft ist, aber trainingsexperiment nicht direkt auf diese Weise verknüpft. Wenn Sie ursprüngliche trainingsexperiment ändern, klicken Sie auf **Web-Service**wird erstellen *neue* vorhersehbaren experimentieren, die bei der Bereitstellung einer *neuen* Webdienst erstellt. Den ursprünglichen Webdienst aktualisieren nicht einfach.

    Benötigen Sie trainingsexperiment zu ändern, öffnen Sie und **Speichern unter** zum Kopieren auf. Das ursprüngliche trainingsexperiment vorhersageexperiment intakt und Webdienst. Sie können jetzt einen neuen Webdienst mit geändertem erstellen. Nachdem Sie den neuen Webdienst bereitgestellt haben, den Sie entscheiden können, ob vorherigen Webdienst beenden oder neue parallel.

**Ein anderes Modell trainieren möchten**

Wenn Ihre ursprünglichen vorhersageexperiment wie wählen einen anderen Computer Learning Algorithmus ändern, versuchen eine andere Methode usw. möchten, müssen Sie das zweite Verfahren beschriebenen Modell Umschulung: Experiment Schulung zu öffnen, klicken Sie auf **Speichern als** Kopie und neuen Weg der Entwicklung Ihres Modells starten , vorhersageexperiment erstellen und Bereitstellen des Webdienstes. Dies erstellt ein neues Web Service unabhängig von der ursprünglichen Ebene - Sie entscheiden können, welche eine oder beide ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über den Prozess und Experiment finden Sie in folgenden Artikeln:

-   Konvertieren des Experiments - [ein Computer Learning trainingsexperiment vorhersehbaren Experiment konvertieren](machine-learning-convert-training-experiment-to-scoring-experiment.md)

-   Bereitstellen des Webdienstes - [Bereitstellen einer Azure Machine Learning-Webdienst](machine-learning-publish-a-machine-learning-web-service.md)

-   Modell - [Machine Learning umschulen Modelle programmgesteuert](machine-learning-retrain-models-programmatically.md) Umschulung

Beispiele für den gesamten Prozess finden Sie unter:

-   [Computer lernen Tutorial: Erstellen der ersten Versuch in Azure Machine Learning Studio](machine-learning-create-experiment.md)

-   [Exemplarische Vorgehensweise: Entwickeln einer Lösung Vorhersageanalytik für Gutschrift Risikoanalyse in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)