<properties
pageTitle="Feld Mappings in Azure Search Indexer"
description="Konfigurieren von Azure Search Indexer Feld Zuordnung Unterschiede in Feldnamen und Darstellung von Daten"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/17/2016"
ms.author="eugenesh" />

# <a name="field-mappings-in-azure-search-indexers"></a>Feld Mappings in Azure Search Indexer

Bei Azure Search Indexer finden gelegentlich selbst Sie in Situationen, wo Eingabedaten Recht der Zielindex Schema übereinstimmt. In diesen Fällen können Sie **Feld Zuordnung** zum Transformieren der Daten in die gewünschte Form. 

Situationen, in dem Feld Zuordnung nützlich sind:
 
- Die Datenquelle enthält ein Feld `_id`, aber Azure Search nicht Feldnamen mit einem Unterstrich beginnen. Zuordnung für ein Feld können Sie ein Feld "Umbenennen". 
- Möchten Sie mehrere Felder in den Index mit denselben Quelldaten Daten beispielsweise auffüllen, da verschiedene Analysatoren auf Felder angewendet werden soll. Feld Zuordnung können Sie ein Feld der Datenquelle "Verzweigung".
- Base64 codiert oder entschlüsseln die Daten. Feld Zuordnung unterstützt mehrere **Zuordnungsfunktionen**Funktionen Base64-Codierung und Decodierung.   


> [AZURE.IMPORTANT] Feld Zuordnung Funktionalität ist in der Vorschau. Sie steht nur in der Version **2015-02-28-Vorschau**REST-API. Bitte denken Sie daran, Vorschau APIs für Test- und Evaluierungszwecke vorgesehen und sollte nicht in der Produktion verwendet werden.

## <a name="setting-up-field-mappings"></a>Einrichten von Feld Zuordnung

Feld Zuordnung können beim Erstellen eines neuen Indexers über [Indexer erstellen](search-api-indexers-2015-02-28-preview.md#create-indexer) API verwenden. Feld Zuordnung für einen Index mit dem [Update Indexer](search-api-indexers-2015-02-28-preview.md#update-indexer) API Indexer können verwaltet werden. 

Zuordnung für ein Feld besteht aus 3 Teilen: 

1. Ein `sourceFieldName`, der ein Feld in der Datenquelle darstellt. Diese Eigenschaft ist erforderlich. 

2. Eine optionale `targetFieldName`, das ein Feld im Suchindex darstellt. Wenn nicht angegeben, wird der gleiche Namen wie die Datenquelle verwendet. 

3. Eine optionale `mappingFunction`, das Transformieren von Daten mit mehreren können vordefinierte Funktionen. Die vollständige Liste der Funktionen ist [unten](#mappingFunctions).

Zuordnung der Felder hinzugefügt werden die `fieldMappings` Array für die Definition eines Indexers. 

Beispielsweise sieht wie Unterschiede in Feldnamen aufnehmen kann: 

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 
} 
```

Indexer können mehrere Feld Zuordnung. Z. B. hier, wie Sie "ein Feld verzweigen können":

```JSON

"fieldMappings" : [ 
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }, 
] 
```

> [AZURE.NOTE] Azure Suche verwendet und Kleinschreibung Feld und Funktion Namen im Feld Zuordnung auflösen. Dies ist bequem (Sie nicht die Schreibweise richtig) bedeutet, dass die Datenquelle oder Index Felder haben kann, die nur durch Fall unterscheiden  

<a name="mappingFunctions"></a>
## <a name="field-mapping-functions"></a>Zuordnung von Feldfunktionen

Diese Funktionen werden derzeit unterstützt: 

- [base64Encode](#base64EncodeFunction)
- [base64Decode](#base64DecodeFunction)
- [extractTokenAtPosition](#extractTokenAtPositionFunction)
- [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>
### <a name="base64encode"></a>base64Encode 

*URL-sichere* Base64-Codierung der Eingabezeichenfolge führt. Vorausgesetzt, dass die Eingabe UTF-8 kodiert ist. 

#### <a name="sample-use-case"></a>Beispiel-Anwendungsfall 

Nur URL-sichere Zeichen stehen in Azure Dokument Suchschlüssel (da Kunden an das Dokument z. B. API Suche mit dürfen). Wenn Daten URL-unsichere Zeichen enthält um ein Schlüsselfeld im Suchindex füllen möchten, verwenden Sie diese Funktion.   

#### <a name="example"></a>Beispiel 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Path", 
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" } 
  }] 
```

<a name="base64DecodeFunction"></a>
### <a name="base64decode"></a>base64Decode

Base64-Decodierung der Eingabezeichenfolge führt. Die Eingabe wird in einen *URL-sichere* Base64-codierte Zeichenfolge. 

#### <a name="sample-use-case"></a>Beispiel-Anwendungsfall 

BLOB-benutzerdefinierte Metadatenwerte müssen ASCII codiert sein. Sie können Base64-Codierung beliebige Unicode-Zeichenfolgen im Blob benutzerdefinierte Metadaten darstellen. Jedoch um Suche sinnvoll, können diese Funktion Sie die codierten Daten zurück zu "regular" Zeichenfolgen, sobald Suchindex auffüllen.  

#### <a name="example"></a>Beispiel 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Base64EncodedMetadata", 
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" } 
  }] 
```

<a name="extractTokenAtPositionFunction"></a>
### <a name="extracttokenatposition"></a>extractTokenAtPosition

Teilt ein Zeichenfolgenfeld mit dem angegebenen Trennzeichen und nimmt das Token an der angegebenen Position in der resultierenden teilen.

Beispielsweise ist die Eingabe `Jane Doe`, `delimiter` ist `" "`(Leerzeichen) und die `position` ist 0, ist das Ergebnis `Jane`; Wenn die `position` ist 1, das Ergebnis ist `Doe`. Wenn die Position mit einem Token, der nicht vorhanden ist verweist, wird ein Fehler zurückgegeben.

#### <a name="sample-use-case"></a>Beispiel-Anwendungsfall 

Die Datenquelle enthält einen `PersonName` und als zwei Separate indizieren möchten `FirstName` und `LastName` Felder. Diese Funktion können Sie die Eingabe mit Leerzeichen als Trennzeichen teilen.

#### <a name="parameters"></a>Parameter

- `delimiter`: eine Zeichenfolge, die als Trennzeichen verwendet, bei der Eingabezeichenfolge.
- `position`: eine ganzzahlige nullbasierte Position des Tokens nach dem Teilen der Eingabezeichenfolge auswählen.    

#### <a name="example"></a>Beispiel

```JSON 

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } } 
  }, 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } } 
  }] 
```

<a name="jsonArrayToStringCollectionFunction"></a>
### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection

Wandelt eine Zeichenfolge in ein Zeichenfolgenarray zum Füllen eines JSON-Arrays von Zeichenfolgen formatiert ein `Collection(Edm.String)` im Index. 

Wenn die Eingabezeichenfolge beispielsweise `["red", "white", "blue"]`, dann das Zielfeld des Typs `Collection(Edm.String)` mit drei Werten aufgefüllt `red`, `white` und `blue`. Für Eingabewerte, die als JSON Zeichenfolgenarrays analysiert werden können, wird ein Fehler zurückgegeben. 

#### <a name="sample-use-case"></a>Beispiel-Anwendungsfall

Azure SQL-Datenbank keinen integrierten Datentyps, die natürlich zuordnet `Collection(Edm.String)` Felder in Azure. Zum Auffüllen der Auflistung Zeichenfolgenfelder formatieren Sie Quelldaten als Zeichenfolgenarray JSON, und verwenden Sie diese Funktion. 

#### <a name="example"></a>Beispiel 

```JSON

"fieldMappings" : [ 
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
] 
```

## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns, Azure Suche zu verbessern.

Wenn Sie Wünsche oder Verbesserungsvorschläge haben, bitte erreichen Sie uns auf unserer [Website UserVoice](https://feedback.azure.com/forums/263029-azure-search/).