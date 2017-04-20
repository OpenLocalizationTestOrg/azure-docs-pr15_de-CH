<properties
    pageTitle="Service Bus Relay-Übersicht | Microsoft Azure"
    description="Überblick über Service Bus Relay."
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
    ms.date="09/01/2016"
    ms.author="sethm"/>


# <a name="overview-of-service-bus-relay"></a>Überblick über Service Bus relay

Eine wichtige Komponente des Service Bus ist ein zentrales (aber sehr Lastenausgleich) *Relay* -Dienst, mit dem Sie hybride Anwendung erstellen, die in Azure-Rechenzentrum und Ihrer lokalen Enterprise-Umgebung ausgeführt.  Service Bus Relay unterstützt eine Vielzahl von verschiedenen Protokollen und Webdienstestandards. Dies schließt SOAP, WS-*, und REST. -Dienst ermöglicht hybride Anwendung aktivieren Sie sicheren Windows Communication Foundation (WCF)-Dienste verfügbar machen, die in einem Unternehmensnetzwerk für die öffentliche Cloud ohne Öffnen einer firewallverbindung befinden oder intrusive Änderung der Netzwerk-Infrastruktur. 

![Relay-Konzepte](./media/service-bus-relay-overview/sb-relay-01.png)

Der Relaydienst unterstützt herkömmliche unidirektionales messaging, Anforderung/Antwort-messaging und Peer-to-Peer-messaging. Ereignis Verteilung Internetbereich Veröffentlichen/Abonnieren-Szenarien und bidirektionale Socketkommunikation Point erhöhte Effizienz unterstützt. 

Weitergeleitete Nachrichten Muster ein lokalen Dienst Relay-Service über einen ausgehenden Port her und erstellt einen Socket bidirektionale Kommunikation an bestimmte Rendezvous gebunden. Der Client kann dann Senden von Nachrichten an den Relay-Service für die Adresse Rendezvous mit vor-Ort-Service kommunizieren. Der Relay-Dienst wird dann "Nachrichten an den lokalen Dienst über bidirektionale Socket bereits relay". Der Client eine direkte Verbindung mit dem lokalen Dienst nicht benötigen, ist es nicht erforderlich, wo sich der Dienst befindet und der lokalen Dienst benötigt keine eingehenden Ports in der Firewall geöffnet.

Die Verbindung initiieren zwischen den lokalen Dienst und den Relay-Service mit einer Reihe von WCF "Relay" Bindungen. Hinter den Kulissen ordnen Relay-Bindungen neue Transport Bindungselemente WCF-Kanal-Komponenten erstellen, die Integration mit Service Bus in der Cloud entwickelt. 

## <a name="next-steps"></a>Nächste Schritte

Ausführliche Informationen über den Service Bus Relay finden.

- [Übersicht über die Architektur von Azure Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Verwendung den Service Bus-Dienst](service-bus-dotnet-how-to-use-relay.md)

 