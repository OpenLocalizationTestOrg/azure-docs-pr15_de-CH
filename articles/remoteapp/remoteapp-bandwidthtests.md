<properties 
    pageTitle="Azure RemoteApp - die Netzwerkbandbreite mit einigen Szenarien testen | Microsoft Azure"
    description="Erfahren Sie, wie allgemeine Szenarien, die Ihnen helfen können der Bedarf an Netzwerkbandbreite für Azure RemoteApp herauszufinden."
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
    
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp - Testen der Netzwerkbandbreite mit einigen Szenarien

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Wie wir im [Schätzung Azure RemoteApp Auslastung der Netzwerkbandbreite](remoteapp-bandwidth.md), testet am einfachsten herausfinden, was die Auswirkung von Azure RemoteApp auf das Netzwerk einige Verwendung ausführen. Diese Tests für einen bestimmten Zeitraum und messen Sie für jedes Szenario erforderliche Bandbreite. Haben Sie die Möglichkeit, können Sie auch das Netzwerk Paket Verlust und Netzwerk Zittern um Netzwerk-Muster vertraut machen, die in Ihrer Umgebung entstehen messen.

    
Bei der Nutzung der Netzwerkbandbreite daran hängt von verschiedenen Benutzern in Ihrem Unternehmen. Z. B. TextReader und Writer in der Regel weniger Bandbreite verbraucht als Benutzer mit Video. Am besten eigene Benutzer Bedürfnisse, und erstellen Sie eine Kombination der folgenden Szenarien, die Benutzer in Ihrem Unternehmen am besten darstellt. Denken Sie daran, [Überprüfen Sie die Faktoren, die Einfluss Bandbreite und User experience](remoteapp-bandwidthexperience.md) -, mit denen Sie die ideale Tests identifizieren.

Erste Lesevorgang zu den Tests die Mischung auswählen und diese ausführen. In der folgenden Tabelle können Sie Leistung verfolgen.

>[AZURE.NOTE] Wenn eigene Tests nicht oder Sie haben nicht die Zeit, lesen Sie [grundlegende Bandbreite Vorkalkulationen/Recommendations](remoteapp-bandwidthguidelines.md). Die Leistung kann jedoch variieren, so *können* Ihre eigenen Tests ausführen, Sie sollten.


## <a name="the-usage-tests"></a>Verwendung-tests
Diese Tests für lange ausgeführt und anderen Funktionen/Funktionen, die Netzwerkbandbreite verbrauchen. Beachten Sie aus Test auswählen, am besten Ihr einzelner Benutzer entspricht.
 
### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>Executive-komplexe PowerPoint - 900-1000 Sekunden

Ein Benutzer stellt zwischen 45-50 High-Fidelity-Folien mithilfe von Microsoft Office PowerPoint im Vollbildmodus. Die Folien sollte Bilder, Übergänge (mit Animationen) und Hintergründe mit Farbverlauf, die für Ihr Unternehmen. Der Benutzer sollte mindestens 20 Sekunden auf jeder Folie verbringen.
    
In diesem Szenario wird gefährlichen Datenverkehr beim Übergang von einer Folie zur nächsten Folie in der Präsentation.
    
### <a name="simple-powerpoint---run-for-620-seconds"></a>Einfache PowerPoint - 620 ~ Sekunden

Ein Benutzer stellt eine einfache PowerPoint-Datei mit etwa 30 mit Microsoft Office PowerPoint im Vollbildmodus. Die Folien sind mehr Text als Executive-komplexe PowerPoint-Szenario und einfacher Hintergründe und Bilder (schwarze Diagramme). 
    
### <a name="internet-explorer---run-for-250-seconds"></a>InternetExplorer - ca. 250 Sekunden

Der Benutzer navigiert im Web mit Internet Explorer. Der Benutzer navigiert und Bildlauf aus Text, natürliche Bilder und einige Schemata. Auf der Festplatte des Servers Remotedesktop-Sitzungshost (RD-Sitzungshost) gespeicherte Webseiten ein. MHT-Datei. Bildlauf durch unterschiedliche Intervalle für jeden Schlüssel/Rollen Bild-auf, Bild-ab, nach oben und unten, mit:
    
    - Unten - 250 Tastenanschläge sehr 500 ms
    - Bild-auf - 36 Tastenanschläge alle 1000 ms
    - -Alle 100 ms Tastatureingaben 75
    - Bild-ab - 20 Tastenanschläge alle 500 ms
    - -Alle 300 ms Tastatureingaben 120
    
### <a name="pdf-document---simple---run-for-610-seconds"></a>PDF-Dokument – einfache - ~ 610 Sekunden ausführen
Ein Benutzer liest und sucht ein PDF-Dokument auf verschiedene Arten mit Adobe Acrobat Reader. Das Dokument sollte aus Tabellen, einfache Grafiken und mehrere Schriftarten bestehen. Das Dokument ist 35-40 Seiten. Der Benutzer führt einen Bildlauf durch zwei unterschiedlich zurück und leitet bei vier unterschiedliche Zoom (, Seite anpassen an die Breite anpassen 100 % und einem anderen Ihrer Wahl). Das Zoomen wird sichergestellt, dass Text (Font) in verschiedenen Größen rendert. Bildlauf ist die Bild-auf, Bild-ab, nach oben und unten, mit unterschiedlichen Abständen für die einzelnen Rollen.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>PDF-Dokument ~ 320 - gemischten - Lauf
Ein Benutzer liest und sucht ein PDF-Dokument auf verschiedene Arten mit Adobe Acrobat Reader. Das Dokument besteht aus qualitativ hochwertiger Bilder (einschließlich Fotos), Tabellen, einfache Grafiken und mehrere Schriftarten. Der Benutzer führt einen Bildlauf durch zwei unterschiedlich zurück und leitet bei vier unterschiedliche Zoom (, Seite anpassen an die Breite anpassen 100 % und einem anderen Ihrer Wahl). Das Zoomen wird sichergestellt, dass Text (Font) in verschiedenen Größen rendert. Bildlauf ist die Bild-auf, Bild-ab, nach oben und unten, mit unterschiedlichen Abständen für die einzelnen Rollen.

### <a name="flash-video-playback---run-for-180-seconds"></a>Flash video-Wiedergabe - ~ 180 Sekunden
Ein Benutzer zeigt eine Adobe Flash-codiertes Video in eine Webseite eingebettet. Die Webseite wird in der lokalen Festplatte des Remotedesktop-Sitzungshostserver gespeichert. Das Video wird durch einen eingebetteten Player-Plug-in in Internet Explorer wiedergegeben.

Dieses Szenario emuliert Benutzer Webseiten rich Content mit multimedia. Die meisten Daten sollten Bo durch VOBR.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Wort tippen remote - ~ 1800 Sekunden
Ein Benutzer gibt ein Dokument über eine RDP-Sitzung. Tastatureingaben werden vom Client über die RDP-Sitzung mit einem Dokument in Microsoft Word gesendet in der Remotesitzung ausgeführt. Die Eingabe ist ein Zeichen alle 250 ms (7050 Zeichen). 

Dies ist eines der häufigsten Szenarios für einen Mitarbeiter. Dieses Szenario testet die Reaktionsfähigkeit von einem Benutzer in einem modernen Prozessor. Dieses Szenario ist auch kleine Änderungen Bandbreite.

## <a name="tracking-the-test-results"></a>Überwachen der Testergebnisse

In der folgenden Tabelle können Sie um die Szenarien in Ihrer Umgebung zu evaluieren. Die unten angegebenen Daten nur zur Veranschaulichung - möglicherweise erheblich unterscheidet was Sie beachten. 

Der Einfachheit halber wird angenommen, dass alle Szenarien getestet mit derselben Auflösung von 1920 x 1080 Pixel und jitter TCP-Transporte in einem Netzwerk mit 200 ms und Netzwerk Latenz (Verzögerung) in der ms 120 % 1.

Über die Tabelle:
- **Durchschnittliche Erfahrung** enthält die Netzwerkbandbreite, Produktivität nicht wesentlich beeinträchtigt aber nicht die gelegentliche Video- oder audio-Probleme. Das System ist nutzen die dynamische Logik schnell wiederherstellen. Netzwerk Bandbreite Vorkalkulationen Versuch des Benutzererlebnisses Qualität.
 - **Deutliche Probleme (Haltepunkt)** enthält die Netzwerkbandbreite, wo Benutzer bemerken möglicherweise Probleme in ihrer Erfahrung und ihre Produktivität wird beeinträchtigt, messbare Zeiträumen. Zu diesem Zeitpunkt RDP-Algorithmen kämpfen und Qualität des Benutzers aufgrund unzureichender Bandbreite nicht garantieren.
 - **Empfohlene** enthält die Netzwerkbandbreite für gut oder hervorragend Erfahrung empfohlen. Es ist in der Regel einen Schritt höher als der Wert in der entsprechenden Spalte **Durchschnitt auftreten** .
 - **Notizen** sind Stellungnahmen und Kommentare.
 
| Test                  | Durchschnittliche Erfahrung | Deutliche Probleme (Haltepunkt) | Empfohlene Netzwerkbandbreite | Notizen                                                              |
|-----------------------|--------------------|---------------------------------|-------------------------------|--------------------------------------------------------------------|
| Executive-komplexe PPT | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MBIT/s bevorzugt    | Bei 1 MBIT/s gehen viele Animationen                                   |
| Einfache PPT            | 5 MB/s              | 256 KB/s                         | 10 MB/s                        | Laden Folien mit 256 KB/s mit spürbaren Verzögerung                   |
| InternetExplorer     | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MBIT/s bevorzugt    | Bei 1 MBIT/s Webvideos unscharf und abgehackt, schnelles Scrollen Probleme |
| Einfache PDF            | 1 MB/s              | 256 KB/s                         | 5 MB/s                         | 256 KB/s dauert eine Weile zum Laden der Seite                       |
| Gemischte PDF             | 1 MB/s             | 256 KB/s                         | 5 MB/s                         | 256 KB/s wird die Seite viel Zeit zum Laden    |
| Flash video-Wiedergabe  | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MBIT/s bevorzugt    | Bei 1 MBIT/s das Bild körnig ist und einige Frames werden gelöscht           |
| Word remote eingeben    | 256 KB/s            | 128 KB/s                         | 1 MB/s                         | 256 KB/s kann Benutzer zwischen Tastenanschläge feststellen.             |

Zum Auswerten der Netzwerk-Bandbreite pro Benutzer erstellen Sie aus obigen Szenarios und der entsprechende Anteil der erforderlichen Netzwerkbandbreite. Wählen Sie die höchste Zahl für Szenarien erforderlich. Da Benutzer nie allein System verwenden, sollten Sie einige Reserve für Benutzer, die gleichzeitig im selben Netzwerk arbeiten.
     
## <a name="learn-more"></a>Weitere Informationen
- [Schätzen von Azure RemoteApp Auslastung der Netzwerkbandbreite](remoteapp-bandwidth.md)

- [Erleben Azure RemoteApp - wie Bandbreite und Qualität der Arbeit zusammen?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp Bandbreite - Richtlinien (Wenn Sie Ihre eigenen nicht testen)](remoteapp-bandwidthguidelines.md)