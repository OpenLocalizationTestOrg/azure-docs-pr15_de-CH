<properties
    pageTitle="Skalierung Media Verarbeitung Übersicht | Microsoft Azure"
    description="Dieses Thema bietet eine Übersicht über Skalierung Medien mit Azure Media Services verarbeiten."
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
    ms.date="08/29/2016"
    ms.author="juliako"/>


# <a name="scaling-media-processing-overview"></a>Skalierung Verarbeitung von Medien (Übersicht)

Diese Seite gibt wie und warum Media Verarbeitung skalieren. 

## <a name="overview"></a>Übersicht

Media Services-Konto ist mit einem reservierten Einheitentyp, bestimmt die Geschwindigkeit mit der Medien Aufgaben verarbeitet werden. Sie können zwischen folgenden reservierten Einheit auswählen: **S1**, **S2**oder **S3**. Beispielsweise schneller gleichen Codierungsauftrags Wenn **S2** reserviert Einheit Typ vergleichen **S1** -Typ verwenden. Weitere Informationen finden Sie unter [Reservierte Einheitentypen](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Neben den reservierten Einheitentyp, können Sie Ihr Konto mit reservierten bereitstellen. Die Anzahl der bereitgestellten reservierten Einheiten bestimmt die Anzahl von Media-Aufgaben, die in ein Konto gleichzeitig verarbeitet werden können. Beispielsweise wenn Ihr Konto fünf reservierte Einheiten hat fünf Aufgaben gleichzeitig lang ausgeführt werden gibt als Aufgaben verarbeitet werden. Die übrigen Aufgaben warten in der Warteschlange und für nacheinander verarbeitet, wenn eine aktive Aufgabe abgeschlossen ist abgeholt erhalten. Wenn ein Konto nicht reservierten Einheiten bereitgestellt haben, werden dann Aufgaben nacheinander abgeholt. In diesem Fall hängen die Wartezeit zwischen einer Aufgabe beenden und die nächste Verfügbarkeit der Ressourcen im System.

## <a name="choosing-between-different-reserved-unit-types"></a>Auswählen zwischen verschiedenen reservierten Einheiten

In der folgenden Tabelle können Sie entscheiden bei der Wahl zwischen Codierung Geschwindigkeiten. Außerdem bietet Benchmark wenige und bietet SAS-URLs, mit denen Sie Videos herunterladen auf die eigene Tests durchführen können:

Szenarien|**S1**|**S2**|**S3**|
----------|------------|----------|------------
Beabsichtigte Verwendung| Einfache Auswahl. <br/>Dateien auf SD oder unter Auflösung, Zeit nicht vertrauliche, niedrige Kosten.|Einzelne Bitrate und mehrere Bitrate codieren.<br/>Normale Verwendung für SD und HD-Codierung. |Einzelne Bitrate und mehrere Bitrate codieren.<br/>Voller HD und 4K Auflösung Videos. Zeit vertrauliche, schnellere Abwicklung Codierung. 
Benchmark|[Eingabedatei: 5 Minuten lang 640x360p in 29,97 Frames pro Sekunde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Codierung in einer einzigen Bitrate MP4-Datei mit der gleichen Auflösung dauert ca. 11 Minuten.|[Eingabedatei: 5 Minuten lang 1280x720p in 29,97 Frames pro Sekunde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>Kodierung mit "H264 einzelne Bitrate 720p" Voreinstellung dauert ca. 5 Minuten.<br/><br/>Kodierung mit "H264 mehrere Bitrate 720p" Vorgabe Minuten etwa 11,5.|[Eingabedatei: 5 Minuten lang 1920x1080p in 29,97 Frames pro Sekunde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>Kodierung mit "H264 einzelne Bitrate 1080p" Voreinstellung verwendet 2.7 Minuten.<br/><br/>Kodierung mit "H264 mehrere Bitrate 1080p" Vorgabe Minuten ca. 5,7.

##<a name="considerations"></a>Hinweise

>[AZURE.IMPORTANT] Überprüfen Sie in diesem Abschnitt beschriebenen Aspekte.  

- Reservierte Einheiten arbeiten alle medienverarbeitung mit Azure Media Indexer die Indizierung parallelisieren.  Aber im Gegensatz zur Codierung Indizierung Aufträge nicht schneller mit schneller reservierte verarbeitet.

- Verwenden den freigegebenen Pool haben, ohne alle reservierten Einheiten dann codieren Aufgaben dieselbe Leistung mit S1 RUs. Allerdings gibt es keine Obergrenze Zeit Ihre Aufgaben können in Warteschlange und zu jedem Zeitpunkt nur einer Aufgabe ausgeführt werden.

- Die folgenden Rechenzentren bieten nicht den Einheitentyp **S2** reserviert: Süden Brasiliens, Indien West Central Indien und Indien Süd.

- Die folgenden Rechenzentren bieten nicht den Einheitentyp **S3** reserviert: Brasilien Süd Indien West Indien Central.

- Höchste Anzahl von Einheiten für 24 Stunden wird bei der Berechnung der Kosten verwendet.


##<a name="quotas-and-limitations"></a>Kontingente und Einschränkungen

Informationen zu Kontingenten und Grenzen und ein Support-Ticket finden Sie unter [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).

##<a name="next-step"></a>Nächstes

Erreichen Sie Skalierung Media-Aufbereitungstask mit dieser Technologie: 

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [Portal](media-services-portal-scale-media-processing.md)
- [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
