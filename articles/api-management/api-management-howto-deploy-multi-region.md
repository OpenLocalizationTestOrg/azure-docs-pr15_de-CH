<properties
    pageTitle="Zum Bereitstellen einer Dienstinstanz Azure API Management in Azure mehrere Regionen"
    description="Informationen Sie zum Bereitstellen einer Azure API Management Service-Instanz in Azure mehrere Regionen." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Zum Bereitstellen einer Dienstinstanz Azure API Management in Azure mehrere Regionen

API-Management unterstützt mit mehreren Deployment API Herausgeber einen einzigen API Management-Dienst über eine beliebige Anzahl von gewünschten Azure Regionen verteilen kann. Dies verringert Anforderung Wartezeit wahrgenommen geografisch verteilte API Verbraucher sowie eine verbesserte Verfügbarkeit wenn Region offline geschaltet wird. 

Wenn ein API-Verwaltungsdienst erstmalig erstellt wird, enthält nur eine [Einheit][] und befindet sich in einer einzigen Azure Region als primäre Region bezeichnet. Zusätzliche Bereiche können einfach über Azure-Verwaltungsportal hinzugefügt werden. API-Gateway Verwaltungsserver für die Region bereitgestellt und Aufruf Datenverkehr zum nächsten Gateway weitergeleitet werden. Ein Bereich offline geht, ist der Datenverkehr automatisch zum nächsten nächsten Gateway weitergeleitet. 

> [AZURE.IMPORTANT] Mit mehreren Bereitstellung ist nur verfügbar in der **[Premium][]** -Ebene.

## <a name="add-region"> </a>Eine API Management Service-Instanz in einen neuen Bereich bereitgestellt

Klicken Sie zunächst auf **Verwalten** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst. Dadurch gelangen Sie zu Publisher API Verwaltungsportal.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Dienstinstanz API Management noch nicht erstellt haben, finden Sie im Lernprogramm [Erste Schritte mit Azure API Management][] [Erstellen API Management Service][] .

Navigieren Sie zu der Registerkarte **Skalierung** in Azure-Verwaltungsportal für die Dienstinstanz API Management. 

![Registerkarte Skalierung][api-management-scale-service]

Um einen neuen Bereich bereitzustellen, klicken auf die Dropdown-Liste unterhalb des primären Bereichs und wählen Sie eine Region aus der Liste.

![Bereich hinzufügen][api-management-add-region]

Wählt der Bereich Wählen Sie die Anzahl der Einheiten für den neuen Bereich aus der Dropdownliste Einheit.

![Geben Sie Einheiten][api-management-select-units]

Sobald die gewünschten Bereiche und konfiguriert sind, klicken Sie auf **Speichern**.

## <a name="remove-region"> </a>Eine API Management Service-Instanz aus einem Bereich löschen

Um eine API Management Service-Instanz aus einem Bereich entfernen, navigieren Sie zur Registerkarte **Skalierung** in Azure-Verwaltungsportal für die Dienstinstanz API Management. 

![Registerkarte Skalierung][api-management-scale-service]

Klicken Sie auf das **X** neben der gewünschten entfernen.  

![Bereich entfernen][api-management-remove-region]

Sobald die gewünschten Bereiche entfernt werden, klicken Sie auf **Speichern**.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance
[Erste Schritte mit Azure API Management]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[Einheit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

