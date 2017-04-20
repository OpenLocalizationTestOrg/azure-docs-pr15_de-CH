
# <a name="azure-and-internet-of-things"></a>Azure und das Internet der Dinge

Willkommen bei Microsoft Azure und das Internet der Dinge (IoT). Dieser Artikel stellt eine IoT Lösungsarchitektur, die gemeinsame Merkmale einer IoT-Lösung beschreibt, die Sie mithilfe von Azure Services bereitstellen können. IoT-Lösungen sicherer, bidirektionale Kommunikation zwischen Geräten Nummerierung möglicherweise Millionen und Back-End Lösung. Z. B. können Lösung Back-End automatisierte, prognostische Analysen Einblicke aus Ihrem Gerät Cloud aufzudecken.

Azure IoT Hub ist ein Schlüssel beim Implementieren dieses IoT Lösungsarchitektur Azure Services verwenden. IoT Suite bietet vollständige, End-to-End-Implementierung dieser Architektur für Szenarien IoT. Zum Beispiel: 

- Die *Remoteüberwachung* Lösung können Sie den Status von Geräten wie Automaten. 
- *Vorbeugende Wartung* -Lösung hilft Ihnen Wartung Bedarf Geräte wie Pumpen in entfernten pumpstationen und ungeplante Ausfallzeiten zu vermeiden.

## <a name="iot-solution-architecture"></a>IoT Lösungsarchitektur

Das folgende Diagramm zeigt eine normale IoT Lösungsarchitektur. Das Diagramm enthält keine Namen von bestimmten Azure Dienste jedoch beschreibt die wichtigsten Elemente einer generischen IoT Lösungsarchitektur. In dieser Architektur Datenerfassung IoT-Geräte, die sie an ein Cloud-Gateway senden. Cloud-Gateway stellt die Daten zur Verarbeitung durch andere Back-End-Dienste, Daten andere Line-of-Business-Anwendung oder Bedienpersonal über ein Dashboard oder andere präsentationsgerät übermittelt werden.

![IoT Lösungsarchitektur][img-solution-architecture]

> [AZURE.NOTE] Eine ausführliche Erläuterung der IoT-Architektur finden Sie unter [Microsoft Azure IoT Referenzarchitektur][lnk-refarch].

### <a name="device-connectivity"></a>Device-Konnektivität

In dieser Architektur IoT Lösung senden Geräte Telemetrie wie Sensorwerte Pumpen entfernt an einen Endpunkt Cloud gespeichert und verarbeitet. In einem wartungsszenario vorbeugende können Back-End den Datenstrom Sensor um zu ermitteln, wann eine Pumpe Wartung benötigt. Geräte können auch erhalten und Lesen von Nachrichten von einem Endpunkt Cloud Cloud Gerät Befehle reagieren. Beispielsweise kann im wartungsszenario vorbeugende Lösung Back-End Befehle an andere Pumpen in der Pumpen beginnen erneutes Flows vor Wartung soll sicherstellen, dass die Wartungstechniker beginnen sie kommt.

Die größten Probleme IoT Projekte gehört wie zuverlässig und sicher zu Lösung Geräte. IoT-Geräte haben unterschiedliche Eigenschaften gegenüber anderen Kunden wie Browser und apps. IoT-Geräte:

- Sind oft eingebettete Systeme menschlichen Operator.
- An remote-Standorten einsetzbar teuer der physische Zugriff ist.
- Kann nur über Back-End Lösung erreicht werden. Es gibt keine andere Möglichkeit zur Interaktion mit dem Gerät.
- Möglicherweise macht und Ressourcen besitzen.
- Möglicherweise zeitweise langsame und teure Netzwerkkonnektivität.
- Müssen eigene, benutzerdefinierte oder branchenspezifische Anwendungsprotokolle verwenden.
- Können mithilfe einer Reihe von gängigen Hardware- und Softwareplattformen.

Neben den oben genannten Vorschriften muss IoT-Lösung auch skalieren, Sicherheit und Zuverlässigkeit bieten. Die resultierende Konnektivitätsanforderungen ist schwierig und zeitaufwändig herkömmliche Technologien wie Web-Container und messaging Broker. Azure IoT Hub und IoT Geräte-SDKs erleichtern Lösungen, die diese Anforderung erfüllen.

Ein Gerät kommunizieren direkt mit einem Cloud Gateway oder wenn das Gerät die Kommunikationsprotokolle nicht möglich, die Cloud-Gateway unterstützt verwenden, können Sie durch ein Zwischengateway verbinden. Das [Azure IoT Protokollgateway] [ lnk-protocol-gateway] können übersetzen Protokoll Wenn Geräte Protokolle nicht verwenden können, die IoT Hub unterstützt.

### <a name="data-processing-and-analytics"></a>Verarbeitung der Daten und Analysen

In der Cloud ist ein IoT Lösung wieder beenden die meisten der Datenverarbeitung, Filtern und aggregieren Telemetrie routing zu anderen Diensten. IoT-Lösung wieder beenden:

- Ihre Geräte Telemetrie Ebene empfängt und bestimmt, wie Sie die Daten speichern. 
- Können Sie Befehle aus der Cloud auf Gerät senden.
- Enthält Funktionen, mit denen Sie auf Geräte bereitstellen und auf welche Geräte Ihrer Infrastruktur herstellen dürfen Registrierung.
- Können Sie den Status Ihrer Geräte und ihre Aktivitäten überwachen.

Im wartungsszenario vorbeugende speichert Lösung Backend historisch Telemetriedaten. Diese Daten können Back-End Muster identifizieren, die Wartung auf eine Pumpe ist.

IoT-Lösungen können automatische Rückkopplungsschleifen enthalten. Z. B. kann ein Analysemodul im Back-End von Telemetrie identifizieren, die die Temperatur eines bestimmten Geräts über normale Betriebslevel. Die Lösung kann einen Befehl an das Gerät anweist, Maßnahmen senden.

### <a name="presentation-and-business-connectivity"></a>Präsentations- und Konnektivität

Die Konnektivitätsebene Präsentations- und ermöglicht Endbenutzern IoT-Lösung und die Geräte interagieren. Es ermöglicht Benutzern anzeigen und Analysieren der Daten von ihren Geräten. Diese Ansichten haben die Form eines Dashboards oder BI-Berichten, die beide Verlaufsdaten anzeigen kann oder in Echtzeit. Beispielsweise kann ein Operator den Status eines bestimmten pumpstation und Alarme durch das System ausgelöst. Diese Ebene ermöglicht auch Integration IoT Backend-Lösung mit vorhandenen Line of Business Applications in Geschäftsprozessen des Unternehmens oder Workflows binden. Beispielsweise kann vorbeugende wartungslösung scheduling System integrieren, die Bücher Ingenieur pumpende Station besuchen, wenn die Lösung eine Pumpe benötigen Wartung bezeichnet.

![IoT lösungsdashboard][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
