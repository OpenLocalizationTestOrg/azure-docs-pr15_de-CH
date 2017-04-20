<properties
   pageTitle="Skalierung Webdienst | Microsoft Azure"
   description="Erfahren Sie, wie einen Webdienst Parallelität und neue Endpunkte hinzufügen."
   services="machine-learning"
   documentationCenter=""
   authors="neerajkh"
   manager="srikants"
   editor="cgronlun"
   keywords="Azure-Computer lernen, Webdienste, operationalisierung, Skalierung, Endpunkt, Parallelität"
   />
<tags
   ms.service="machine-learning"
   ms.devlang="NA"
   ms.workload="data-services"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.date="10/05/2016"
   ms.author="neerajkh"/>

# <a name="scaling-a-web-service"></a>Skalieren Sie einen Webdienst

>[AZURE.NOTE] Dieses Thema beschreibt Verfahren für eine klassische Machine Learning-Webdienst. 

Standardmäßig jeden veröffentlichten Webdienst konfiguriert 20 gleichzeitige Anfragen und kann bis zu 200 gleichzeitige Anfragen. Klassische Azure-Portal bietet eine Möglichkeit, diesen Wert Azure Machine Learning optimiert automatisch die Einstellung, um die optimale Leistung für den Webdienst und Portal Wert ignoriert. 

Wenn Plan Aufruf die API mit höher als 200 der Wert Max-Anrufe unterstützt, sollten Sie mehrere Endpunkte auf denselben Webdienst erstellen. Sie können dann Ihre zufällig über alle verteilen.

## <a name="add-new-endpoints-for-same-web-service"></a>Hinzufügen von neuen Endpunkten für denselben Webdienst

Die Skalierung eines Webdienstes ist eine häufige Aufgabe. Gründe, skaliert werden 200 gleichzeitige Supportanfragen, Verfügbarkeit über mehrere Endpunkte oder separate Endpunkte für den Webdienst. Zusätzliche Endpunkte für denselben Webdienst [klassischen Azure-Portal](https://manage.windowsazure.com/) oder das Portal [Azure Machine Learning-Webdienst](https://services.azureml.net/) hinzufügen, um die Skalierung zu erhöhen.

Weitere Informationen zum Hinzufügen von neuen Endpunkten finden Sie [Endpunkte erstellen](machine-learning-create-endpoint.md).

Denken Sie daran, die mit hoher Parallelität Anzahl kann schädlich, wenn Sie nicht entsprechend viele API aufrufen. Gelegentliche Timeouts oder Spitzen vermutlich in der Wenn Sie relativ geringe Auslastung auf eine API für hohe Last konfiguriert.

Die synchronen APIs werden normalerweise in Situationen verwendet, in denen ein geringe Latenz gewünscht wird. Wartezeit hier impliziert die Zeit für die API eine Anforderung, und berücksichtigt alle Netzwerkprobleme nicht. Angenommen Sie haben eine API mit 50 ms Wartezeit. Die verfügbare Kapazität mit Schubkontrolle hoch und Max. gleichzeitige Aufrufe vollständig nutzen = 20, müssen zum Aufruf dieser API 20 * 1000 / 50 = 400 Mal pro Sekunde. Weitere Verlängerung, ermöglicht eine maximale gleichzeitige Aufrufe von 200 API 4000 Male pro Sekunde, 50 ms Latenz vorausgesetzt aufrufen.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
