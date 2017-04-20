
<properties 
    pageTitle="Schätzen von Azure RemoteApp Auslastung der Netzwerkbandbreite | Microsoft Azure"
    description="Lernen Sie die Netzwerkbandbreite für Azure RemoteApp-Sammlungen und apps."
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

# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Schätzen von Azure RemoteApp Auslastung der Netzwerkbandbreite 

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Azure RemoteApp verwendet das Remote Desktop Protocol (RDP) zur Kommunikation zwischen Azure Cloud und Ihre Benutzer ausgeführt. Dieser Artikel enthält einige grundlegenden Richtlinien, Nutzung und potenziell auswerten Netzwerkbandbreite pro Azure RemoteApp Benutzer verwendeten.

Schätzen der Bandbreite pro Benutzer ist sehr komplex und erfordert mehrere Programme gleichzeitig Multitasking Szenarien, Programme auswirken könnte die Leistung auf die Netzwerkbandbreite. Sogar der Typ des Remotedesktopclients (z. B. Macintosh-Client im Vergleich zu HTML5 Client) kann zu verschiedenen Bandbreite Ergebnissen führen. Um Hilfe beim Durcharbeiten dieser Komplikationen brechen wir Verwendungsszenarios in mehreren allgemeinen Kategorien reale Szenarios zu replizieren. (Wobei die Praxis natürlich aus Kategorien ist und Benutzer unterscheidet.)

Bevor wir fortfahren-Beachten Sie, dass wir RDP bietet eine gute ausgezeichnete Erfahrung für die meisten Szenarien in Netzwerken Wartezeit 120 ms und Bandbreite über 5 MB - basiert auf RDP Fähigkeit, dynamisch mithilfe der verfügbaren Netzwerkbandbreite und geschätzte Anwendung Bandbreite benötigt. Dieser Artikel geht über die "die meisten Anwendungsszenarios" Suchen am Rand, wo Szenarien beginnen entladen und Benutzer abzusinken beginnt.

Jetzt checken Sie folgenden Artikeln ausführliche Faktoren berücksichtigen, einschließlich Baseline Empfehlung und was wir nicht in den Schätzungen enthalten aus.

- [Wie erleben Bandbreite und Qualität der Arbeit zusammen?](remoteapp-bandwidthexperience.md)
- [Die Netzwerkbandbreite mit einigen Szenarien testen](remoteapp-bandwidthtests.md)
- [Schnelle Richtlinien haben Sie Zeit oder testen](remoteapp-bandwidthguidelines.md)


## <a name="what-are-we-not-including"></a>Was einschließlich wir nicht?

Beachten Sie bei der Überprüfung der vorgeschlagenen Tests und unsere Empfehlung insgesamt (und zwar generische) gibt es mehrere Faktoren, die wir nicht. Benutzer Erfahrung Komplikationen asymmetrische Natur der Upload und download z. B. Bandbreite. Die asymmetrische Natur der meisten WLANs wirkt außerdem Leistung und User Experience Wahrnehmung. Interaktive Szenarios möglicherweise downstream Datenverkehr unter den upstream priorisiert werden mehr verloren Video- oder audio-Frames und beeinträchtigt daher Benutzer empfindet streaming Erfahrung. Sie können eigene Experimente für den bestimmten Anwendungsfall und das Netzwerk ist ausführen.

Obwohl wir Umleitung behandelt, wir keine Auswirkung auf die Bandbreite des Netzwerkverkehrs durch angeschlossene Geräte wie Speicher, Drucker, Scanner, Webcams und andere USB-Geräte verursacht berücksichtigen. Der Effekt dieser Geräte in der Regel den Bandbreitenbedarf vorübergehend Spitzen und verschwinden, wenn der Vorgang abgeschlossen ist. Aber wenn häufig Bandbreitenbedarf auffällig werden.

Wir auch sprechen nicht wie andere Benutzer auf demselben Netzwerk auswirken können. Beispielsweise könnte ein Benutzer verbraucht 4 K Video 100 MB/s-Netzwerk andere Benutzer im gleichen Netzwerk versuchen, dieselbe Aufgabe auszuführen wesentlich. Leider wird es immer schwieriger, die Auswirkung der gleichzeitigen Verwendung zu Allgemeine oder umfassende Empfehlung zur Leistung des Systems am Aggregat bestimmt. Wir sagen ist die zugrunde liegende Protokoll Technologie wird nutzen die verfügbare Netzwerkbandbreite jedoch hat ihre Grenzen.