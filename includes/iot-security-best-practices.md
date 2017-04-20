# <a name="internet-of-things-security-best-practices"></a>Bewährte Methoden für die Sicherheit von Internet der Dinge

Sichern einer Internet der Dinge (IoT) erfordert Infrastruktur eine strenge Sicherheit-Strategie. Diese Strategie erfordert die Daten in der Cloud und Schutz der Datenintegrität bei der Übertragung über das Internet Bereitstellen von Geräten. Jede Ebene wird mehr Sicherheit in der gesamten Infrastruktur erstellt.

## <a name="secure-an-iot-infrastructure"></a>Sichere Infrastruktur IoT

Diese Sicherheit Verteidigungsstrategie kann entwickelt und aktiv an verschiedenen Akteure mit Produktion, Entwicklung und Bereitstellung von IoT-Geräten und Infrastruktur ausgeführt werden. Es folgt eine allgemeine Beschreibung der Spieler.  

- **IoT Hardware Hersteller/Integrator**: Normalerweise sind Hersteller IoT Hardware bereitgestellt, Integratoren Montage Hardware von verschiedenen Herstellern oder Lieferanten Hardware für eine Bereitstellung IoT hergestellt oder anderer Anbieter integriert.
- **Lösungsentwickler IoT**: ein IoT-Lösung wird normalerweise von einem Lösungsentwickler ausgeführt. Dieser Entwickler kann Teil ein oder diese Aktivität spezialisiert Systemintegrator (SI). Lösungsentwickler IoT kann verschiedene Komponenten IoT völlig entwickeln, Standard oder Open-Source-Komponenten integriert oder vorkonfigurierte Lösungen geringfügige Anpassung.
- **IoT Lösung Bereitsteller**: nach einem IoT-Lösung entwickelt, in das Feld bereitgestellt werden muss. Dies schließt die Bereitstellung von Hardware, Verbindung von Geräten und Bereitstellung in Hardware-Geräten oder die Cloud.
- **IoT Lösung Operator**: nach IoT-Lösung bereitgestellt, langfristige Vorgänge überwachen, Upgrades und Wartung erfordert. Dies kann durch ein erfolgen, die IT-Spezialisten Hardwareoperationen und und wartungsteams Domäne Spezialisten das Verhalten der gesamten IoT Infrastruktur überwachen umfasst.

Die folgenden Abschnitten best Practices für diesen Spieler zu entwickeln, bereitstellen und eine sichere Infrastruktur IoT betreiben.

## <a name="iot-hardware-manufacturerintegrator"></a>IoT Hardware Hersteller/integrator

Im folgenden werden best Practices für IoT Hardwareherstellern und Integratoren Hardware.

- **Bereich Hardware Mindestanforderungen**: Hardware-Design sollte die minimalen Eigenschaften für die Hardware, und nichts mehr. Ein Beispiel ist die USB-Anschlüsse sind nur bei Bedarf für den Betrieb des Geräts. Diese zusätzlichen Funktionen öffnen für unerwünschte Angriffsmethoden, die vermieden werden sollte.
- **Hardware fälschungssicher zu machen**: Mechanismen erkennen physischen Übergriffen oder Öffnen der geräteabdeckung ein Teil des Geräts erstellen. Diese Signale manipulieren kann Teil des Datenstroms in die Cloud, die Operatoren Ereignisse informiert hochgeladen werden.
- **Um sichere Hardware erstellen**: Wenn COGS ermöglicht Sicherheitsfunktionen wie Sicherheit und verschlüsselten Speicher erstellt bzw. Funktionen auf Trusted Platform Module (TPM) starten. Diese Funktionen machen Geräte sicherer und IoT Gesamtinfrastruktur schützen.
- **Dabei sicher**: Firmware-Upgrades während der Lebensdauer des Geräts sind unvermeidbar. Geräte mit sicheren für Upgrades und kryptografischen Sicherheit Firmware-Versionen können das Gerät während und nach dem Update sicher.

## <a name="iot-solution-developer"></a>IoT-Lösungsentwickler

Es folgen die best Practices für Entwickler IoT:

- **Führen Sie sichere Software Development-Methodik**: Entwicklung sicherer Software erfordert Boden denken über Sicherheit seit Beginn des Projekts bis seine Implementierung, Test und Bereitstellung. Die Auswahl von Plattformen, Sprachen und Tools werden mit dieser Methode beeinflusst. Microsoft Security Development Lifecycle bietet eine schrittweise Entwicklung sicherer Software.
- **Wählen Sie Open Source-Software mit Vorsicht**: Open Source-Software ermöglicht die schnelle Lösungen. Als Open Source-Software wählen, sollten Sie die Aktivitätsstufe der Gemeinschaft für jede Open Source-Komponente. Eine aktive Community wird sichergestellt, dass Software unterstützt wird und Probleme erkannt und behoben werden. Alternativ eine undurchsichtige und inaktive Open Source-Software möglicherweise nicht unterstützt und Probleme möglicherweise nicht erkannt.
- **Integration mit Vorsicht**: an der Grenze von Bibliotheken und APIs viele Software-Sicherheitslücken vorhanden sind. Funktionen, die nicht für die aktuelle Bereitstellung möglicherweise noch über eine API-Ebene. Allgemeine Sicherheit überprüfen Sie alle Schnittstellen für Sicherheitslücken integriert.      

## <a name="iot-solution-deployer"></a>IoT Lösung Bereitsteller

Folgende sind best Practices für IoT Lösung Anwender:

- **Hardware sicher bereitstellen**: IoT Installationen erfordern Hardware an unsicheren Speicherorten wie Räume oder unbeaufsichtigt Gebietsschemas bereitgestellt werden. In solchen Situationen, dass Hardware Bereitstellung manipulationssichere ist soweit. Wenn USB oder andere Ports auf Hardware verfügbar sind, sicherstellen Sie, dass sie sicher. Viele Angriffsmethoden können diese Einstiegspunkte verwenden.
- **Schützen Sie Authentifizierungsschlüssel**: während der Bereitstellung jedes Gerät benötigt Geräte-IDs sowie die zugehörigen Authentifizierungsschlüssel Cloud-Dienst generiert. Schützen Sie diese Schlüssel physisch auch nach der Bereitstellung. Ein gefährdeter Schlüssel von einem böswilligen Gerät lässt sich als ein vorhandenes Gerät auszugeben.

## <a name="iot-solution-operator"></a>IoT Lösung operator

Es folgen die best Practices für IoT Lösung Operatoren:

- **Das System aktuell halten**: sicherzustellen, dass Gerätebetriebssysteme und alle Treiber auf die neuesten Version aktualisiert werden. Wenn Sie automatische Updates in Windows 10 (IoT oder anderen SKUs) aktivieren, informiert Microsoft es, ein sicheres Betriebssystem für IoT-Geräte. Beibehalten von anderen Betriebssystemen (Linux) sichergestellt aktuell auch vor bösartigen Angriffen geschützt.
- **Schutz vor unberechtigten**: das Betriebssystem zulässt, installieren Sie die neuesten Viren und Malware-Funktionen auf jedem Gerät. Dadurch können die meisten externen Gefahren. Geeignete Maßnahmen, um Moderne Betriebssysteme vor Gefahren zu schützen.
- **Häufig Audit**: Überwachung IoT Infrastruktur für Sicherheit ist auf Sicherheitsvorfälle reagieren. Die meisten Betriebssysteme bieten integrierte Ereignisprotokoll häufig überprüfen, um sicherzustellen, dass keine Sicherheitslücken aufgetreten ist. Überwachungsinformationen kann gesendet werden als separate Telemetrie Stream zum Cloud-Dienst, wo diese analysiert werden.
- **IoT Infrastruktur physisch geschützt**: schlechtesten Sicherheitsangriffe auf IoT Infrastruktur sind mit physischen Zugriff auf Geräte gestartet. Eine wichtige Methode ist zum Schutz gegen böswillige Verwendung der USB-Ports und anderen physischen Zugriff. Ein Schlüssel zur Aufdeckung Verstöße aufgetreten sind möglicherweise ist der Zugang wie USB-Anschluss protokollieren. Windows 10 (IoT und anderen SKUs) können auch ausführliche Protokollierung dieser Ereignisse.
- **Cloud-Anmeldeinformationen schützen**: zur Konfiguration und den Betrieb einer Bereitstellung IoT Cloud-Anmeldeinformationen sind möglicherweise am einfachsten Zugriff und IoT-System. Schützen der Anmeldeinformationen durch das Kennwort häufig ändern und verwenden diese Anmeldeinformationen auf öffentlichen Computern.

Die Funktionen der verschiedenen IoT-Geräte variieren. Einige Geräte möglicherweise Betriebssysteme common desktop und einige Geräte unter den Betriebssystemen sehr leicht. Best Practices für die Sicherheit beschrieben möglicherweise zuvor für diese Geräte in unterschiedlichem. Sofern zusätzliche Sicherheit und Bereitstellung best Practices von den Herstellern dieser Geräte beachten.

Einige ältere und eingeschränkte Geräte möglicherweise nicht speziell für IoT Bereitstellung entwickelt haben. Diese Geräte möglicherweise die Funktion zum Verschlüsseln von Daten, Verbinden mit dem Internet oder bieten erweiterte Überwachung fehlen. In diesen Fällen ein modernes und sichereres Feld Gateway aggregieren Daten aus älteren Geräten und die Sicherheit für diese Geräte über das Internet Verbindung erforderlich. Feld Gateways bieten sichere Authentifizierung, Aushandlung der verschlüsselte Sitzung, Erhalt der Befehle aus der Cloud und viele andere Sicherheitsfunktionen.
