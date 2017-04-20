<properties
    pageTitle="Service Bus Premium und Standard Messaging Preise Ebenen Übersicht | Microsoft Azure"
    description="Service Bus Premium und Standard Messaging"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/02/2016"
    ms.author="darosa;sethm"/>

# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Service Bus Premium und Standard Messaging-Stufen 

Service Bus messaging die messaging Entitäten wie Warteschlangen sowie Themen kombiniert Enterprise messaging-Funktionen mit Semantik Cloud Ebene veröffentlichen-abonnieren. Messaging Service Bus als Backbone Kommunikation anspruchsvolle Cloudlösungen dient.

Die *Premium* -Ebene Messaging Service Bus behandelt allgemeine Kundenanfragen Skalierung, Performance und Verfügbarkeit für wichtige Einsatzbereiche. Obwohl die Funktion fast identisch sind, dienen diese zwei Stufen des Service Bus messaging verschiedene dienen.

In der folgenden Tabelle werden einige allgemeine Unterschiede hervorgehoben.

| Premium                               | Standard                       |
|---------------------------------------|--------------------------------|
| Hoher Durchsatz                       | Variable Durchsatz            |
| Vorhersagbare Leistung               | Variable Latenz               |
| Vorhersehbare Preise                   | Quellenbesteuerung Variablen Preisen |
| Nach Arbeitslast skalieren | N/A                            |
| Nachricht Größe > 256KB                  | Nachrichtengröße beträgt 256KB          |

**Service Bus Premium-Messaging** bietet eine Ressource Isolierung Ebene CPU- und damit jede Arbeitslast Kunden isoliert ausgeführt wird. Diese Ressourcencontainer wird eine *Einheit messaging*bezeichnet. Jeder Namespace Premium ist mindestens eine Messaging-Einheit zugewiesen. Sie können 1, 2 oder 4 messaging Einheiten für jeden Premium-Service Bus-Namespace erwerben. Eine einzelne Arbeitslast oder Entität kann mehrere messaging Einheiten umfassen und messaging Stückzahl zwar Abrechnung 24 Stunden oder die täglichen Kosten wird, geändert werden. Das Ergebnis ist vorhersehbar und wiederholbare Leistung für Ihr Service Bus-basierte Lösung.

Nicht nur ist diese Leistung besser vorhersagbar und verfügbar, sondern ist außerdem schneller. Service Bus Premium messaging basiert auf das Speichermodul in [Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/)eingeführt. Spitzenleistung ist viel schneller als mit der standardmäßigen Premium-messaging.

## <a name="premium-messaging-technical-differences"></a>Premium Messaging Unterschiede

Es folgen einige Unterschiede zwischen Premium und Standard Messaging-Stufen.

### <a name="partitioned-queues-and-topics"></a>Partitionierte Warteschlangen und Themen

Partitionierte Warteschlangen und Themen Premium messaging unterstützt, jedoch funktionieren genauso wie die Ebenen Standard und Basic Service Bus Messaging nicht. Premium-messaging wird keine SQL als Datenspeicher und mehr Wettbewerb Quelle zugeordnet einer gemeinsamen Plattform. Daher ist es nicht erforderlich, die Partitionierung. Zusätzlich wurde die Anzahl der Partitionen von 16 Partitionen 2 Partitionen Premium Standard Messaging geändert. Zwei Partitionen Verfügbarkeit und geeigneter Nummer für die Premium-Runtime-Umgebung. Weitere Informationen zur Partitionierung finden Sie unter [partitionierte Warteschlangen und Themen](service-bus-partitioning.md).

### <a name="express-entities"></a>Express-Entitäten

Da Premium-Messaging in einer vollständig isoliert Runtime-Umgebung ausgeführt wird, werden express Entitäten in Premium-Namespaces nicht unterstützt. Weitere Informationen zu express-Funktion finden Sie unter [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx) -Eigenschaft.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Service Bus messaging finden.

- [Einführung in Azure Service Bus Premium messaging (Blogbeitrag)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Einführung in Azure Service Bus Premium messaging (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Übersicht über Service Bus](service-bus-messaging-overview.md)
- [Wie Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md)
