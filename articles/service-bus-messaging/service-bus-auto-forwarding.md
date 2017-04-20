<properties 
    pageTitle="Automatische Weiterleitung Service Bus messaging Entitäten | Microsoft Azure"
    description="Zum Verketten einer Warteschlange oder das Abonnement weiterleiten oder Thema."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Verkettung Service Bus Entitäten mit automatische Weiterleitung

Das Feature *Automatische Weiterleitung* können Sie Verketten einer Warteschlange oder das Abonnement weiterleiten oder Thema denselben Namespace gehört. Automatische Weiterleitung aktiviert, Service Bus automatisch wird entfernt Nachrichten Warteschlange oder Abonnement (Quelle) befinden und sie in die zweite Warteschlange oder Thema (Ziel). Beachten Sie, dass man eine Nachricht direkt an die Zielentität senden. Es ist auch nicht möglich eine Unterwarteschlange wie eine Warteschlange für unzustellbare Nachrichten weiterleiten oder Thema zu verketten.

## <a name="using-auto-forwarding"></a>Automatische Weiterleitung verwenden

Automatische Weiterleitung können durch Festlegen der [QueueDescription.ForwardTo][] oder der [SubscriptionDescription.ForwardTo][] -Eigenschaft auf [QueueDescription][] oder [SubscriptionDescription][] -Objekte für die Quelle, wie im folgenden Beispiel.

```
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Die Zielentität bestehen Zeitpunkt erstellten Quellentität. Wenn die Zielentität nicht existiert, gibt Service Bus Ausnahme gefragt Quellentität erstellen.

Automatische Weiterleitung können Sie ein einzelnes Thema skalieren. Service Bus beschränkt die [Anzahl von Abonnements zu einem bestimmten Thema](service-bus-quotas.md) zu 2.000. Zusätzliche Abonnements bietet L2-Themen erstellen. Beachten Sie, dass selbst wenn Sie nicht durch Service Bus-Einschränkung für die Anzahl der Abonnements gebunden sind, Hinzufügen einer zweiten Themen den Gesamtdurchsatz Thema verbessern kann.

![Automatische Weiterleitung Szenario][0]

Sie können auch automatische Weiterleitung um Absender, Empfänger zu entkoppeln. Angenommen, ein ERP-System besteht aus drei Modulen: Auftragsabwicklung, Lagerverwaltung und Customer Relations Management. Jedes dieser Module generiert Nachrichten die Warteschlange in einem entsprechenden Thema. Alice und Bob sind Vertriebsmitarbeiter, die Interesse an alle Nachrichten, die Kunden beziehen. Um die Nachrichten erstellen Alice und Bob eine Persönliche Warteschlange und ein Abonnement auf der ERP, der automatisch alle Nachrichten in der Warteschlange weiterleiten

![Automatische Weiterleitung Szenario][1]

Wenn Alice auf Urlaub, ihre Persönliche Warteschlange ERP-Thema geht, füllt. In diesem Szenario da Vertriebsmitarbeiter keine Nachrichten erhalten hat erreichen keine Themen ERP Kontingent.

## <a name="auto-forwarding-considerations"></a>Automatische Weiterleitung Aspekte

Die Zielentität angesammelt viele Nachrichten und überschreitet das Kontingent oder die Zielentität deaktiviert hinzugefügt Quellentität Nachrichten der [Warteschlange](service-bus-dead-letter-queues.md) bis Speicherplatz im Ziel (oder die Entität aktiviert ist). Nachrichten werden weiterhin in der Warteschlange, Leben müssen explizit empfangen und aus der Warteschlange zu verarbeiten.

Themen zu zusammengesetzten Thema viele Abonnements verketteten wird empfohlen haben eine überschaubare Anzahl an Abonnements auf dem ersten Thema und viele Abonnements auf der zweiten Ebene Themen. Ersten Thema mit 20, jede davon mit einer zweiten Ebene Thema mit 200 Abonnements verkettet können beispielsweise für höheren Durchsatz als ersten Thema 200 Abonnements jeweils ein zweitrangiges Thema mit 20 verkettet.

Service Bus Rechnungen einmal für jedes weitergeleitete Nachricht. Beispielsweise ist Senden einer Nachricht an ein Thema mit 20 Abonnements Sie so konfiguriert, dass Auto-Forward-Nachrichten weiterleiten oder Thema 21 Operationen berechnet alle ersten Abonnements erhält eine Kopie der Nachricht.

Um ein Abonnement zu erstellen, die mit einer anderen Warteschlange oder Thema verkettet ist, muss der Ersteller des Abonnements **Verwalten** der Quell- und der Zielentität berechtigt. Senden von Nachrichten an die Quelle Thema erfordert nur Berechtigungen Thema Quelle **Senden** .

## <a name="next-steps"></a>Nächste Schritte

Ausführliche Informationen über die automatische Weiterleitung finden Sie unter den folgenden Themen:

- [SubscriptionDescription.ForwardTo][]
- [QueueDescription][]
- [SubscriptionDescription][]

Um weitere Informationen zu Service Bus Leistungssteigerungen Siehe [partitionierte messaging Entitäten][].

  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [SubscriptionDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.forwardto.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [SubscriptionDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.aspx
  [0]: ./media/service-bus-auto-forwarding/IC628631.gif
  [1]: ./media/service-bus-auto-forwarding/IC628632.gif
  [Partitionierte messaging Entitäten]: service-bus-partitioning.md