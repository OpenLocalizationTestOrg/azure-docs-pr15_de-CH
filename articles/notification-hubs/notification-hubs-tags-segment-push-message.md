<properties
    pageTitle="Routing und Tagausdrücke"
    description="Dieses Thema erklärt routing und Tag Ausdrücke für Azure Notification Hubs."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="routing-and-tag-expressions"></a>Routing und Tag-Ausdrücke

##<a name="overview"></a>Übersicht

Tag von Ausdrücken können Sie bestimmte Zielsätze Geräte oder genauer Registrierung beim Senden einer Pushbenachrichtigung durch Benachrichtigungshubs.


## <a name="targeting-specific-registrations"></a>Für bestimmte Registrierung

Die einzige Möglichkeit auf Benachrichtigung Registrierung ist das Zuordnen von Tags, zielgerichtet Tags. Wie beschrieben in [Registrierung Management](notification-hubs-push-notification-registration-management.md)muss Push-Benachrichtigung eine Anwendung ein Handle für einen an einen benachrichtigungshub registrieren. Nach Erstellung eine Registrierung eine Benachrichtigung Hubs kann Backend Anwendung Pushbenachrichtigungen zu senden.
Anwendung Backend können Registrierungen auf eine bestimmte Benachrichtigung auf folgende Weise:

1. **Übertragung**: alle Einträge im benachrichtigungshub darüber informiert.
2. **Tags**: benachrichtigt alle Einträge, die das angegebene Tag enthalten.
3. **Tag-Ausdruck**: alle Einträge, deren Tags entsprechen den angegebenen Ausdruck, benachrichtigt.

## <a name="tags"></a>Tags

Ein Tag kann eine beliebige Zeichenfolge mit alphanumerischen bis zu 120 Zeichen und folgenden nicht-alphanumerische Zeichen: '_' ‘@’, '#', '. ',':', '-'. Das folgende Beispiel zeigt eine Anwendung, die Fehlermeldung Popupbenachrichtigungen zu bestimmten Gruppen. In diesem Szenario wird eine einfache Möglichkeit, Route Benachrichtigungen Bezeichnung Registrierung mit Tags, die die verschiedenen Bereiche wie in der folgenden Abbildung dargestellt.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

In diesem Bild erreicht die Meldung tagged **Beatles** Tablet, die mit dem Tag **Beatles**registriert.

Weitere Informationen zum Erstellen der Registrierung für Tags finden Sie unter [Registrierung Management](notification-hubs-push-notification-registration-management.md).

Benachrichtigungen können Tags Methoden senden Benachrichtigung von der `Microsoft.Azure.NotificationHubs.NotificationHubClient` -Klasse in [Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. Sie können auch Node.js oder Push Notifications REST-APIs.  Hier ist ein Beispiel mit dem SDK.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Tags müssen nicht vorab bereitgestellt werden und mehrere app-spezifische Konzepte finden. Beispielsweise können Benutzer dieser Anwendung wird auf Kommentieren und Popups, nicht nur für die Kommentare zu ihren Lieblingsbands, sondern für alle Kommentare von ihren Freunden, unabhängig von der Band erhalten die sie kommentieren möchten. Die folgende Abbildung zeigt ein Beispiel für dieses Szenario:



![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

In diesem Bild Alice möchte Updates für die Beatles, und Bob Updates für die Wailers interessiert ist. Bob ist Charlie Kommentare und Charlie in der Wailers interessiert. Beim Senden einer Benachrichtigung für Charlie Kommentar auf die Beatles erhalten Alice und Bob.

Beim Codieren mehrere Probleme in Tags (z. B. "Band_Beatles" oder "Follows_Charlie") sind Tags einfache Zeichenfolgen und nicht an Eigenschaften mit Werten. Eine Registrierung ist nur in Anwesenheit oder Abwesenheit eines bestimmten Tags gesucht.

Ein Schritt für Schritt zur Verwendung von Tags an Gruppen finden Sie unter [Breaking News](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).


## <a name="using-tags-to-target-users"></a>Mithilfe von Tags für Zielgruppen

Eine weitere Möglichkeit zum Verwenden von Tags werden alle Geräte eines bestimmten Benutzers ermitteln. Registrierung können mit einem Tag versehen, die eine Benutzer-, wie in der folgenden Abbildung ID:


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

In diesem Bild erreicht Uid:Alice Nachricht markiert alle Registrierungen markierte Uid:Alice; Damit alle Geräte von Alice.


##<a name="tag-expressions"></a>Tagausdrücke

Es gibt Fälle in der Benachrichtigung auf einen Satz von Einträgen, der nicht durch ein einzelnes Tag, sondern einen booleschen Ausdruck Tags identifiziert wird.

Betrachten Sie eine sportanwendung, die eine Erinnerung an alle in Boston ein Spiel zwischen Red Sox und Cardinals sendet. Die Clientanwendung Tags Zinsen und Standort registriert, sollte die Benachrichtigung an alle Boston ausgerichtet werden, Red Sox oder die Cardinals ist. Diese Bedingung kann mit den folgenden booleschen Ausdruck ausgedrückt werden:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Tagausdrücke enthalten alle boolesche Operatoren wie AND (& &), oder (|) und (!). Sie können auch Klammern enthalten. Tagausdrücke sind auf 20 Tags enthält nur oder Andernfalls sind sie auf 6 Tags.

Hier ist ein Beispiel für das Senden von Nachrichten mit Tags mit dem SDK.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)"; 

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
