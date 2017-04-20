<properties
    pageTitle="Was ist Azure Relay? | Microsoft Azure"
    description="Übersicht über Azure Relay"
    services="service-bus"
    documentationCenter=".net"
    authors="banisadr"
    manager="timlt"
    editor="" />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="babanisa" />

# <a name="what-is-azure-relay"></a>Was ist Azure Relay?

Azure-Dienst ermöglicht hybride Anwendung ermöglicht es Ihnen, sichere Dienste verfügbar machen, die in einem Unternehmensnetzwerk für die öffentliche Cloud ohne Öffnen einer firewallverbindung befinden oder intrusive Änderung der Netzwerk-Infrastruktur. Relay unterstützt eine Vielzahl von verschiedenen Protokollen und Webdienstestandards.

Relay-Dienst unterstützt herkömmliche unidirektionale Anforderung/Antwort und Peer-to-Peer-Datenverkehr. Ereignis Verteilung Internetbereich Veröffentlichen/Abonnieren-Szenarien und bidirektionale Socketkommunikation Point erhöhte Effizienz unterstützt. 

Muster Übertragung weitergeleitete Daten ein lokalen Dienst Relay-Service über einen ausgehenden Port her und erstellt einen Socket bidirektionale Kommunikation an bestimmte Rendezvous gebunden. Der Client kann dann Senden von Datenverkehr an den Relay-Service für die Adresse Rendezvous mit dem lokalen Dienst kommunizieren. -Dienst wird anschließend "Daten vom lokalen Dienst über einen bidirektionalen Socket zu dedizierten relay". Der Client eine direkte Verbindung mit dem lokalen Dienst nicht benötigen, ist es nicht erforderlich, wo sich der Dienst befindet und der lokalen Dienst benötigt keine eingehenden Ports in der Firewall geöffnet.

Die Hauptfunktionen Elemente von Relay sind bidirektionale, ungepuffert Kommunikation über Netzwerkgrenzen hinweg TCP wie Drosselung, Endpunkt Discovery, Konnektivitätsstatus und überlagert Endgeräte-Sicherheit. Relay Funktionen unterscheiden sich auf integrationstechnologien wie VPN in, dass es kann, einen einzelnen Endpunkt auf einem einzigen Computer begrenzt werden während der VPN-Technologie ist weitaus aufdringlich, wie er im Netzwerk ändern.

Azure Relay hat zwei Funktionen:

1. [Hybrid-Verbindungen](#hybrid-connections) - verwendet den offenen standard Web Sockets für Szenarien

2. [WCF-Relais](#wcf-relays) - verwendet Windows Communication Foundation prozeduralen Remoteaufrufe aktivieren

Hybridverbindungen und WCF-Relays aktivieren sichere Verbindung mit Assests, die innerhalb eines Unternehmen. Einer der anderen hängt von Ihren Erfordernissen unten:

|                                    | WCF-Relay | Hybridverbindungen |
| ---------------------------------- |:---------:|:------------------:|
| **WCF**                            |     x     |                    |
| **.NET core**                      |           |         x          |
| **.NET Framework**                 |     x     |         x          |
| **JavaScript-NodeJS**              |           |         x          |
| **Java***                          |           |         x          |
| **Standardbasierte offenes Protokoll**  |           |         x          |
| **Mehrere Modelle der RPC-Programmierung** |           |         x          |
*<sub>Allgemeine Verfügbarkeit</sub>

## <a name="hybrid-connections"></a>Hybridverbindungen

Azure Relay Hybridverbindungen ist eine sichere, offene Protokoll Entwicklung der vorhandenen Relay-Funktionen, die können implementiert auf einer beliebigen Plattform und in jeder Sprache, die eine grundlegende WebSocket-Funktion umfasst explizit die WebSocket-API gemeinsam Webbrowser. Hybrid-Verbindung basiert auf HTTP und WebSockets.

## <a name="wcf-relays"></a>WCF-Relays

WCF-Relay funktioniert für das vollständige.NET Framework (NETFX) und WCF. Die Verbindung initiieren zwischen den lokalen Dienst und den Relay-Service mit einer Reihe von WCF "Relay" Bindungen. Hinter den Kulissen ordnen Relay-Bindungen neue Transport Bindungselemente WCF-Kanal-Komponenten erstellen, die Integration mit Service Bus in der Cloud entwickelt.

## <a name="service-history"></a>Service Geschichte

Hybridverbindungen transplantationen ehemaligen Namen gleichmäßig auf Azure Service Bus WCF Relay "BizTalk-Dienste"-Funktion. Neue Hybrid-Verbindungen Funktionalität ergänzt das vorhandene WCF-Relay und diese zwei Funktionen sind nebeneinander in den Relay-Service absehbare vorhanden. Freigeben einer common Gateway, sondern werden je.

WCF-Relay ist legacy Relay bietet viele Kunden bereits mit ihren WCF-Programmierung verwenden können.

## <a name="next-steps"></a>Nächste Schritte:

- [Relay – häufig gestellte Fragen](relay-faq.md)
- [Erstellen eines Namespaces](relay-create-namespace-portal.md)
- [Erste Schritte mit .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Erste Schritte mit Knoten](relay-hybrid-connections-node-get-started.md)