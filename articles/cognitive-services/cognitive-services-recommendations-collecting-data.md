<properties
    pageTitle="Sammeln Daten, um Ihr Modell: maschinelles lernen empfohlene API | Microsoft Azure"
    description="Azure-Computer empfohlene Learning - sammeln Daten, um Ihr Modell"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="luisca"/>

#  <a name="collecting-data-to-train-your-model"></a>Sammeln von Daten auf Ihr Modell #

Die empfohlene API lernt aus Ihrem letzten Elemente finden für einen bestimmten Benutzer empfohlen.

Nachdem Sie ein Modell erstellt haben, müssen zwei Stück Informationen bereitstellen, bevor Sie alle trainieren: eine Katalogdatei und Daten.

>   Wenn Sie nicht bereits getan haben, empfehlen wir Ihnen die [Schnellstartübersicht](cognitive-services-recommendations-quick-start.md)abzuschließen.


## <a name="catalog-data"></a>Katalogdaten ##

### <a name="catalog-file-format"></a>Katalog-Dateiformat ###

Die Katalogdatei enthält Informationen über die Elemente, die Sie Ihrem Kunden anbieten.
Die Katalogdaten muss das folgende Format:

- Ohne-`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Features-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

#### <a name="sample-rows-in-a-catalog-file"></a>Beispielzeilen in einer Katalogdatei

Ohne:

    AAA04294,Office Language Pack Online DwnLd,Office
    AAA04303,Minecraft Download Game,Games
    C9F00168,Kiruna Flip Cover,Accessories

Funktionen:

    AAA04294,Office Language Pack Online DwnLd,Office,, softwaretype=productivity, compatibility=Windows
    BAB04303,Minecraft DwnLd,Games, softwaretype=gaming,, compatibility=iOS, agegroup=all
    C9F00168,Kiruna Flip Cover,Accessories, compatibility=lumia,, hardwaretype=mobile

#### <a name="format-details"></a>Format-details

| Name | Obligatorisch | Typ |  Beschreibung |
|:---|:---|:---|:---|
| Artikelnummer |Ja | [A-Z], [a-Z] [0-9] [_] & #40; Unterstrich & #41; [-] & #40; Strich & #41;<br> Maximallänge: 50 | Eindeutiger Bezeichner eines Elements. |
| Artikelname | Ja | Alphanumerischen Zeichen<br> Maximallänge: 255 | Name des Elements. |
| Kategorie | Ja | Alphanumerischen Zeichen <br> Maximallänge: 255 | Kategorie, der Artikel (z.B. Kochen Büchern, Filmen...) gehört. kann leer sein. |
| Beschreibung | Nein, wenn Features vorhanden (können aber leer sein) | Alphanumerischen Zeichen <br> Maximallänge: 4000 | Beschreibung des Elements. |
| Liste | Nein | Alphanumerischen Zeichen <br> Maximallänge: 4000; Max. Anzahl von Funktionen: 20 | Durch Kommas getrennte Liste von Funktionsnamen = Feature-Wert, der mit der Empfehlung Modell erweitern.|

#### <a name="uploading-a-catalog-file"></a>Eine Katalogdatei hochladen

[API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1) zum Hochladen einer betrachten.  

Beachten Sie, dass der Inhalt der Katalogdatei als Hauptteil der Anforderung übergeben werden soll.

Wenn Sie das gleiche Modell mit mehreren mehrere Katalogdateien hochladen, fügen wir nur neue Katalogelemente. Vorhandene Elemente bleiben die ursprünglichen Werte. Katalogdaten können nicht mit dieser Methode aktualisiert werden.

>   Hinweis: Die maximale Größe ist 200MB.
>   Die maximale Anzahl von Elementen im Katalog unterstützt ist 100.000.


## <a name="why-add-features-to-the-catalog"></a>Warum hinzufügen Features zum Katalog?

Empfohlene Engine erstellt ein statistisches Modell, das angibt, welche Elemente sind gefällt oder von einem Kunden erworbenes. Bei ist ein neues Produkt, das nie mit interagiert hat nicht möglich, ein Modell auf Co Vorkommen erstellen. Angenommen, Sie beginnen mit einer neuen "Kinder Violine" in Ihrem Shop, da Sie nie, Violine verkauft haben, bevor Sie, welche Elemente feststellen können, Violine empfehlen.

Das heißt, wenn das Modul Informationen, Violine weiß (heißt ein Musikinstrument für Kinder Alter 7 bis 10 ist, ist keine teure Violine usw.) und das Modul von anderen Produkten mit ähnlichen Funktionen erfahren. Beispielsweise verkauft Violine haben in der Vergangenheit und in der Regel die Violine kaufen neigen dazu, kaufen Sie Musik-CDs und Musik.  Das System kann diese Verbindungen zwischen den Funktionen und Vorschläge anhand der Features und neue Violine wenig Verwendung.

Funktionen werden als Teil der Katalogdaten importiert und dann ihrem Rang (oder die Bedeutung des KE im Modell) ist zugeordneten Rang erstellen. Feature Rang kann nach dem Muster der Daten und Objekte ändern. Aber für konsistente Verwendung/Artikel Rang sollten nur kleine Fluktuationen. Der Rang eines Features ist eine nicht Negative Zahl. Die Nummer 0 bedeutet, dass die Funktion nicht eingestuft wurde (Wenn diese API vor Abschluss des ersten Rang Build Aufrufen geschieht). Das Datum, an dem Rang zurückzuführen, ist frische Ergebnis aufgerufen.


###<a name="features-are-categorical"></a>Features sind kategorischen

Daher sollten Sie Funktionen erstellen, die eine Kategorie entsprechen. Beispielsweise Preis = 9.34 ist kein kategorischen Feature. Auf der anderen Seite wie eine Preisklasse = Under5Dollars wird eine Kategorieliste. Hat den Namen des Artikels als Funktion verwenden. Dadurch wird der Name eines Elements eindeutig sein, damit es keine Kategorie beschreiben. Sicherstellen Sie, dass die Funktionen Kategorien Elemente darstellen.


###<a name="how-manywhich-features-should-i-use"></a>Wie viele die Funktionen sollte ich verwenden?


Schließlich erstellen die Empfehlung unterstützt ein Modell mit bis zu 20. Konnte Sie Features höchstens 20 Elemente im Katalog zuweisen, jedoch dürften Rangfolge erstellen und die Features mit hohen Werten auswählen. (Ein Feature mit dem Rang mindestens 2.0 ist eine wirklich Funktion!). 


###<a name="when-are-features-actually-used"></a>Verwendet Funktionen werden als tatsächlich?

Funktionen werden vom Modell verwendet, wenn nicht genügend Buchungsdaten Buchungsinformationen allein Empfehlung abgeben. So haben Funktionen am stärksten auf "kalt Elemente" – mit einigen Transaktionen. Haben alle Elemente ausreichend Buchungsinformationen müssen Sie nicht mit Funktionen bereichern.


###<a name="using-product-features"></a>Verwenden der Produktmerkmale

Funktionen des Builds verwenden möchten:

1. Sicherstellen Sie, dass Ihr Katalog verfügt beim Hochladen.

2. Ranking Build ausgelöst. Dazu wird die Analyse Bedeutung Rang Features.

3. Auslösen eines Builds Recommendations, Festlegen der Folgendes Parameter erstellen: Legen Sie UseFeaturesInModel auf True, true, AllowColdItemPlacement und ModelingFeatureList sollten durch Kommas getrennte Liste der Features, die Sie zu Ihrem Modell verwenden möchten. Weitere Informationen finden Sie unter [Empfohlene Parameter erstellen](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d0) .





## <a name="usage-data"></a>Daten ##
Verwendung-Datei enthält Informationen wie diese Elemente verwendet werden oder die Transaktionen aus Ihrem Unternehmen.

#### <a name="usage-format-details"></a>Verwendungsdetails Format
Eine Verwendung ist CSV-Datei (durch Trennzeichen getrennte Wert), wobei jede Zeile einer Datei Verwendung eine Interaktion zwischen einem Benutzer und einem Element darstellt. Jede Zeile wird wie folgt formatiert:<br>
`<User Id>,<Item Id>,<Time>,[<Event>]`



| Name  | Obligatorisch | Typ | Beschreibung
|-------|------------|------|---------------
|Benutzer-Id|         Ja|[A-Z], [a-Z] [0-9] [_] & #40; Unterstrich & #41; [-] & #40; Strich & #41;<br> Maximallänge: 255 |Eindeutiger Bezeichner des Benutzers.
|Artikelnummer|Ja|[A-Z], [a-Z] [0-9] [& #95;] & #40; Unterstrich & #41; [-] & #40; Strich & #41;<br> Maximallänge: 50|Eindeutiger Bezeichner eines Elements.
|Zeit|Ja|Datum im Format: JJJJ/MM/TTThh (z.B. 2013/06/20T10:00:00)|Zeitpunkt der Daten.
|Ereignis|Nein | Einer der folgenden:<br>• Klicken Sie auf<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Kauf| Die Art der Buchung. |

#### <a name="sample-rows-in-a-usage-file"></a>Beispielzeilen in eine Verwendung

    00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase
    0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase
    0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase
    00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase
    0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase
    00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase

#### <a name="uploading-a-usage-file"></a>Hochladen einer Datei Verwendung

Betrachten Sie die [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2) für verwendete Dateien hochladen.
Beachten Sie, dass den Inhalt der Datei Verwendung als Hauptteil der HTTP-Aufruf übergeben.

>  Hinweis:

>  Maximale Dateigröße: 200MB. Sie können mehrere verwendete Dateien hochladen.

>  Sie müssen eine Katalogdatei hochladen, bevor Sie Daten zum Modell hinzufügen. Nur Elemente in der Katalogdatei werden während der Schulungsphase verwendet. Alle anderen Elemente werden ignoriert.

## <a name="how-much-data-do-you-need"></a>Wie viele Daten benötigen Sie?

Die Qualität des Modells ist stark von der Qualität und Quantität der Daten.
Das System lernt, wenn Benutzer verschiedene Artikel kaufen (Wir bezeichnen diese gemeinsam vorkommen). FBT-Builds unbedingt auch die Elemente in der gleichen Transaktionen erworben werden. 

Eine gute Faustregel soll mindestens 20 Transaktionen werden die meisten Elemente hätten Sie 10.000 Elemente im Katalog, wir empfehlen mindestens 20 Mal, Anzahl oder 200.000 Transaktionen haben. Wieder ist dies eine Faustregel. Sie müssen Ihre Daten experimentieren.

Nachdem Sie ein Modell erstellt haben, führen Sie [offline Bewertung](cognitive-services-recommendations-buildtypes.md) überprüfen, wie gut Ihr Modell führen dürfte.
