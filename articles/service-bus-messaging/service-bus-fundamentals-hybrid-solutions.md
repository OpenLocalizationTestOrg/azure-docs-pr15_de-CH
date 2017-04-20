<properties 
    pageTitle="Azure Servicebus | Microsoft Azure" 
    description="Eine Einführung in Service Bus Verbindung Azure Applications in anderen Softwareprogrammen." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/31/2016" 
    ms.author="sethm"/>

# <a name="azure-service-bus"></a>Azure Servicebus

Ob eine Anwendung oder ein Dienst in der Cloud oder lokal ausgeführt wird, ist häufig andere Programme oder Dienste interagieren. Um Allgemein Weise dazu, bietet Microsoft Azure Service Bus. Dieser Artikel wird diese Technologie beschreiben, was und warum Sie verwenden möchten.

## <a name="service-bus-fundamentals"></a>Service Bus-Grundlagen

In unterschiedlichen Situationen für verschiedene Arten der Kommunikation. Clientanwendungen senden und Empfangen von Nachrichten über eine einfache Warteschlange lassen ist manchmal die beste Lösung. In anderen Fällen reicht eine normale Warteschlange; eine Warteschlange mit veröffentlichen und abonnieren ist besser. In einigen Fällen wirklich erforderlich, ist eine Verbindung zwischen Programmen. Warteschlangen sind nicht erforderlich. Service Bus bietet alle drei Optionen Ihre Anwendung auf verschiedene Arten interagieren.

Service Bus ist ein Multi-Tenant Cloud-Dienst, was bedeutet, dass der Dienst durch mehrere Benutzer freigegeben ist. Jeder Benutzer, z. B. ein Anwendungsentwickler erstellt einen *Namespace*und Kommunikationsmechanismen braucht in diesem Namespace definiert. Abbildung 1 zeigt, wie diese aussieht.

![][1]
 
**Abbildung 1: Servicebus bietet einen Multi-Tenant für Applikationen über die Cloud verbinden.**

Innerhalb eines Namespace können eine oder mehrere Instanzen von vier verschiedene Kommunikationsmechanismen Sie, die jeweils anders Applikationen verbindet. Die Optionen sind:

- *Warteschlangen*, die eine Kommunikation ermöglichen. Jede Warteschlange fungiert als Vermittler ( *Broker*bezeichnet), der speichert Nachrichten, bis sie empfangen. Jede Nachricht wird von einem Empfänger empfangen.
- *Themen*, unidirektionales Kommunikation mit *Abonnements*- ein einzelnes Thema kann mehrere Abonnements haben. Wie eine Warteschlange ein Thema fungiert als Vermittler, aber jedes Abonnement Optional können einen Filter nur Nachrichten, die bestimmte Kriterien entsprechen.
- *Relays*, die bidirektionale Kommunikation. Im Gegensatz zu Warteschlangen und Themen speichern nicht Relay Flight-Nachrichten. Es ist kein Broker. Stattdessen übergibt er sie an die Zielanwendung.

Wenn Sie eine Warteschlange, Thema oder Relay erstellen, geben Sie einen Namen. In Kombination mit was Sie Ihren Namespace erstellt diesen Namen eine eindeutige Kennung für das Objekt. Applikationen können Bereitstellen dieser Name Service Bus und dann diese Warteschlange Thema oder Relay miteinander kommunizieren. 

Um Objekte in der Relay-Szenario verwenden, können Windows-Programme Windows Communication Foundation (WCF). Warteschlangen und Themen können Windows Applications messaging APIs Service Bus definiert. Um diese Objekte aus Windows-Programmen erleichtern, stellt Microsoft SDKs für Java und Node.js Fremdsprachen. Sie können auch Warteschlangen und Themen mit anderen APIs über HTTP(s) zugreifen. 

Es ist wichtig zu verstehen, obwohl Service Bus selbst führt in der Cloud (d. h. in den Rechenzentren von Microsoft Azure), die sie verwenden können überall ausführen. Service Bus können Sie z. B. in Azure ausgeführt oder in Ihrem eigenen Datencenter verbinden. Außerdem können Sie sie für die Verbindung eine Anwendung in Azure oder anderen Cloud-Plattform mit einer lokalen Anwendung oder Telefonen und Tablets. Es kann sogar Haushaltsgeräte, Sensoren und andere Geräte zu einer zentralen Anwendung oder miteinander verbinden. Service Bus ist ein Mechanismus zur Kommunikation in der Wolke von überall aus zugänglich ist. Wie Sie hängt was Anwendung müssen.

## <a name="queues"></a>Warteschlangen

Angenommen Sie, zwei Clientanwendungen mithilfe einer Warteschlange Service Bus eine Verbindung herstellen möchten. Abbildung 2 veranschaulicht dies.

![][2]
 
**Abbildung 2: Einfache asynchrone queuing Servicebuswarteschlangen bereitstellen.**

Der Prozess ist einfach: ein Absender sendet eine Nachricht an eine Warteschlange Service Bus und ein Empfänger nimmt die Nachricht zu einem späteren Zeitpunkt. Eine Warteschlange können nur einzelnen Empfänger, wie in Abbildung 2 dargestellt. Oder mehrere Programme aus derselben Warteschlange lesen. Im letzten Fall wird jede Nachricht nur ein Empfänger gelesen. Für einen Dienst mit mehreren cast sollten Sie ein Thema verwenden.

Jede Nachricht besteht aus zwei Teilen: ein Satz von Eigenschaften, jedes Schlüssel-Wert-Paar und binären Nachrichtentext. Wie sie verwendet werden, hängt davon ab, was eine Anwendung versucht. Eine Anwendung sendet eine Nachricht über den letzten Verkauf könnte beispielsweise die Eigenschaften *Verkäufer = "Ava"* und *Betrag = 10000*. Nachrichtentext möglicherweise enthalten ein gescanntes Bild mit Kaufvertrag oder wenn es nicht, einfach leer.

Empfänger kann eine Nachricht aus einer Warteschlange Service Bus auf zwei verschiedene Arten lesen. Die erste Option, *ReceiveAndDelete*eine Meldung aus der Warteschlange entfernt und sofort gelöscht. Dies ist einfach, aber wenn Empfänger stürzt ab, bevor die Verarbeitung der Nachricht abgeschlossen, die Nachricht geht verloren. Da aus der Warteschlange entfernt wurde, kann kein anderer Empfänger darauf zugreifen. 

Die zweite Option *PeekLock*soll dieses Problem. Wie **ReceiveAndDelete**entfernt eine Nachricht aus der Warteschlange lesen Sie **PeekLock** . Es wird jedoch die Nachricht löschen. Stattdessen sperrt die Nachricht an andere Empfänger unsichtbar machen und wartet darauf, dass eines der drei Ereignisse:

- Wenn der Empfänger die Meldung erfolgreich verarbeitet, **abgeschlossen**wird und die Warteschlange wird die Nachricht gelöscht. 
- Wenn der Empfänger entscheidet, dass er die Nachricht erfolgreich verarbeiten kann, wird **verlassen**. Die Warteschlange die Sperre aus der Nachricht entfernt und für andere Empfänger zur Verfügung.
- Wenn der Empfänger diese in eine konfigurierbare Zeitspanne (standardmäßig 60 Sekunden) aufruft, nimmt die Warteschlange Empfänger fehlgeschlagen. In diesem Fall verhält es sich, als ob Empfänger **verlassen**, da die Nachricht an andere Empfänger aufgerufen hat.

Beachten Sie hier geschehen können: dieselbe Nachricht möglicherweise zugestellt, vielleicht zwei verschiedene Empfänger. Applikationen mit Servicebuswarteschlangen müssen vorbereitet werden. Um Duplikaterkennungsregeln erleichtern, jede Nachricht verfügt über eine eindeutige **MessageID** -Eigenschaft, die standardmäßig gleich wie oft bleibt wird die Nachricht aus einer Warteschlange gelesen. 

Warteschlangen sind in einige Situationen nützlich. Sie können Programme kommunizieren, auch wenn beide gleichzeitig, etwas ausgeführt werden, die mobile Anwendung mit Batch besonders praktisch. Eine Warteschlange mit mehreren Empfängern bietet auch die automatischen Lastenausgleich, da diese Empfänger gesendete Nachrichten verteilt werden.

## <a name="topics"></a>Themen

Nützlich sind, sind nicht Warteschlangen immer die richtige Lösung. Manchmal sind Service Bus Topics besser. Abbildung 3 illustriert dieses Konzept.

![][3]
 
**Abbildung 3: Basierend auf Filter, den eine abonnierende Anwendung gibt, kann es einige oder alle Nachrichten zu einem Thema Service Bus empfangen.**

Ein *Thema* entspricht in vielerlei Hinsicht einer Warteschlange. Absender senden Nachrichten an ein Thema wie Übermitteln von Nachrichten an eine Warteschlange und die Nachrichten aussehen mit Warteschlangen gleich. Der Unterschied ist, dass Themen jeder empfangende Anwendung eigene *Abonnements* erstellen, indem Sie einen *Filter*definieren können. Abonnent wird nur die Nachrichten angezeigt, die diesem Filter entsprechen. Abbildung 3 zeigt z. B. Absender und ein Thema mit drei Teilnehmer jeweils eigene Filter:

- Teilnehmer 1 erhält nur Nachrichten mit der Eigenschaft *Verkäufer = "Ava"*.
- Teilnehmer 2 empfängt Nachrichten mit der Eigenschaft *Verkäufer = "Ruby"* oder eine *Menge* Eigenschaft, deren Wert größer als 100.000 enthalten. Vielleicht ist Ruby Vertriebs-Manager so eigene Verkaufs- und alle großen Sales egal, wer sie sehen will.
- Teilnehmer 3 hat den Filter auf *True*festgelegt, sodass alle Nachrichten empfängt. Z. B. Anwendung möglicherweise einen Überwachungspfad beibehalten und muss daher alle Nachrichten anzuzeigen.

Mit Warteschlangen können Abonnenten zu einem Thema mit **ReceiveAndDelete** oder **PeekLock**Nachrichten lesen. Im Gegensatz zu Warteschlangen kann jedoch zu einem Thema pro Nachricht mehrere Abonnements eingehen. Dieses so genannte *Veröffentlichen und abonnieren* (oder *Pub-Sub*) ist nützlich, wenn mehrere Anträge auf dieselben Nachrichten interessieren. Definieren Sie den richtigen Filter, kann jeden Abonnenten nur den Teil der Nachrichtenstrom nutzen, die sie sehen.

## <a name="relays"></a>Relais

Warteschlangen und Themen bieten einfache asynchrone Kommunikation durch einen Makler. Datenfluss in nur eine Richtung und keine direkte Verbindung zwischen Sender und Empfänger. Aber was ist, wenn Sie nicht diese? Angenommen Sie, Ihre Anwendung müssen zum Senden und Empfangen von Nachrichten oder vielleicht soll eine direkte Verbindung zwischen ihnen und keinen Broker Nachrichten gespeichert. Um Szenarien zu behandeln, Service Bus bietet *weiterleitet*, wie in Abbildung 4 dargestellt.

![][4]
 
**Abbildung 4: Servicebus Relay bietet synchrone, bidirektionale Kommunikation zwischen Applikationen.**

Die offensichtliche Frage Relays Fragen: Warum dienen? Auch Warteschlangen brauche ich nicht, warum sollten Programme über einen Cloud-Dienst nicht nur direkt kommunizieren? Die Antwort lautet, dass sprechen direkt schwieriger als Sie vielleicht denken.

Angenommen, Sie möchten zwei lokale Anwendung verbinden, Ausführen in Rechenzentren von Unternehmen. Jede befindet sich hinter einer Firewalls und jedem Datencenter verwendet wahrscheinlich Netzwerkadressübersetzung (NAT). Die Firewall blockiert eingehende Daten auf allen Ports und NAT bedeutet, dass der Computer jede Anwendung läuft eine feste IP-Adresse besitzt, die Sie direkt von außerhalb des Datencenters erreichen. Ohne zusätzliche Hilfe ist diese Anwendung über das Internet problematisch.

Relay Service Bus hilft. Um bidirektionale Kommunikation über Relay jede Anwendung eine ausgehende TCP-Verbindung mit Service Bus und geöffnet hält. Die gesamte Kommunikation zwischen den beiden Programmen übertragen über diese Verbindung. Da jede Verbindung im Datenzentrum eingerichtet wurde, kann die Firewall eingehenden Datenverkehr für jede Anwendung ohne neue Ports öffnen. Dieser Ansatz wird auch NAT-Problem, jede Anwendung verfügt über eine konsistente Endpunkt in der Cloud in der Kommunikation. Durch den Austausch von Daten über das Relay, können die Anwendung Probleme vermeiden, die andernfalls Kommunikation erschweren würde. 

Verwendung von Service Bus Relays verlassen Applikationen auf Windows Communication Foundation (WCF). Service Bus bietet WCF-Bindung, die für Windows-Anwendung zu interagieren über Relays machen. Bereits WCF verwenden, können normalerweise nur diese Bindung angeben und über Relay miteinander kommunizieren. Warteschlangen und Themen erfordert jedoch mit Relais nicht Windows Applications zwar möglich, einige Programmieraufwand. keine Standardbibliotheken dienen.

Im Gegensatz zu Warteschlangen und Themen Erstellen nicht Applikationen explizit Relays. Stattdessen wird bei eine Anwendung, die Nachrichten empfangen möchte eine TCP-Verbindung mit Service Bus, Relay automatisch erstellt. Wenn die Verbindung beendet wird, wird das Relay gelöscht. Um einer Anwendung einen bestimmten Listener erstellt Relay zu ermöglichen bietet Service Bus eine Registrierung, die Anwendung einem bestimmten Relay suchen kann.

Relays sind die richtigen direkte Kommunikation zwischen Programmen erforderlich. Beispielsweise Reservierungssystem einer Fluggesellschaft aus einem lokalen Rechenzentrum, die Schalter, mobilen Geräten und anderen Computern zugreifen muss. Auf allen Systemen ausgeführt konnte Service Bus Relays in der Cloud zu kommunizieren, hängen, wo sie ausgeführt werden können.

## <a name="summary"></a>Zusammenfassung

Verbinden Applikationen schon immer Teil des vollständigen Lösungen, und Bandbreite von Szenarien, die Programme und Dienste miteinander kommunizieren müssen mehr Applikationen zu festgelegt Geräte mit dem Internet verbunden sind Mit Cloud-Technologie erreichen Sie dies über Warteschlangen, Themen und Relais soll Service Bus wesentliche Funktion einfacher zu implementieren und allgemein verfügbar.

## <a name="next-steps"></a>Nächste Schritte

Grundlagen von Azure Service Bus bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

- Wie [Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md)
- Wie Sie [Service Bus-Themen](service-bus-dotnet-how-to-use-topics-subscriptions.md)
- Wie Sie [Service Bus relay](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
- [Service Bus-Beispiele](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
