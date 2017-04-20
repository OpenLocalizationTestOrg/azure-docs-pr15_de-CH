<properties 
    pageTitle="Service-Architektur | Microsoft Azure"
    description="Beschreibt die Meldung und die Architektur von Azure Service Bus Relay."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/11/2016"
    ms.author="sethm" />

# <a name="service-bus-architecture"></a>Service-Architektur

Dieser Artikel beschreibt die Meldung und die Architektur von Azure Service Bus Relay.

## <a name="service-bus-scale-units"></a>Service Bus Maßeinheiten

*Maßeinheiten*Service Bus organisiert. Eine Skalierungseinheit ist ein und enthält alle Komponenten des Dienstes ausgeführt. Jede Region stellt mindestens Service Bus Maßeinheiten.

Eine Skalierungseinheit ist ein Service Bus-Namespace zugeordnet. Die Mengen-Einheit behandelt Service Bus Elementtypen: Relays und vermittelten messaging Entitäten (Warteschlangen, Themen Abonnements). Eine Skalierungseinheit Dienstbus besteht aus folgenden Komponenten:

- **Eine Reihe von Gateway-Knoten.** Gateway Knoten Anfragen zu authentifizieren und Relay-Anfragen behandelt. Jeder gatewayknoten hat eine öffentliche IP-Adresse.

- **Eine Reihe von messaging Broker-Knoten.** Messaging-Broker-Knoten die Anfragen über messaging Entitäten.

- **Ein Gateway-Speicher.** Gatewayspeicher enthält die Daten für jede Entität in dieser Skalierungseinheit definiert ist. Gatewayspeicher ist eine SQL Azure-Datenbank implementiert.

- **Mehrere messaging speichert.** Messaging-Speicher enthalten die Nachrichten aller Warteschlangen, Themen und Abonnements, die in dieser Skalierungseinheit definiert sind. Es enthält auch alle Abonnementdaten. Wenn [partitionierte messaging Entitäten](service-bus-partitioning.md) aktiviert ist, wird eine Warteschlange oder ein Thema ein Nachrichtenspeicher zugeordnet. Abonnements sind in der gleichen messaging als ihre übergeordneten Thema gespeichert. Messaging Shops werden außer Service Bus [Premium-Messaging](service-bus-premium-messaging.md)auf SQL Azure-Datenbanken.

## <a name="containers"></a>Container

Jede Entität messaging ist einen bestimmten Container zugeordnet. Ein Container ist ein logisches Konstrukt, das genau ein messaging zu speichern alle relevante Daten für diesen Container verwendet. Jeder Container zugewiesen messaging Broker-Knoten. Normalerweise sind mehrere Container als messaging Broker-Knoten. Daher lädt jeden messaging Broker-Knoten mehrere Container. Die Verteilung von Containern, messaging Broker-Knoten ist so organisiert, dass auch alle Messaging-Broker-Knoten geladen werden. Die Last Muster ändern (z. B. eines der Container wird ausgelastet) oder messaging Broker-Knoten vorübergehend nicht verfügbar ist, werden die Container unter messaging Broker-Knoten verteilt.

## <a name="processing-of-incoming-messaging-requests"></a>Verarbeitung von eingehenden Nachrichten

Wenn ein Client eine Anforderung an Service Bus sendet, von Azure Lastenausgleich an Gateway Knoten weitergeleitet. Gatewayknoten wird die Anforderung autorisiert. Betrifft die Anforderung messaging Entität (Warteschlange, Thema Abonnement), gatewayknoten der Entität im gatewayspeicher sucht und bestimmt, in welche Nachrichtenspeicher Entität befindet. Es sucht dann nach messaging Broker Knoten bedient derzeit Container und sendet die Anforderung an diesen messaging Broker-Knoten. Messaging-Broker-Knoten verarbeitet und aktualisiert den Entitätszustand im Container Informationsspeicher. Der messaging Broker sendet dann der Antwort zurück an gatewayknoten sendet eine entsprechende Antwort an den Client zurück, der die ursprüngliche Anforderung ausgegeben.

![Verarbeitung von eingehenden Nachrichten](./media/service-bus-architecture/IC690644.png)

## <a name="processing-of-incoming-relay-requests"></a>Verarbeitung von Anfragen weiterleiten

Wenn ein Client eine Anforderung an Service Bus sendet, von Azure Lastenausgleich an Gateway Knoten weitergeleitet. Wenn die Anforderung eine Anforderung empfangen wird, erstellt gatewayknoten neue Relay. Ist die Anforderung einer Verbindungsanfrage zu einem bestimmten Relay leitet gatewayknoten die Verbindungsanfrage Gateway-Knoten, der die Weiterleitung besitzt. Der gatewayknoten, der das Relay besitzt, sendet eine Anforderung Rendezvous an den überwachenden Client fragt den Listener temporären Kanal zum gatewayknoten erstellen, die die Verbindungsanfrage erhalten.

Wenn der Relay-Verbindung tauschen Clients Nachrichten über den gatewayknoten für das Rendezvous verwendet wird.

![Verarbeitung von Anfragen weiterleiten](./media/service-bus-architecture/IC690645.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie gelesen haben eine Übersicht über Service Bus-Architektur für den Einstieg finden Sie auf der folgenden Links:

- [Übersicht über Service Bus](service-bus-messaging-overview.md)
- [Service Bus-Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
- [Eine Warteschlange mit Servicebuswarteschlangen Messaginglösung](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)
