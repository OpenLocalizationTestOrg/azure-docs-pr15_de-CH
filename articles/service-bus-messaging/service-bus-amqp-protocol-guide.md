<properties 
    pageTitle="AMQP 1.0 in Azure Service Bus und Ereignis Hubs Protokoll Guide | Microsoft Azure" 
    description="Protokoll-Leitfaden für Ausdrücke und Beschreibung AMQP 1.0 in Azure Service Bus und Ereignis-Hubs" 
    services="service-bus,event-hubs" 
    documentationCenter=".net" 
    authors="clemensv" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="07/01/2016"
    ms.author="clemensv;jotaub;hillaryc;sethm"/>

# <a name="amqp-10-in-azure-service-bus-and-event-hubs-protocol-guide"></a>AMQP 1.0 in Azure Service Bus und Ereignis Hubs Protokoll-Handbuch

Erweiterte Message Queuing Protocol 1.0 ist ein standardisiertes Gestaltung und Transfer Protokoll asynchron und zuverlässig übertragen von Nachrichten zwischen zwei Parteien. Es ist das primäre Protokoll Azure Service Bus Messaging und Azure Ereignis Hubs. Beide Dienste unterstützen auch HTTPS. Das SBMP Protokoll, das auch für AMQP läuft aus.

AMQP 1.0 ist branchenweit Zusammenarbeit, die zusammengeführt Middlewarehersteller wie Microsoft und Red Hat mit vielen messaging Middleware Benutzern wie JP Morgan Chase Finanzdienstleister darstellt. Die technischen Normen für die AMQP-Protokoll und Erweiterung ist OASIS und Entlastung als internationaler Standard wie ISO/IEC 19494 erreicht.

## <a name="goals"></a>Ziele

Dieser Artikel fasst die grundlegenden Konzepte von AMQP 1.0 messaging Spezifikation mit kleinen Erweiterung Spezifikationen, die derzeit in dem Fachausschuß OASIS AMQP abgeschlossen sind kurz und erläutert, wie Azure Service Bus implementiert und auf diese Spezifikationen.

Das Ziel ist für alle Entwickler alle vorhandenen AMQP 1.0-Client-Stack auf jeder Plattform Azure Service Bus AMQP 1.0 interagieren können.

Gemeinsame allgemeine AMQP 1.0-Stapel oder Apache Proton AMQP.NET Lite implementieren bereits alle zentralen AMQP 1.0 Gesten. Die grundlegenden Aktionen werden manchmal auf höherer Ebene API eingeschlossen; Apache Proton bietet zwei imperative Messenger-API und reaktiven Reaktor-API.

Im folgenden wird davon ausgegangen, dass die Verwaltung von AMQP, Sitzung, und die Behandlung von Frame-Übertragung und Flusskontrolle erfolgt mit dem jeweiligen Stapel wie Apache Proton-C und erfordern nicht viel gegebenenfalls besondere Anwendungsentwickler. Angenommen das Vorhandensein von einigen API primitiven abstrakt wie die Möglichkeit, eine Verbindung herzustellen, und eine Form von *Sender* und *Empfänger* Abstraktion Objekte haben dann eine Form des `send()` und `receive()` Vorgänge bzw..

Wenn erweiterte Funktionen von Azure Service Bus oder Nachricht durchsuchen Management Sitzungen, die hinsichtlich der AMQP, sondern als Ebenen Pseudo-Implementierung auf diese angenommene API Abstraktion erläutert.

## <a name="what-is-amqp"></a>Was ist AMQP?

AMQP ist ein Rahmen und Transfer-Protokoll. Rahmen bedeutet, dass diese Struktur für binäre Datenströme bereitstellt, die in einer Richtung eine Netzwerk Verbindung. Die Struktur enthält Abgrenzung für Blöcke Daten-Frames – zwischen verbundenen Parteien ausgetauscht werden. Transfer-Funktionen sicher, dass beide miteinander kommunizierenden Parteien Verständnis bei Frames übertragen werden und wenn Übertragung abgeschlossen gelten herstellen können.

Im Gegensatz zu früher abgelaufene Entwurfsversionen AMQP-Arbeitsgruppe, die noch von wenigen Nachrichtenmakler werden, schreibt der Gruppe final und standardisierte AMQP 1.0-Protokoll nicht das Vorhandensein einer Kommunikationsserver oder speziellen Topologie für Entitäten innerhalb einer Nachricht beim Nachrichtenmakler.

Das Protokoll kann für die symmetrischen Peer-to-Peer-Kommunikation für die Interaktion mit Nachrichtenmakler verwendet werden, die Warteschlangen und Veröffentlichen/Abonnieren Entitäten wie Azure Service Bus unterstützen. Es kann auch verwendet werden für die Interaktion mit messaging-Infrastruktur, unterscheiden die Interaktionsmuster reguläre Warteschlangen wie bei Azure Ereignis Hubs. Event-Hub fungiert eine Warteschlange, wenn Ereignisse gesendet werden, aber fungiert mehr serielle Speicherdienst Wenn Ereignisse gelesen werden. Es ähnelt eher ein Bandlaufwerk. Der Client wählt einen Offset im Stream verfügbaren Daten und wird dann alle Ereignisse aus der Versatz die neuesten.

AMQP 1.0-Protokoll soll eine weitere Spezifikationen zur Verbesserung seiner Funktionen erweitert werden. Die drei Erweiterung Spezifikationen in diesem Dokument beschrieben verdeutlichen dies. Für die Kommunikation über HTTPS-WebSockets-Infrastruktur, die systemeigene AMQP TCP-Ports konfigurieren schwierig sein kann, definiert eine Bindungsspezifikation AMQP über WebSockets Ebene. Für die Interaktion mit der messaging-Infrastruktur auf Anforderung/Antwort Weise für die Verwaltung oder eine erweiterte Funktionalität definiert AMQP Management-Spezifikation erforderliche grundlegende Interaktion Primitives. Föderierte Autorisierung Modell Integration definiert AMQP Ansprüche-Basis-Security-Spezifikation zuordnen und erneuern Autorisierungstoken Links zugeordnet.

## <a name="basic-amqp-scenarios"></a>Einfache AMQP-Szenarien

Dieser Abschnitt erläutert die grundlegende Verwendung AMQP 1.0 Azure Service Bus Verbindungen, Sitzung, und Links und Übertragen von Nachrichten an und von Service Bus Entitäten wie Warteschlangen, Themen und Abonnements.

Die maßgebliche Quelle erfahren Sie mehr über die Funktionsweise von AMQP AMQP 1.0-Spezifikation ist die Spezifikation geschrieben wurde, genau Implementierung führt und nicht um das Protokoll zu unterrichten Dieser Abschnitt konzentriert sich auf viele Terminologie Bedarf für die Beschreibung der Verwendung des Service Bus AMQP 1.0 Einführung. Überprüfen Sie für eine umfassende Einführung in AMQP sowie eine umfassendere Erörterung der AMQP 1.0 [video-Kurs][].

### <a name="connections-and-sessions"></a>Anschlüsse und sessions

![][1]

AMQP Ruft die kommunizierenden Programme *Container*. Diese enthalten *Knoten*sind die kommunizierenden Objekte in diesen Containern. Eine Warteschlange kann ein Knoten sein. AMQP ermöglicht multiplexing, damit eine Verbindung für viele Kommunikationswege zwischen den Knoten verwendet werden kann. ein Anwendungsclient kann beispielsweise gleichzeitig von einer Warteschlange empfangen und Senden an eine andere Warteschlange über dieselbe Netzwerkverbindung.

Die Verbindung ist so des Containers verankert. Es wird durch den Container Empfänger Rolle, überwacht eingehende TCP-Verbindungen akzeptiert eine ausgehende TCP-Socketverbindung mit einem Container bei Clientrolle initiiert. Verbindungshandshake enthält die Protokollversion deklarieren oder die Verwendung von Transport Layer Security (TLS/SSL) und eine Authentifizierung Handshake am Verbindungsbereich auf SASL Verhandlungen aushandeln.

Azure Service Bus erfordert den Einsatz von TLS. Verbindung über TCP-Port 5671 unterstützt, wobei die TCP-Verbindung zunächst mit TLS überlagert vor AMQP Protokoll-Handshake und unterstützt auch Verbindungen über TCP-Port 5672, wobei der Server eine verbindliche Aktualisierung der Verbindung sofort TLS bietet die AMQP vorgeschriebenen Modell. AMQP WebSockets Bindung erstellt einen Tunnel über TCP-Port 443 dann AMQP 5671 Verbindung entspricht.

Nach dem Einrichten der Verbindung und TLS, bietet Service Bus zwei SASL Mechanismus:

-   SASL-Ebene wird häufig für die Übergabe von Benutzernamen und Kennwort auf einem Server verwendet. Service Bus hat keine Konten jedoch benannte [freigegebene Sicherheit Regeln](service-bus-shared-access-signature-authentication.md), welche Rechte verleihen und ein Schlüssel zugeordnet. Der Name der Regel den Benutzernamen und den Schlüssel verwendet (wie base64 Text codiert) wird als Kennwort verwendet. Die Rechte der ausgewählten Regel bestimmen die zulässigen Operationen für die Verbindung.

-   ANONYME SASL wird zum Umgehen der Autorisierung SASL Client möchte Ansprüche basierte Sicherheit (CBS) Modell verwenden, das weiter unten beschrieben. Mit dieser Option kann eine Clientverbindung anonym hergestellt werden für kurze Zeit während der Client kann nur mit dem CBS-Endpunkt und CBS-Handshake abgeschlossen werden muss.

Nachdem die Transport-Verbindung Deklarieren der Container die maximale Rahmengröße bereit, behandeln und nach der Leerlauftimeout müssen sie einseitig ist keine Aktivität auf der Verbindung trennen.

Sie deklarieren außerdem eine Anzahl gleichzeitiger Kanäle unterstützt werden. Ein Channel ist eine unidirektionale, ausgehende, virtuelle Übertragungsweg über die Verbindung. Eine Sitzung wird einen Kanal aus miteinander verbundenen Container eine bidirektionale Kommunikation Pfad.

Sessions haben einen Windows-basierten Steuerelements Flussmodell; Beim Erstellen einer Sitzung jedes erklärt wie viele Frames akzeptieren ist seine Empfangsfenster. Rahmen Exchange Parteien übertragenen Frames ausfüllen, das Fenster und Übertragung beenden, wenn Fenster voll ist und das Fenster zurücksetzen oder erweiterten mit *performativen übertragen* wird (*performative* AMQP Begriff Protokollebene Gesten zwischen den beiden Parteien ausgetauscht wird).

Dieses Fenster basierendes Modell entspricht etwa TCP-Konzept der Fenster-basierte Steuerung, aber auf Sitzungsebene in den Sockel. Das Protokoll ermöglicht mehrere gleichzeitige Sitzungen Konzept ist hohe Priorität nach Drosselung normalen Datenverkehr wie auf einer Autobahn Überholspur gebracht werden konnte.

Azure Service Bus verwendet derzeit genau einmal für jede Verbindung. Die maximale Service Bus-Frame-Größe ist 262.144 Byte (256 KB) für Service Bus Standard und Ereignis-Hubs. Es ist 1.048.576 (1 MB) für Service Bus. Service Bus nicht insbesondere auf Sitzungsebene Fenster Drosselung, aber das Fenster Link auf Rahmen regelmäßig zurückgesetzt (siehe [nächsten Abschnitt](#links)).

Clientverbindungen, Kanäle und Sessions sind flüchtig. Wenn die zugrunde liegende Verbindung Verbindungen reduziert müssen TLS-Tunnel, SASL-Autorisierungskontext und Sessions wiederhergestellt.

### <a name="links"></a>Links

![][2]

AMQP überträgt Nachrichten über Links. Eine Verknüpfung ist ein Kommunikationspfad erstellt über eine Sitzung, die Nachrichten in eine Richtung übertragende kann. Transfer Status Aushandlung ist über den Link und bidirektional zwischen verbundenen Parteien.

Links können entweder Container erstellt werden, und über eine bestehende Sitzung, wodurch AMQP unterschiedlich viele Protokolle wie HTTP und MQTT ist die Einleitung der Übertragung und Übertragungspfad eine exklusive Recht die Socket-Verbindung erstellt.

Verbindung initiieren Container stellt gegenüberliegenden Container einen Link an und wählt eine Rolle des Absenders oder Empfängers. Daher Container kann initiieren erstellen unidirektionale oder bidirektionale Kommunikationspfade mit paarweise links modelliert.

Hyperlinks sind benannt und Knoten zugeordnet. Wie am Anfang, sind Knoten kommunizieren Entitäten in einem Container.

In Azure Service Bus ist ein Knoten eine Warteschlange, ein Thema, ein Abonnement oder eine Warteschlange für unzustellbare Nachrichten unter einer Warteschlange oder Abonnement direkt entspricht. Verwendet in AMQP Knotenname ist daher relative Name der Entität in den Service Bus-Namespace. Wenn eine Warteschlange **Berichtnachrichten**benannt, ist auch die AMQP Knotenname. Ein Thema Abonnement folgt der Konvention HTTP-API durch eine "Abonnements" durchlaufen und so Abonnement **Sub** sortiert oder Thema **meinthema** wurde AMQP Knoten namens **meinthema, Abonnements/Sub**.

Der verbindende Client muss auch einen lokaler Knotenname zum Erstellen von Links zu verwenden; Service Bus nicht zu diesen Knoten unterstützende und nicht interpretieren. AMQP 1.0-Client Stapel verwenden im Allgemeinen ein sicherzustellen, die in den Geltungsbereich der Client diese flüchtigen Knotennamen eindeutig sind.

### <a name="transfers"></a>Überträgt

![][3]

Nachdem eine Verbindung können Nachrichten über diese Verbindung übertragen werden. In AMQP erfolgt eine Übertragung mit einer expliziten Protokoll (der *Transfer* performative), die eine Nachricht vom Absender an Empfänger über einen Link. Übertragung ist abgeschlossen, wenn sie "ausgeglichen", was bedeutet, dass beide Parteien ein gemeinsames Verständnis über das Ergebnis dieser Übertragung eingerichtet haben.

Im einfachsten Fall können Absender Nachrichten "bereits ausgeglichen", was bedeutet, dass der Client nicht in das Ergebnis und der Empfänger nicht Feedback über das Ergebnis der Operation. Dieser Modus wird von Azure Service Bus auf Protokollebene AMQP unterstützt, aber nicht in einer Client-API verfügbar gemacht.

Reguläre ist, nicht ausgeglichene Nachrichten werden, und des Empfängers wird dann Annahme oder Ablehnung mit performativen *Disposition* . Ablehnung, wenn der Empfänger die Nachricht aus irgendeinem Grund nicht annehmen kann und die Ablehnungsnachricht enthält Informationen über den Grund ein Fehler AMQP definiert ist. Wenn Nachrichten aufgrund interner Fehler in Azure Service Bus abgelehnt werden, gibt der Dienst zusätzliche Informationen innerhalb dieser Struktur, für die Bereitstellung von Diagnose-Tipps Mitarbeiter unterstützen, wenn Sie Support-Anfragen Einreichung verwendet werden. Sie erfahren mehr über Fehler später.

Eine besondere Form der Ablehnung steht *zur Verfügung* , gibt an, dass der Empfänger keine technischen Einwände gegen die Übertragung, sondern auch kein Interesse an die Übertragung ausgleichen. Fall, z. B. wenn eine Nachricht vorhanden ist Service Bus-Client übermittelt und der Client wählt aufzugeben "Nachricht", da sie durch die Nachricht verarbeitet, während die Nachrichtenübermittlung selbst nicht fehlerhaft arbeiten durchführen kann. Eine Variante dieses Staates steht *geändert* , wodurch Änderung der Nachricht nach der Veröffentlichung. Dieser Zustand wird derzeit nicht Service Bus verwendet.

AMQP 1.0-Spezifikation definiert eine weitere Disposition Status *erhalten* , die speziell Link Recovery behandeln kann. Link-Recovery ermöglicht rekonstruieren Zustand ein Link und alle ausstehenden Übermittlungen eine neue Verbindung und die Sitzung, wenn frühere Verbindung und Sitzung verloren.

Azure Service Bus unterstützt keine Verknüpfung wiederherstellen. verliert der Client die Verbindung Service Bus mit einem offenen ausstehende übertragen, Nachrichtenübermittlung geht verloren und der Client wiederherstellen, Wiederherstellen der Verknüpfung und Übertragung wiederholen muss.

So unterstützen Azure Service Bus Event Hubs "einmal" und überträgt der Absender der Nachricht gespeichert und akzeptiert wurde sicher sein, wobei ein nicht unterstützt "einmalig" Ebene AMQP, das System versucht die Verknüpfung wieder zu den Stand der Übertragung der Nachricht Duplizierung vermeiden zu übertragen.

Ausgleich für mögliche Dublette sendet, Azure Service Bus zu Warteschlangen und Themen Duplikaterkennungsregeln als optionale Funktion unterstützt. Duplikate erkennen Nachrichten-IDs aller eingehenden Nachrichten in einem Zeitfenster von benutzerdefinierten und alle Nachrichten mit derselben Message-IDs in demselben Fenster automatisch löschen.

### <a name="flow-control"></a>Datenflusskontrolle

![][4]

Neben beschrieben auf Sitzungsebene Flow-Steuerelementmodell hat jede Verknüpfung eigene Flussmodell. Auf Sitzungsebene Flusskontrolle schützt Container müssen viele Frames am behandeln, wenn verbinden Flusskontrolle Anwendung für wie viele Nachrichten stellt über einen Link behandeln will und.

Auf einen Link möglich überträgt nur, wenn genügend "link Gutschrift" hat. Link ist ein Indikatorensatz mit *fortlaufenden* performativ, Empfänger auf einen Link beschränkt. Wenn und Absender Link Gutschrift zugewiesen ist, wird versucht, diese Guthaben verwenden, indem Sie Nachrichten. Jede Nachricht Übermittlung verringert bestehende Verknüpfung durch eine Gutschrift. Bei einer Verknüpfung von beenden beliefert.

Bei Service Bus Rolle Empfänger liefert sofort Absender mit umfangreichen Link, es damit Nachrichten sofort gesendet werden können. Verknüpfen einer verwendet wird, senden Service Bus gelegentlich einen *Fluss* performativen Absender Guthaben Link aktualisieren.

Die Rolle Absender senden Service Bus sorgfältig Nachrichten Guthaben ausstehenden Link verwenden.

Ein "empfangen" Aufruf auf API-Ebene in einen *Fluss* an Service Bus vom Client performativen übersetzt und Service Bus Nutzen dieser Gutschrift vom erste verfügbare, nicht gesperrte Nachricht aus der Warteschlange, Sperren und es. Es ist keine Meldung für die Lieferung verfügbar, hergestellt ausstehenden Guthaben von Links mit bestimmten Entität in der Reihenfolge ihres Eintreffens aufgezeichnete und Nachrichten gesperrt und übertragen alle ausstehenden verwendet werden.

Die Sperre für eine Nachricht erscheint bei die Übertragung in terminal *akzeptiert*, *abgelehnt*oder *freigegeben*wird. Die Nachricht wird aus Service Bus entfernt, wird der Endstatus *akzeptiert*. Es verbleibt im Service Bus und andere Staaten erreicht die Übertragung an den nächsten Empfänger übermittelt. Service Bus wird automatisch Verschieben der Nachricht in der Warteschlange für unzustellbare Nachrichten des Unternehmens erreicht maximale Lieferung Count für die Entität aufgrund wiederholter abgelehnte oder Versionen zulässig.

Obwohl die offizielle Service Bus APIs diese Option heute nicht direkt verfügbar können ein Protokollclient auf niedrigerer Ebene AMQP Modell einer Verknüpfung "Pull-Style" Interaktion Ausstellen einer Einheit der Gutschrift für jede Anforderung empfangen in ein "Push" Modell mit sehr viele Link Credits und empfangen Nachrichten ohne weitere Interaktion werden. Push wird durch die Eigenschaften [MessagingFactory.PrefetchCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.prefetchcount.aspx) und [MessageReceiver.PrefetchCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.prefetchcount.aspx) unterstützt. Sind NULL verwendet beim Client AMQP Verbindung haben.

Dabei ist es wichtig zu verstehen, dass die Uhr für den Ablauf auf die Nachricht in der Entität beginnt bei die Meldung von der Entität und nicht, wenn die Nachricht bei der Übertragung gestellt wird. Wenn der Client Bereitschaft zum Empfangen von Nachrichten mit einer Verknüpfung gibt an, es dürfte Nachrichten im Netzwerk aktiv ziehen und deren Behandlung. Andernfalls Nachricht Sperren möglicherweise abgelaufen vor selbst die Nachricht übermittelt wird. Die Verwendung von Link-Gutschrift Flusskontrolle sollten direkt verfügbaren Nachrichten an den Empfänger versandt sofort bereit.

Die folgenden Abschnitte geben zusammenfassend schematischen Überblick performativen Fluss während andere API-Interaktionen. Jeder Abschnitt beschreibt eine andere logische Operation. Einige dieser Interaktionen möglicherweise "träge", was bedeutet, dass sie nur ausgeführt werden können einmal erforderlich. Erstellen einen Absender nicht Netzwerk-Aktivität möglicherweise bis die erste Nachricht gesendet oder angefordert wird.

Die Pfeile zeigen die Richtung performativen.

#### <a name="create-message-receiver"></a>Nachrichtenempfänger erstellen

| Client                                                                                                                                                | Servicebus                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------    |--------------------------------------------------------------------------------------------------------------------------------------------   |
| -->--> (Anfügen<br/>Name = {Linkname}<br/>Behandeln = {numerischen Handle}<br/>Rolle =**Empfänger**<br/>Quelle = {Entitätsname}<br/>Target = {Client Link-Id}<br/>)         | Client legt Entität als Empfänger                                                                                                         |
| Ende der Verbindung anfügen Service Bus-Antworten                                                                                                     | <--(Anfügen<br/>Name = {Linkname}<br/>Behandeln = {numerischen Handle}<br/>Rolle =**Absender**<br/>Quelle = {Entitätsname}<br/>Target = {Client Link-Id}<br/>)       |

#### <a name="create-message-sender"></a>Absender erstellen

| Client                                                                                                            | Servicebus                                                                                                           |
|------------------------------------------------------------------------------------------------------------------ |--------------------------------------------------------------------------------------------------------------------   |
| -->--> (Anfügen<br/>Name = {Linkname}<br/>Behandeln = {numerischen Handle}<br/>Rolle =**Absender**<br/>Quelle = {Client Link Id}<br/>Target = {Entitätsname}<br/>)   | Keine Aktion                                                                                                                     |
| Keine Aktion                                                                                                                 | <--(Anfügen<br/>Name = {Linkname}<br/>Behandeln = {numerischen Handle}<br/>Rolle =**Empfänger**<br/>Quelle = {Client Link Id}<br/>Target = {Entitätsname}<br/>)     |

#### <a name="create-message-sender-error"></a>Erstellen Sie Absender (Fehler)

| Client                                                                                                            | Servicebus                                                           |
|------------------------------------------------------------------------------------------------------------------ |---------------------------------------------------------------------  |
| -->--> (Anfügen<br/>Name = {Linkname}<br/>Behandeln = {numerischen Handle}<br/>Rolle =**Absender**<br/>Quelle = {Client Link Id}<br/>Target = {Entitätsname}<br/>)   | Keine Aktion                                                                     |
| Keine Aktion                                                                                                                 | <--(Anfügen<br/>Name = {Linkname}<br/>Behandeln = {numerischen Handle}<br/>Rolle =**Empfänger**<br/>Quelle = Null<br/>Target = Null<br/>)<br/><br/><--trennen ()<br/>Behandeln = {numerischen Handle}<br/>geschlossen =**true**<br/>Fehler = {Fehlerinformationen}<br/>)  |

#### <a name="close-message-receiversender"></a>Schließen Absender/Empfänger

| Client                                            | Servicebus                                       |
|-------------------------------------------------  |-------------------------------------------------  |
| -->--> trennen ()<br/>Behandeln = {numerischen Handle}<br/>geschlossen =**true**<br/>)    | Keine Aktion                                                 |
| Keine Aktion                                                 | <--trennen ()<br/>Behandeln = {numerischen Handle}<br/>geschlossen =**true**<br/>)    |

#### <a name="send-success"></a>Senden (Erfolg)

| Client                                                                                                                        | Servicebus                                                                                           |
|------------------------------------------------------------------------------------------------------------------------------ |------------------------------------------------------------------------------------------------------ |
| Transfer (--><br/>Lieferung-Id = {numerischen Handle}<br/>Lieferung Tag = {binäre Handle}<br/>Ausgeglichene =**false**, mehr =**false**<br/>Status =**null**<br/>fortsetzen =**false**<br/>)   | Keine Aktion                                                                                                     |
| Keine Aktion                                                                                                                             | <--Disposition (<br/>Rolle = Empfänger<br/>First = {Beschreib}<br/>Last = {Beschreib}<br/>Ausgeglichene =**true**<br/>Status =**akzeptiert**<br/>)   |

#### <a name="send-error"></a>Senden (Fehler)

| Client                                                                                                                        | Servicebus                                                                                                                   |
|------------------------------------------------------------------------------------------------------------------------------ |-----------------------------------------------------------------------------------------------------------------------------  |
| Transfer (--><br/>Lieferung-Id = {numerischen Handle}<br/>Lieferung Tag = {binäre Handle}<br/>Ausgeglichene =**false**, mehr =**false**<br/>Status =**null**<br/>fortsetzen =**false**<br/>)   | Keine Aktion                                                                                                                             |
| Keine Aktion                                                                                                                             | <--Disposition (<br/>Rolle = Empfänger<br/>First = {Beschreib}<br/>Last = {Beschreib}<br/>Ausgeglichene =**true**<br/>Status = (**abgelehnt**<br/>Fehler = {Fehlerinformationen}<br/>)<br/>)     |

#### <a name="receive"></a>Empfangen

| Client                                                                                                | Servicebus                                                                                                                   |
|------------------------------------------------------------------------------------------------------ |------------------------------------------------------------------------------------------------------------------------------ |
| -->--> Flow (<br/>Link-Gutschrift = 1<br/>)                                                                                 | Keine Aktion                                                                                                                             |
| Keine Aktion                                                                                                     | < übertragen ()<br/>Lieferung-Id = {numerischen Handle}<br/>Lieferung Tag = {binäre Handle}<br/>Ausgeglichene =**false**<br/>mehr =**false**<br/>Status =**null**<br/>fortsetzen =**false**<br/>)     |
| Disposition (--><br/>Rolle =**Empfänger**<br/>First = {Beschreib}<br/>Last = {Beschreib}<br/>Ausgeglichene =**true**<br/>Status =**akzeptiert**<br/>)   | Keine Aktion                                                                                                                             |

#### <a name="multi-message-receive"></a>Multi-Meldung

| Client                                                                                                    | Servicebus                                                                                                                       |
|--------------------------------------------------------------------------------------------------------   |--------------------------------------------------------------------------------------------------------------------------------   |
| -->--> Flow (<br/>Link-Gutschrift = 3<br/>)                                                                                 | Keine Aktion                                                                                                                                 |
| Keine Aktion                                                                                                         | < übertragen ()<br/>Lieferung-Id = {numerischen Handle}<br/>Lieferung Tag = {binäre Handle}<br/>Ausgeglichene =**false**<br/>mehr =**false**<br/>Status =**null**<br/>fortsetzen =**false**<br/>)     |
| Keine Aktion                                                                                                         | < übertragen ()<br/>Lieferung-Id = {numerischen Handle + 1}<br/>Lieferung Tag = {binäre Handle}<br/>Ausgeglichene =**false**<br/>mehr =**false**<br/>Status =**null**<br/>fortsetzen =**false**<br/>)   |
| Keine Aktion                                                                                                         | < übertragen ()<br/>Lieferung-Id = {numerischen Handle + 2}<br/>Lieferung Tag = {binäre Handle}<br/>Ausgeglichene =**false**<br/>mehr =**false**<br/>Status =**null**<br/>fortsetzen =**false**<br/>)   |
| Disposition (--><br/>Rolle = Empfänger<br/>First = {Beschreib}<br/>Last = {Beschreib + 2}<br/>Ausgeglichene =**true**<br/>Status =**akzeptiert**<br/>)     | Keine Aktion                                                                                                                                 |

### <a name="messages"></a>Nachrichten

Die folgenden Abschnitte erläutern die Eigenschaften aus der standardmäßigen AMQP Message Service Bus verwendet werden und deren Zuordnung zu offiziellen Service Bus-APIs.

#### <a name="header"></a>Kopfzeile

| Feldname        | Verwendung                             | API-Name          |
|----------------   |-------------------------------    |---------------    |
| robuste           | -                                 | -                 |
| Priorität          | -                                 | -                 |
| Gültigkeitsdauer               | Gültigkeitsdauer für diese Nachricht     | [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)     |
| erste Erwerber    | -                                 | -                 |
| Anzahl der Lieferung    | -                                 | [DeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.deliverycount.aspx)   |

#### <a name="properties"></a>Eigenschaften

| Feldname            | Verwendung                                                                                                                             | API-Name                                      |
|---------------------- |---------------------------------------------------------------------------------------------------------------------------------  |--------------------------------------------   |
| Nachrichten-id            | Anwendungsspezifische, Freiform-Bezeichner für diese Nachricht. Zur Erkennung von Duplikaten.                                         | [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx)                                   |
| Benutzer-id               | Anwendungsdefinierte Benutzerbezeichner Service Bus nicht interpretiert.                                                              | Die Bus-API nicht verfügbar.   |
| An                    | Anwendungsspezifische Zielbezeichner, Service Bus nicht interpretiert.                                                       | [An](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.to.aspx)                                             |
| Betreff               | Anwendungsdefinierte Meldung Zweck Bezeichner Service Bus nicht interpretiert.                                                   | [Bezeichnung](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)                                       |
| Antwort an              | Antwort-Pfad anwendungsdefinierte Indikator Service Bus nicht interpretiert.                                                         | [ReplyTo](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.replyto.aspx)                                       |
| Korrelations-id        | Anwendungsspezifische Korrelations-ID Service Bus nicht interpretiert.                                                       | [CorrelationId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.correlationid.aspx)                               |
| Inhaltstyp          | Anwendungsdefinierte Inhaltstyp Indikator für den Textkörper von Service Bus nicht interpretiert.                                          | [ContentType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx)                                   |
| Codierung von Inhalt      | Anwendungsspezifische Content-encoding Indikator für den Textkörper von Service Bus nicht interpretiert.                                      | Die Bus-API nicht verfügbar.   |
| Absolute Ablaufzeit  | Bei der absoluten sofort die Nachricht abläuft, erklärt. Bei der Eingabe ignoriert (Header Ttl beobachtet), Ausgabe autorisierend.   | [ExpiresAtUtc](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.expiresatutc.aspx)                                 |
| Zeitpunkt der Erstellung         | Erklärt bei der Erstellung der Nachricht. Service Bus verwendet nicht                                                           | Die Bus-API nicht verfügbar.   |
| Gruppen-id              | Anwendungsspezifische Bezeichner für eine zusammengehörige Gruppe von Nachrichten. Zum Service Bus-Sessions.                                      | [SessionId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx)                                   |
| Gruppe-Sequenz        | Zähler die relativen Sequenznummer der Nachricht innerhalb einer Sitzung identifiziert. Service Bus ignoriert.                         | Die Bus-API nicht verfügbar.   |
| Antwort Gruppen-id     | -                                                                                                                                 | [ReplyToSessionId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.replytosessionid.aspx)                             |

## <a name="advanced-service-bus-capabilities"></a>Erweiterter Service Bus-Funktionen

Dieser Abschnitt enthält erweiterte Funktionen von Azure Service Bus Entwurf Erweiterungen derzeit in OASIS Technical Committee für AMQP AMQP basieren. Azure Service Bus wird Stand Entwürfe implementiert und Änderungen Entwürfe standard Status erreicht.

> [AZURE.NOTE] Service Bus Messaging erweiterte Vorgänge unterstützt betrachtet eine Anforderung-Antwort-Muster. Die Details dieser Vorgänge im Dokument beschriebenen [AMQP 1.0 Service Bus: Anforderung/Antwort-basierte Operationen](https://msdn.microsoft.com/library/azure/mt727956.aspx).

### <a name="amqp-management"></a>AMQP-management

AMQP Management-Spezifikation ist der erste Entwurf-Erweiterungen, die wir hier besprechen. Diese Spezifikation definiert einen Satz von AMQP Protokoll überlagern Protokoll Gesten, die AMQP Management-Interaktionen mit der messaging-Infrastruktur zu ermöglichen. Die Spezifikation definiert allgemeine Vorgänge wie *Erstellen*, *Lesen*, *Aktualisieren*und *Löschen* für die Verwaltung von Entitäten eine Infrastruktur und eine Reihe von Abfrageoperationen.

Diese Gesten müssen Anfrage/Antwort-Interaktion zwischen dem Client und der Messaginginfrastruktur und daher die Spezifikation definiert, wie das Interaktionsmuster auf AMQP Modell: der Client eine Verbindung mit der messaging-Infrastruktur leitet eine Sitzung und erstellt zwei Links. Auf einem Client fungiert als Sender und andererseits fungiert als Empfänger, wodurch zwei Links einen bidirektionalen Kanal darstellen kann.

| Logische Operation            | Client                      | Servicebus                 |
|------------------------------|-----------------------------|-----------------------------|
| Anforderung-Antwort-Pfad erstellen | -->--> (Anfügen<br/>Name = {*Link-Name*}<br/>behandeln = {*numerischen Handle*}<br/>Rolle =**Absender**<br/>Quelle =**null**<br/>Target = "Myentity-$Management"<br/>)                            |Keine Aktion                             |
|Anforderung-Antwort-Pfad erstellen                              |Keine Aktion                             | \<--Befestigen ()<br/>Name = {*Link-Name*}<br/>behandeln = {*numerischen Handle*}<br/>Rolle =**Empfänger**<br/>Quelle = Null<br/>Target = "Myentity"<br/>)                            |
|Anforderung-Antwort-Pfad erstellen                              | -->--> (Anfügen<br/>Name = {*Link-Name*}<br/>behandeln = {*numerischen Handle*}<br/>Rolle =**Empfänger**<br/>Quelle = "Myentity-$Management"<br/>Target = "Myclient$ Id"<br/>)                            |                             |Keine Aktion
|Anforderung-Antwort-Pfad erstellen                              |Keine Aktion                             | \<--Befestigen ()<br/>Name = {*Link-Name*}<br/>behandeln = {*numerischen Handle*}<br/>Rolle =**Absender**<br/>Quelle = "Myentity"<br/>Target = "Myclient$ Id"<br/>)                            |

Dieses Paar von Links, die Anforderung/Antwort-Implementierung ist einfach: eine Anforderung ist eine Nachricht an eine Entität in der messaging-Infrastruktur, die dieses Muster versteht. In diesem Anforderungsnachricht wird die *Ziel* -ID für den Link auf die Antwort zu Feld *Antwortadresse* im Abschnitt *Eigenschaften* fest. Behandlung Entität die Anforderung nicht verarbeiten und übermitteln die Antwort über die Verknüpfung, deren *Ziel* die ID angegebene *Antwortadresse* übereinstimmt.

Das Muster ist offensichtlich, dass Client-Container und Client generierte ID für das Ziel mit Antworten auf allen Clients und aus Sicherheitsgründen auch schwer vorherzusagen sind.

Für das Verwaltungsprotokoll und andere Protokolle, mit denen das gleiche Muster Nachrichtenaustausch kommen auf Anwendungsebene. Definieren sie neue AMQP Protokollebene Gesten. Das ist absichtlich Applikationen diese Erweiterung mit kompatiblen AMQP 1.0 auch sofort nutzen können.

Azure Service Bus derzeit implementiert keine der Hauptfunktionen von Management-Spezifikation, aber durch die Managementspezifikation definierten Anforderung/Antwort-Muster ist grundlegend für die Ansprüche-Basis-Sicherheitsfunktion und fast alle erweiterten Funktionen in den folgenden Abschnitten beschrieben.

### <a name="claims-based-authorization"></a>Anspruchsbasierte Autorisierung

AMQP Ansprüche basierte Autorisierung (CBS) Spezifikation Entwurf basiert auf der Managementspezifikation Anforderung/Antwort-Muster und beschreibt ein allgemeines Modell für Verwendung verbundenen Sicherheitstoken mit AMQP.

Beim Standardsicherheitsmodell von AMQP in der Einführung behandelten basiert auf SASL und AMQP Verbindungshandshake integriert. SASL hat den Vorteil, den es ein erweiterbares Modell, für das eine Reihe von Mechanismen ist von zugute Protokollbefehle formell SASL lehnt definiert wurden. Diese Mechanismen sind zum Übertragen von Benutzernamen und Kennwörtern an Sicherheit TLS auf "Anonym" keine explizite Authentifizierung/Autorisierung, und eine Vielzahl von zusätzlichen Mechanismen, mit denen die Authentifizierung oder Anmeldeinformationen oder Token "extern" "PLAIN".

AMQP des SASL Integration hat zwei Nachteile:

-   Alle Anmeldeinformationen und Token sind auf die Verbindung beschränkt. Eine Messaginginfrastruktur möchten differenzierte Zugriffskontrolle auf Basis pro Einheit. Dies ermöglicht z. B. Träger ein Token an eine Warteschlange jedoch nicht Warteschlange b Mit der Autorisierung verankert die Verbindung kann keine einzelne Verbindung noch verwenden und verschiedene Zugangs-Token für eine Warteschlange und B.

-   Zugriffstoken gelten in der Regel nur für einen begrenzten Zeitraum. Dies zwingt den Benutzer regelmäßig Token erneut und bietet die Möglichkeit, dem Herausgeber des token zu verweigern neue Token ausstellen, wenn Zugriffsrechte des Benutzers geändert haben. AMQP-Verbindung können sehr lange Zeit dauern. SASL-Modell bietet nur eine Chance, einen Token zum Zeitpunkt der Verbindung festgelegt, die die messaging-Infrastruktur hat, trennen Sie den Client, wenn das Token abläuft oder das Risiko eine kontinuierliche Kommunikation mit einem Client akzeptieren muss, hat Zugriffsrechte wurde in der Zwischenzeit gesperrt.

Azure Service Bus durchgeführte AMQP CBS-Spezifikation ermöglicht eine elegante Lösung für diese Probleme: ermöglicht einem Client jeden Knoten Zugriffstoken zugeordnet und diese Token zu aktualisieren, vor deren Ablauf ohne Unterbrechung des Nachrichtenflusses.

CBS definiert einen virtuellen Knoten mit dem Namen *$cbs*von der messaging-Infrastruktur bereitgestellt werden. Knoten Verwaltung akzeptiert Token für alle Knoten in der messaging-Infrastruktur.

Protokoll-Bewegung ist ein Anforderung-Antwort als Management-Spezifikation. Das bedeutet, der Client richtet ein Paar mit *$cbs* Knoten übergibt dann eine Anforderung für die ausgehende Verbindung und wartet dann auf die Antwort auf die eingehende Verbindung.

Die Anforderungsnachricht wurde die folgenden Anwendung:

| Schlüssel        | Optionale | Werttyp | Inhalts                             |
|------------|----------|------------|--------------------------------------------|
| Vorgang  | Nein       | Zeichenfolge     | **Put-token**                                |
| Typ       | Nein       | Zeichenfolge     | Der Typ des Tokens wird.            |
| Name       | Nein       | Zeichenfolge     | Das Token gilt "Zielgruppe". |
| Ablaufdatum | Ja      | Zeitstempel  | Die Ablaufzeit des Tokens.              |

Die *Name* -Eigenschaft identifiziert die Entität mit der Token zugeordnet werden. In Service Bus ist dies der Pfad auf die Warteschlange oder ein Thema-Abonnement. Die *Type* -Eigenschaft gibt den Tokentyp:

| Tokentyp                      | Token Beschreibung      | Texttyp           | Notizen                                                    |
|---------------------------------|------------------------|---------------------|----------------------------------------------------------|
| amqp:jwt                        | JSON Web Token (JWT)   | AMQP Wert (Zeichenfolge) | Noch nicht verfügbar.  |
| amqp:SWT                        | Simple Web Token (SWT) | AMQP Wert (Zeichenfolge) | Nur unterstützt für SWT AAD-ACS ausgestellten Token          |
| Servicebus.Windows.NET:sastoken" | Service Bus SAS-Token  | AMQP Wert (Zeichenfolge) | -                                                        |

Token übertragen Rechte. Service Bus weiß drei Grundrechte: "Senden" empfangen können "Abhören" und "Verwalten" ermöglicht das Bearbeiten von Entitäten senden können. SWT explizit von AAD-ACS ausgestellte Token enthalten diese Rechte als Ansprüche. Service Bus SAS-Token finden Sie Namespace oder Entität konfigurierten Regeln und Vorschriften mit konfiguriert. Das Token mit dieser Regel zugeordnete Schlüssel signieren so macht token Express Rechte. Eine Entität mit *Put-Token* zugeordnete Token lässt verbundenen Client Entität pro token Rechte interagieren. Erfordert eine Verknüpfung der Client für die Rolle als *Absender findet* "Senden" rechts *Empfänger* Rolle Berechtigung "Empfangen" ist erforderlich.

Die Antwortnachricht hat folgende *Eigenschaften Anwendung*

| Schlüssel                | Optionale | Werttyp | Inhalts                    |
|--------------------|----------|------------|-----------------------------------|
| Status-code        | Nein       | int        | HTTP-Antwortcode **[RFC2616]**. |
| Beschreibung des | Ja      | Zeichenfolge     | Beschreibung des Status.        |

Der Client kann *Put-Token* wiederholt und für eine beliebige Entität in der Messaginginfrastruktur aufrufen. Token werden dem aktuellen Client begrenzt und verankert die aktuelle Verbindung, d. h. der Server beibehaltenen Tokens trennt, wenn die Verbindung beendet.

Die aktuelle Implementierung des Service Bus lässt nur CBS zusammen mit SASL-Methode "Anonym". SSL/TLS-Verbindung muss immer vor der SASL-Handshake vorhanden sein.

ANONYME Methode muss daher vom ausgewählten AMQP 1.0-Client unterstützt werden. Anonymer Zugriff bedeutet, dass der Handshake Verbindungsaufbau einschließlich der Erstellung der ersten Sitzung geschieht ohne Service Bus kennen, der die Verbindung erstellt wird.

Sobald die Verbindung und die Sitzung hergestellt wurde, werden *$cbs* Links zuordnen Knoten und Senden der Anforderung *Put-Token* nur Operationen zulässig. Ein gültiges Token mithilfe erfolgreich eine Anforderung *Put-Token* für Entity-Knoten innerhalb von 20 Sekunden nach dem Herstellen der Verbindung festgelegt werden muss, andernfalls die Verbindung einseitig Service Bus getrennt.

Der Client ist dann Verfolgen von Tokengültigkeitsdauer verantwortlich. Wenn ein Sicherheitstoken abläuft, fallen Service Bus unverzüglich alle Links für die Verbindung mit der jeweiligen Entität. Um dies zu verhindern, kann der Client das Token für den Knoten mit einer neuen jederzeit über die virtuellen *$cbs* Management Knoten mit der gleichen *Put-Token* und ohne in der Payload-Datenverkehr, der auf verschiedenen ersetzen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu AMQP finden Sie unter den folgenden Links:

- [Service Bus AMQP Übersicht]
- [AMQP 1.0-Unterstützung für partitionierte Servicebuswarteschlangen und Themen]
- [AMQP Service Bus für WindowsServer]

[Dieses video-Kurs]: https://www.youtube.com/playlist?list=PLmE4bZU0qx-wAP02i0I7PJWvDWoCytEjD
[1]: ./media/service-bus-amqp/amqp1.png
[2]: ./media/service-bus-amqp/amqp2.png
[3]: ./media/service-bus-amqp/amqp3.png
[4]: ./media/service-bus-amqp/amqp4.png

[Service Bus AMQP Übersicht]: service-bus-amqp-overview.md
[AMQP 1.0-Unterstützung für partitionierte Servicebuswarteschlangen und Themen]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP Service Bus für WindowsServer]: https://msdn.microsoft.com/library/dn574799.aspx