<properties 
    pageTitle="Vorgänge an eine API in Azure API Management hinzufügen | Microsoft Azure" 
    description="Erfahren Sie, wie eine API in Azure API Management Vorgänge hinzufügen." 
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

# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Wie eine API in Azure API Management Operationen hinzugefügt

Bevor eine API in API Management verwendet werden kann, müssen Vorgänge hinzugefügt. Dabei wird das Hinzufügen und Konfigurieren verschiedener Operationen eine API in API Management.

## <a name="add-operation"> </a>Einen Vorgang hinzufügen

Hinzugefügt und so konfiguriert, dass eine API im Publisher-Portal. Klicken Sie Publisher-Portal **Verwalten** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Dienstinstanz API Management noch nicht erstellt haben, finden Sie im Lernprogramm [Erste Schritte mit Azure API Management][] [Erstellen API Management Service][] .

Wählen Sie die gewünschte API im Publisher-Portal und anschließend die Registerkarte **Vorgänge** . 

![Vorgänge][api-management-operations]

Klicken Sie auf **Vorgang hinzufügen** , um einen neuen Vorgang hinzufügen. Der **neue Vorgang** angezeigt, und die Registerkarte **Signatur** standardmäßig aktiviert.

![Hinzufügen][api-management-add-operation]

Angeben der **http-Verb** aus der Dropdown-Liste auswählen.

![HTTP-Methode][api-management-http-method]

<a name="url-template"></a>

Definieren Sie URL-Vorlage durch Eingabe in ein URL-Fragment aus ein oder mehrere URL-Pfadsegmente und NULL oder mehr Abfragezeichenfolgen-Parameter. Die URL-Vorlage auf Basis der API-URL angefügt identifiziert eine einzelne HTTP-Operation. Enthalten eine oder mehrere benannte Variable Teile durch geschweifte Klammern gekennzeichnet sind. Diese Variable Teile werden als Vorlagenparameter und Werte extrahiert aus der URL der Anforderung bei der Verarbeitung der Anforderung durch die API-Plattform werden dynamisch zugewiesen.

![URL-Vorlage][api-management-url-template]

<a name="rewrite-url-template"></a>

**Schreiben Sie URL-Vorlage**fest. Dadurch können Sie mithilfe die URL-Standardvorlage für die Verarbeitung von Anfragen auf dem Front-End und Back-End über eine konvertierte URL nach der Vorlage schreiben. Vorlagenparameter aus der URL-Vorlage sollte in der Vorlage zum Umschreiben von Adressen verwendet werden. Das folgende Beispiel zeigt wie Inhaltstyp codiert Pfadsegment im Web Service aus dem vorherigen Beispiel als Abfrageparameter in der API veröffentlicht über den URL Vorlagen API Management-Plattform bereitgestellt werden kann.

![URL-Vorlage schreiben][api-management-url-template-rewrite]

Aufrufer der Operation verwenden das Format `/customers?customerid=ALFKI` und werden dadurch zugeordnet `/Customers('ALFKI')` Wenn der Back-End-Dienst aufgerufen wird.


**Anzeigename** und **Beschreibung** eine Beschreibung des Vorgangs und dienen zur Dokumentation der Entwickler diese API im Entwicklerportal.

![Beschreibung][api-management-description]

Die Beschreibung des Vorgangs kann als Text oder HTML in das Textfeld **Beschreibung** angegeben werden.

## <a name="operation-caching"> </a>Vorgang Zwischenspeichern

Antwort Zwischenspeichern reduziert Wartezeit wahrgenommen API Verbraucher, geringere Bandbreite und verringert die Belastung der HTTP-Web service API implementieren. 

Um schnell und einfach Zwischenspeichern für den Vorgang, wählen Sie die Registerkarte **Zwischenspeichern** und Kontrollkästchen Sie **Aktivieren** .

![Zwischenspeichern][api-management-caching-tab]

**Dauer** gibt den Zeitraum in dem Antwort der Vorgang im Cache verbleiben. Der Standardwert ist 3600 Sekunden oder 1 Stunde.

Cacheschlüssel zum Antworten unterscheiden, so dass die Antwort jedes andere Cacheschlüssel für eigene separate zwischengespeicherten Wert. Geben Sie optional bestimmte Abfragezeichenfolgen-Parameter und Header Cache Werte in die Textfelder **Unterscheidung nach Abfragezeichenfolgeparametern** und **verschieden Header** bzw. computing verwendet werden. Wenn keine URL angegeben, vollständige Anforderung und HTTP-Headerwerte wird im Cache Schlüsselerzeugung: **akzeptieren** und **Accept-Charset**.

>Weitere Informationen zum Zwischenspeichern und Zwischenspeichern von Richtlinien finden Sie unter [Cache Socketvorgangsergebnisse in Azure API Management][].


## <a name="request-parameters"> </a>Parameter anfordern

Parameter werden auf der Registerkarte Parameter verwaltet. Parameter in der **URL-Vorlage** auf der Registerkarte **Signatur** automatisch hinzugefügt und können nur durch Bearbeiten der URL-Vorlage geändert werden. Zusätzliche Parameter können manuell eingegeben werden.

Einen neuen Abfrageparameter, **Abfrageparameter hinzufügen** und geben Sie folgende Informationen:

-   **Name** - Parameternamen.
-   **Beschreibung** - eine kurze Beschreibung des Parameters (optional).
-   **Typ** - Parametertyp unten in der Dropdownliste ausgewählt.
-   **Werte** - Werte, die diesem Parameter zugewiesen werden können. Einer der Werte kann als Standard (optional) gekennzeichnet werden.
-   **Erforderlich** - den Parameter durch Aktivieren des Kontrollkästchens notwendig machen. 

![Anforderungsparameter][api-management-request-parameters]

## <a name="request-body"> </a>Anforderungsinhalt

Lässt der Vorgang (PUT, POST) und Text können ein Beispiel in allen unterstützten Darstellungsformate (z. B. Json, XML). 

>Der Anforderungstext nur für Dokumentationszwecke verwendet und ist nicht gültig.

Um einen Anforderungstext einzugeben, wechseln Sie zur Registerkarte **Körper** .

Klicken Sie auf **Darstellung hinzufügen**, geben Sie gewünschte Inhaltstypnamen (z. B. Anwendung/Json) in der Dropdown-Liste auswählen und gewünschte Anfrage Text wird im ausgewählten Format in das Textfeld einfügen. 

![Anfragetext][api-management-request-body]

In zusätzlichen Darstellung auch im Textfeld **Beschreibung** eine optionale Beschreibung soll.

## <a name="responses"> </a>Antworten

In diesem Zusammenhang empfiehlt es sich, Antworten für alle Statuscodes Beispiele, die die Operation erzeugen kann. Jeder Code, der möglicherweise mehr als eine Antwort Körper wird für jeden unterstützten Inhaltstypen. 

Um eine Antwort hinzuzufügen, klicken Sie auf **Hinzufügen** und geben Sie den gewünschten Status-Code. In diesem Beispiel wird der Statuscode **200 OK**. Nachdem der Code in der Dropdown-Liste angezeigt wird, wählen sie und Antwortcode erstellt und der Vorgang hinzugefügt.

![Antwortcode][api-management-response-code]

Klicken Sie auf **Darstellung hinzuzufügen**, geben Sie den gewünschten Inhaltstypnamen (z. B. Anwendung/Json) aus, die in der Drop down.

![Inhaltstyp Text][api-management-response-body-content-type]

Das Textfeld Antwort Text wird im ausgewählten Format einfügen. 

![Antwort][api-management-response-body]

Fügen Sie ggf. eine Beschreibung in das Textfeld **Beschreibung** ein.

Sobald der Vorgang konfiguriert ist, klicken Sie auf **Speichern**.


## <a name="next-steps"> </a>Nächste Schritte

Wenn eine API Operationen hinzugefügt werden, besteht der nächste Schritt die API mit einem Produkt und veröffentlichen, sodass Entwickler die Operationen aufrufen können.

-   [Erstellen und Veröffentlichen eines Produkts][]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Erste Schritte mit Azure API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[Erstellen und Veröffentlichen eines Produkts]: api-management-howto-add-products.md
[Wie Cache Socketvorgangsergebnisse in Azure API Management]: api-management-howto-cache.md