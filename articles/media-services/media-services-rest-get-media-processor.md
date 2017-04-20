<properties 
    pageTitle="Media-Prozessor erstellen | Microsoft Azure" 
    description="Informationen Sie zum Erstellen einer Media Prozessorkomponente zum Codieren, Format konvertieren, verschlüsseln und Entschlüsseln von Medieninhalten für Azure Media Services." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="how-to-get-a-media-processor-instance"></a>Gewusst wie: eine Medien-Prozessor-Instanz abrufen


> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [REST](media-services-rest-get-media-processor.md)

##<a name="overview"></a>Übersicht

Media Services ist ein Medienprozessor eine Komponente, die eine bestimmte Verarbeitungsaufgabe Codierung behandelt eine Formatumwandlung durchgeführt, verschlüsseln oder Entschlüsseln von Medien-Content. Media-Prozessor wird normalerweise beim Erstellen einer Aufgabe codieren, verschlüsseln oder konvertieren Sie das Format der Medieninhalte erstellen.

Die folgende Tabelle enthält die Namen und eine Beschreibung jeder Prozessor verfügbaren Medien.

Medienname Prozessor|Beschreibung|Weitere Informationen
---|---|---
Media Encoder Standard|Bei Bedarf Codierung bereit standard Funktionen. |[Vergleich von Azure auf Anforderung Media Encoder](media-services-encode-asset.md)
Media Encoder Premium-Workflow|Können Sie mit Media Encoder Premium-Workflow Codierungsaufgaben ausführen.|[Vergleich von Azure auf Anforderung Media Encoder](media-services-encode-asset.md)
Azure Media Indexer| Können Sie Dateien und Inhalte durchsucht werden, sowie Generieren von geschlossenen Untertiteln Titel und Schlüsselwörter.|[Azure Media Indexer](media-services-index-content.md)
Azure Media Hyperlapse (Vorschau)|Ermöglicht das "Beulen" im Video mit video Stabilisierung zu glätten. Auch können Sie den Inhalt in konsumierbare Clips beschleunigen.|[Azure Media Hyperlapse](media-services-hyperlapse-content.md)
Azure Media Encoder|Abgeschrieben
Speicher-Entschlüsselung| Abgeschrieben|
Azure Media Manager|Abgeschrieben|
Azure Media Encryptor|Abgeschrieben|

##<a name="get-mediaprocessor"></a>MediaProcessor abrufen

>[AZURE.NOTE] Beim Arbeiten mit Media Services REST-API berücksichtigen die folgenden Aspekte:
>
>Wenn Entitäten in Media Services zugreifen, müssen Sie bestimmten Feldern und Werten in der HTTP-Anfragen festlegen. Weitere Informationen finden Sie unter [Setup für Media Services REST API-Entwicklung](media-services-rest-how-to-use.md).

>Nach dem erfolgreichen Verbindungsaufbau zu https://media.windows.net erhalten Sie eine 301-Umleitung einer anderen Media Services URI angegeben. Nachfolgende Aufrufe der neuen URI Siehe [Herstellen einer Verbindung mit Media Services REST-API verwenden](media-services-rest-connect-programmatically.md)müssen. 


Folgende weiteren Aufruf veranschaulicht eine Media Prozessorinstanz Namens (in diesem Fall **Media Encoder Standard**). 



    
Anforderung:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net
    
Antwort:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Nächste Schritte

Jetzt wissen Sie wie ein Media Prozessor Instanz, gehen Sie [das Codieren einer Anlage](media-services-rest-get-started.md) Thema der Verwendung von Media Encoder Standard zum Codieren einer Anlage angezeigt.
