<properties 
    pageTitle="Computerlernen veröffentlichen Webdienst Azure Marketplace | Microsoft Azure" 
    description="Azure Marketplace den Azure Machine Learning-Webdienst veröffentlichen" 
    services="machine-learning" 
    documentationCenter="" 
    authors="BharathS" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="bharaths"/>

# <a name="publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a>Azure Marketplace veröffentlichen Sie Azure Machine Learning-Webdienst 

Azure Marketplace ermöglicht Azure Machine Learning-Webdiensten wie Veröffentlichen oder Diensten zur Nutzung von externen Kunden. Dieser Artikel enthält einen Überblick, Links zu Richtlinien, die Ihnen beim Einstieg. Mithilfe dieses Verfahrens können Sie Ihre Webdienste für andere Entwickler in ihrer Anwendung verwenden verfügbar machen.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a>Übersicht über das Veröffentlichen 

Es folgen die Schritte zum Veröffentlichen eines Webdienstes Azure Machine Learning Azure Marketplace:

1. Erstellen und Veröffentlichen eines Dienstes Computer lernen Anforderungsantwort (RRS)
2. Bereitstellen Sie den Dienst für die Produktion und Informationen Sie der API-Schlüssel und OData-Endpunkt.
3. Verwenden Sie den URL der veröffentlichten Webdienst [Azure](https://publish.windowsazure.com/workspace/) Marketplace (Markt) veröffentlichen 
4. Sobald übermittelt, wird Ihr Angebot geprüft und muss vor Ihrer Kunden können es kaufen. Der Veröffentlichungsprozess kann einige Werktage. 

## <a name="walk-through"></a>Durchlaufen
###<a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Schritt 1: Erstellen und Veröffentlichen eines Dienstes Computer lernen Anforderungsantwort (RRS)###
 Wenn Sie dies noch nicht geschehen ist, schauen Sie in dieser [Anleitung](machine-learning-walkthrough-5-publish-web-service.md).

###<a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a>Schritt 2: Bereitstellen Sie des Dienstes zur Produktion und erhalten API-Schlüssel und OData-Endpunktinformationen###
1. [Azure-Verwaltungsportal](http://manage.windowsazure.com)die Option **COMPUTERLERNEN** in der linken Navigationsleiste und wählen Sie den Arbeitsbereich. 

2. Klicken Sie auf die Registerkarte **WEB SERVICES** , und wählen Sie den Webdienst auf dem Markt veröffentlichen möchten.

    ![Azure Marketplace][workspace]

3. Wählen Sie den Endpunkt am Markt Nutzen haben möchten. Wenn Sie zusätzliche Endpunkte erstellt haben, können Sie den **Standard** -Endpunkt auswählen.

4. Nachdem Sie auf den Endpunkt geklickt haben, sein können Sie den **API-Schlüssel**. Sie benötigen diese Information Informationen in Schritt 3 so kopieren Sie es.

    ![Azure Marketplace][apikey]

5. Klicken Sie auf die Methode **Anforderung und Antwort** an dieser Stelle veröffentlichen Batch Execution Services auf dem Markt nicht unterstützt. Sie zur Hilfeseite für Anforderung-Antwort-Methode API gelangen.

6. Die **Endpunktadresse OData**, benötigen Sie diese Informationen in Schritt 3.

    ![Azure Marketplace][odata]




Bereitstellen des Dienstes in der Produktion.



###<a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a>Schritt 3: Verwenden der URL der veröffentlichten Webdienst veröffentlichen Azure Marketplace (DataMarket)###

1.  Navigieren Sie zu [Azure Marketplace (Markt)](http://datamarket.azure.com/home) 
2.  Klicken Sie am oberen Rand der Seite auf **Veröffentlichen** . Hierdurch gelangen Sie zum [Microsoft Azure-Veröffentlichungsportal](https://publish.windowsazure.com)
3.  Klicken Sie auf den **Verleger** als Verleger zu registrieren.
4.  Wenn Sie ein neues Angebot erstellen, **Datendienste**, klicken Sie auf **einen neuen Datendienst erstellen**. 
 
    ![Azure Marketplace][image1]

    <br />


5.  Angaben Sie unter **plant die** zu Ihrem Angebot inklusive einer Preisgestaltung. Entscheiden Sie, ob Sie einen kostenlosen oder kostenpflichtigen Dienst anbieten. Um bezahlt, bieten Sie Informationen wie Ihre Bank und steuern.

6.  Informationen Sie unter **Marketing** Ihr Angebot wie Titel und Beschreibung für das Angebot.

7.  Unter **Preise** können Festpreis für Ihre Pläne für bestimmte Länder oder lassen Sie "Autoprice" das Angebot.

8. Klicken Sie auf der Registerkarte **Datendienst** **Webdienst** als **Datenquelle**.

    ![Azure Marketplace][image2]

9.  Rufen Sie den Web Service URL und API-Schlüssel von Azure-Verwaltungsportal wie in Schritt 2 beschrieben.

10. Fügen Sie im Datendienst Marketplace-Dialogfeld die Endpunktadresse OData im Textfeld **Dienst-URL** .

11. Wählen Sie für die **Authentifizierung** **Header** als **Authentifizierungsschema**.

    - Geben Sie für den **Header namens**"Autorisierung".
    - **Headerwert**Geben Sie ein "Inhaber" (ohne Anführungszeichen), klicken Sie auf **die LEERTASTE** und fügen Sie den API-Schlüssel.
    - Aktivieren Sie das Kontrollkästchen **diesen Dienst OData ist** .
    - Klicken Sie auf **Verbindung testen** , um die Verbindung zu testen.

12. Klicken Sie unter **Kategorien**sicherstellen Sie, dass **Computerlernen** ausgewählt ist.

13. Wenn die Metadaten zu Ihren Preisvorschlag eingeben klicken Sie auf **Veröffentlichen**, und **Staging Push**. Zu diesem Zeitpunkt benachrichtigt Sie alle verbleibenden Probleme, die behoben werden müssen.

14. Nach Abschluss aller Probleme sichergestellt, klicken Sie auf **Genehmigung auf Produktion anfordern**. Der Veröffentlichungsprozess kann einige Werktage. 


[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png
 
