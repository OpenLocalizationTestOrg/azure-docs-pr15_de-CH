<properties
    pageTitle="Übersicht über Service Bus | Microsoft Azure"
    description="Service Bus Messaging: flexible Datenübertragung in der cloud"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="sethm"/>


# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Service Bus messaging: flexible Datenübertragung in der Cloud

Microsoft Azure Service Bus ist eine zuverlässige Informationen. Dieser Dienst soll Kommunikation zu erleichtern. Wenn zwei oder mehr Parteien Informationen austauschen möchten, benötigen sie einen Kommunikationsmechanismus. Service Bus ist ein Mechanismus vermittelten oder Drittanbieter-Kommunikation. Dies ähnelt einer Post in der realen Welt. Postdienste machen es sehr leicht an verschiedene Briefe und Pakete mit einer Vielzahl von Lieferung garantiert überall in der Welt.

Bereitstellung von Buchstaben POST, ähnelt Service Bus flexible Datenübertragung von Absender und Empfänger. Messaging-Dienstes wird sichergestellt, dass die Informationen geliefert wird, auch wenn die beiden Parteien nie beide online gleichzeitig oder sie genaue gleichzeitig nicht verfügbar sind. Auf diese Weise ähnelt messaging senden Sie einen Brief nicht vermittelte Kommunikation ist ähnlich zu Anruf (oder wie ein Telefonanruf werden vor dem Anruf warten und Anrufer-ID sind eher vermittelten messaging - verwendet).

Absender der Nachricht kann auch eine Übermittlung Eigenschaften einschließlich Transaktionen, Duplikaterkennungsregeln zeitlichen Ablauf und Batchverarbeitung erfordern. Diese Postleitzahl Analogien sowie haben: Wiederholen Sie die Übermittlung, erforderliche Signatur, Änderung oder erinnern.

Servicebus unterstützt zwei unterschiedliche messaging Muster: *Relay* und *vermittelten messaging*.

## <a name="service-bus-relay"></a>Service Bus Relay

[Relaykomponente des Service Bus](../service-bus-relay/service-bus-relay-overview.md) ist ein zentrales (aber sehr Lastenausgleich) Dienst, der eine Vielzahl von verschiedenen Transportprotokollen und Webdienstestandards unterstützt. Dies schließt SOAP, WS-*, und REST. [-Dienst](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) können bietet eine Vielzahl von verschiedenen Relay Konnektivitätsoptionen und direkte Peer-to-Peer-Verbindung aushandeln, wenn möglich. Service Bus für .NET Entwickler, die Windows Communication Foundation (WCF), hinsichtlich Leistung und Handhabung, optimiert und bietet vollständigen Zugriff auf die Relay-Service über SOAP und anderen Schnittstellen. Dadurch können für SOAP oder REST Programmierumgebung Service Bus integrieren.

Der Relaydienst unterstützt herkömmliche unidirektionales messaging, Anforderung/Antwort-messaging und Peer-to-Peer-messaging. Unterstützt auch Ereignis Verteilung Internetbereich aktivieren Veröffentlichen-Abonnieren Szenarien und bidirektionale Socketkommunikation Point erhöhte Effizienz. Weitergeleitete Nachrichten Muster ein lokalen Dienst Relay-Service über einen ausgehenden Port her und erstellt einen Socket bidirektionale Kommunikation an bestimmte Rendezvous gebunden. Der Client kann dann Senden von Nachrichten an den Relay-Service für die Adresse Rendezvous mit vor-Ort-Service kommunizieren. Der Relay-Dienst wird dann "Nachrichten an den lokalen Dienst über bidirektionale Socket bereits relay". Der Client eine direkte Verbindung mit dem lokalen Dienst nicht erforderlich und muss wissen, wo sich der Dienst befindet und der lokalen Dienst benötigt keine eingehenden Ports in der Firewall geöffnet.

Sie initiiert die Verbindung zwischen den lokalen Dienst und -Dienst eine Reihe von WCF "Relay" Bindings. Hinter den Kulissen Karte Relay-Bindungen transport Bindungselemente WCF-Kanal-Komponenten erstellen, die Integration mit Service Bus in der Cloud entwickelt.

Service Bus Relay bietet viele Vorteile, erfordert jedoch, dass Server und Client sowohl zum Senden und Empfangen von Nachrichten gleichzeitig online sein. Dies ist nicht optimal für HTTP-Stil Kommunikation in der Anfragen in der Regel langlebig dürfen nicht für Clients, die nur gelegentlich verbinden wie Browser, mobile Clientanwendungen usw.. Vermittelte messaging unterstützt entkoppelte Kommunikation und Vorteile; Clients und Server können bei Bedarf und ihre Vorgänge asynchron ausführen.

## <a name="brokered-messaging"></a>Vermittelte messaging

Im Gegensatz zu Relay-Schema kann [vermittelten messaging](service-bus-queues-topics-subscriptions.md) als asynchron vorstellen oder "zeitlich entkoppelt." (Absender) Erzeugern und Verbrauchern (Empfänger) müssen nicht gleichzeitig online sein. Die messaging-Infrastruktur speichert zuverlässig Nachrichten in "Bank" (z. B. eine Warteschlange) bis konsumierenden Partei empfangen werden. Dadurch können die Komponenten einer verteilten Anwendung, entweder freiwillig getrennt werden. z. B. für Wartung oder aufgrund eines Absturzes Komponente ohne Beeinträchtigung des gesamten Systems. Darüber hinaus haben die empfangende Anwendung nur online zu bestimmten Zeiten des Tages wie ein Lagerverwaltungssystem stammen, die nur zum Ende des Tages erforderlich ist.

Die Hauptkomponenten der Service Bus vermittelten Messaginginfrastruktur sind Warteschlangen, Themen und Abonnements.  Der wichtigste Unterschied ist, dass Themen Veröffentlichen/Abonnieren-Funktionen unterstützen, die für anspruchsvolle Inhaltsbasiertes routing und Zustellung Logik, einschließlich senden an mehrere Empfänger verwendet werden kann. Diese Komponenten können neue asynchrone Messaging-Szenarien, z. B. zeitliche Entkopplung, Veröffentlichen/Abonnieren und Lastenausgleich. Weitere Informationen zu diesen messaging Entitäten finden Sie unter [Servicebuswarteschlangen, Themen und Abonnements](service-bus-queues-topics-subscriptions.md).

Mit der Infrastruktur Relay wird vermittelten Messagingfunktion WCF und.NET Framework-Programmierer und über REST bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Service Bus messaging finden.

- [Service Bus-Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
- [Servicebuswarteschlangen, Themen und Abonnements](service-bus-queues-topics-subscriptions.md)
- [Wie Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md)
- [Zum Service Bus Topics und Abonnements](./service-bus-dotnet-how-to-use-topics-subscriptions.md)
 
