<properties
   pageTitle="Einen neuen Webdienst bereitstellen"
   description="Der Workflow bereitstellen ein ARM basierten Webdienst"
   services="machine-learning"
   documentationCenter=""
   authors="vDonGlover"
   manager="raymondl"
   editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>

# <a name="deploy-a-new-web-service"></a>Einen neuen Webdienst bereitstellen

Microsoft Azure Machine jetzt lernen bietet [Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md) auf Webdienste für neue Abrechnung Optionen und den Webdienst auf mehrere Bereiche bereitstellen.

Der allgemeine Arbeitsablauf ein Webdienstes mit Microsoft Azure Machine Learning Webdienste bereitgestellt ist:

* Erstellen einer vorhersageexperiment
* Bereitstellen
* Konfigurieren Sie die
* Fakturierungsplan
* Testen
* verbrauchen.

Die folgende Abbildung veranschaulicht den Workflow.

![Web Service-Bereitstellungsworkflow][1]
 
## <a name="deploy-web-service-from-studio"></a>Webdienst Studio bereitstellen 

Versuch als neue WebService bereitstellen. Machine Learning Studio anmelden und einen vorhersehbaren Webdienst erstellen. 

**Hinweis**: Wenn bereits ein Experiment als klassische Webdienst bereitgestellt haben kann nicht es als neue WebService bereitstellen.
 
Klicken Sie am unteren Rand der Leinwand Experiment **Ausführen** und dann auf **Webdienst bereitstellen** und **Webdienst bereitstellen [New]**. Die Bereitstellungsseite der Computer lernen Web Service Manager wird geöffnet.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Computer Lernmanager Web Service bereitstellen Experiment Seite
Geben Sie einen Namen für den Webdienst auf der Seite Bereitstellung experimentieren.
Wählen Sie einen Zahlungsplan aus. Haben Sie eine vorhandene Zahlungsplan können Sie auswählen, müssen andernfalls Sie einen neuen Preisplan für den Dienst erstellen. 

1.  **Preisplan** Dropdown-Liste, wählen Sie einen vorhandenen Plan oder die Option **neuen Plan auswählen** .
2.  Geben Sie in **Name für**ein Name, den Plan auf Ihrer Rechnung identifizieren.
3.  Wählen Sie eine **Monatliche Planung Ebenen**. Beachten Sie, dass die Plan Ebenen standardmäßig die Pläne für die Standardregion und den Webdienst Region bereitgestellt wird.

Klicken Sie auf **Bereitstellen** und die Seite "Schnellstart" für Ihren Web Service zu öffnen.

## <a name="quickstart-page"></a>Seite "Schnellstart"
Der Schnellstart-Webdienstseite gibt Hinweise und Zugriff auf die Aufgaben, die Sie nach dem Erstellen eines neuen Webdiensts ausführen. Hier können Sie die **Testseite** und **verbrauchen** Seite bequem.

## <a name="testing-your-web-service"></a>Testen den Webdienst

Klicken Sie auf Schnellstart unter Allgemeine Aufgaben auf Webdienst testen.   

Zum Testen des Webdienstes als eine Anforderung-Antwort-Dienst (RRS):

* Klicken Sie auf der Menüleiste auf **Test** .
* Klicken Sie auf **Anforderung-Antwort**.
* Geben Sie geeignete Werte für Eingabespalten Test.
* Klicken Sie auf Test- **Anforderung / Antwort**.

Ergebnisse werden auf der rechten Seite der Seite angezeigt.

Um einen Webdienst Batch Execution Service (BES) zu testen, verwenden Sie eine CSV-Datei:

* Klicken Sie auf der Menüleiste auf **Test** .
* Klicken Sie auf **Stapel**.
* Klicken Sie unter Ihre Eingabe auf Durchsuchen und navigieren Sie zu der Beispieldatei.
* Klicken Sie auf **Test**.

Der Test wird unter **Stapelverarbeitungen testen**Status.

## <a name="consuming-your-web-service"></a>Ihren Web Service nutzt

Bei der Bereitstellung als Webdienst bereitstellen Azure Machine Learning Experimente eine REST-API, die von einer Vielzahl von Geräten und Plattformen verwendet werden kann. Ist die einfache REST API akzeptiert und mit JSON-formatierten Nachrichten. Azure Machine Learning-Portal enthält Code zum Aufruf des Webdiensts R C# und Python verwendet werden kann.
 
Auf der Seite Verbrauch finden Sie:

* API-Schlüssel und URI des Webdiensts Apps Nutzung.
* Der Verbrauch starten in Excel und Web app Vorlagen starten.
* Der Beispielcode in C#, Python und R Einstieg.

Weitere Informationen über Webdienste finden Sie unter [ein Azure Machine Learning Webdienst, der von einem Computer lernen Experiment bereitgestellt wurde](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Webdienste finden Sie unter:

[Wie Sie einen Webdienst Azure maschinelles lernen, der von einem Computer lernen Experiment bereitgestellt wurde](machine-learning-consume-web-services.md)

[Azure Machine Learning Web Services: Bereitstellung und Verbrauch](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
