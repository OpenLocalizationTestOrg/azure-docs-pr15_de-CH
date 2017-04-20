<properties 
    pageTitle="Erleben Azure RemoteApp - wie Bandbreite und Qualität der Arbeit zusammen? | Microsoft Azure"
    description="Erfahren Sie, wie Netzwerkbandbreite in Azure RemoteApp Qualität des Benutzers beeinträchtigen kann."
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

# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Erleben Azure RemoteApp - wie Bandbreite und Qualität der Arbeit zusammen?

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Beim Betrachten der [gesamte Netzwerkbandbreite](remoteapp-bandwidth.md) für Azure RemoteApp dabei folgende - sind alle Teil eines dynamischen, die optimiert auswirkt. 

- **Verfügbare Netzwerkbandbreite und aktuellen Netzwerkprobleme** - Parameter (Verlust, Latenz, jitter) im selben Netzwerk zu einem bestimmten Zeitpunkt kann die Anwendung Streamingfunktionen, d. h. eine geringere optimiert auswirken. Bandbreite im Netzwerk ist eine Funktion der Überlastung, zufälligen Verlust, Latenzzeit, da diese Parameter Kontrollmechanismus Überlastung die wiederum Steuerelemente die Übertragungsrate beeinträchtigt, um Konflikte zu vermeiden.  Beispielsweise machen eine verlustreich oder Netzwerk mit hoher Latenz den Benutzer ungültige auch in einem Netzwerk mit 1000 MB Bandbreite. Verlust und Wartezeit variieren basierend auf der Anzahl der Benutzer im gleichen Netzwerk und tun Benutzer (z. B. Videos ansehen, herunterladen oder Hochladen von großen Dateien drucken).
- **Verwendungsszenario** - Erfahrung hängt die Benutzer als Einzelpersonen und im gleichen Netzwerk tun. Lesen einer Folie erfordert beispielsweise nur einen einzelnen Frame aktualisiert werden. der Benutzer gleitet und führt einen Bildlauf durch den Inhalt eines Textdokuments, benötigen sie eine höhere Anzahl von Frames pro Sekunde aktualisiert werden. Die Kommunikation mit dem Server in diesem Szenario hin-und werden schließlich mehr Netzwerkbandbreite beansprucht. Auch eine extreme Beispiel: mehrere Benutzer sind HD-Videos (wie Auflösung von 4K) ansehen, HD Telefonkonferenzen halten, 3D Videospiele oder auf CAD-Systemen. Alle diese machen auch wirklich hohe Netzwerkbandbreite praktisch unbrauchbar.
- **Auflösung und die Anzahl der Bildschirme** - mehr Netzwerkbandbreite muss vollständige Aktualisierung größere Bildschirme als kleine Bildschirme. Die zugrunde liegende Technologie wird ziemlich gut Codierung und nur die Bereiche der Seiten, die aktualisiert wurden, aber dabei, der gesamte Bildschirm aktualisiert werden muss. Wenn der Benutzer einen Bildschirm mit höheren Auflösung (z. B. 4K-Auflösung), das Update mehr Netzwerkbandbreite benötigt als einem Bildschirm mit niedriger Auflösung (z. B. 1024x768px). Diese Logik gilt, verwenden Sie mehr als einen Bildschirm für die Umleitung. Bandbreite muss mit der Anzahl der Bildschirme.
- **Zwischenablage und Gerät Umleitung** - dieses Problem nicht offensichtlich ist, aber in vielen Fällen, wenn ein Benutzer eine große Datenmenge in die Zwischenablage speichert dauert ein wenig Zeit, Informationen von Remote Desktop Client an den Server übertragen. Die Erfahrung der Inhalt der Zwischenablage vor senden kann downstream Erfahrung beeinflusst. Gilt für Umleitung - einen Scanner oder eine Webcam produziert große Datenmengen, die upstream an den Server gesendet werden muss, ob ein Drucker benötigt ein großes Dokument oder lokaler Speicher für eine Anwendung in der Cloud, eine große Datei kopieren verfügbar sein, Benutzer bemerken ausgelassenen Frames oder vorübergehend "eingefroren" Video denn die Daten für die Umleitung die Netzwerkbandbreite. 

Beim Auswerten der Bedarf an Netzwerkbandbreite müssen Sie alle diese Faktoren als ein System.

Jetzt im [Netzwerk Bandbreite Artikel](remoteapp-bandwidth.md)zurück oder gehen Sie zum Testen der [Netzwerkbandbreite](remoteapp-bandwidthtests.md).

## <a name="learn-more"></a>Weitere Informationen
- [Schätzen von Azure RemoteApp Auslastung der Netzwerkbandbreite](remoteapp-bandwidth.md)

- [Azure RemoteApp - Testen der Netzwerkbandbreite mit einigen Szenarien](remoteapp-bandwidthtests.md)

- [Azure RemoteApp Bandbreite - Richtlinien (Wenn Sie Ihre eigenen nicht testen)](remoteapp-bandwidthguidelines.md)