# <a name="internet-of-things-security-architecture"></a>Internet der Dinge-Sicherheitsarchitektur

Beim Entwerfen eines Systems ist es wichtig zu verstehen der Bedrohung für das System add geeignete Schutzmaßnahmen entsprechend entworfen und Architektur des Systems. Es ist besonders wichtig, das Produkt ab Sicherheit da verstehen, wie ein Angreifer in ein System einzudringen möglicherweise hilft sicherzustellen, dass entsprechende Gegenmaßnahmen sind ab. 

## <a name="security-starts-with-a-threat-model"></a>Sicherheit beginnt mit einem Bedrohungsmodell
 
Microsoft hat Bedrohungsmodelle für seine Produkte bereits seit langem eingesetzt und das Unternehmen Bedrohungsanalyse öffentlich verfügbar. Die Erfahrung zeigt, dass die Modellierung unerwartete Vorteile über das unmittelbare Verständnis der Risiken die über. Beispielsweise wird erstellt auch einen Weg für eine offene Diskussion mit anderen außerhalb der Entwicklungsteam zu neuen Ideen und Verbesserung des Produkts.
  
Die Bedrohungsanalyse soll zu verstehen, wie ein Angreifer möglicherweise ein System und stellen Sie sicher, dass entsprechende Gegenmaßnahmen vorhanden sind. Threat Modeling Kräfte Entwurf Schutzmaßnahmen berücksichtigt das System entwickelt, statt nach einem team bereitgestellt wird. Dies ist von grundlegender Bedeutung ist also Sicherheitsmaßnahmen für eine Vielzahl von Geräten im Feld unmöglich, fehleranfällig und Kunden gefährdet.

Viele Entwicklungsteams sind hervorragend erfassen die funktionalen Anforderungen, die Kunden profitieren. Identifizieren nicht offensichtlich, dass jemand das System missbrauchen könnte ist jedoch schwieriger. Bedrohungsanalyse hilft Entwicklungsteams verstehen, was ein Angreifer tun kann und warum. Erstellen von Bedrohungsmodellen ist ein strukturierter Prozess, der eine Diskussion über die Sicherheit Designentscheidungen im System erstellt sowie für den Entwurf, die unterwegs Auswirkung Sicherheit erfolgen wird. Während ein Bedrohungsmodell einfach ein Dokument ist, stellt dieser Dokumentation auch optimal Continuity Kenntnisse Aufbewahrung Lektionen gelernt und Hilfe neue team an Bord schnell. Schließlich ist ein Ergebnis der Bedrohungsanalyse können Sie andere Aspekte der Sicherheit was Sicherheit Zusagen für Ihre Kunden bereitstellen möchten. Diese Verpflichtung in Verbindung mit Bedrohungsmodellen informieren und Laufwerk Testen der Projektmappe Internet der Dinge (IoT).
 

### <a name="when-to-threat-model"></a>Beim Modell Bedrohung

[Threat modeling](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) bietet den größten Wert in der Entwurfsphase aufgenommen wird. Beim Entwerfen, müssen Sie die größte Flexibilität Gefahren zu ändern. Gefahren ist durch Design das gewünschte Ergebnis. Es ist viel einfacher als Gegenmaßnahmen hinzufügen, testen und die aktuellen bleiben und außerdem eine solche Löschung ist nicht immer möglich. Es wird immer schwieriger zu Sicherheitsrisiken ein Produkts wird reifer und wiederum letztendlich erfordert mehr Arbeit und viel schwieriger Kompromisse als Bedrohungsmodelle frühzeitig in der Entwicklung.

### <a name="what-to-threat-model"></a>Was Bedrohungsmodell

Thread-Modell die Lösung als Ganzes, und konzentrieren Sie sich auch in den folgenden Bereichen:

- Sicherheit und Datenschutz-Funktionen
- Funktionen, deren Sicherheit relevant sind
- Die Funktionen, die eine Vertrauensgrenze touch 

### <a name="who-threat-models"></a>Die Modelle Bedrohung

Erstellen von Bedrohungsmodellen ist ein Vorgang wie jedes andere.  Es empfiehlt sich zu behandeln das Bedrohungsdokument wie jede andere Komponente der Lösung zu überprüfen. Viele Entwicklungsteams sind hervorragend erfassen die funktionalen Anforderungen, die Kunden profitieren. Identifizieren nicht offensichtlich, dass jemand das System missbrauchen könnte ist jedoch schwieriger. Bedrohungsanalyse hilft Entwicklungsteams verstehen, was ein Angreifer tun kann und warum.

### <a name="how-to-threat-model"></a>Wie Bedrohungsmodell

Erstellen von Bedrohungsmodellen besteht aus vier Schritten. die Schritte sind:

- Modellieren der Anwendungdes
- Auflisten der Risiken
- Minimieren Sie Risiken
- Überprüfen von Gegenmaßnahmen

#### <a name="the-process-steps"></a>Prozessschritte

Drei Faustregeln zu Erstellung eines Bedrohungsmodells:

1. Erstellen eines Diagramms aus Referenzarchitektur. 
2. Starten Sie zuerst der Breite. Überblick, und das System als Ganzes vor Tiefsee verstehen.  Dadurch wird sichergestellt, dass Sie tiefer gehende an den richtigen stellen.
3. Der Prozess, den Prozess Fahrt lassen. Wenn Sie ein Problem in der Entwurfsphase und erforschen, gehen möchten!  Fühlen Sie sich nicht diese Servicekraft Schritte.  

#### <a name="threats"></a>Gefahren

Die vier Kernelemente einem Bedrohungsmodell sind:

- Prozesse (Webdienste, Win32-Dienste * nix-Daemons usw.. Beachten Sie, dass einige komplexen Entitäten (Feld Gateways und Sensoren) abstrahiert werden können, wie ein Prozess beim technischen Drilldown in diesen Bereichen nicht möglich ist.
- Datenspeicher (anywhere, z. B. eine Datei oder Datenbank gespeichert wird)
- Datenfluss (wobei Daten zwischen anderen Elementen in der Anwendung verschoben)
- Externe Entitäten (nichts mit dem System interagiert jedoch nicht unter der Kontrolle der Anwendung, Beispiele für Benutzer und Satelliten-Feeds)

Alle Elemente im Architekturdiagramm unterliegen Gefahren. Wir verwenden das mnemonische STRIDE. [Threat Modeling wieder STRIDE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) zu STRIDE-Elemente lesen.

Verschiedene Elemente des Anwendungsdiagramms unterliegen bestimmten STRIDE-Risiken:

- Prozesse sind STRIDE
- Daten sind TID
- Datenspeicher sind TID und manchmal R, wenn die Datenspeicher Protokolldateien.
- Externe Entitäten sind SRD

## <a name="security-in-iot"></a>Sicherheit in IoT

Spezielle Geräte haben zahlreiche mögliche Interaktion Flächen und Interaktionsmuster, die einen Rahmen für den sicheren Zugriff auf diese Geräte digitale berücksichtigt werden müssen. Der Begriff "digital Access" wird hier verwendet Vorgänge unterscheiden, die durch direkte geräteinteraktion ausgeführt werden, in denen Sicherheit über physischen Zugriff Steuerelement bereitgestellt. Z. B. Inverkehrbringen das Gerät in einem Raum mit einer Tür. Beim Zugang verweigert werden kann, mit Software und Hardware können Maßnahmen führende System Eingriff physischen Zugriff verhindern. 

Erfahren die Interaktionsmuster betrachten "Device Control" und "Gerätedaten" mit derselben Aufmerksamkeit wir. "Device Control" kann Informationen klassifiziert werden, die ein Gerät Parteien mit dem Ziel ändern oder beeinflussen das Verhalten Zustand oder den Zustand der Umgebung bereitgestellt wird. "Gerätedaten" können Informationen klassifiziert werden, die ein Gerät an eine andere Partei über den Zustand und die beobachtete Umgebung ausgibt.
   
Zur Optimierung der bewährten Sicherheitsmethoden empfohlen typische IoT-Architektur als Teil der Bedrohungsmodell mehrere Komponenten-Zonen unterteilt werden. Diese Zonen vollständig in diesem Abschnitt beschrieben und umfassen:

-   Gerät
-   Feld Gateway
-   Cloud-Gateways und
-   Dienste.

Zonen sind breit segment eine Lösung. Jede Zone ist häufig eigene Daten sowie Authentifizierung und Autorisierung erforderlich. Zonen können auch zu Isolation und Wirkung niedriger Vertrauenswürdigkeit Zonen auf höhere Vertrauenszonen einschränken.

Jede Zone wird durch eine Grenze vertrauen die gepunktete rote Linie im Diagramm vermerkt ist getrennt. Es stellt einen Übergang dar Daten und Informationen von einer Quelle zu einem anderen. Während dieses Übergangs kann die Daten Spoofing, Tampering abstreiten, Offenlegung von Informationen, Denial of Service und Erhöhung von Berechtigungen (Schritt) unterliegen.

![IoT Sicherheitszonen](media/iot-security-architecture/iot-security-architecture-fig1.png) 

Innerhalb jedes verwendeten Komponenten unterliegen auch Schritt ermöglicht eine vollständige 360 Bedrohungsmodelle Anzeigen der Lösung. Die folgenden Abschnitte erläutern aller Komponenten und bestimmte Sicherheitsaspekte und Solutions eingeführt werden sollten.

In den folgenden Abschnitten besprechen Standardkomponenten typischerweise in diesen Zonen.

### <a name="the-device-zone"></a>Bereich Geräte

Geräteumgebung wird sofort Raum um das Gerät bei physischen Zugriff oder "LAN" Peer-to-Peer digitalen Zugriff auf das Gerät möglich ist. "Lokales Netzwerk" wird zu einem Netzwerk, die distinct und – isoliert ist jedoch potenziell bridged, Internetzugang und alle eine drahtlose Technologie, die Peer-to-Peer-Kommunikation von Geräten ermöglicht. Es ist *nicht* enthalten die Illusion eines lokalen Netzwerks Netzwerk Virtualisierungstechnologie und bietet auch nicht öffentlichen Betreiber Netzwerk zwei Geräte zum öffentlichen Netzwerk kommunizieren erfordert, wären sie eine Peer-to-Peer-kommunikationsbeziehung eingeben.

### <a name="the-field-gateway-zone"></a>Feld Gateway Zone

Feld Gateway ist eine Gerät-Appliance oder allgemeiner Server Computersoftware, die als Kommunikation Enabler und möglicherweise als Steuerung Gerät Gerät Datenverarbeitung Hub dient. Feld Gateway Zone enthält das Feld Gateway und alle Geräte, die damit verbunden sind. Wie der Name schon sagt, Feld Gateways außerhalb dedizierte Datenverarbeitungsanlagen fungieren, sind in der Regel gebunden werden möglicherweise physische Intrusion und werden betriebliche Redundanz begrenzt ist. Um alles zu sagen, dass ein Feld Gateway häufig etwas kann berühren und sabotage wissen, was ihre Funktion ist. 

Feld Gateway unterscheidet nur Verkehr Routers aktiv verwalten Zugriff hat und Informationsfluss, also eine Anwendung Entität und Netzwerk Verbindung oder Terminalserver-Sitzung behandelt. Ein NAT-Gerät oder eine Firewall im Gegensatz dazu gelten nicht als Feld Gateways nicht explizite Verbindung oder Sitzung Terminals jedoch eher Route (oder Block) angeschlossen sind oder gemacht. Das Feld Gateway hat zwei unterschiedliche Bereiche. Zeigt die angeschlossenen Geräten und stellt innerhalb der Zone und der externe beteiligten Flächen und dem Rand der Zone.   

### <a name="the-cloud-gateway-zone"></a>Die Cloud Gateway zone

Cloud-Gateway ist ein System, die Remotekommunikation aus und Geräte oder Feld Gateways von verschiedenen Seiten öffentliches Netzwerk Raum normalerweise zu einer Cloud-basierte Kontrolle und Analyse-System, eine Föderation solcher Systeme ermöglicht. In einigen Fällen möglicherweise Cloud Gateway sofort spezielle Geräte von Terminals Tablets oder Telefone erleichtern. Im Kontext hier besprochenen "Cloud" soll an einen dedizierten Daten verweisen, die nicht am selben Standort wie die angeschlossenen Geräte oder Feld Gateways gebunden ist. Eine Zone Cloud Betriebskennzahlen gezielte physischen Zugriff verhindern und nicht notwendigerweise eine "Öffentliche Cloud" Infrastruktur ausgesetzt.  

Cloud-Gateway kann möglicherweise ein Netzwerk Virtualisierung Überlagerung Cloud Gateway und alle angeschlossenen Geräte oder Feld Gateways von anderem Netzwerkverkehr isolieren zugeordnet werden. Das Cloud Gateway ist ein Gerätesystem noch einer Verarbeitung weder Aufbewahrungsort für Daten. die Fertigungseinrichtungen Schnittstelle mit Cloud-Gateway. Die Cloud Gateway Zone umfasst Cloud Gateway selbst sowie alle Gateways Feld direkt oder indirekt angeschlossen. Rand der Zone ist eine unterschiedliche Oberfläche, alle externe Parteien über kommunizieren.

### <a name="the-services-zone"></a>Bereich services

"Dienst" ist für diesen Kontext Softwarekomponente oder Modul mit Geräten über ein Feld oder Cloud Gateway für die Datensammlung und-Analyse sowie für die Steuerung eine Schnittstelle definiert.  Vermittler sind. Sie handeln unter ihrer Identität Gateways und andere Subsysteme, speichern und Analysieren von Daten, autonom Befehle Geräte basierend auf Daten Einblicke oder Zeitpläne Informationen und Funktionen für autorisierte Endbenutzer steuern.

### <a name="information-devices-vs-special-purpose-devices"></a>Informationen-Geräte und spezielle Geräte

PCs, Smartphones und Tablets sind hauptsächlich interaktive Geräte. Telefone und Tablet-PCs sind explizit auf die Maximierung der Lebensdauer optimiert. Sie deaktivieren Sie vorzugsweise teilweise Umgang nicht direkt mit einer Person oder bei nicht-Dienste wie Musik oder Besitzer an einem bestimmten Ort führen. Systeme im Hinblick auf handeln diese informationstechnischen Geräten hauptsächlich als Proxys für Menschen. "Menschen Aktuatoren" Aktionen und "Personen Sensoren" Erfassen von Eingaben vorgeschlagen werden. 

Spezielle Geräte unterscheiden sich von einfachen Temperatursensoren Produktionslinien mit Tausenden von Komponenten, komplexe Factory. Diese Geräte sind weitaus Zweck beschränkt und auch wenn sie eine Benutzeroberfläche bereitstellen, sind weitgehend mit Gültigkeitsbereich oder in der physischen Welt integriert werden. Sie messen und Umwelt Umständen melden, Ventile aktivieren Servos steuern, sound Alarm, schalten Sie Lichter und viele andere Aufgaben. Sie helfen arbeiten, für die ein Gerät Informationen zu Allgemein, zu teuer, zu groß oder zu spröde ist. Der konkrete Zweck bestimmt sofort ihre technische als auch das verfügbare Budget für ihrer Produktion und geplanten Lebensdauer. Die Kombination dieser zwei Faktoren eingeschränkt verfügbaren betrieblichen energiebudget Platzbedarf und so Speicherplatz, Compute und Sicherheitsfunktionen.  

Wenn etwas "schief geht" mit automatisierten oder remote steuerbare Mängel beispielsweise physische Mangelfreiheit oder Steuerelementlogik vorsätzliche unberechtigten Zugriff und Manipulation. Viele Produktion zerstört Gebäude geplündert oder verbrannt werden und Benutzer möglicherweise verletzt oder sogar Die. Dies ist natürlich eine ganz andere Klasse Schaden als maxing einer gestohlenen Kreditkarte Limit. Das Sicherheitsniveau für Geräte, die Dinge bewegen und für Sensordaten schließlich in Befehle, die Dinge zu bewegen, muss im e-Commerce oder Szenarium übersteigen. 


### <a name="device-control-and-device-data-interactions"></a>Steuerung und Gerät Dateninteraktionen

Spezielle Geräte haben zahlreiche mögliche Interaktion Flächen und Interaktionsmuster, die einen Rahmen für den sicheren Zugriff auf diese Geräte digitale berücksichtigt werden müssen. Der Begriff "digital Access" wird hier verwendet Vorgänge unterscheiden, die durch direkte geräteinteraktion ausgeführt werden, in denen Sicherheit über physischen Zugriff Steuerelement bereitgestellt. Z. B. Inverkehrbringen das Gerät in einem Raum mit einer Tür. Beim Zugang verweigert werden kann, mit Software und Hardware können Maßnahmen führende System Eingriff physischen Zugriff verhindern. 
 
Erfahren die Interaktionsmuster betrachten "Device Control" und "Gerätedaten" mit derselben Aufmerksamkeit beim Erstellen von Bedrohungsmodellen wir. "Device Control" kann Informationen klassifiziert werden, die ein Gerät Parteien mit dem Ziel ändern oder beeinflussen das Verhalten Zustand oder den Zustand der Umgebung bereitgestellt wird. "Gerätedaten" können Informationen klassifiziert werden, die ein Gerät an eine andere Partei über den Zustand und die beobachtete Umgebung ausgibt. 

## <a name="threat-modeling-the-azure-iot-reference-architecture"></a>Bedrohungsmodelle Azure IoT-Referenzarchitektur

Microsoft verwendet das Framework beschriebenen, Azure IoT Modellierung Bedrohung. Im Abschnitt verwenden wir daher konkrete Beispiel Azure IoT Referenzarchitektur wie Bedrohung IoT Modellierung denken und wie Gefahren identifiziert. In unserem Fall stellten wir vier Hauptbereiche konzentrieren:

-   Geräte und Quellen
-   Datenübertragung,
-   Gerät und Ereignisse zu verarbeiten, und
-   Präsentation

![Bedrohungsmodelle für Azure IoT](media/iot-security-architecture/iot-security-architecture-fig2.png) 

Im folgenden Diagramm bietet eine vereinfachte Ansicht des IoT-Architektur von Microsoft ein Datenflussdiagramm Modell, die von Microsoft Threat Modeling Tool verwendet:

![Bedrohungsmodelle für Azure IoT mit MS Threat Modeling Tool](media/iot-security-architecture/iot-security-architecture-fig3.png)

Es ist wichtig zu beachten, dass die Architektur Funktionen Gerät und Gateway trennt. Dies ermöglicht Gatewaygeräte genutzt, die mehr Sicherheit: sie können mit Cloud Gateway über sichere Protokolle erfordern in der Regel mehr Verarbeitungsaufwand, systemeigene Device - wie ein - selbst bereitstellen konnte. In der Zone Azure Services gehen wir Cloud Gateway Service Azure IoT Hub dargestellt wird.

### <a name="device-and-data-sourcesdata-transport"></a>Geräte- und Quellen/Datenübertragung

Dieser Abschnitt untersucht die Architektur durch die Linse des Erstellens von Bedrohungsmodellen beschriebenen und gibt einen Überblick darüber, wie wir einige der inhärenten Probleme behandeln. Wir konzentrieren uns auf die Kernelemente eines Bedrohungsmodells:

- Prozesse (die unter unserer Kontrolle und externe Elemente)
- Kommunikation (auch Datenströme genannt)
- Speicher (auch als Datenspeicher)

#### <a name="processes"></a>Prozesse

In die Kategorien in Azure IoT Architektur beschrieben versuchen wir eine Anzahl von unterschiedlichen Risiken in den verschiedenen Stufen mindern Data-Informationen in existiert: Prozess, Übermittlung und Speicherung. Im folgenden geben wir eine Übersicht über die häufigsten Kategorie "Prozess", gefolgt von einem Überblick darüber, wie diese am besten verringert werden konnte: 

**Spoofing (S)**: Angreifer kann kryptografische Schlüssel Material von einem Gerät auf Software oder Hardware, extrahieren und anschließend Zugriff auf das System mit einem anderen physischen oder virtuellen Gerät unter der Identität des Geräts das Schlüsselmaterial stammen aus. Ein gutes Beispiel ist Fernbedienungen, die kann jedes Fernsehgerät und beliebte Hackers Tools sind.

**Denial of Service (D)**: ein Gerät nicht funktioniert oder Kommunikation stören Funkfrequenzen oder schneiden Drähte gerendert werden kann. Beispielsweise Berichten eine Überwachungskamera, die die Stromversorgung oder Netzwerk Verbindung absichtlich schlug nicht Daten zu.

**Manipulation (T)**: Angreifer kann ganz oder teilweise ersetzen die Software auf dem Gerät und damit ersetzende Software der Echtheit des Geräts nutzen, wenn das Schlüsselmaterial oder kryptografischen Funktionen halten wichtige Materialien zur unerlaubten Programm. Ein Angreifer kann z. B. extrahierten Schlüsselmaterial abzufangen und Daten vom Gerät auf den Kommunikationspfad unterdrücken und Ersetzen mit falschen Daten mit gestohlenen Schlüsselmaterial authentifiziert nutzen.

**Informationsfreigabe (I)**: manipulierte Software läuft der Gerät konnte manipulierte Software möglicherweise Daten an unbefugte auslaufen. Beispielsweise kann ein Angreifer extrahierten Schlüsselmaterial selbst in die Kommunikationswege zwischen Gerät und Controller oder Feld Gateway oder Cloud Gateway Informationen Abschöpfen injizieren nutzen.

**Erhöhung von Berechtigungen (E)**: ein Gerät, das bestimmte Funktion kann gezwungen, etwas anderes tun. Ein Ventil programmiert halb öffnen kann beispielsweise dazu verleitet werden, um ganz zu öffnen.

| **Komponente** | **Bedrohung** | **Minderung**                                                                                                                                                | **Risiko**                                                                                                                                                                                                    | **Implementierung**                                                                                                                                                                                                                                                                                                                                     |
|---------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gerät        | S          | Das Gerät Identität zuweisen und das Gerät authentifiziert                                                                                                | Gerät oder Teil des Geräts ersetzt durch ein anderes Gerät. Wie wissen wir, geht mit dem richtigen Gerät?                                                                                           | Gerät mit Transport Layer Security (TLS) oder IPSec-Authentifizierung. Infrastruktur unterstützt mit vorinstallierten Schlüssel (PSK) auf die Geräte, die vollständige asymmetrische Kryptografie behandeln kann. Nutzung von Azure AD [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt)                             |
|               | TRID       | Zuweisen Sie nicht manipulierbaren Mechanismen Gerät beispielsweise dadurch schwer zu Schlüsseln und andere kryptografische Material vom Gerät extrahiert. | Besteht die Gefahr, wenn jemand das Gerät (physische Störung) Manipulationen ist. Wir sind sicher, dass Gerät nicht manipuliert.                                                                                 | Die effektivste Minderung ist eine vertrauenswürdige Platform Module (TPM) Funktion, die Speichern von Schlüsseln in spezielle integrierte Schaltung aus dem Schlüssel können nicht gelesen werden ermöglicht, aber nur für kryptografische Operationen, die die Taste jedoch niemals offen legen den Schlüssel verwendet werden. Verschlüsselung Speicher des Geräts. Schlüsselmanagement für das Gerät. Signieren des Codes. |
|               | E          | Zugriffskontrolle des Geräts aufgetreten. Autorisierungsschema.                                                                                                    | Lässt das Gerät für die einzelnen Aktionen auf Befehle von einer externen Quelle oder sogar gefährdeten Sensoren, können Angriff Operationen nicht zugreifen. | Dass Authentifizierungsschema für das Gerät                                                                                                                                                                                                                                                                                                             |
| Feld Gateway | S          | Authentifizierung Feld Gateway Cloud Gateway (Cert basiert, PSK, Anspruch,...)                                                                           | Wenn jemand Feld Gateway unzulässigerweise verwenden kann, können sie sich wie andere Geräte darstellen.                                                                                                                               | TLS RSA/PSK, IPSe, [RFC 4279](https://tools.ietf.org/html/rfc4279). Dieselben Speicher- und Bescheinigung Fragen von Geräten im Allgemeinen – beste ist Anwendungsfall TPM. 6LowPAN-Erweiterung für IPSec Wireless Sensor Networks (WSN) unterstützen.                                                                                                              |
|               | TRID       | Schützen Sie das Feld Gateway gegen Manipulationen (TPM)?                                                                                                            | Spoofing-Angriffe, die Kommunikation mit Feld Gateway Cloud Gateway denken trick führen Offenlegung und Datenmanipulation                                                             | Speicher-Verschlüsselung TPM, Authentifizierung.                                                                                                                                                                                                                                                                                                              |
|               | E          | Zugriffssteuerungsmechanismus Feld Gateway                                                                                                                    |                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                        |

Einige Beispiele von Risiken dieser Kategorie:

Spoofing: Angreifer kann extrahieren kryptografischen Schlüsseln von einem Gerät auf Software oder Hardware-Ebene und dann Zugriff auf das System mit einem anderen physischen oder virtuellen Gerät unter der Identität des Geräts das Schlüsselmaterial entnommen wurde.

**Denial of Service**: ein Gerät nicht funktioniert oder Kommunikation stören Funkfrequenzen oder schneiden Drähte gerendert werden kann. Beispielsweise Berichten eine Überwachungskamera, die die Stromversorgung oder Netzwerk Verbindung absichtlich schlug nicht Daten zu.

**Tampering**: Angreifer kann ganz oder teilweise ersetzen die Software auf dem Gerät und damit ersetzende Software der Echtheit des Geräts nutzen, wenn das Schlüsselmaterial oder kryptografischen Funktionen halten wichtige Materialien zur unerlaubten Programm.

**Tampering**: konnte eine Überwachungskamera, die ein Bild sichtbare Spektrum des leeren Flur zeigt ein Foto von einem Gang abzielen. Rauch oder Feuer Sensor konnte jemandem einen leichteren darunter melden. In jedem Fall das Gerät möglicherweise technisch voll vertrauenswürdigen System jedoch meldet manipulierten Informationen.

**Tampering**: Angreifer nutzen extrahierten Schlüsselmaterial abzufangen und Daten vom Gerät auf den Kommunikationspfad unterdrücken und Ersetzen mit falschen Daten mit gestohlenen Schlüsselmaterial authentifiziert werden.

**Tampering**: Angreifer kann ganz oder teilweise ersetzen die Software auf dem Gerät und damit ersetzende Software der Echtheit des Geräts nutzen, wenn das Schlüsselmaterial oder kryptografischen Funktionen halten wichtige Materialien zur unerlaubten Programm.
   
**Offenlegung von Informationen**: manipulierte Software läuft der Gerät konnte manipulierte Software möglicherweise Daten an unbefugte auslaufen.

**Offenlegung von Informationen führen**: Angreifer nutzen extrahierten Schlüsselmaterial selbst in Kommunikationspfad zwischen dem Gerät und einem Controller oder Feld Gateway oder Cloud Gateway Abschöpfen Informationen eingefügt.

**Denial of Service**: das Gerät kann werden ein- oder ausgeschaltet in einem Modus ist Kommunikation nicht möglich (Dies ist in vielen industrielle Maschinen beabsichtigt).

**Tampering**: das Gerät umkonfiguriert werden in einem unbekannten Steuerung (außerhalb bekannte Kalibrierungsparameter) arbeiten und damit Daten fehlinterpretiert werden können

**Elevation of Privilege**: ein Gerät, das bestimmte Funktion kann gezwungen, etwas anderes tun. Ein Ventil programmiert halb öffnen kann beispielsweise dazu verleitet werden, um ganz zu öffnen.

**Denial of Service**: das Gerät in einen Zustand, Kommunikation nicht kann, aktiviert werden.

**Tampering**: das Gerät umkonfiguriert werden in einem unbekannten Steuerung (außerhalb bekannte Kalibrierungsparameter) arbeiten und damit Daten fehlinterpretiert werden können.
 
**Spoofing, Tampering/Ablehnung**: Wenn nicht gesichert (also nur selten bei Consumer-Fernbedienungen) Angreifer kann den Status eines Geräts anonym bearbeiten. Ein gutes Beispiel ist Fernbedienungen, die kann jedes Fernsehgerät und beliebte Hackers Tools sind.

#### <a name="communication"></a>Kommunikation

Gefahren um Kommunikationspfad zwischen Geräten, und Feld-Gateways und Gerät und Cloud-Gateway. Die Tabelle hat einige Richtlinien um offene Sockets auf dem Gerät/VPN:

| **Komponente**               | **Bedrohung** | **Minderung**                                      | **Risiko**                                                                                                      | **Implementierung**                                                                                                                                                                                                                                                                                                                                                               |
|-----------------------------|------------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gerät IoT Hub              | TID        | (D) TLS (PSK/RSA) zur Verschlüsselung des Datenverkehrs             | Abhören oder Störung der Kommunikation zwischen dem Gerät und gateway                             | Sicherheit auf Protokollebene. Benutzerdefinierte Protokolle müssen wir herausfinden, wie Sie sie schützen. In den meisten Fällen erfolgt die Kommunikation vom Gerät IoT Hub (Gerät initiiert die Verbindung).                                                                                                                                                                 |
| Gerät               | TID        | (D) TLS (PSK/RSA) zur Verschlüsselung des Datenverkehrs.            | Lesen von Daten zwischen Geräten übertragen. Manipulation der Daten. Überladen mit neuen Verbindungen | Sicherheit auf der Protokollebene (MQTT/AMQP/HTTP/CoAP. Benutzerdefinierte Protokolle müssen wir herausfinden, wie Sie sie schützen. Die Abhilfe für DoS Bedrohung ist Geräte über ein Gateway Cloud oder Feld und lassen nur handeln als Clients auf dem Netzwerk. Die peering eine direkte Verbindung zwischen den Peers nach müssen vom Gateway vermittelt wurde möglicherweise |
| Externe Entität Gerät      | TID        | Starke Kopplung der externen Entität, die das Gerät | Die Verbindung zum Gerät abhören. Die Kommunikation stören mit dem Gerät                     | Sichere Verbindung externe Entität auf LE NFC-Bluetooth-Gerät. Steuern der Bedienfeld des Geräts (physisch)                                                                                                                                                                                                                                                  |
| Feld Gateway Cloud Gateway | TID        | TLS (PSK/RSA) zur Verschlüsselung des Datenverkehrs.               | Abhören oder Störung der Kommunikation zwischen dem Gerät und gateway                             | Sicherheit auf der Protokollebene (MQTT/AMQP/HTTP/CoAP). Benutzerdefinierte Protokolle müssen wir herausfinden, wie Sie sie schützen.                                                                                                                                                                                                                                                       |
| Gerät Cloud Gateway        | TID        | TLS (PSK/RSA) zur Verschlüsselung des Datenverkehrs.               | Abhören oder Störung der Kommunikation zwischen dem Gerät und gateway                             | Sicherheit auf der Protokollebene (MQTT/AMQP/HTTP/CoAP). Benutzerdefinierte Protokolle müssen wir herausfinden, wie Sie sie schützen.                                                                                                                                                                                                                                                       |

Einige Beispiele von Risiken dieser Kategorie:

**Denial of Service**: eingeschränkte Geräte sind im Allgemeinen DoS bedroht Wenn sie für eingehende Verbindungen oder unerwünschte Datagramme in einem Netzwerk aktiv zuhören Angreifer öffnen kann viele Verbindungen parallel und nicht service oder sehr langsam service oder sein mit unerwünschten Datenverkehr. In beiden Fällen kann das Gerät effektiv im Netzwerk nicht mehr gerendert werden.

**Spoofing, Offenlegung von Informationen**: eingeschränkte und spezielle oft eine für alle Sicherheitsfunktionen wie Kennwort oder PIN Schutz, oder sie vollständig vertrauen Netzwerk, d. h. sie gewährt Zugriff auf Informationen, wenn ein Gerät im gleichen Netzwerk und das Netzwerk nur durch einen gemeinsamen Schlüssel geschützt abhängig. Dies bedeutet, dass geheime Gerät oder Netzwerk weitergegeben werden, es möglich ist, das Gerät steuern oder beobachten Daten vom Gerät ausgegeben.  

**Spoofing**: Angreifer kann abfangen teilweise Übertragung überschreiben oder fälschen den Absender (Mann in der Mitte)

**Tampering**: Angreifer kann abfangen teilweise Übertragung überschreiben oder falsche Informationen 

**Offenlegung:** Angreifer möglicherweise Broadcast abzuhören und Informationen ohne Autorisierung **Denial of Service:** Angreifer kann die Signale jam und Verteilung verweigern

#### <a name="storage"></a>Speicher

Jedes Gerät und Feld Gateway hat einige Speicher (temporär Queuing Daten, os-Speicherung).

| **Komponente**                            | **Bedrohung** | **Minderung**                       | **Risiko**                                                                                                                                                                                                                                                                                                                | **Implementierung**                                                                                                                                                     |
|------------------------------------------|------------|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Speicher des Geräts                           | TRID       | Speicherverschlüsselung Signieren der Protokolle | Lesen von Daten aus dem Speicher (PII-Daten), Verfälschung von Daten. Manipulieren der Warteschlange oder Befehlsdaten zwischengespeichert. Konfiguration oder Firmware-Pakete manipuliert kann zwischengespeichert oder in der lokalen Warteschlange OS und/oder Komponenten beeinträchtigt werden                                         | Verschlüsselung Nachrichtenauthentifizierungscode (MAC) oder Signatur. Wo möglich, sichere Zugriff über Zugriff auf Ressourcen steuern Zugriffssteuerungslisten oder Berechtigungen. |
| Bildtyp OS                          | TRID       |                                      | Verfälschung von OS / OS Komponenten ersetzen                                                                                                                                                                                                                                                                         | Schreibgeschützte Betriebssystempartition signiert Betriebssystemabbild, Verschlüsselung                                                                                                                    |
| Feld Gateway Speicher (queuing Daten) | TRID       | Speicherverschlüsselung Signieren der Protokolle | Lesen von Daten aus dem Speicher (PII-Daten), Verfälschung von Daten, Manipulationen Warteschlange oder Befehlsdaten zwischengespeichert. Manipulationen mit Konfiguration oder Firmware Update (bestimmte Geräte oder Feld Gateway) kann zwischengespeichert oder in der lokalen Warteschlange OS und/oder Komponenten beeinträchtigt werden | BitLocker                                                                                                                                                              |
| Feld Gateway Betriebssystemabbilds                   | TRID       |                                      | Verfälschung von OS / OS Komponenten ersetzen                                                                                                                                                                                                                                                                          | Schreibgeschützte Betriebssystempartition signiert Betriebssystemabbild, Verschlüsselung                                                                                                                    |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Gerät und Ereignis verarbeiten Cloud Gateway zone

Ein Cloud-Gateway ist Remotekommunikation aus und Geräte oder Feld Gateways von verschiedenen Seiten öffentliches Netzwerk Raum normalerweise zu einer Cloud-basierte Kontrolle und Analyse-System, eine Föderation solcher Systeme ermöglicht. In einigen Fällen möglicherweise Cloud Gateway sofort spezielle Geräte von Terminals Tablets oder Telefone erleichtern. Im Kontext hier besprochenen "Cloud" soll eine dedizierte Datenverarbeitung auf, die dem gleichen Standort wie die angeschlossenen Geräte oder Feld Gateways gebunden ist und Betriebskennzahlen, verhindern Zugang vorgesehen, aber nicht "öffentlichen Cloud" Infrastruktur.  Cloud-Gateway kann möglicherweise ein Netzwerk Virtualisierung Überlagerung Cloud Gateway und alle angeschlossenen Geräte oder Feld Gateways von anderem Netzwerkverkehr isolieren zugeordnet werden. Das Cloud Gateway ist ein Gerätesystem noch einer Verarbeitung weder Aufbewahrungsort für Daten. die Fertigungseinrichtungen Schnittstelle mit Cloud-Gateway. Die Cloud Gateway Zone umfasst Cloud Gateway selbst sowie alle Gateways Feld direkt oder indirekt angeschlossen.

Cloud Gateway ist meist benutzerdefinierte erstellte Software als Dienst mit verfügbar gemachten Endpunkte mit denen Feld Gateway und Geräte verbinden. So müssen sie Sicherheit entwickelt. Führen Sie [SDL](http://www.microsoft.com/sdl) -Prozess zum Entwerfen und Erstellen dieser Dienst. 

#### <a name="services-zone"></a>Services-zone

Ein Verwaltungssystem (oder Controller) ist eine softwarelösung, die mit einem Gerät oder Feld Gateway oder Cloud Gateway für ein oder mehrere Geräte oder für sammeln oder speichern und Analysieren von Daten für die Präsentation oder nachfolgende Kontrolle. Sind die einzigen Entitäten im Rahmen dieser Diskussion, die unmittelbar mit Menschen erleichtern. Die Ausnahme sind intermediate Steuerelement Oberflächen auf Geräten wie ein Schalter zu deaktivieren Sie das Gerät oder andere Eigenschaften zu ändern und für die es keine funktionale Entsprechung Digital zugegriffen werden kann. 

Intermediate Steuerelement Oberflächen sind, jede Art von Logik EZB schränkt die Funktion des Steuerelements Oberfläche, eine entsprechende Funktion Remote eingeleitet werden kann oder input Konflikte mit remote vermieden sind solche vermittelten Ruder konzeptionell angeschlossen an eine lokale, die dieselben Basisfunktionen wie jede andere Fernbedienung nutzt die parallel Gerät zugeordnet werden kann. Sicherheitsrisiken für das Cloud-computing, [Cloud Security Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) Seite gelesen werden kann.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

Finden Sie in folgendem Artikel Weitere Informationen:

- [SDL Threat Modeling Tool](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
- [Microsoft Azure IoT Referenzarchitektur](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)
 
