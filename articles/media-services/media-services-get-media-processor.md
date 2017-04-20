<properties 
    pageTitle="Media-Prozessor erstellen | Microsoft Azure" 
    description="Informationen Sie zum Erstellen einer Media Prozessorkomponente zum Codieren, Format konvertieren, verschlüsseln und Entschlüsseln von Medieninhalten für Azure Media Services. Code-Beispiele sind in C# geschrieben und Media Services SDK für .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
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

##<a name="get-media-processor"></a>Media Prozessor

Die folgende Methode veranschaulicht eine Medien-Prozessor-Instanz. Im Codebeispiel wird von der Verwendung der Variablen auf Modulebene **_context** auf den Serverkontext gemäß Abschnitt [wie: Verbinden mit Media Services programmgesteuert](media-services-dotnet-connect-programmatically.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
        return processor;
    }


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-steps"></a>Nächste Schritte

Jetzt wissen Sie wie ein Media Prozessor Instanz, gehen Sie [das Codieren einer Anlage](media-services-dotnet-encode-with-media-encoder-standard.md) Thema der Verwendung von Media Encoder Standard zum Codieren einer Anlage angezeigt.


