<properties 
    pageTitle="Azure Media Services fragmentierte MP4 live Aufnahme Spezifikation | Microsoft Azure" 
    description="Diese Spezifikation beschreibt das Protokoll und Format für fragmentierte MP4 basierte live streaming Einnahme für Microsoft Azure Media Services. Microsoft Azure Media Services bietet live streaming Service Kunden stream live Ereignisse und Inhalte in Echtzeit mit Microsoft Azure Cloud-Plattform. Dieses Dokument beschreibt auch Best Practices in hochgradig redundante und robuste live Aufnahme Mechanismen." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"     
    ms.author="cenkdin;juliako"/>

#<a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure Media Services fragmentierte MP4 live Aufnahme Spezifikation

Diese Spezifikation beschreibt das Protokoll und Format für fragmentierte MP4 basierte live streaming Einnahme für Microsoft Azure Media Services. Microsoft Azure Media Services bietet live streaming Service Kunden stream live Ereignisse und Inhalte in Echtzeit mit Microsoft Azure Cloud-Plattform. Dieses Dokument beschreibt auch Best Practices in hochgradig redundante und robuste live Aufnahme Mechanismen.


##<a name="1-conformance-notation"></a>1. Übereinstimmung Notation

Die Schlüsselwörter "muss" werden "Darf nicht", "Erforderlich", "Soll", "Nicht", "Sollte", "Soll nicht", "Empfehlenswert", "MAY" und "OPTIONAL" in diesem Dokument interpretiert sollen wie in RFC 2119 beschrieben.

##<a name="2-service-diagram"></a>2. Diagramm 

Das Diagramm zeigt die Architektur des live-Streaming-Service in Microsoft Azure Media Services:

1.  Live Encoder schiebt Feeds über Microsoft Azure Media Services SDK in Kanäle erstellt und bereitgestellt werden.
2.  Kanäle, Programme und Streaming-Endpunkt in Microsoft Azure Media Services Handle alle live-Streaming-Funktionen einschließlich Erfassung, Formatierung, cloud DVR, Sicherheit, Skalierbarkeit und Redundanz.
3.  Optional können Kunden eine CDN-Ebene zwischen den Streaming-Endpunkt und den Clientendpunkten bereitstellen.
4.  Client Endpunkte Stream vom Endpunkt Streaming über HTTP Adaptives Streaming-Protokolle (Smooth Streaming, Bindestrich, HDS oder HLS).

![Bild1][image1]


##<a name="3-bit-stream-format--iso-14496-12-fragmented-mp4"></a>3. Bitstrom Format ISO 14496-12 fragmentiert MP4

Das Übertragungsformat für Livestreaming aufnehmen, die in diesem Dokument erläutert [ISO-14496-12] basiert. Weitere [[MS voller]](http://msdn.microsoft.com/library/ff469518.aspx) ausführliche Erläuterung MP4 fragmentiert und Extensions für beide Dateien Demand und live-Streaming-Aufnahme.

###<a name="live-ingest-format-definitions"></a>Live Aufnahme Formatdefinitionen 

Nachfolgend eine Liste der speziellen Format anwenden, um live Definitionen in Microsoft Azure Media Services aufnehmen:

1. "Ftyp", LiveServerManifestBox und Moov'-Feld müssen bei jeder Anforderung (HTTP-POST) gesendet werden.  MUSS am Anfang des Streams gesendet werden und jederzeit Encoder wiederherstellen muss, um fortsetzen Stream aufnehmen.  Weitere Informationen finden Sie unter Abschnitt 6 [1].
2. [1] Abschnitt 3.3.2 definiert ein optionales Feld StreamManifestBox für live aufnehmen. Durch die Routinglogik des Microsoft Azure Lastenausgleich Verwendung dieses Feldes ist veraltet und sollte nicht bei Einnahme in Microsoft Azure Media Service. Wenn dieses Feld vorhanden ist, ignoriert Azure Media Services im Hintergrund.
3. 3.2.3.2 [1] gemäß TrackFragmentExtendedHeaderBox muss für jedes Fragment vorhanden sein.
4. Version 2 des TrackFragmentExtendedHeaderBox sollte verwendet werden, um mehrere Rechenzentren Media Segmente mit identischen URLs generieren. Das Indexfeld Fragment ist erforderlich für Cross-Rechenzentrum Failover von streaming-Index-basierte Formate wie Apple HTTP-Live-Streaming (HLS) und MPEG-DASH indexbasierte.  Um Cross-Rechenzentrum Failover zu aktivieren, muss der Index Fragment Encoder und Erhöhung hinweg 1 für jedes Fragment aufeinander folgende Medien auch über Encoder Neustarts oder Fehler synchronisiert werden.
5. [1] Abschnitt 3.3.6 definiert Feld MovieFragmentRandomAccessBox ("Mfra") am Ende des live-Aufnahme an EOS (Ende des Datenstroms) an den Kanal gesendet werden kann. Durch die Erfassung Logik Azure Media Services Verwendung EOS (Ende des Datenstroms) ist veraltet und Feld 'Mfra' für live-Aufnahme nicht gesendet. Wenn gesendet, ignoriert Azure Media Services im Hintergrund. Empfohlen wird mit [Kanal zurücksetzen](https://msdn.microsoft.com/library/azure/dn783458.aspx#reset_channels) der Zustand der Erfassung Punkt zurückgesetzt und außerdem [Programm beenden](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) einer Präsentation und Stream verwenden sollten.
6. Dauer des MP4-Fragments sollte konstant zu verkleinern Sie die Manifeste Client Client Download Heuristik mithilfe von wiederholten Tags verbessern.  Die Dauer kann schwanken, nicht ganzzahlige Frameraten auszugleichen.
7. Die Dauer des MP4-Fragments sollte ca. 2 bis 6 Sekunden.
8. MP4-Fragment Zeitstempel und Indizes (TrackFragmentExtendedHeaderBox Fragment_ Absolute_ Zeit und Fragment_index) sollte in aufsteigender Reihenfolge eintreffen.  Azure Media Services gegen doppelte Fragmente, er hat sehr begrenzte Fähigkeit, Fragmente nach der Medienzeitachse neu anordnen.

##<a name="4-protocol-format--http"></a>4. Protokoll HTTP-Format

ISO fragmentiert MP4 basierte live Aufnahme für Microsoft Azure Media Services verwendet ein Standard HTTP POST mit langer Anforderung codierten Mediendaten fragmentiert MP4-Format an den Dienst übertragen. Jede HTTP POST sendet eine vollständige fragmentiert MP4 Bitstrom ("Stream") ab mit Header ("Ftyp", "Live Manifest im Server" und 'Moov'-Feld) und mit Fragmenten ('Moof' und 'Mdat' Felder). URL-Syntax für HTTP POST-Anforderung finden Sie unter Abschnitt 9.2 In [1]. Ein Beispiel für den POST-URL ist: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

###<a name="requirements"></a>Vorschriften

Hier werden detaillierten Vorschriften:

1. Encoder sollte die Übertragung per HTTP POST-Anforderung mit einem leeren"" (null Inhaltslänge) mit der gleichen URL Aufnahme starten. Dadurch können schnell erkennen, wenn live Einnahme Endpunkt gültig ist und keine Authentifizierung oder Sonstiges erforderlich ist. HTTP-Protokoll möglich nicht Server, HTTP-Antwort zurück zu senden, bis die gesamte POST einschließlich eingeht. Angesichts der langer live Ereignis ohne diesen Schritt der Encoder möglicherweise nicht Fehler erkennen, bis es beendet alle Daten senden.
2. Encoder muss Fehler oder Probleme Authentifizierung durch (1) behandeln. Wenn (1) erfolgreich eine Antwort 200 fortsetzen.
3. Encoder muss eine neue HTTP POST-Anforderung mit dem fragmentierte MP4 beginnen.  Die Nutzlast muss mit den Header-Feldern gefolgt von Fragmenten beginnen.  Hinweis Das "Ftyp", "Live Manifest im Server" und "Moov" (in dieser Reihenfolge) mit jeder Anforderung gesendet werden muss, selbst wenn der Encoder wiederherstellen muss, da die vorherige Anforderung vor dem Ende des Streams beendet wurde. 
4. Encoder verwenden aufgeteilter Codierung übertragen zum Hochladen ist nicht die gesamte Inhaltslänge der live-Ereignis.
5. Wenn das Ereignis, nach dem Senden des letzten Fragments muss der Encoder ordnungsgemäß Nachrichtenreihenfolge aufgeteilter Codierung übertragen enden (die meisten HTTP-Client Stapel behandelt automatisch). Encoder muss der Dienst den endgültige Antwort-Code zurück und beendet die Verbindung warten. 
6. Veranstaltungen() Nomen verwenden darf Encoder wie 9.2 in [1] für live-Aufnahme in Microsoft Azure Media Services beschrieben.
7. Falls HTTP POST-Anforderung beendet oder vor dem Ende des Streams mit TCP-Fehler Timeout, muss der Encoder neue POST Anforderung eine neue Verbindung her und Vorschriften über zusätzliche Anforderung der Encoder die vorherigen beiden MP4 Fragmenten für jeden Track im Stream senden muss, und ohne Diskontinuitäten in der Medienzeitachse fortgesetzt.  Die letzten beiden MP4-Fragmente für jeden Track erneut wird sichergestellt, dass keine Daten verloren.  Stream enthält eine Audio- und verfolgen und die aktuelle POST-Anforderung fehlschlägt, muss der Encoder gesagt, schließen und erneut den letzten beiden Fragmenten Audiospur, die zuvor gesendet wurden, und die letzten beiden Fragmente für die Videospur, die zuvor gesendet wurden um sicherzustellen, dass keine Daten verloren.  Der Encoder muss es beim erneuten Verbinden sendet "Vorwärts" Puffer Media Fragmente aufrechterhalten.

##<a name="5-timescale"></a>5. Zeitskala 

[[MS voller]](https://msdn.microsoft.com/library/ff469518.aspx) beschreibt die Verwendung des "Zeitskala" für SmoothStreamingMedia (Abschnitt 2.2.2.1), StreamElement (Abschnitt 2.2.2.3) StreamFragmentElement(2.2.2.6) und LiveSMIL (Abschnitt 2.2.7.3.1). Zeitskala-Wert ist nicht vorhanden, der Standardwert 10.000.000 (10 MHz). Obwohl reibungslose Streaming Format-Spezifikation nicht blockiert Verwendung anderer Werte Zeitskala, die meisten der Encoder Implementierung verwendet diesen Standardwert (10 MHz) zu aufnehmen Smooth Streaming Daten. Durch [Dynamische Verpackung von Azure Media](media-services-dynamic-packaging-overview.md) Feature wird empfohlen mit 90 kHz Zeitskala für video-Streams und 44,1 oder 48,1 kHz audio-Streams. Zeitskala verschiedene Werte für verschiedene Streams verwendet, muss die Stream auf Zeitskala gesendet. [[MS voller]](https://msdn.microsoft.com/library/ff469518.aspx)finden.     

##<a name="6-definition-of-stream"></a>6. Definition "Stream"  

"Stream" ist der Vorgang live Einnahme für live-Präsentation verfassen, streaming Failover und Redundanz Szenarien behandeln. "Stream" bezeichnet eine eindeutige fragmentiert MP4 Bitstrom mit einem Track oder mehreren Tracks. Eine live-Präsentation kann eine oder mehrere Datenströme je live Livepräsentation enthalten. Die Beispiele veranschaulichen verschiedene Optionen auf gesamte eine live-Präsentation zu erstellen.

**Beispiel:** 

Kunde eine live-streaming-Präsentation erstellen, die folgenden Audio/Video-Bitrate enthält:

Video – 3000kbps, 1500 Kbit/s, 750 Kbit/s

Audio – 128 Kbit/s

###<a name="option-1-all-tracks-in-one-stream"></a>Option 1: Alle Spuren in einen stream

Bei dieser Option ein Encoder generiert alle Audio-/Video-Tracks und bündeln in eine fragmentierte MP4 Bitstrom über eine HTTP POST-Verbindung gesendet wird. In diesem Beispiel ist nur ein Stream für diese Livepräsentation:

![Bild2][image2]

###<a name="option-2-each-track-in-a-separate-stream"></a>Option 2: Jede Spur in einem separaten stream

Diese Option nur legen eine Livepräsentation in jedes Fragment MP4 Bitstrom verfolgen und alle Datenströme über mehrere separate HTTP-Verbindungen zu buchen. Dies kann mit einem Encoder oder mehrere Encoder erfolgen. Aus Sicht des live Aufnahme besteht diese Livepräsentation vier Streams.

![image3][image3]

###<a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Option 3: Bundle Audiospur mit der niedrigsten Bitrate Videospur in einen stream

Diese Option wählt der Kunde zu bündeln die Audiospur mit der niedrigsten Bitrate Videospur in einem Fragment MP4 Bitstrom zwei anderen Videospuren jedes seinen eigenen Stream wird. 


![image4][image4]


###<a name="summary"></a>Zusammenfassung

Wie oben gezeigt ist keine vollständige Liste aller möglichen Aufnahme-Optionen für dieses Beispiel. Der Tat unterstützt Gruppierung von Spuren in Streams live Aufnahme. Debitoren und Kreditoren Encoder können ihre eigenen Implementierungen technische Komplexität, Encoder Kapazität und Redundanz und Failover Faktoren abhängig. Jedoch darauf hinzuweisen, dass in den meisten Fällen gibt es nur eine Audiospur für die gesamte Präsentation live ist wichtig, die Gesundheit des Streams Erfassung, die Audiospur enthält. Diese Überlegung häufig führt Audiospur in seinen eigenen Stream (in Option 2) oder niedrigsten Videospur Bitrate (siehe Option 3) erweitert. Auch für bessere Redundanz und Fehlertoleranz aufnehmen in zwei verschiedene Streams (Option 2 mit redundanten Audiospuren) senden derselben Audiospur oder bündeln, Audiospur mindestens zwei der niedrigsten Bitrate Videospuren (Option 3 mit mindestens zwei Videostreams gebündelt) für live empfehlen, in Microsoft Azure Media Services.

##<a name="7-service-failover"></a>7. Service-Failover 

Bereitstellung des live-streaming ist gut Failover-Unterstützung für die Verfügbarkeit des Dienstes. Microsoft Azure Media Services dient, verschiedene Arten von Fehlern einschließlich Netzwerkfehler, Serverfehler, Speicherprobleme behandeln. Bei Verwendung in Verbindung mit ordnungsgemäßen Failover auf der Seite live Encoder erzielen Kunden einen zuverlässigen live streaming Service aus der Cloud.

In diesem Abschnitt erläutern wir Service Failover-Szenarien. In diesem Fall die irgendwo innerhalb des Dienstes geschieht und äußert sich ein Netzwerkfehler. Hier sind einige Vorschläge für die Encoder-Implementierung für die Behandlung von Service-Failover:


1. Verwenden Sie 10 zweite Timeout für TCP-Verbindung.  Versucht die Verbindung dauert länger als 10 Sekunden den Vorgang abbrechen, und versuchen Sie es erneut. 
2. Verwenden Sie einen kurzen Timeout zum Senden der HTTP-Anforderung Nachricht Abschnitte.  Verwenden der Ziel MP4 Fragment N Sekunden dauert, senden Timeout zwischen N und 2N Sekunden; Verwenden Sie einen Timeout von 6 bis 12 Sekunden z. B. MP4 Fragment Sekunden dauert.  Wenn ein Timeout auftritt, Zurücksetzen die Verbindung eine neue Verbindung und Stream fortsetzen auf die neue Verbindung aufnehmen. 
3. Verwalten von parallelen Puffer die letzten zwei Fragmente für jeden Track, die erfolgreich und vollständig an den Dienst gesendet wurden.  HTTP POST-Anforderung für einen Stream beendet oder Timeout vor das Ende des Streams eine neue Verbindung und anderen HTTP POST-Anforderung starten, Stream Header senden senden die letzten zwei Fragmente für jeden Track und Stream ohne Unterbrechung in der Medienzeitachse fortsetzen.  Dies verringert das Risiko von Datenverlusten.
4. Es wird empfohlen, der Encoder nicht wird die Anzahl der Versuche, Verbindung oder streaming nach einem TCP-Fehlers fortsetzen.
5. Nach einem TCP-Fehler:
    1. Die aktuelle Verbindung muss geschlossen und eine neue Verbindung muss eine neue HTTP POST-Anforderung erstellt werden.
    2. Neue HTTP-POST-URL muss sein den ersten POST-URL identisch.
    3. Neue HTTP POST muss der einschließen Stream Header ("Ftyp", "Live Manifest im Server" und 'Moov' Feld) mit Stream-Header im ursprünglichen Beitrag.
    4. Die letzten zwei Fragmente für jeden Track gesendet erneut gesendet werden müssen und streaming ohne Unterbrechung in der Medienzeitachse fortgesetzt.  Zeitstempel Fragment MP4 müssen auch über HTTP POST-Anfragen, erhöhen.
6. Der Encoder sollte HTTP POST-Anforderung beendet werden, wenn Daten nicht mit der Dauer des MP4-Fragments Rate gesendet wird.  HTTP POST-Anforderung, die keine Daten senden kann verhindern, dass Azure Media Services Encoder bei ein Update schnell trennen.  Aus diesem Grund HTTP POST für geringe Datendichte (Ad) Leiterbahnen sollte kurz lebte Beenden als geringe Datendichte Fragment gesendet wird.

##<a name="8-encoder-failover"></a>8. Encoder Failover

Encoder-Failover ist die zweite Failover-Szenario, das für die End-to-End live streaming Übermittlung berücksichtigt werden. In diesem Szenario ist der Fehlerzustand auf Encoder passiert. 

![image5][image5]


Unten erwarten die live-Aufnahme Endpunkt Encoder Failover geschieht:

1. Eine neue Encoderinstanz sollte um streaming weiterhin erstellt werden, wie im obigen Diagramm (3000 k mit gestrichelten Linie video-Stream).
2. Neue Encoder verwenden dieselbe URL für HTTP POST-Anfragen wie die fehlerhafte Instanz.
3. Der neue Encoder POST-Anforderung muss dieselbe fragmentierte MP4 Header-Felder als fehlerhafte Instanz enthalten.
4. Der neue Encoder muss mit allen anderen ausgeführten für die Präsentation live, synchronisierten Audio-/Video-Grenzen ausgerichtet Fragment generiert ordnungsgemäß synchronisiert werden.
5. Der neue Stream muss semantisch gleichwertig mit dem vorhergehenden und austauschbar auf Kopf- und Fragment.
6. Der neue Encoder sollten Datenverluste zu minimieren.  Fragment_absolute_time und Fragment_index von Medien Fragmenten soll ab dem Zeitpunkt, in dem der Encoder zuletzt beendet wurde.  Fragment_absolute_time und Fragment_index sollten in kontinuierlich erhöhen, aber es ist zulässige Diskontinuität ggf. vor.  Azure Media Services ignorieren Fragmente, die er bereits empfangen und verarbeitet, so ist es besser, auf der Seite erneut Fragmente als Diskontinuitäten in der Medienzeitachse vorzustellen. 

##<a name="9-encoder-redundancy"></a>9. Encoder-Redundanz 

Für bestimmte kritische live Ereignisse bei Bedarf noch höhere Verfügbarkeit und Qualität sollten aktive redundante Encoder nahtloses Failover ohne Datenverlust zu beschäftigen.

![Image6][image6]

Wie in der Abbildung oben werden zwei Gruppen von Encodern Einschieben zwei Kopien der einzelnen Streams gleichzeitig live-Dienstes. Dieses Setup wird unterstützt, da Microsoft Azure Media Services doppelte basierend auf Stream-ID und das Fragment Timestamp Fragmente filtern kann. Die resultierende Livestream und Archivierung werden einer Kopie aller Streams, die die beste Aggregation aus zwei Quellen. Beispielsweise in einem hypothetischen Extremfall, solange es einen Encoder (muss dasselbe sein) ausführen jederzeit Zeit für jeden Datenstrom resultierende Livestream aus dem Dienst werden kontinuierlich ohne Datenverlust. 

Die Anforderung für dieses Szenario ist fast identisch mit der Encoder Failover Anforderungen mit Ausnahme der zweite Satz der Encoder primäre Encoder gleichzeitig ausgeführt werden.

##<a name="10-service-redundancy"></a>10. Service Redundanz  

Für eine hochgradig redundante globale muss manchmal Cross-Region Sicherung regionale Katastrophen behandeln. "Encoder-Redundanz" Topologie erweitern, können Kunden redundante Service-Bereitstellung in einer anderen Region mit 2. Encoder verbunden. Kunden können auch mit CDN-Anbieter GTM (Global Traffic Manager) bereitstellen vor zwei Bereitstellungen problemlos Client Datenverkehr arbeiten. An den Encoder sind identisch "Encoder-Redundanz" die einzige Ausnahme, die die zweite Gruppe von Encodern auf einen anderen live zeigt bei Endpunkt aufnehmen. Das Diagramm unten zeigt Setup:

![image7][image7]

##<a name="11-special-types-of-ingestion-formats"></a>11. spezielle Formattypen Einnahme 

Dieser Abschnitt beschreibt einige spezielle Typ live-Aufnahme-Formate, die einige Szenarien behandelt werden.

###<a name="sparse-track"></a>Geringe Datendichte verfolgen

Bei einer Livepräsentation streaming mit rich Client-Erfahrung ist häufig erforderlich, Synchronisierung Ereignisse übertragen oder Band mit den wichtigsten Mediendaten signalisiert. Ein Beispiel dafür ist dynamisches live anzeigen einfügen. Diese Art von Ereignis signalisiert unterscheidet sich von regulären sparse naturgemäß streaming Audio und Video. Also die Signaldaten normalerweise geschieht nicht kontinuierlich und das Intervall ist schwer vorherzusagen. Das Konzept der Sparse Track wurde speziell aufnehmen und in-Band-Signaldaten.

Unten ist eine empfohlene Implementierung für Einnahme sparse verfolgen:

1. Erstellen Sie eine separate fragmentiert MP4 Bitstrom mit nur geringe Datendichte Titel ohne Audio/Video-Tracks.
2. Verwenden Sie im"Live Server Manifest" im Sinne von Abschnitt 6 [1] Parameter "ParentTrackName" der Name der übergeordneten Spur an. Siehe Abschnitt 4.2.1.2.1.2 [1] Weitere Informationen.
3. Im"Live Server Manifest", müssen Sie ManifestOutput festlegen auf "True".
4. Angesichts der sparse signalisierendes Ereignis, wird empfohlen, dass:
    1. Zu Beginn des live-Ereignis sendet Encoder die ursprünglichen Header-Feldern an einen Dienst, der Dienst sparse Track im Clientmanifest registrieren kann.
    2. Der Encoder sollte HTTP POST-Anforderung beendet werden, wenn Daten nicht gesendet werden.  Ein langer HTTP POST, die keine Daten senden kann Azure Media Services schnell Trennen vom Encoder bei einem Update oder Server Neustart eines Dienstes als Medienserver vorübergehend in einen Receive-Vorgang auf dem Sockel blockiert werden. 
    3. Encoder sollte schließen Zeit Signaldaten nicht verfügbar werden HTTP POST anfordern.  Die POST-Anforderung aktiv ist, sollte der Encoder Daten senden 
    4. Beim Senden von sparse Fragmente kann Encoder explizite Content-Length-Header festlegen, wenn es verfügbar ist.
    5. Beim Senden von sparse Fragment mit einer neuen Verbindung sollte Encoder starten Header Felder gefolgt von neuen Fragmenten senden. Dies ist der Fall behandelt, wobei Failover zwischen passiert und sparse neu auf einen neuen Server mit geringer Datendichte Spur vor nicht gesehen hat hergestellt wird.
    6. Sparse Track-Fragment wird zur Verfügung gestellt werden Client der entsprechenden übergeordneten Verfolgen von Fragment, die gleich oder größer Timestampwert wird dem Client zur Verfügung gestellt. Beispielsweise hat das sparse Fragment Timestamp t = 1000, soll der Client sieht Video (übernimmt Spurnamen übergeordneten video) fragment Timestamp 1000 oder hinaus können sie mit geringer Datendichte Fragment t = 1000. Beachten Sie, dass das tatsächliche Signal sehr gut für eine andere Position in der Zeitleiste Präsentation für den vorgesehenen Zweck verwendet werden kann. Im obigen Beispiel ist es möglich, die sparse-Fragment von t = 1000 hat eine XML-Nutzlast ist zum Einfügen einer Anzeigenklicks in Sekunden ist höher.
    7. Die Nutzlast sparse Spur Fragment kann in unterschiedlichen Formaten (z. B. XML oder Text oder binär usw.) je nach Szenarien. 


###<a name="redundant-audio-track"></a>Redundante Audiospur

In einem normalen HTTP Adaptives Streaming-Szenario (Smooth Streaming oder Bindestrich) ist oft nur eine Audiospur in der gesamten Präsentation. Im Gegensatz zu Videospuren mit mehreren Qualitätsstufen für den Client aus Fehlerzustände, kann die Audiospur eine einzelne Fehlerquelle ist Aufnahme der Stream, der die Audiospur enthält, fehlerhafte. 

Zur Lösung dieses Problems unterstützt Microsoft Azure Media Services live Aufnahme redundante Audiospuren. Soll dieselbe Audiospur mehrmals in verschiedene Streams gesendet werden kann. Während der Dienst die Audiospur nur einmal im Clientmanifest registriert wird, ist es können redundante Audiospuren als Sicherungsserver für primäre Audiospur Probleme audio Fragmente abgerufen. Um redundante Audiospuren aufnehmen zu können, muss der Encoder:

1. Erstellen der gleichen Audiospur in mehrere Fragment MP4 Bit-Streams. Redundanten Audiospuren müssen auf Kopf- und Fragment semantisch gleichwertig mit genau den gleichen Fragment Timestamps und austauschbar sein.
2. Stellen Sie sicher, dass "Audio" Eintrag in das Live-Server-Manifest (Abschnitt 6 [1]) für alle redundanten Audiospuren identisch sein.

Unten ist eine empfohlene Implementierung für redundante Audiospuren:

1. Senden Sie jede eindeutige Audiospur selbst in einen Stream.  Auch redundanten Stream senden, für jede dieser Audiospur Streams, 2. Stream 1. unterscheidet sich nur durch den Bezeichner der HTTP-POST-URL: {Protokoll} :// {Serveradresse} / {Punkt path}/Streams({identifier}) veröffentlichen.
2. Verwenden Sie separate Streams zwei niedrigsten video Bitraten senden. Jeder dieser Streams sollten auch eine Kopie jeder eindeutige Audiospur enthalten.  Beispielsweise sollte mehrere Sprachen unterstützt, diese Streams Audiospuren für jede Sprache enthalten.
3. Verwenden Sie separate Serverinstanzen (Encoder) codiert und redundante Streams gemäß Absatz 1 und 2 senden. 



##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png

 