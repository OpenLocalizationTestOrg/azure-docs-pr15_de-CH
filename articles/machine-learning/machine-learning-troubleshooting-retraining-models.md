<properties
    pageTitle="Umschulung ein Azure Machine Learning klassische Webdienst Fehlerbehebung | Microsoft Azure"
    description="Identifizieren Sie und korrigieren Sie allgemeine Probleme ist, wenn Sie das Modell für einen Webdienst Azure Machine Learning Umschulung."
    services="machine-learning"
    documentationCenter=""
    authors="VDonGlover"
   manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Ein Azure Machine Learning klassische Webdienst Umschulung Problembehandlung

## <a name="retraining-overview"></a>Umschulung (Übersicht)

Beim Bereitstellen von vorhersageexperiment als Punktzahl Webdienst ist ein statisches Modell. Sobald neue Daten verfügbar sind oder der API Daten verfügt, muss das Modell einarbeiten. 

Umfassende Umschulung Prozess eines klassischen Web Service finden Sie unter [Programmgesteuertes umschulen Computer lernen Modelle](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Umschulung Prozess

Wenn Sie den Webdienst umschulen müssen, müssen Sie einige zusätzliche Punkte hinzufügen:

* Ein Webdienst aus dem Experiment Schulung bereitgestellt. Das Experiment muss ein **Web Service Ausgabe** Modul die Ausgabe des Moduls **Zug Modell** .  

    ![Das Modell Zug Web Service Ausgabe zuordnen.][image1]

* Einen neuen Endpunkt Punktzahl Webdienst hinzugefügt.  Programmgesteuert mithilfe den Beispielcode umschulen maschinelles lernen Modelle programmgesteuert verwiesen Endpunkt hinzufügen Thema oder klassischen Azure-Portal.

Den Beispielcode C# aus Training Web Service API-Hilfeseite können Sie Modell umschulen. Die Ergebnisse ausgewertet haben und damit zufrieden sind aktualisieren ausgebildete Modell bewerten Webdienst mit den neuen Endpunkt hinzufügen.

Alle Teile an sind die wichtigsten Schritte nehmen Sie das Modell umschulen müssen wie folgt:

1.  Aufrufen des Webdienstes Schulung: aufgerufen wird, der Batch Ausführung Service (BES), nicht die Anforderung Response Service (RRS). Auf der Hilfeseite API können C# Beispielcode Sie anrufen. 
2.  Suchen Sie die Werte für *BaseLocation*, *RelativeLocation*und *SasBlobToken*: Diese Werte werden in der Ausgabe durch den Aufruf an den Webdienst Training zurückgegeben. 
      ![Zeigt die Ausgabe des Beispiels Umschulung und die Werte BaseLocation, RelativeLocation und SasBlobToken.][image6]
3.  Hinzugefügten Endpunkt Punktzahl Webdienst mit dem neuen ausgebildeten Modell aktualisieren: Beispielcode umschulen maschinelles lernen Modelle programmgesteuert mit den neuen Endpunkt Bewertungsmodell neu ausgebildeten Modell vom Webdienst Training hinzugefügt aktualisieren.

## <a name="common-obstacles"></a>Allgemeine Hindernisse

### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Überprüfen Sie, haben Sie die richtige PATCH-URL

Der PATCH URL verwendeten muss eine neue Punktzahl Endpunkt hinzugefügt Punktzahl Webdienst zugeordnet sein. Es gibt verschiedene Arten zu PATCH-URL:

**Option 1: programmgesteuert**

Zu den richtigen PATCH-URL:

1.  Den Beispielcode [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) ausführen.
2.  Aus der Ausgabe von AddEndpoint *HelpLocation* gesucht, und kopieren Sie die URL.

    ![HelpLocation in der Ausgabe des Beispiels AddEndpoint.][image2]

3.  Fügen Sie die URL in einem Browser Navigieren zu einer Seite, die Links zu Hilfethemen für den Webdienst bereitstellt.
4.  Klicken Sie auf **Ressource** , um die Patch-Hilfeseite zu öffnen.

**Option 2: Verwenden der Azure-Verwaltungsportal**

1.  Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.
2.  Öffnen Sie die Registerkarte Computerlernen. 
     ![Schiefe Registerkarte Rechner.][image4]
3.  Klicken Sie auf den Arbeitsbereich ein, dann **Webdienste**.
4.  Klicken Sie auf die Punktzahl Webdienst, mit dem Sie arbeiten. (Wenn nicht den Standardnamen des Webdiensts geändert haben, es in [Bewertung Exp] endet.)
5.  Klicken Sie auf **Endpunkt**.
6.  Nachdem der Endpunkt hinzugefügt wurde, klicken Sie auf den Endpunktnamen. Klicken Sie auf **Ressource** , um Patches Hilfeseite zu öffnen.

>[AZURE.NOTE] Wenn den Endpunkt an den Webdienst Schulung vorhersehbaren Webdienst hinzugefügt haben, wird die folgende Fehlermeldung beim Klicken auf den Link **Ressource** : Es tut uns leid, aber dieses Feature wird nicht unterstützt oder in diesem Kontext. Dieses Web hat keine aktualisierbaren Ressourcen. Wir entschuldigen uns für die Unannehmlichkeiten und arbeiten daran, diesen Workflow.

![Neuen Endpunkt Dashboard.][image3]

Hilfeseite PATCH enthält den PATCH URL müssen Sie verwenden und bietet Beispielcode aufrufen Verwendung.

![Patch-URL.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Überprüfen Sie, dass Sie den richtigen Punktzahl Endpunkt aktualisieren

* Schulung-Webdienst nicht patch: Patch-Vorgang Punktzahl Webdienst durchzuführen.
* Der Standardendpunkt Webdienst nicht patch: Patch-Vorgang muss für die neue Punktzahl Webdienst-Endpunkt, den Sie hinzugefügt durchgeführt werden.

Sie können überprüfen, welche Webdienst der Endpunkt ist auf klassische Azure-Portal besuchen. 

>[AZURE.NOTE] Achten Sie vorbeugende Webdienst nicht den Webdienst Training Endpunkt hinzufügen. Wenn eine Schulung und einem vorhersehbaren Webdienst ordnungsgemäß bereitgestellt wurde, sollte zwei separate Webdienste aufgeführt angezeigt werden. Prädiktive Webdienst muss mit "[vorhersehbaren exp.]" enden.

1.  Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.
2.  Öffnen Sie die Registerkarte Computerlernen. 
     ![Computer lernen Arbeitsbereich Benutzeroberfläche.][image4]
3.  Wählen Sie den Arbeitsbereich.
4.  Klicken Sie auf **Webdienste**.
5.  Wählen Sie den vorbeugende Webdienst.
6.  Überprüfen Sie, ob der Webdienst der neue Endpunkt hinzugefügt wurde.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>Überprüfen Sie den Arbeitsbereich, die der Webdienst in die richtige Region ist

1.  Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.
2.  Wählen Sie im Menü Computerlernen.
      ![Computer lernen Region Benutzeroberfläche.][image4]
3.  Überprüfen Sie den Speicherort des Arbeitsbereichs.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png