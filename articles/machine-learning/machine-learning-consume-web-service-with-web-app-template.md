<properties
    pageTitle="Nutzen eines Webdiensts maschinelles Lernen mit einer Webvorlage app | Microsoft Azure"
    description="Verwenden Sie eine Webvorlage app Azure Marketplace zu einem vorhersehbaren Webdienst in Azure Machine Learning."
    keywords="Webdienst operationalisierung, REST API maschinelles lernen"
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
    ms.date="10/10/2016"
    ms.author="garye;raymondl"/>

# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Verwenden Sie einen Azure Machine Learning-Webdienst mit einer Webvorlage app

>[AZURE.NOTE] Dieses Thema beschreibt Verfahren für klassische Webdienst. 

Einmal Sie Ihre Vorhersagemodell entwickelt und bereitgestellt als Azure Web Service mit Machine Learning Studio oder Sie können mit Tools wie R oder Python standardisierten Modell eine REST-API zugreifen.

Es gibt verschiedene Arten der REST-API und Zugriff auf den Webdienst. Beispielsweise können Sie eine Anwendung schreiben, in C#, R oder Python mit Beispielcode für Sie generiert, wenn den Webdienst (verfügbar auf der API-Hilfeseite im Web Service-Dashboard in Machine Learning Studio) bereitgestellt. Oder Beispiel Microsoft Excel-Arbeitsmappe erstellt (auch verfügbar im Web Service-Dashboard in Studio) verwenden.

Aber Web App Vorlagen in [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/)ist die schnellste und einfachste Möglichkeit, den Webdienst zuzugreifen.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>Azure-Computer lernen App Webvorlagen

Web app Vorlagen in Azure Marketplace können benutzerdefinierte Web app erstellen, die Daten und Ergebnisse des Webdiensts kennt. Müssen Sie lediglich die Web app Zugriff auf Webdienst und Daten, und die Vorlage erledigt den Rest.

Zwei Vorlagen sind verfügbar:

- [Azure ML Anforderungsantwort Servicevorlage Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
- [Azure ML Batch Execution Web App Servicevorlage](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Jede Vorlage erstellt eine ASP.NET Anwendung mit API-URI und Schlüssel für den Webdienst und als Website in Azure bereitgestellt. Anforderung-Antwort-Dienst (RRS) Vorlage erstellt eine Web app eine einzelne Datenzeile an den Webdienst ein einzelnes Ergebnis zu senden. Batch Execution Service (BES) Vorlage erstellt eine Webanwendung, die viele Zeilen Daten mehrerer Ergebnisse senden kann.

Keine Codierung muss diese Vorlagen verwenden. Sie geben das API-URI und den Schlüssel und die Vorlage erstellt die Anwendung.

## <a name="how-to-use-the-request-response-service-rrs-template"></a>Verwendung die Anforderung / Antwort-Dienst (RRS) Vorlage

Nachdem Sie den Webdienst bereitgestellt haben, können so die Ressourceneinträge app Webvorlage verwenden, wie im folgenden Diagramm dargestellt folgen

![Ressourceneinträge Webvorlage einsetzen][image1]

1. Machine Learning Studio öffnen Sie **Webdienste** Registerkarte und dann den Webdienst zugreifen möchten. Kopieren Sie die Schlüssel unter **API-Schlüssel** und speichern.

    ![API-Schlüssel][image3]

2. **Anforderung-Antwort** -API-Hilfeseite zu öffnen. Am oberen Rand der Hilfeseite unter **anfordern** **Request-URI** -Wert kopieren und speichern. Dieser Wert sieht folgendermaßen aus:

        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true

    ![Anfrage-URI][image4]

3. [Azure-Portal](https://portal.azure.com) **Anmelden**, klicken Sie auf **neu**, suchen und wählen Sie **Azure ML Anforderungsantwort Service Web App**, klicken **Erstellen**. 

    - Geben Sie Ihrer Anwendung einen eindeutigen Namen. Die URL der Web-app werden diesen Namen gefolgt von `.azurewebsites.net.` beispielsweise`http://carprediction.azurewebsites.net.`

    - Wählen Sie Azure-Abonnement und Dienste unter denen der Webdienst ausgeführt wird.

    - Klicken Sie auf **Erstellen**.

    ![WebApp erstellen][image5]

4. Nach Azure bereitstellen der Web app, klicken Sie auf die **URL** auf der Seite Web app Settings in Azure oder geben Sie den URL in einem Webbrowser. Zum Beispiel`http://carprediction.azurewebsites.net.`

5. Beim Web app ersten fragt es Sie **URL ablegen API** und **API-Schlüssel**.
Geben Sie die zuvor gespeicherten Werte:
    - **Request-URI** von der API-Hilfeseite für **API-Post-URL**
    - **API-Schlüssel** aus dem Web Service-Dashboard für den **API-Schlüssel**.

    Klicken Sie auf **Senden**.

    ![Geben Sie Post URI und API-Schlüssel][image6]

6. Web app zeigt die Seite **Web App Konfiguration** mit aktuellen Web Service. Hier können Sie die Einstellungen der Webanwendung ändern.

    > [AZURE.NOTE] Ändern die Einstellung ändert nur diese Web App. Es ändert nicht die Standardeinstellungen für den Webdienst. Ändern Sie die **Beschreibung** hier ändern nicht es auf dem Web Service Dashboard angezeigten Machine Learning Studio Beschreibung.

    Wenn Sie fertig sind, klicken Sie auf **Speichern**und dann auf **zur Homepage**.

7. Auf der Homepage können auf Werte an den Webdienst **Senden**und das Ergebnis zurückgegeben.

Wenn zur Seite **Konfiguration** zurückgegeben werden soll, fahren Sie mit dem `setting.aspx` der Web app. Beispiel: `http://carprediction.azurewebsites.net/setting.aspx.` werden Sie aufgefordert, den API-Schlüssel erneut eingeben - Sie benötigen, zu Seite Einstellungen.

Beenden können, starten oder Web app im Azure-Portal wie jede andere Web app löschen. Als Ausführung können Sie home Webadresse suchen und neue Werte eingeben.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>Verwenden die Vorlage Batch Execution Service (BES)

Die Webvorlage app BES können genauso wie die Vorlage Ressourceneinträge, entsteht Web-app können Sie mehrere Datenzeilen und mehrere Ergebnisse.

Die Ergebnisse in einem Batch Execution-Webdienst werden in einem Container Azure-Speicher gespeichert. die Eingabewerte können Azure-Speicher oder eine lokale Datei stammen.
Daher benötigen Sie ein Azure Speichercontainer für die Ergebnisse von Web app zurückgegeben und müssen die Eingabedaten bereit.

![BES Webvorlage einsetzen][image2]

1. Gehen Sie außer BES Web app für Ressourceneinträge Vorlage erstellen:
    - Erhalten der **Request-URI** die **BATCHAUSFÜHRUNG** API-Hilfeseite für den Webdienst.
    - Gehen Sie zu [Azure ML Batch Execution Service Web App-Vorlage](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) zu öffnen die Vorlage BES Azure Marketplace **Web App erstellen**.

2. Geben Sie die Ergebnisse gespeichert werden soll, die Zielinformationen Container auf der Homepage von Web app. Auch Geben Sie an, die Webanwendung die Eingabewerte in einer lokalen Datei oder einem Container Azure-Speicher abrufen können.
Klicken Sie auf **Senden**.

    ![Speicherinformationen][image7]

Die Webanwendung wird eine Seite mit angezeigt.
Wenn der Auftrag abgeschlossen ist den Speicherort der Ergebnisse in Azure BLOB-Speicher erhalten Sie. Sie können auch die Ergebnisse in eine lokale Datei heruntergeladen.

## <a name="for-more-information"></a>Weitere Informationen

Weitere Informationen zu...

- Erstellen eines Experiments maschinelles Lernen mit Computer lernen, finden Sie unter [den ersten Test in Azure Machine Learning Studio erstellen](machine-learning-create-experiment.md)

- zum Bereitstellen von computerlernen als Webdienst experimentieren, finden Sie unter [Bereitstellen einer Azure Machine Learning-Webdienst](machine-learning-publish-a-machine-learning-web-service.md)

- [ein Azure Machine Learning-Webdienst](machine-learning-consume-web-services.md) von anders auf den Webdienst finden Sie unter


[image1]: media\machine-learning-consume-web-service-with-web-app-template\rrs-web-template-flow.png
[image2]: media\machine-learning-consume-web-service-with-web-app-template\bes-web-template-flow.png
[image3]: media\machine-learning-consume-web-service-with-web-app-template\api-key.png
[image4]: media\machine-learning-consume-web-service-with-web-app-template\post-uri.png
[image5]: media\machine-learning-consume-web-service-with-web-app-template\create-web-app.png
[image6]: media\machine-learning-consume-web-service-with-web-app-template\web-service-info.png
[image7]: media\machine-learning-consume-web-service-with-web-app-template\storage.png
