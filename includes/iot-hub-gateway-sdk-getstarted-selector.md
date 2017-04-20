> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

Dieser Artikel enthält eine detaillierte Erläuterung der [Beispielcode Hello World] [ lnk-helloworld-sample] veranschaulichen die grundlegenden Komponenten von [Azure IoT Gateway SDK] [ lnk-gateway-sdk] Architektur. Das Beispiel verwendet IoT Hub Gateway SDK einfache Gateway erstellen, die eine Meldung "Hello World" in einer Datei alle fünf Sekunden protokolliert.

Diese exemplarische Vorgehensweise umfasst:

- **Konzepte**: eine grundlegende Übersicht über die Komponenten, die Gateway verfassen IoT Gateway SDK erstellen.  
- **Hello World Beispielarchitektur**: Beschreibt die Konzepte wie zum Beispiel Hello World anwenden und wie die Komponenten zusammenwirken.
- **Wie zum Beispiel**: die Schritte zum Erstellen des Beispiels.
- **Wie das Beispiel ausgeführt**: die erforderlichen Schritte zum Ausführen des Beispiels. 
- **Normale Ausgabe**: ein Beispiel für die Ausgabe zu erwarten, wenn Sie das Beispiel ausführen.
- **Codeausschnitte**: eine Sammlung von Codeausschnitten angezeigt wie das Hello World-Beispiel wichtiges Komponenten implementiert.

## <a name="azure-iot-gateway-sdk-concepts"></a>Azure IoT Gateway SDK-Konzepte

Untersuchen Sie den Beispielcode und erstellen eigene Feld Gateway IoT Gateway SDK verwenden, sollten Sie die wichtigsten Konzepte verstehen, die die Architektur des SDK unterstützen.

### <a name="modules"></a>Module

Sie erstellen ein Gateway mit Azure IoT Gateway SDK erstellen und *Module*zusammenstellen. Module verwenden *Nachrichten* untereinander Daten austauschen. Ein Modul eine Nachricht empfängt, führt eine Aktion für diese optional eine neue Nachricht verwandelt und veröffentlicht, damit andere Module zu. Einige Module können nur neue Nachrichten und eingehende Nachrichten nicht verarbeiten. Eine Kette von Modulen erstellt eine Pipeline Datenverarbeitung mit jedes Modul eine Transformation der Daten einmal in dieser Pipeline durchführen.

![Eine Kette von Modulen Gateway Azure IoT Gateway SDK integriert][1]
 
Das SDK enthält Folgendes:

- Vorgefertigte Module common Gateway-Funktionen.
- Die Schnittstellen können Entwickler benutzerdefinierte Module schreiben.
- Infrastruktur bereitstellen und Ausführen eines Satzes von Modulen.

Das SDK bietet eine Abstraktionsschicht, die Gateways auf eine Vielzahl von Betriebssystemen und Plattformen erstellen kann.

![Azure IoT Gateway SDK Abstraktionsschicht][2]

### <a name="messages"></a>Nachrichten

Zwar denken Module Nachrichten sich bequem wie ein Gateway konzipieren, ist es nicht genau passiert. Mithilfe der Module einen Broker miteinander kommunizieren, Nachrichten Broker (Bus, Pubsub oder andere Messaging-Muster) veröffentlichen und Broker das Routing der Nachricht an die Module angeschlossen lassen.

Ein Module verwendet die **Broker_Publish** Funktion der Broker eine Nachricht veröffentlichen. Der Broker übermittelt Nachrichten an ein Modul durch eine Callback-Funktion aufrufen. Eine Nachricht besteht aus einem Satz von Schlüssel-Wert-Eigenschaften und den Inhalt als Speicherblock.

![Die Rolle der Broker in Azure IoT Gateway SDK][3]

### <a name="message-routing-and-filtering"></a>Nachrichtenrouting und Filtern

Es gibt zwei Arten leiten Nachrichten an die richtigen Module. Hyperlinks kann Broker übergeben werden, damit Broker Quelle und Empfänger für jedes Modul weiß, oder das Modul auf die Eigenschaften der Nachricht filtern. Soll die Meldung Es sollte ein Modul nur eine Nachricht handeln. Links und Nachrichtenfilter entsteht effektiv einer.

## <a name="hello-world-sample-architecture"></a>Hello World-Beispielarchitektur

Hello World-Beispiel veranschaulicht die im vorherigen Abschnitt beschriebenen Konzepte. Hello World-Beispiel implementiert einen Gateway, das eine Rohrleitung besteht aus zwei Modulen:

-   *Hello World* -Modul alle fünf Sekunden für eine Nachricht erstellt und Protokollierung Modul übergibt.
-   *Protokollierung* -Modul empfangenen Nachrichten in eine Datei geschrieben.

![Architektur von Azure IoT Gateway SDK integriert Hello World-Beispiel][4]

Wie im vorherigen Abschnitt beschrieben, wird Hello World-Modul nicht Nachrichten direkt an das Modul Protokollierung alle fünf Sekunden übergeben. In diesem Fall veröffentlicht sie eine Nachricht Broker alle fünf Sekunden.

Protokollierung-Modul empfängt die Nachricht von der Broker und fungiert, den Inhalt der Nachricht in eine Datei schreiben.

Protokollierung Modul verbraucht nur Nachrichten vom Broker, nie veröffentlicht neue Nachrichten auf der Broker.

![Wie der Broker leitet Nachrichten zwischen Azure IoT Gateway SDK][5]

Die obige Abbildung zeigt die Architektur von Hello World-Beispiel und die relativen Pfade zu den Quelldateien, die verschiedene Teile des Beispiels im [Repository]implementieren[lnk-gateway-sdk]. Untersuchen Sie den Code selbst, oder verwenden Sie Codeausschnitte unten als Leitfaden.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk