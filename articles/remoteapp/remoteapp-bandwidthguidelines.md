<properties 
    pageTitle="Azure RemoteApp Bandbreite - Richtlinien | Microsoft Azure"
    description="Kennen Sie einige grundlegende Bandbreite Richtlinien für Azure RemoteApp-Sammlungen und apps."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />
    
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Azure RemoteApp Bandbreite - Richtlinien (Wenn Sie Ihre eigenen nicht testen)

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Haben Sie nicht die Zeit oder die Fähigkeit zum Ausführen von [Netzwerktests Bandbreite](remoteapp-bandwidthtests.md) für Azure RemoteApp, sind hier einige sehr allgemeinen Richtlinien, mit denen Sie die Bandbreite pro Benutzer zu ermitteln.

Haben Sie eine Kombination dieser Szenarien nicht empfohlen etwas kleiner als (oder gleich) 10 MB/s als MINIMUM Netzwerkbandbreite für moderne Internetzugang apps in einer remote-Umgebung. (Obwohl wie bereits erwähnt, nicht besser sichergestellt wird als durchschnittliche Benutzer.)

## <a name="complex-powerpoint-simple-powerpoint"></a>Komplexe PowerPoint einfache PowerPoint

Azure RemoteApp ist am besten 100 MB LAN. 10 MB/s-Netzwerk-Profil wird Jitter über 120 ms mehr als 5 % sehen der Benutzer eine durchschnittliche Erfahrung. Bei 1 MBIT/s, die verschiedenen - beispielsweise in einer Bildschirmpräsentation langen ist, kann der Benutzer nicht animierte Übergänge erkennen da Frames ausgelassen werden.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Explorer gemischten PDF, PDF, Text

10 MB/s Profil liegt LAN in die meisten Aspekte. 1 MB/s bietet Erlebnis OK, obwohl möglicherweise einige Jitter bei einem Bildlauf auf dem Bildschirm Bilder gibt.

## <a name="flash-video-youtube"></a>Flash Video (YouTube)

100 MBIT/s LAN bietet optimalen 10 MB/s ist zulässig (d. h.) wir mit der Framerate aber jitter erhöht. Bei 1 MBIT/s ist Jitter sehr hoch und bemerkbar.

## <a name="word-typing-word-remote-input"></a>Wort eingeben (Word remote Eingabe)
Dies ist ein Szenario niedriger Bandbreite. 256 KB/s bietet so gut wie LAN.

## <a name="learn-more"></a>Weitere Informationen
- [Schätzen von Azure RemoteApp Auslastung der Netzwerkbandbreite](remoteapp-bandwidth.md)

- [Erleben Azure RemoteApp - wie Bandbreite und Qualität der Arbeit zusammen?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp - Tseting die Netzwerkbandbreite mit einigen Szenarien](remoteapp-bandwidthtests.md)