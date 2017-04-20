<properties 
    pageTitle="Übersicht über Media Services REST API | Microsoft Azure" 
    description="Media Services REST-API (Übersicht)" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako"/>


# <a name="media-services-rest-api-overview"></a>Media Services REST-API (Übersicht) 

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Microsoft Azure Media Services ist ein Dienst, der OData-basierte HTTP-Anforderungen akzeptiert und in ausführlichen JSON oder Atom + Pub reagieren können. Da Richtlinien Azure Media Services entspricht, gibt es eine Reihe von erforderlichen HTTP-Header, die jeder Client, beim Verbinden mit Media Services verwenden müssen sowie eine Reihe von optionalen Header verwendet werden kann. Die folgenden Abschnitte beschreiben die Header und HTTP-Verben Sie beim können Erstellen von Anfragen Media Services Antworten erhalten.

##<a name="considerations"></a>Hinweise 

Verwendung von REST ist Folgendes zu berücksichtigen.

- Beim Abfragen von Entitäten ist maximal 1000 Elemente gleichzeitig zurückgegeben werden, da Öffentliche REST v2 Abfrageergebnisse auf 1000 Ergebnisse begrenzt. Müssen Sie **Überspringen** und **nehmen** (.NET) **oben** (REST) als in [diesem Beispiel .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) und [dabei REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)beschrieben. 

- Wenn JSON und um das **__metadata** -Schlüsselwort in der Anforderung (z. B., verweist auf ein verknüpftes Objekt) müssen Sie **Accept** -Header auf [JSON ausführlichen Format](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) festlegen (siehe folgende Beispiel). OData versteht die **__metadata** -Eigenschaft in der Anforderung nicht auf ausführliche festlegen.  

        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
        
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 
        

## <a name="standard-http-request-headers-supported-by-media-services"></a>Standard-HTTP-Anforderungsheader von Media Services unterstützt

Für jeden Aufruf in Media Services machen, gibt es eine Reihe von erforderlichen Header in der Anforderung enthalten muss und auch eine Reihe von optionalen Header können sollen. Die folgende Tabelle enthält die erforderlichen Header:


Kopfzeile|Typ|Wert
---|---|---
Autorisierung|Träger|Träger ist der einzige akzeptierte Autorisierungsmechanismus. Der Wert muss außerdem von ACS gelieferte Zugriffstoken enthalten.
X-ms-version|Dezimal|2.11
DataServiceVersion|Dezimal|3.0
MaxDataServiceVersion|Dezimal|3.0



>[AZURE.NOTE] Da Media Services OData die zugrunde liegenden Ressourcenspeicher Metadaten über REST APIs verfügbar machen verwendet, sollten die DataServiceVersion und MaxDataServiceVersion-Header in jeder Anforderung enthalten; Allerdings sind sie nicht, übernimmt derzeit Media Services ist der Nutzungswert DataServiceVersion 3.0.

Im folgenden finden eine Reihe von optionalen Header:

Kopfzeile|Typ|Wert
---|---|---
Datum|RFC 1123 Datum|Timestamp der Anforderung
Akzeptieren|Inhaltstyp|Der angeforderte Inhaltstyp der Antwort wie die folgende:<p> -Application/Json; Odata = verbose<p> -Application/Atom + Xml<p> Antworten möglicherweise einen anderen Inhaltstyp wie ein Blob-Abruf, eine erfolgreiche Antwort BLOB-Stream als Nutzlast enthält.
Übernehmen-Codierung|GZIP, verkleinern|GZIP und KOMPRIMIERUNGSALGORITHMEN Codierung, falls zutreffend. Hinweis: Für große Ressourcen Media Services kann diesen Header ignoriert und nicht komprimierte Daten zurück.
Accept-Language|"En", "es" und So weiter.|Gibt die bevorzugte Sprache für die Antwort.
Charset akzeptieren|CharSet Typ "UTF-8"|Standardwert ist UTF-8.
X-HTTP-Methode|HTTP-Methode|Ermöglicht Clients oder Firewalls, die HTTP-Methoden wie setzen oder löschen diese Methoden Tunneling über einen GET-Aufruf nicht unterstützt.
Inhaltstyp|Inhaltstyp|Der Inhaltstyp des Anforderungstexts PUT oder POST-Anfragen.
Anfrage-id|Zeichenfolge|Ein vom Aufrufer definierten Wert, der die angegebene Anforderung bezeichnet. Wenn angegeben, wird dieser Wert in der Antwortnachricht als Möglichkeit zum Zuordnen der Anforderung enthalten. <p><p>**Wichtig**<p>Werte sollten auf 2096b begrenzt (2k).

## <a name="standard-http-response-headers-supported-by-media-services"></a>Standardmäßige HTTP-Antwortheader von Media Services unterstützt

Im folgenden finden eine Reihe von Headern, die möglicherweise zurückgegeben werden Ihnen die angeforderte Ressource und die Aktion, die ausgeführt werden soll.


Kopfzeile|Typ|Wert
---|---|---
Anfrage-id|Zeichenfolge|Ein eindeutiger Bezeichner für den aktuellen Arbeitsgang Service generiert.
Anfrage-id|Zeichenfolge|Ein Bezeichner, der Aufrufer in der ursprünglichen Anforderung angegeben, falls vorhanden.
Datum|RFC 1123 Datum|Das Datum, das die Anforderung verarbeitet wurde.
Inhaltstyp|Variiert|Der Inhaltstyp des Antworttexts.
Content-Encoding|Variiert|Gzip oder verkleinern entsprechend.


## <a name="standard-http-verbs-supported-by-media-services"></a>Standardmäßigen HTTP-Verben von Media Services unterstützt

Folgendes ist eine vollständige Liste der HTTP-Verben, die ausführenden HTTP-Anfragen verwendet werden können:


Verb|Beschreibung
---|---
Erhalten|Gibt den aktuellen Wert eines Objekts.
Bereitstellen|Erstellt ein Objekt basierend auf den Daten und sendet einen Befehl.
EINFÜGEN|Ersetzt ein Objekt oder ein benanntes Objekt (falls zutreffend) erstellt.
LÖSCHEN|Löscht ein Objekt.
ZUSAMMENFÜHREN|Aktualisiert ein vorhandenes Objekt mit benannten Eigenschaft ändert.
KOPF|Gibt die Metadaten eines Objekts für eine Antwort erhalten.

##<a name="limitation"></a>Einschränkung

Beim Abfragen von Entitäten ist maximal 1000 Elemente gleichzeitig zurückgegeben werden, da Öffentliche REST v2 Abfrageergebnisse auf 1000 Ergebnisse begrenzt. Müssen Sie **Überspringen** und **nehmen** (.NET) **oben** (REST) als in [diesem Beispiel .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) und [dabei REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)beschrieben. 


## <a name="discovering-media-services-model"></a>Erkennen von Media Services-Modell

Media Services Unternehmen leichter machen kann $metadata Vorgang verwendet werden. Dadurch werden alle gültigen Entitätstypen, Elementeigenschaften, Verbände, Funktionen, Aktionen usw. abzurufen. Das folgende Beispiel veranschaulicht die URI erstellen: https://media.windows.net/API/$ Metadaten.

Sollten anhängen "? version=2.x api" bis zum Ende des URI, wenn die Metadaten im Browser anzeigen oder X-ms-Version-Header in der Anforderung nicht enthalten.



##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





 
