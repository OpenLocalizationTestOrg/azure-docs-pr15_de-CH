<properties
    pageTitle="Einen Azure-Suchdienst mithilfe von Azure-Portal erstellen | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Erfahren Sie, wie einen Azure-Suchdienst mithilfe von Azure-Portal bereitstellen."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-service-using-the-azure-portal"></a>Erstellen Sie einen Azure-Suchdienst mithilfe von Azure-Portal

Dieses Handbuch führt Sie durch den Prozess der Erstellung (oder Bereitstellung) eine Azure-Suchdienst mithilfe der [Azure-Portal](https://portal.azure.com/).

Dieses Handbuch wird davon ausgegangen, dass bereits ein Azure-Abonnement und Azure-Portal anmelden können.

## <a name="find-azure-search-in-the-azure-portal"></a>Azure Suche in Azure-Portal suchen
1. Zum [Azure-Portal](https://portal.azure.com/) und Anmeldung.
1. Klicken Sie auf das Pluszeichen ("+") in der oberen linken Ecke.
2. Wählen Sie **Daten und Speicher**.
3. Wählen Sie **Azure suchen**.

![](./media/search-create-service-portal/find-search.png)

## <a name="pick-a-service-name-and-url-endpoint-for-your-service"></a>Wählen Sie einen Namen und URL-Endpunkt für den Dienst
1. Dienstnamen werden Teil der Azure-Suchdienst Endpunkt-URL, gegen die Sie API telefonieren werden verwalten und verwenden den Suchdienst.
2. Geben Sie im Feld **URL** Dienstnamen. Der Dienstname:
  * darf nur Kleinbuchstaben, Ziffern oder Bindestriche ("-")
  * können keinen Bindestrich ("-") als die ersten 2 Zeichen oder letzten Zeichens
  * dürfen nicht aufeinander folgende Bindestriche ("-")
  * zwischen 2 und 60 Zeichen


## <a name="select-a-subscription-where-you-will-keep-your-service"></a>Wählen Sie ein Abonnement, Sie den Dienst halten
Haben Sie mehrere Abonnements können Sie auswählen, welche dieser Azure-Suchdienst enthalten.

## <a name="select-a-resource-group-for-your-service"></a>Wählen Sie für den Dienst
Eine neue Ressourcengruppe erstellen oder eine vorhandene auswählen. Eine Ressourcengruppe ist eine Sammlung von Azure Services und Ressourcen, die zusammen verwendet werden. Beispielsweise verwenden Sie Azure Suche indizieren eine SQL-Datenbank, sollten dann beide Dienste Teil derselben Ressourcengruppe sein.

## <a name="select-the-location-where-your-service-will-be-hosted"></a>Wählen Sie den Speicherort, in dem der Dienst gehostet werden
Azure Suche steht als Azure Service in Rechenzentren auf der ganzen Welt befinden. Beachten Sie diese [Preise sind](https://azure.microsoft.com/pricing/details/search/) Geography.

## <a name="select-your-pricing-tier"></a>Wählen Sie die Preisstufe
[Azure Search wird momentan in mehreren Tarifen](https://azure.microsoft.com/pricing/details/search/): frei, Basic oder Standard. Jede Ebene hat ihre eigene [Kapazität und Grenzen](search-limits-quotas-capacity.md). Hinweise finden Sie unter [Wählen Sie eine Preiskalkulation Tier oder SKU](search-sku-tier.md) .

In diesem Fall haben wir die Standard-Stufe für den Dienst.

## <a name="select-the-create-button-to-provision-your-service"></a>Klicken Sie auf "Erstellen", den Dienst bereitstellen

![](./media/search-create-service-portal/create-service.png)

## <a name="scale-your-service"></a>Ihr Dienst skalieren

Nachdem der Dienst bereitgestellt wird, können Sie es Ihren Bedürfnissen skalieren. Standard-Stufe für Ihren Azure Search-Dienst ausgewählt haben, können Sie Ihren Dienst in zwei Dimensionen skalieren: Replikate und Partitionen. Wenn Sie die grundlegende Ebene ausgewählt haben, können Sie nur Replikate hinzufügen.

*__Partitionen__* ermöglichen Ihren Dienst zum Speichern und weitere Dokumente durchsuchen.

*__Replikate__* ermöglichen Ihren Dienst für höhere Belastung von Suchabfragen - [Dienst erfordert 2 Replikate zu schreibgeschützten SLA und erfordert 3 Replikate zu einer SLA Lese-/Schreibzugriff](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Gehen Sie zu der Azure-Suchdienst Management Blade im Azure-Portal.
2. Wählen Sie im Blatt **Einstellungen** **Skalieren**.
3. Hinzufügen von Replikaten oder Partitionen, um Ihren Dienst zu skalieren.
  * Den Dienst über 36 Suche Einheiten kann nicht skaliert werden. Die Gesamtanzahl der Suche ist das Produkt der Replikate und Partitionen (Replikate * Partitionen = Gesamtstückzahl suchen).
  * Wenn Sie die grundlegende Ebene ausgewählt haben, können Sie nur 3 Replikate skalieren. Basic Services sind einer Partition gebunden.

![](./media/search-create-service-portal/scale-service.png)

## <a name="next"></a>Weiter
Nach der Bereitstellung eines Azure-Suchdienst werden Sie definieren [einen Suchindex Azure](search-what-is-an-index.md) hochladen und Ihre Daten zu suchen.

Ein kurzes Lernprogramm finden Sie unter [Erste Schritte mit Azure Suche im Portal](search-get-started-portal.md) .
