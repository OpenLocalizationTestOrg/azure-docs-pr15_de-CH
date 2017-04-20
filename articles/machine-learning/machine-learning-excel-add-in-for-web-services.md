<properties
    pageTitle="Excel-add-in für Computer Learning Webdienste | Microsoft Azure"
    description="Wie Azure Machine Learning Webdienste direkt in Excel ohne Code."
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/05/2016"
    ms.author="tedway;garye" />

# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Excel-Add-in für Azure Machine Learning-Webdiensten

Excel vereinfacht das Aufrufen von Webdiensten direkt ohne Code schreiben zu müssen.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Schritte zum Verwenden eines vorhandenen Webdiensts in der Arbeitsmappe

1. Öffnen Sie die [Excel-Beispieldatei](http://aka.ms/amlexcel-sample-2)enthält Excel-add-in und Daten auf der Titanic.
2. Wählen Sie durch Klicken den Webdienst-"Titanic Hinterbliebenenversorgung Vorhersagedienst (Excel Add-in-Beispiel) [Ergebnis]" in diesem Beispiel.

    ![Wählen Sie WebService][01]

3. Hierdurch gelangen Sie zum Abschnitt **Vorhersagen** .  Diese Arbeitsmappe enthält bereits Daten, aber eine leere Arbeitsmappe können Sie markieren Sie eine Zelle in Excel und auf **Beispieldaten verwenden**.
4. Wählen Sie die Daten mit Headern und Eingabedaten Bereich Symbol auf.  Stellen Sie sicher, dass das Kontrollkästchen "Daten haben Überschriften" aktiviert ist.
5. Geben Sie unter **Ausgabe**die Zellennummer Ausgabe sein, z. B. "H1" Hier soll.
6. Klicken Sie auf **vorherzusagen**.

    ![Abschnitt Vorhersagen][02]

## <a name="steps-to-add-a-new-web-service"></a>Einen neuen Webdienst hinzufügen

Bereitstellen eines Webdiensts oder einen vorhandenen Webdienst verwenden. Weitere Informationen über einen Webdienst bereitstellen, finden Sie unter [Exemplarische Vorgehensweise Schritt 5: Bereitstellen von Azure Machine Learning-Webdienst](machine-learning-walkthrough-5-publish-web-service.md).

API-Schlüssel für den Webdienst zu erhalten. Wo Sie hierzu abhängig, ob Sie einen klassische Computer lernen Webdienst einen neuen Computer Learning-Webdienst veröffentlicht.

**Klassische Webdienst verwenden** 

1. Machine Learning Studio **Webdienstbereich im linken Bereich** auf und wählen Sie den Webdienst.

    ![Wählen Sie einen Webdienst Studio][04]

2. Kopieren Sie den API-Schlüssel für den Webdienst.

    ![Studio API-Schlüssel][05]

3. Klicken Sie auf die Registerkarte **DASHBOARD** für den Webdienst **Anforderung/Antwort** .
4. Suchen Sie nach Abschnitt **URI anzufordern** .  Kopieren und Speichern der URL.

>[AZURE.NOTE] Es ist jetzt möglich anmelden Portal [Azure Machine Learning Web Services](https://services.azureml.net) API für einen klassischen Machine Learning-Webdienst abrufen.

**Einen neuen Webdienst verwenden**

1. Im Portal [Azure Machine Learning Webdienste](https://services.azureml.net) **Webdienste**klicken Sie den Webdienst. 
2. Klicken Sie auf **verwenden**.
3. Suchen Sie nach Abschnitt **grundlegende Verbrauch** . Kopieren Sie und speichern Sie der **Primärschlüssel** und der **Anforderung / Antwort-** URL.


## <a name="steps-to-add-a-new-web-service"></a>Einen neuen Webdienst hinzufügen

1. Bereitstellen eines Webdiensts oder einen vorhandenen Webdienst verwenden. Weitere Informationen über einen Webdienst bereitstellen, finden Sie unter [Exemplarische Vorgehensweise Schritt 5: Bereitstellen von Azure Machine Learning-Webdienst](machine-learning-walkthrough-5-publish-web-service.md).
2. Klicken Sie auf **verwenden**.
3. Suchen Sie nach Abschnitt **grundlegende Verbrauch** . Kopieren Sie und speichern Sie der **Primärschlüssel** und der **Anforderung / Antwort-** URL.
2. Gehen Sie in Excel **Web Services** -Abschnitt (Abschnitt **Predict** , klicken Sie auf den Pfeil, um die Liste der WebServices gehen).

    ![Web Service wählen][03]
    
3. Klicken Sie auf **Webdienst**.
4. Das Excel-add-in Textfeld **URL**fügen Sie den URL ein.
5. Das Textfeld **API-Schlüssel**fügen Sie API/Primärschlüssel ein.
6. Klicken Sie auf **Hinzufügen**.

    ![URL und API-Schlüssel für eine klassische Webdienst.][06]

10. Um den Webdienst zu verwenden, befolgen Sie vorhergehenden, "Schritte einen vorhandene Webdienst verwenden."

## <a name="sharing-your-workbook"></a>Freigabe der Arbeitsmappe

Wenn Sie die Arbeitsmappe speichern, werden API-primären Schlüssel für die Webdienste hinzugefügten auch. Das bedeutet, dass die Arbeitsmappe nur für Personen freigeben sollte, denen Sie vertrauen.

Fragen Sie im folgenden Kommentar oder [Forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
