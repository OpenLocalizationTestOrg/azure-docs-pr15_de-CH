<properties 
    pageTitle="Computer lernen Empfehlung: JavaScript-Integration | Microsoft Azure" 
    description="Azure Machine Learning Recommendations - Integration JavaScript - Dokumentation" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="javascript" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Azure maschinelles lernen Recommendations - JavaScript-Integration

>[AZURE.NOTE]Sie sollten diese Version anstelle des empfohlenen API kognitive starten. Empfohlene kognitive Service wird dieser Dienst ersetzt und alle neuen Features es entwickelt. Es verfügt über neue Funktionen wie Batchverarbeitung Support besser API Explorer eine sauberere API-Oberfläche und Anmeldung/Abrechnung Erfahrung, usw..
> Weitere Informationen zu [neuen kognitive Dienst migrieren](http://aka.ms/recomigrate)


Dieses Dokument zeigen Ihre Site mithilfe von JavaScript zu integrieren. JavaScript können Sie die Datenerfassung Ereignisse senden und empfohlene verbraucht nach eines empfehlungsmodells erstellen. Alle Operationen über JS können auch auf Server erfolgen.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1. allgemeine Übersicht
Integration Ihrer Website in Azure ML Recommendations in 2 Phasen bestehen:

1.  Senden Sie Ereignisse in Azure ML Empfehlung. Dadurch wird eine empfehlungsmodell erstellen.
2.  Die empfohlene verbraucht. Nach Erstellung des Modells können Sie die empfohlenen verbrauchen. (Lesen Sie dieses Dokument nicht wie ein Modell erläutert Schnellstarthandbuch für Weitere Informationen zu erhalten).


<ins>Phase I</ins>

In der ersten Phase fügen Sie HTML-Seiten eine kleine JavaScript-Bibliothek, die die Seite Ereignisse senden sie der HTML-Seite in Azure ML Recommendations Server (über Daten) auftreten kann:

![Zeichnung1][1]

<ins>Phase II</ins>

In der zweiten Phase, wenn die Informationen auf der Seite angezeigt werden soll, wählen Sie eine der folgenden Optionen:

1. Server (in der Phase der Seite) Ruft Vorschläge zu Azure ML Recommendations Server (über Daten). Die Ergebnisse enthalten eine Liste der Artikel-Id. Der Server muss die Ergebnisse mit den Metadaten (z. B. Bilder, Beschreibung) bereichern und Senden der Seite an den Browser.

![Drawing2][2]

2. die Option ist mit kleine JavaScript-Datei aus einer einfachen empfohlene Elemente aufzulisten. Die Daten hier sind schlanker als die erste Option.

![Drawing3][3]

##<a name="2-prerequisites"></a>2. erforderliche Komponenten

1. Erstellen Sie ein neues Modell mit den APIs. Informationen dazu finden Sie Quick Start Guide.
2. Codieren der &lt;DataMarketUser&gt;:&lt;DataMarketKey&gt; mit base64. (Dies wird für die Standardauthentifizierung verwendet werden JS Code die APIs aufrufen kann).


##<a name="3-send-data-acquisition-events-using-javascript"></a>3. senden Sie Datenerfassung Ereignisse mit JavaScript
Sendende von Ereignissen Vereinfachung der folgenden Schritte:

1.  JQuery-Bibliothek in Ihren Code aufnehmen. Sie können es von Nuget in die folgende URL herunterladen.

        http://www.nuget.org/packages/jQuery/1.8.2
2.  Das Empfehlungsskript Java Bibliothek über den folgenden URL: http://aka.ms/RecoJSLib1

3.  Azure ML Recommendations Bibliothek mit den entsprechenden Parametern zu initialisieren.

        <script>
            AzureMLRecommendationsStart("<base64encoding of username:key>",
            "<model_id>");
        </script>

4.  Das entsprechende Ereignis zu senden. Detaillierte Abschnitt unten auf alle Ereignisse (Beispiel klicken Sie auf Ereignis)

        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") {      
                        AzureMLRecommendationsEvent = [];
                    }
            AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" });
        </script>


###<a name="31-limitations-and-browser-support"></a>3.1. Grenzen und Browserunterstützung
Dies ist eine referenzimplementierung und erfolgt ist. Es unterstützt alle gängigen Browser.

###<a name="32-type-of-events"></a>3.2. Ereignistypen
Gibt 5 das unterstützt Ereignis: klicken, Empfehlung, Hinzufügen zum Warenkorb, daraus Warenkorb und kaufen. Es ist ein weiteres Ereignis mit dem Namen Login festgelegt.

####<a name="321-click-event"></a>3.2.1. Klicken Sie auf Ereignis
Dieses Ereignis sollte ein Benutzer auf ein Element klicken verwendet werden. Wenn Benutzer auf ein Element klickt, wird eine neue Seite mit dem Element geöffnet. auf dieser Seite sollte dieses Ereignis ausgelöst werden.

Parameter:
- Ereignis (String, obligatorisch) - "hier klicken"
- Eindeutiger Bezeichner des Elements (String, obligatorisch) - Artikel
- Elementname (String, optional) – der Name des Elements
- ItemDescription (String, optional) - die Beschreibung des Artikels
- ItemCategory (String, optional) - Kategorie des Artikels
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Mit optionalen Daten:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


####<a name="322-recommendation-click-event"></a>3.2.2. Empfehlung Click-Ereignis
Dieses Ereignis sollte die ein Benutzer klicken auf ein Element Azure ML Recommendations als empfohlene Artikel erhielt verwendet werden. Wenn Benutzer auf ein Element klickt, wird eine neue Seite mit dem Element geöffnet. auf dieser Seite sollte dieses Ereignis ausgelöst werden.

Parameter:
- Ereignis (String, obligatorisch) - "Recommendationclick"
- Eindeutiger Bezeichner des Elements (String, obligatorisch) - Artikel
- Elementname (String, optional) – der Name des Elements
- ItemDescription (String, optional) - die Beschreibung des Artikels
- ItemCategory (String, optional) - Kategorie des Artikels
- Samen (Zeichenfolgenarray optional) - die Körner, die die Empfehlung Abfrage generiert.
- RecoList (Zeichenfolgenarray optional) - Ergebnis der Empfehlung, die das Element generiert, auf die geklickt wurde.
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Mit optionalen Daten:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]               });
        </script>


####<a name="323-add-shopping-cart-event"></a>3.2.3. Shopping Cart-Ereignis hinzufügen
Dieses Ereignis sollte verwendet werden Wenn Benutzer ein Element dem Einkaufskorb hinzufügen.
Parameter:
* Ereignis (String, obligatorisch) - "Addshopcart"
* Eindeutiger Bezeichner des Elements (String, obligatorisch) - Artikel
* Elementname (String, optional) – der Name des Elements
* ItemDescription (String, optional) - die Beschreibung des Artikels
* ItemCategory (String, optional) - Kategorie des Artikels
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

####<a name="324-remove-shopping-cart-event"></a>3.2.4. Entfernen Sie Shopping Cart Ereignis
Dieses Ereignis sollte verwendet werden, wenn der Benutzer ein Element in den Warenkorb entfernt.

Parameter:
* Ereignis (String, obligatorisch) - "Removeshopcart"
* Eindeutiger Bezeichner des Elements (String, obligatorisch) - Artikel
* Elementname (String, optional) – der Name des Elements
* ItemDescription (String, optional) - die Beschreibung des Artikels
* ItemCategory (String, optional) - Kategorie des Artikels
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

####<a name="325-purchase-event"></a>3.2.5. Einkauf-Ereignis
Dieses Ereignis sollte verwendet werden, wenn der Benutzer seinen Warenkorb erworben.

Parameter:
* Ereignis (String) - "kaufen"
* bestellten ([]) - Array hält einen Eintrag für jeden Artikel gekauft.<br><br>
Gekaufte Format:
    * Element (String) - eindeutige Kennung des Artikels.
    * Anzahl (Int oder Zeichenfolge) der eingekauften Artikel.
    * Preis (Float oder Zeichenfolge) optionales Feld - Preis des Artikels.

Das folgende Beispiel zeigt die Bestellung 3 Elemente (33, 34, 35) zwei und alle Felder (Artikel, Anzahl, Preis) (Artikel 34) Preis.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

####<a name="326-user-login-event"></a>3.2.6. Benutzer-Login-Ereignis
Azure ML Recommendations Ereignis Bibliothek erstellt und einen Cookie verwenden, um Ereignisse zu identifizieren, die den gleichen Browser stammt. Verbesserung der Ergebnisse Azure ML Vorschläge können eine eindeutige Kennung für Benutzer festlegen, die die Verwendung von Cookies überschreiben.

Dieses Ereignis sollte nach der Benutzername Ihrer Site verwendet werden.

Parameter:
* Ereignis (String) - "Userlogin"
* Benutzer (String) - Identifikation des Benutzers.
        <script>
  Wenn (Typeof AzureMLRecommendationsEvent == "undefined") {AzureMLRecommendationsEvent = [;]} AzureMLRecommendationsEvent.push ({Ereignis: "Userlogin" Benutzer: "ABCD10AA"});      </script>

##<a name="4-consume-recommendations-via-javascript"></a>4. verwenden Sie Vorschläge über JavaScript
Der Code, der die Empfehlung ist der Client-Seite durch ein JavaScript-Ereignis ausgelöst. Die Empfehlung Antwort enthält empfohlene Elemente Ids, ihren Namen und ihre Bewertung. Es empfiehlt sich, diese Option nur für eine Liste an der empfohlenen Artikel - komplexere Verarbeitung (wie das Element Metadaten hinzufügen) auf der Seite Serverintegration erfolgen.

###<a name="41-consume-recommendations"></a>4.1 empfohlene verbraucht
Zur Nutzung einschließen Empfehlung müssen Sie die erforderlichen JavaScript-Bibliotheken in Ihrer Seite und AzureMLRecommendationsStart Siehe Abschnitt 2.

Nutzen Sie Vorschläge für ein oder mehrere Elemente müssen Sie eine Methode aufrufen: AzureMLRecommendationsGetI2IRecommendation.

Parameter:
* Elemente (Zeichenfolgenarray) - ein oder mehrere Rahmen zu. Wenn Sie einen Fbt-Build verwenden, können Sie hier nur ein Element festlegen.
* NumberOfResults (Int) - Anzahl der gewünschten Ergebnisse.
* IncludeMetadata (Boolean, optional) - Wenn auf "True" gibt an, dass das Metadatenfeld im Ergebnis gefüllt werden muss.
* Verarbeitungsfunktion - Funktion, die die zurückgegebenen Informationen behandelt. Die Daten werden als Array zurückgegeben:
    * Artikel - Element eindeutige id
    * Name - Element Name (wenn im Katalog vorhanden)
    * Bewertung - Empfehlung Bewertung
    * Metadaten - eine Zeichenfolge, die Metadaten des Elements

Beispiel: Der folgende Code fordert 8 Vorschläge für Element "64f6eb0d-947a-4c18-a16c-888da9e228ba" (und nicht IncludeMetadata - es implizit besagt, dass keine Metadaten erforderlich ist), dann verketten die Ergebnisse in einen Puffer.

        <script>
            var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                var buff = "";
                for (var ii = 0; ii < reco.length; ii++) {
                    buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                }
                alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
 
