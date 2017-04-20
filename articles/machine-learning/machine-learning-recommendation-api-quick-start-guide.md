<properties 
    pageTitle="Schnellstart-Handbuch: Computer lernen empfohlene API | Microsoft Azure" 
    description="Azure maschinelles lernen Recommendations - Quick Start-Handbuch" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-machine-learning-recommendations-api"></a>Schnellstart-Handbuch für Computer lernen empfohlene API

>[AZURE.NOTE]Sie sollten diese Version anstelle des empfohlenen API kognitive starten. Empfohlene kognitive Service wird dieser Dienst ersetzt und alle neuen Features es entwickelt. Es verfügt über neue Funktionen wie Batchverarbeitung Support besser API Explorer eine sauberere API-Oberfläche und Anmeldung/Abrechnung Erfahrung, usw..
> Weitere Informationen zu [neuen kognitive Dienst migrieren](http://aka.ms/recomigrate)


Dieses Dokument beschreibt, wie zu integrierten Ihrem Dienst oder Anwendung Microsoft Azure Machine Learning Recommendations. Weitere finden empfohlene API in der [Galerie](http://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2)Sie.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="general-overview"></a>Übersicht

Um Azure Machine Learning Recommendations verwenden, müssen Sie die folgenden Schritte ausführen:

* Erstellen eines Modells - Modell ist ein Container für die Daten und Katalogdaten empfehlungsmodell.
* Katalogdaten importieren - Katalog enthält Metadaten für die Elemente. 
* 2 Arten (oder beides) können Verwendungsdaten Import - Daten hochgeladen werden:
    * Hochladen einer Datei, die Daten enthält.
    * Durch Übernahme Ereignisse senden.
    Normalerweise laden Sie eine Verwendung um eine anfängliche empfehlungsmodell (bootstrap) und bis System Übernahme Datenformat mit genügend Daten erfasst.
* Empfehlung Modell – Dies ist ein asynchroner Vorgang empfehlungssystem übernimmt alle Daten und erstellt eine Empfehlung. Dieser Vorgang dauert einige Minuten oder mehrere Stunden dauern, je nach Größe der Daten und die Build-Konfigurationsparameter. Beim Auslösen des Builds erhalten Sie Build-ID. Verwenden Sie diese überprüfen, wenn der Buildprozess beendet wurde, vor Recommendations verwenden.
* Verwenden Sie Empfehlung - Get Vorschläge für ein bestimmtes Element oder eine Liste von Elementen.

Alle Schritte werden über Azure Machine Learning empfohlene API ausgeführt.  Sie können eine beispielanwendung mit jedem dieser Schritte aus der [Gallerie.](http://1drv.ms/1xeO2F3)

##<a name="limitations"></a>Grenzen

* Die maximale Anzahl der Modelle pro Abonnement ist 10.
* Die maximale Anzahl von Elementen, die ein Katalog enthalten kann ist 100.000.
* Maximal bleiben Verwendung-Punkt ist ~ 5.000.000. Die älteste gelöscht Wenn neue hochgeladen oder gemeldet werden.
* Die maximale Größe der Daten (z. B. Katalogdaten importieren Daten importieren) POST gesendet werden können beträgt 200MB.
* Die Anzahl der Transaktionen pro Sekunde für eine Empfehlung Modellbuild nicht aktiven ~ 2TPS. Ein Empfehlung Modellbuild aktiv hält, 20TPS.

##<a name="integration"></a>Integration

###<a name="authentication"></a>Authentifizierung
Microsoft Azure Marketplace unterstützt die Basic- oder OAuth Authentifizierungsmethode. Kontoschlüssel finden problemlos den Tasten auf dem Markt unter [Account Settings](https://datamarket.azure.com/account/keys)navigieren. 
####<a name="basic-authentication"></a>Standardauthentifizierung
Den Authorization-Header hinzufügen:

    Authorization: Basic <creds>
               
    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  
    
Konvertieren in Base64 (C#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);
    
Konvertieren in Base64 (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);
    



###<a name="service-uri"></a>Dienst-URI
Service-Stamm-URI Azure Machine Learning Recommendations APIs ist [hier.](https://api.datamarket.azure.com/amla/recommendations/v2/)

Der vollständige Dienst-URI wird mit Elementen der OData-Spezifikation angegeben.

###<a name="api-version"></a>API-version
Jede API-Aufruf haben, am Ende einen Abfrageparameter aufgerufen Anforderungsparameter "1.0" festgelegt werden soll.

###<a name="ids-are-case-sensitive"></a>IDs Groß-/Kleinschreibung
Durch die APIs zurückgegebenen iDs Groß-/Kleinschreibung und sollte als solche verwendet, wenn als Parameter in weiteren API-Aufrufe übergeben. Modell-IDs und Katalog-IDs sind z. B. Groß-/Kleinschreibung beachtet.

###<a name="create-a-model"></a>Modell erstellen
Erstellen eine Anforderung "Modell erstellen":

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27`|

|   Parametername  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelName   |   Nur Buchstaben (A-Z, a-Z), Zahlen (0-9), Bindestriche (-) und Unterstrich (_) sind zulässig.<br>Maximallänge: 20 |
|   Anforderungsparameter      | 1.0 |
|||
| Anfragetext | KEINE |


**Antwort**:

HTTP-Statuscode: 200

- `feed/entry/content/properties/id`-Enthält die Modell-ID.
**Hinweis**: Modell-ID wird Groß-/Kleinschreibung beachtet.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


###<a name="import-catalog-data"></a>Katalogdaten importieren

Wenn Sie das gleiche Modell mit mehreren mehrere Katalogdateien hochladen, fügen wir nur neue Katalogelemente. Vorhandene Elemente bleiben die ursprünglichen Werte.

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Parametername  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung)  |
| Dateiname | Text-ID des Katalogs.<br>Nur Buchstaben (A-Z, a-Z), Zahlen (0-9), Bindestriche (-) und Unterstrich (_) sind zulässig.<br>Maximallänge: 50 |
|   Anforderungsparameter      | 1.0 |
|||
| Anfragetext | Katalogdaten. Format:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>Name</th><th>Obligatorisch</th><th>Typ</th><th>Beschreibung</th></tr><tr><td>Artikelnummer</td><td>Ja</td><td>Alphanumerisch, maximale Länge 50</td><td>Eindeutige Kennung eines Elements</td></tr><tr><td>Artikelname</td><td>Ja</td><td>Alphanumerisch, maximale Länge von 255 Zeichen</td><td>Artikelname</td></tr><tr><td>Kategorie</td><td>Ja</td><td>Alphanumerisch, maximale Länge von 255 Zeichen</td><td>Kategorie, der dieses Element (z. B. Kochen Büchern, Filmen...) gehört</td></tr><tr><td>Beschreibung</td><td>Nein</td><td>Alphanumerisch, Max. Länge 4000</td><td>Beschreibung</td></tr></table><br>Die maximale Dateigröße beträgt 200MB.<br><br>Beispiel:<br><pre>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan Buch<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, den Raum zu vergessen: Fiction (Byzanz Buch), Buch<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Vorarbeiten, Buch<br>552a1940-21e4-4399-82bb-594b46d7ed54, Ruhigstellen der Tiere, Buch</pre> |


**Antwort**:

HTTP-Statuscode: 200

- `Feed\entry\content\properties\LineCount`-Zeilenanzahl akzeptiert.
- `Feed\entry\content\properties\ErrorCount`-Anzahl der Zeilen, die nicht aufgrund eines Fehlers eingefügt wurden.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
        <subtitle type="text">Import catalog file</subtitle>
        <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
     </entry>
    </feed>


###<a name="import-usage-data"></a>Daten importieren

####<a name="uploading-a-file"></a>Hochladen einer Datei
In diesem Abschnitt zeigt, wie Daten mithilfe einer Datei hochladen. Sie können diese API mit Daten aufrufen. Alle Daten werden für alle gespeichert.

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Parametername  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung) |
| Dateiname | Text-ID des Katalogs.<br>Nur Buchstaben (A-Z, a-Z), Zahlen (0-9), Bindestriche (-) und Unterstrich (_) sind zulässig.<br>Maximallänge: 50 |
|   Anforderungsparameter      | 1.0 |
|||
| Anfragetext | Daten. Format:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Name</th><th>Obligatorisch</th><th>Typ</th><th>Beschreibung</th></tr><tr><td>Benutzer-Id</td><td>Ja</td><td>Alphanumerische</td><td>Eindeutiger Bezeichner des Benutzers</td></tr><tr><td>Artikelnummer</td><td>Ja</td><td>Alphanumerisch, maximale Länge 50</td><td>Eindeutige Kennung eines Elements</td></tr><tr><td>Zeit</td><td>Nein</td><td>Datum im Format: JJJJ/MM/TTThh (z.B. 2013/06/20T10:00:00)</td><td>Daten</td></tr><tr><td>Ereignis</td><td>Nein, wenn angegeben, dann muss auch Datum einfügen</td><td>Einer der folgenden:<br>• Klicken Sie auf<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Kauf</td><td></td></tr></table><br>Die maximale Dateigröße beträgt 200MB.<br><br>Beispiel:<br><pre>149452 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951 1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Antwort**:

HTTP-Statuscode: 200

- `Feed\entry\content\properties\LineCount`-Zeilenanzahl akzeptiert.
- `Feed\entry\content\properties\ErrorCount`-Anzahl der Zeilen, die nicht aufgrund eines Fehlers eingefügt wurden.
- `Feed\entry\content\properties\FileId`-ID-Datei.


OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="using-data-acquisition"></a>Verwendung von Daten
Dieser Abschnitt zeigt, wie Ereignisse in Echtzeit an Azure Machine Learning Recommendations normalerweise von Ihrer Website senden.

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Parametername  |   Gültige Werte                        |
|:--------          |:--------                              |
|   Anforderungsparameter      | 1.0 |
|||
|Anfragetext| Ereignis Dateneingabe für jedes Ereignis, das Sie senden möchten. Sie sollten für denselben Benutzer oder Browser-Sitzung die gleiche ID im Feld SessionId senden. (Siehe Beispiel unten Ereignistext.)|


- Beispiel für Ereignis "Click":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Beispiel für Ereignis 'RecommendationClick':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RecommendationClick</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Beispiel für Ereignis 'AddShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Beispiel für Ereignis 'RemoveShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RemoveShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Beispiel für Ereignis 'Kaufen':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>

- Beispiel '2 Ereignisse auf' und 'AddShopCart' senden:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        <ItemName>itemName</ItemName>
        <ItemDescription>item description</ItemDescription>
        <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
        </EventData>
        </Event>

**Antwort**: HTTP-Statuscode: 200

###<a name="build-a-recommendation-model"></a>Empfehlung Modell

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|

|   Parametername  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung)  |
| userDescription | Text-ID des Katalogs. Beachten Sie, dass bei Verwendung von Leerzeichen durch %20 stattdessen codieren müssen. Beispiel anzeigen<br>Maximallänge: 50 |
| Anforderungsparameter | 1.0 |
|||
| Anfragetext | KEINE |

**Antwort**:

HTTP-Statuscode: 200

Dies ist eine asynchrone API. Sie erhalten eine Build-ID als Antwort. Um zu wissen, wann der Build abgeschlossen wurde, und rufen Sie die API "Get erstellt Status der ein Modell" Diese Build-ID in der Antwort gefunden. Beachten Sie, dass ein Build Minuten Stunden, abhängig von der Datenmenge kann.

Empfohlene bis Build kann nicht nutzen endet.

Gültige Buildstatus:

- Erstellen – Modell erstellt wurde.
- – Modellbuild ausgelöst wurde und wird in die Warteschlange aufgenommen.
- Gebäude-Modell wird erstellt.
- Erfolg-Build erfolgreich beendet.
- Fehler-Build mit einem Fehler beendet.
- Storniert – wurde Build abgebrochen.
- Abbrechen – Build abgebrochen wird.


Beachten Sie, dass die Build-ID unter dem folgenden Pfad:`Feed\entry\content\properties\Id`

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
    </entry>
    </feed>

###<a name="get-build-status-of-a-model"></a>Erstellen des Status eines Modells

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|



|   Parametername  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId         |   Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung)    |
|   onlyLastBuild   |   Gibt an, ob der Build Geschichte des Modells oder nur den Status des letzten Builds zurück. |
|   Anforderungsparameter      |   1.0                                 |


**Antwort**:

HTTP-Statuscode: 200

Die Antwort enthält einen Eintrag pro erstellen. Jeder Eintrag enthält die folgenden Daten:

- `feed/entry/content/properties/UserName`Name des Benutzers.
- `feed/entry/content/properties/ModelName`Name des Modells.
- `feed/entry/content/properties/ModelId`-Eindeutiger Bezeichner Modell.
- `feed/entry/content/properties/IsDeployed`– Gibt an, ob der Build (auch bekannt als bereitgestellt aktive Build).
- `feed/entry/content/properties/BuildId`– Erstellen Sie eindeutigen Bezeichner.
- `feed/entry/content/properties/BuildType`-Typ des Builds.
- `feed/entry/content/properties/Status`– Erstellen Sie Status. Kann eine der folgenden: Fehler erstellen, in Warteschlange, Abbrechen, storniert, Erfolg
- `feed/entry/content/properties/StatusMessage`– Detaillierte Statusnachricht (gilt nur für bestimmte Status).
- `feed/entry/content/properties/Progress`– Status (%) erstellen.
- `feed/entry/content/properties/StartTime`– Erstellen Sie Startzeit.
- `feed/entry/content/properties/EndTime`– Erstellen Sie endet.
- `feed/entry/content/properties/ExecutionTime`– Erstellen Sie Dauer.
- `feed/entry/content/properties/ProgressStep`– Informationen über die aktuelle Phase ein Buildvorgang ausgeführt wird.

Gültige Buildstatus:
- – Wurde Build Anforderungseintrag erstellt.
- – Buildanforderung ausgelöst wurde und wird in die Warteschlange aufgenommen.
- Gebäude erstellen wird.
- Erfolg-Build erfolgreich beendet.
- Fehler-Build mit einem Fehler beendet.
- Storniert – wurde Build abgebrochen.
- Abbrechen – Build abgebrochen wird.

Gültige Werte für Buildtyp:
- Rank - Rang erstellen. (Rang erstellen Details, siehe das Dokument "Machine Learning Empfehlung API-Dokumentation")
- Empfehlung - Empfehlung erstellen.
- FBT - häufig zusammen gekauft erstellen.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


###<a name="get-recommendations"></a>Lassen Sie sich

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|



|   Parametername  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung) |
| Artikelnummern ein. | Durch Kommas getrennte Liste der Elemente zu empfehlen.<br>Maximallänge: 1024 |
| numberOfResults | Anzahl der gewünschten Ergebnisse |
| includeMetatadata | Zukünftige Verwendung immer false |
| Anforderungsparameter | 1.0 |

**Antwort:**

HTTP-Statuscode: 200

Die Antwort enthält einen Eintrag pro empfohlen. Jeder Eintrag enthält die folgenden Daten:

- `Feed\entry\content\properties\Id`-Empfohlene Element-ID.
- `Feed\entry\content\properties\Name`-Name des Elements.
- `Feed\entry\content\properties\Rating`-Bewertung der Empfehlung; höhere Nummer bedeutet mehr vertrauen.
- `Feed\entry\content\properties\Reasoning`-Empfehlung Begründung (z.B. Empfehlung Erläuterung).

OData-XML

Die Antwort wird unten gehören 10 empfohlen:

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="update-model"></a>Aktualisierungsmodell
Sie können die Beschreibung oder die aktive Build-ID
*Aktive Buildkonfiguration ID* - jeden Build für jedes Modell hat eine Build-ID. Die aktive Buildkonfiguration ID ist der ersten erfolgreichen Build jedes neue Modell. Sobald Sie eine aktive Buildkonfiguration ID und Sie zusätzliche erstellt für das gleiche Modell, müssen Sie es als Standard-Build-ID festlegen möchten. Wenn Sie Vorschläge, verbrauchen, wenn Sie nicht die Build-ID angeben, die Sie verwenden möchten, wird der Standard automatisch verwendet.

Dieser Mechanismus ermöglicht - haben Sie eine empfehlungsmodell in der Produktion – neue Modelle erstellen und Testen sie vor dem Heraufstufen Produktion.

| HTTP-Methode | URI |
|:--------|:--------|
|EINFÜGEN     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|


|   Parametername  |   Gültige Werte                        |
|:--------          |:--------                              |
| ID | Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung) |
| Anforderungsparameter | 1.0 |
|||
| Anfragetext | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Beachten Sie, dass die XML-Tags, Beschreibung und ActiveBuildId optional. Wenn nicht Beschreibung oder ActiveBuildId festgelegt werden soll, entfernen Sie das gesamte Tag. |

**Antwort**:

HTTP-Statuscode: 200

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Update an Existing Model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
    <rights type="text" />
     <updated>2014-10-05T10:27:17Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

##<a name="legal"></a>Rechtliche
Dieses Dokument wird bereitgestellt "als-ist". Informationen und Ansichten, die in diesem Dokument, einschließlich URLs und anderer Verweise auf Internetwebsites, können ohne Vorankündigung ändern. Einige der genannten Beispiele dienen nur zur Veranschaulichung und sind fiktiv. Keine wirkliche Zuweisung oder Verbindung ist oder ableitbar. Dieses Dokument bietet keinen keinerlei Rechte an geistigem Eigentum in einem Microsoft-Produkt. Sie kopieren und verwenden Sie dieses Dokument zu internen Referenzzwecken. © Microsoft 2014. Alle Rechte vorbehalten. 
 
