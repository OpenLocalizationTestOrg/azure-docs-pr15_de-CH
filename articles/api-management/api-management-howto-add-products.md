<properties 
    pageTitle="Erstellen und Veröffentlichen eines Produkts in Azure API Management" 
    description="Informationen Sie zum Erstellen und Veröffentlichen von Produkten in Azure API Management." 
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

# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>Erstellen und Veröffentlichen eines Produkts in Azure API Management

Ein Produkt enthält in Azure API Management eine oder mehrere APIs sowie Verwendung Kontingent und die Vereinbarung. Veröffentlichtes Produkt können Entwickler das Produkt abonnieren und der Produkt APIs verwenden. Das Thema Leitfaden Erstellen eines Produkts, eine API und Veröffentlichen für Entwickler.

## <a name="create-product"> </a>Erstellen eines Produkts

Hinzugefügt und so konfiguriert, dass eine API im Publisher-Portal. Klicken Sie Publisher-Portal **Verwalten** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Dienstinstanz API Management noch nicht erstellt haben, finden Sie im Lernprogramm [Erste Schritte mit Azure API Management][] [Erstellen API Management Service][] .

Klicken Sie im Menü links auf die Seite **Produkte** anzeigen zu **Produkten** und auf **Produkt hinzufügen**.

![Produkte][api-management-products]

![Neues Produkt][api-management-add-new-product]

Geben Sie einen beschreibenden Namen für das Produkt in den Feldern **Name** und Beschreibung des Produkts in das Feld **Beschreibung** ein.

Produkte in API Management können **geöffnet** oder **geschützt**werden. Geschützte Produkte müssen abonniert werden, bevor sie beim Öffnen verwendet werden können, Produkte können ohne Abonnement verwendet. Überprüfen Sie erstellen ein Abonnements erfordert ein geschütztes Produkt **Abonnement benötigen** . Dies ist die Standardeinstellung.

Aktivieren Sie **Abonnement Genehmigung** Administrator überprüfen und akzeptieren oder ablehnen versucht, dieses Abonnement. Wenn das Kontrollkästchen deaktiviert ist, werden Versuche Abonnement automatisch genehmigt. Weitere Informationen über Abonnements finden Sie unter [Abonnenten zu einem Produkt anzeigen][].

Damit entwicklerkonten mehrmals das Produkt abonnieren können, aktivieren Sie das Kontrollkästchen **mehrere Abonnements zulassen** . Wenn dieses Kontrollkästchen nicht aktiviert ist, kann jede Entwicklerkonto nur einmal für das Produkt abonnieren.

![Unbegrenzte Mehrfachlizenzen][api-management-unlimited-multiple-subscriptions]

Begrenzung für die Anzahl gleichzeitiger Mehrfachlizenzen Kontrollkästchen Sie **Maximalanzahl gleichzeitiger Abonnements** das und geben Sie die zulässige Anzahl. Im folgenden Beispiel werden Abonnements gleichzeitig auf vier Entwicklerkonto.

![Vier Mehrfachlizenzen][api-management-four-multiple-subscriptions]

Alle neuen Produktoptionen konfiguriert wurden, klicken Sie auf **Speichern** , um das neue Produkt erstellen.

![Produkte][api-management-products-page]

>Standardmäßig neue Produkte unveröffentlicht und sind nur für die Gruppe **Administratoren** sichtbar.

Um ein Produkt zu konfigurieren, klicken Sie auf den Produktnamen in **die Produkte** .

## <a name="add-apis"> </a>APIs zu einem Produkt hinzufügen

Die Seite **Produkte** enthält vier Links für Konfiguration: **Zusammenfassung**, **Einstellungen**, **Sichtbarkeit**und **Abonnenten**. Die Registerkarte **Zusammenfassung** ist können APIs hinzufügen und veröffentlichen oder Aufheben der Veröffentlichung eines Produkts.

![Zusammenfassung][api-management-new-product-summary]

Vor der Veröffentlichung des Produkts müssen Sie eine oder mehrere APIs hinzugefügt. Klicken Sie hierzu auf **API Produkt hinzufügen**.

![Hinzufügen von APIs][api-management-add-apis-to-product]

Wählen Sie die gewünschte APIs, und klicken Sie auf **Speichern**.

## <a name="add-description"> </a>Beschreibende Informationen zu einem Produkt hinzufügen

**Die Registerkarte** können Sie detaillierte Informationen zum Produkt Verwendungszweck auf stellt APIs und andere nützliche Informationen. Inhalt richtet sich an Entwickler, die Aufrufen der API im nur-Text oder HTML-Code geschrieben werden kann.

![Produkt][api-management-product-settings]

Überprüfen Sie **Abonnement benötigen** ein geschütztes Produkt erstellen erfordert ein Abonnement verwendet werden, oder deaktivieren Sie das Kontrollkästchen, um ein Produkt erstellen, ohne ein Abonnement aufgerufen werden können.

Soll manuell alle Produkt-Abonnement Anfragen genehmigen wählen Sie **Abonnement genehmigt** . Standardmäßig werden alle Produktabonnements automatisch erteilt.

Damit können entwicklerkonten mehrmals das Produkt abonnieren das Kontrollkästchen Sie **mehrere Abonnements** und optional festlegen Sie einen Grenzwert. Wenn dieses Kontrollkästchen nicht aktiviert ist, kann jede Entwicklerkonto nur einmal für das Produkt abonnieren.

Optional füllen Sie **Verwendung** beschreibt die Vereinbarung für das Produkt die Abonnenten akzeptieren müssen, um das Produkt verwenden aus.

## <a name="publish-product"> </a>Veröffentlichen eines Produkts

Bevor die Produkt-APIs aufgerufen werden kann, muss das Produkt veröffentlicht werden. Klicken Sie auf der Registerkarte **Zusammenfassung** des Produkts auf **Veröffentlichen**und dann auf **Ja, veröffentlichen** zu bestätigen. Ein zuvor veröffentlichten Produkt private, klicken Sie auf **Veröffentlichung**.

![Produkt veröffentlichen][api-management-publish-product]

## <a name="make-visible"> </a>Produkt für Entwickler sichtbar machen

Registerkarte **Sichtbarkeit** können Sie auswählen, welche Rollen können das Produkt auf Entwicklerportal und das Produkt abonnieren.

![Produkt-Sichtbarkeit][api-management-product-visiblity]

Zum Aktivieren oder deaktivieren die Sichtbarkeit eines Produkts für Entwickler in einer Gruppe, überprüfen Sie oder deaktivieren Sie das Kontrollkästchen neben der Gruppe, und klicken Sie auf **Speichern**.

>Weitere Informationen finden Sie unter [Erstellen und Verwenden von Gruppen zum Verwalten von Firmen in Azure API Management Entwickler][].

## <a name="view-subscribers"> </a>Abonnenten zu einem Produkt anzeigen

Die Registerkarte **Abonnenten** Listet die Entwickler das Produkt abonniert haben. Details und Einstellungen für jeden Entwickler können angezeigt werden, indem Sie auf den Namen des Entwicklers. In diesem Beispiel abonniert keine Entwickler noch das Produkt.

![Entwickler][api-management-developer-list]

## <a name="next-steps"> </a>Nächste Schritte

Sobald die gewünschte APIs hinzugefügt und das Produkt veröffentlicht Entwickler können das Produkt abonnieren und die APIs aufrufen. Finden Sie ein Lernprogramm, das diese sowie erweiterte Konfiguration veranschaulicht, [wie erstellen und erweiterten Einstellungen in Azure API Management][].

Weitere Informationen zum Arbeiten mit Produkten finden Sie im folgende Video.

> [AZURE.VIDEO using-products]

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[Abonnenten zu einem Produkt anzeigen]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Erste Schritte mit Azure API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[Das Erstellen und Verwenden von Gruppen zum Verwalten von Benutzerkonten in Azure API Management Entwickler]: api-management-howto-create-groups.md
[Wie erstellen und erweiterten Einstellungen in Azure API Management]: api-management-howto-product-with-rules.md 