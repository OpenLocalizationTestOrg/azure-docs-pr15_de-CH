<properties
    pageTitle="Erstellen Webdienstendpunkte Computer lernen | Microsoft Azure"
    description="Erstellen von Webdienst-Endpunkten in Azure Machine Learning"
    services="machine-learning"
    documentationCenter=""
    authors="hiteshmadan"
    manager="padou"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="10/04/2016"
    ms.author="himad"/>


# <a name="creating-endpoints"></a>Endpunkte erstellen

>[AZURE.NOTE] Dieses Thema beschreibt Verfahren für eine klassische Webdienst.

Beim Erstellen von Webdiensten, die Sie weiterleiten an Ihre Kunden verkaufen müssen Sie geschulte Modelle ermöglichen Kunden, die noch mit dem Experiment aus dem Webdienst erstellt wurde. Darüber hinaus Updates des Experiments selektiv zu einem Endpunkt anzuwenden ohne Anpassungen überschreiben.

Zu diesem Zweck können Azure Machine Learning mehrere Endpunkte für einen bereitgestellten Webdienst erstellen. Jeder Endpunkt im Webdienst unabhängig behandelt, gedrosselt und verwaltet. Jeder Endpunkt ist eine eindeutige URL Autorisierungsschlüssel an Kunden verteilt werden kann.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Hinzufügen von Endpunkten an einen Webdienst

Es gibt drei Möglichkeiten einen Webdienst einen Endpunkt hinzufügen.

* Programmgesteuert
* Über das Portal Azure Machine Learning-Webdienste
* Obwohl der Azure-Verwaltungsportal

Erstellte Endpunkt können durch synchrone API-Batch-APIs nutzen und excel-Arbeitsblättern. Neben Endpunkte über diese Benutzeroberfläche, auch können Endpunkt Management-APIs Sie programmgesteuert Endpunkte hinzufügen.

 >[AZURE.NOTE] Wenn Sie zusätzliche Endpunkte auf den Webdienst hinzugefügt haben, nicht der Standardendpunkt gelöscht werden.

## <a name="adding-an-endpoint-programmatically"></a>Einen Endpunkt hinzufügen programmgesteuert

Sie können den Webdienst programmgesteuert mithilfe den Beispielcode [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) einen Endpunkt hinzufügen.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Einen Endpunkt mit Azure Machine Learning Web Services-Portal hinzufügen

1. Klicken Sie in Machine Learning Studio in der Spalte links auf Webdienste.
2. Klicken Sie am unteren Rand der Web Service-Dashboard **Verwalten Endpunkte**. Das Azure Machine Learning Web Portal wird auf der Seite Endpunkte für den Webdienst.
3. **Klicken Sie auf.**
4. Geben Sie einen Namen und eine Beschreibung für den neuen Endpunkt. Endpunktnamen müssen 24 Zeichen oder weniger lang sein und darf klein-Buchstaben oder Zahlen bestehen. Wählen Sie die Protokollierungsebene und ob Daten aktiviert ist. Weitere Informationen zur Protokollierung finden Sie unter [Aktivieren der Protokollierung für Computer Learning Webdienste](machine-learning-web-services-logging.md).

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a>Einen Endpunkt mit klassischen Azure-Portal hinzufügen


1. Melden Sie das [klassische Azure-Portal an](http://manage.windowsazure.com), klicken Sie in der linken Spalte auf **Computerlernen** . Klicken Sie auf den Arbeitsbereich der Web Service enthält, an dem Sie interessiert sind.

    ![Navigieren Sie zum Arbeitsbereich](./media/machine-learning-create-endpoint/figure-1.png)

2. Klicken Sie auf **Webdienste**.

    ![Navigieren Sie zu Webdiensten](./media/machine-learning-create-endpoint/figure-2.png)

3. Klicken Sie auf den Webdienst, um die Liste der verfügbaren Endpunkte sind.

    ![Navigieren Sie zum Endpunkt](./media/machine-learning-create-endpoint/figure-3.png)

4. Klicken Sie am unteren Rand der Seite auf **Endpunkt hinzufügen**. Geben Sie einen Namen und eine Beschreibung, sicherzustellen Sie, dass keine Endpunkte mit dem gleichen Namen in diesem Web Service. Lassen Sie die Schubkontrolle auf den Standardwert Sie, besondere Vorschriften. Erfahren Sie mehr über das drosseln [Skalierung API-Endpunkte](machine-learning-scaling-webservice.md)anzeigen

    ![Erstellen](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a>Nächste Schritte

[Wie eine veröffentlichte Azure Machine Learning Webdienst](machine-learning-consume-web-services.md).
