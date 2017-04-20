<properties
    pageTitle="Azure Notification Hubs"
    description="Informationen Sie zur Verwendung von Pushbenachrichtigungen in Azure. Beispiele in C# mit .NET API."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="yuaxu"/>


#<a name="azure-notification-hubs"></a>Azure Notification Hubs

##<a name="overview"></a>Übersicht

Azure Notification Hubs bieten eine einfache Verwendung für mehrere Plattformen, skalierte Push Infrastruktur mobile Pushbenachrichtigungen aus jeder Backend (in der Cloud oder lokal) senden auf mobilen Plattformen.

Mit Notification Hubs können problemlos plattformübergreifende, personalisierte Pushbenachrichtigungen gesendet abstrahiert die Details der anderen Plattform Benachrichtigungssysteme (PNS). Mit einer einzigen API-Aufruf können Sie einzelne Benutzer oder gesamte Kundensegmente mit Millionen von Benutzern auf allen Geräten anpassen.

Sie können Notification Hubs für Unternehmen und Verbraucher. Zum Beispiel:

- Benachrichtigung aktuelle News von mit geringer Latenz (Benachrichtigungshubs wird Bing Applikationen vorinstalliert auf allen Windows und Windows Phone-Geräten).
- Standortbasierte Gutscheine an Benutzersegmente senden.
- Ereignis Benachrichtigung an Benutzer oder Gruppen für Sport, Finanzen, Spiele-Applikationen.
- Benachrichtigen der Benutzer über Ereignisse wie neue Nachrichten-e-Mails und Geschäftschancen Enterprise.
- Senden Sie eine einmalige Passwörter für mehrstufige Authentifizierung erforderlich.



##<a name="what-are-push-notifications"></a>Was sind Pushbenachrichtigungen?

Smartphones und Tablets können "Benutzer benachrichtigen" ein Ereignis aufgetreten ist. Diese Benachrichtigung können viele Formen annehmen.

Die Benachrichtigung kann in Windows Store und Windows Phone-Anwendung in Form von _Toast_: ein nicht modales Fenster angezeigt wird, mit einem Sound, um eine neue Benachrichtigung zu signalisieren. Andere Notification unterstützt gehören _nebeneinander_ _raw_und _Badge_ -Benachrichtigung. Finden Sie weitere Informationen zu den Typen von Benachrichtigungen auf Windows-Geräten unterstützt [Kacheln, Abzeichen und Benachrichtigung](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx).

Apple iOS Geräte benachrichtigt der Push auch Benutzer mit einem Dialogfeld Aufforderung anzeigen oder die Benachrichtigung zu schließen. Klicken Sie auf **Ansicht** , öffnet die Anwendung, die die Nachricht empfängt. Weitere Informationen zu iOS-Benachrichtigung finden Sie unter [iOS Notifications](http://go.microsoft.com/fwlink/?LinkId=615245).

Pushbenachrichtigungen können mobile Geräte gleichzeitig energieeffiziente neue Informationen. Benachrichtigungen können durch Backend-Systeme für mobile Geräte gesendet werden, auch wenn entsprechende apps auf einem Gerät nicht aktiv sind. Pushbenachrichtigungen sind eine wichtige Komponente für Consumer apps, sind sie app Engagement und Einsatz. Benachrichtigungen eignen sich auch für Unternehmen, steigt Informationen Mitarbeiter Reaktion auf Geschäftsereignisse.

Einige Beispiele für mobile Engagement Szenarien sind:

1.  Aktualisieren einer Kachel auf Windows 8 oder Windows Phone mit aktuellen Finanzinformationen.
2.  Warnung einen Benutzer mit einem Spruch, der ein Arbeitselement an diesen Benutzer im Workflow Unternehmen app zugewiesen wurde.
3.  Namensschild mit der aktuellen Umsätze anzeigen führt eine CRM-Anwendung (wie Microsoft Dynamics CRM).

##<a name="how-push-notifications-work"></a>Push Benachrichtigungen

Push-Benachrichtigung werden über plattformspezifische Infrastrukturen _Benachrichtigungssysteme Plattform_ (PNS) aufgerufen. Ein PNS Barebones Funktionen (keine Unterstützung für die Übertragung Personalisierung) und keine gemeinsame Schnittstelle verfügen. Um eine Benachrichtigung zu einer Windows Store-app muss ein Entwickler beispielsweise WNS (Windows Notification Service) erhalten. Zum Senden einer Benachrichtigung an einen iOS-Gerät muss dieselbe Entwickler erhalten APN (Apple Push Notification Service), und senden Sie die Nachricht erneut. Azure Notification Hubs helfen, indem eine allgemeine Schnittstelle zusammen mit anderen Push-Benachrichtigung über jede Plattform unterstützt.

Auf einer hohen Ebene folgen dem gleichen Muster, alle Plattform Benachrichtigung:

1.  Die Clientanwendung stellt PNS zum Abrufen der _behandeln_. Der Handletyp hängt vom System ab. WNS, ist es ein URI oder "Benachrichtigung." APN ist es ein Token.
2.  Die Clientanwendung dieses Handle in app- _Back-End_ zur späteren Verwendung gespeichert. Back-End ist für WNS normalerweise ein Cloud-Dienst. Apple wird das System einen _Anbieter_aufgerufen.
3.  Um eine Push-Benachrichtigung zu senden, Kontakte app Back-End PNS über das Handle auf einen bestimmten Client app-Instanz.
4.  Der PNS leitet die Benachrichtigung an das Gerät mit dem Handle angegeben.

![][0]

##<a name="the-challenges-of-push-notifications"></a>Die Herausforderung Pushbenachrichtigungen

Diese Systeme sehr leistungsstark sind, lassen sie noch viel Arbeit app-Entwickler um auch Push Notification Szenarien, wie übertragen oder Senden von Pushbenachrichtigungen an segmentierte Benutzer implementieren.

Pushbenachrichtigungen sind eines der am häufigsten nachgefragten Features in Cloud-Services für mobile apps. Der Grund dafür ist die Infrastruktur arbeiten müssen relativ komplex und meist nicht mit den wichtigsten Geschäftslogik der Anwendung. Die Herausforderung in eine Push-Anforderung Infrastruktur gehören:

- **Plattform-Abhängigkeit.** Um Benachrichtigungen Geräte auf verschiedenen Plattformen müssen mehrere Schnittstellen in Back-End codiert werden. Nicht nur der systemnahen Details unterscheiden, aber die Präsentation der Benachrichtigung (Kachel, Toast oder Badge) ist ebenfalls. Diese Unterschiede führen zu komplex und schwierig zu verwalten Backend-Code.

- **Skalierung.** Skalierung der Infrastruktur hat zwei Aspekte:
    + Richtlinien PNS müssen Geräte Token Start die Anwendung aktualisiert werden. Dies führt zu viel Datenverkehr und konsequente Datenbankzugriffe nur Token Gerät Stand. Die Anzahl der Geräte (möglicherweise Millionen) wächst, wird die Kosten und die Beibehaltung dieser Infrastruktur nonnegligible.

    + Die meisten PNSs unterstützt an mehrere Geräte nicht. Daraus folgt, dass eine Übertragung an Millionen von Geräten Millionen Aufrufe der PNSs führt. Diese Anfragen skalieren ist, da in der Regel Entwickler die Gesamtwartezeit halten möchten. Zum Beispiel das letzte Gerät die Nachricht nicht erhalten die Benachrichtigung 30 Minuten nach der Benachrichtigung gesendet wurde, wie vielen Fällen widerspricht Pushbenachrichtigungen haben soll.
- **Routing.** PNSs bieten eine Möglichkeit, eine Nachricht an ein Gerät senden. In den meisten apps sind jedoch Benachrichtigung an Benutzer oder Gruppen (z. B. alle Mitarbeiter einem bestimmten Kundenkonto) ausgelegt. So um Benachrichtigungen an die richtigen Geräte weiterleiten müssen app Back-End eine Registrierung, die Gruppen mit Geräte-Token. Dieser Aufwand fügt die Gesamtzeit für die Wartung und die Kosten einer app.

##<a name="why-use-notification-hubs"></a>Warum verwenden Sie Benachrichtigungshubs?

Benachrichtigung Hubs Komplexität: keine Probleme Pushbenachrichtigungen verwalten müssen. Stattdessen können Sie eine Benachrichtigungshub. Benachrichtigungshubs eine vollständige Multiplattform, skalierte Push Notification Infrastruktur und erheblich Push-spezifischen Code, der im Backend app ausgeführt wird. Benachrichtigungshubs implementieren Sie die Funktionalität einer Push-Infrastruktur. Geräte sind nur verantwortlich für das Registrieren von PNS behandelt und Backend obliegt plattformunabhängige Nachrichtenversand an Benutzer oder Gruppen, wie in der folgenden Abbildung dargestellt:

![][1]


Benachrichtigungshubs bieten eine fertige-Push Notification Infrastruktur folgende Vorteile:

- **Mehrere Plattformen.**
    +  Unterstützung für alle wichtigen mobilen Plattformen. Benachrichtigungshubs können Windows Store, iOS und Android, Windows Phone Apps Push-Benachrichtigung senden.

    +  Benachrichtigungshubs bieten eine allgemeine Schnittstelle zum Senden von Benachrichtigungen auf allen unterstützten Plattformen. Plattformspezifische Protokolle sind nicht erforderlich. App-Back-End kann plattformspezifischen oder plattformunabhängige Formate Benachrichtigungen. Die Anwendung kommuniziert nur mit Notification Hubs.

    +  Gerätemanagement-Handle. Benachrichtigungshubs verwaltet die Handle-Registrierungsschlüssel und PNSs.

- **Mit jeder Backend**: Cloud oder lokal .NET, PHP, Java, Knoten usw..

- **Skalierung.** Benachrichtigungshubs Skalieren auf Millionen von Geräten ohne die Umstrukturierung und Splitter.


- **Umfassender Satz an Lieferung Muster**:

    - *Senden*: ermöglicht fast gleichzeitige Übertragung an Millionen von Geräten mit einem einzelnen API-Aufruf.

    - *Unicast-Multicast*: Push auf Tags darstellt einzelner Benutzer alle Geräte; oder Gruppe. beispielsweise separate Formfaktoren (Tablet oder Telefon).

    - *Segmentierung*: komplexe Segment festgelegten tagausdrücke (z. B. Geräte in New York nach der Hertha) drücken.

    Jedes Gerät, wenn das Handle mit einem benachrichtigungshub senden kann ein oder mehrere _Tags_angeben. Weitere Informationen über [Tags]. Tags müssen nicht vorab bereitgestellt oder entsorgt werden. Tags bieten eine einfache Möglichkeit, Benutzern oder Gruppen Benachrichtigungen. Da Tags alle anwendungsspezifischen Bezeichner (z. B. Benutzer oder Gruppen-IDs) enthalten können, gibt ihre Verwendung app-Back-End von der Last, speichern und Verwalten von Gerät Handles frei.

- **Personalisierung**: jedes Gerät können eine oder mehrere Vorlagen pro Gerät Lokalisierung und Personalisierung ohne Back-End-Code erreichen.

- **Sicherheit**: Shared Access Secret (SAS) oder Verbundauthentifizierung.

- **Rich Telemetrie**: verfügbar in das Portal und programmgesteuert.


##<a name="integration-with-app-service-mobile-apps"></a>Integration mit App Service mobiler Apps

Um eine nahtlose und einheitliche Erfahrung in Azure Services zu ermöglichen, unterstützt [App Service Mobile Apps] integrierten Pushbenachrichtigungen Benachrichtigungshubs verwenden. [App Service Mobile Apps] bietet eine hochgradig skalierbare, global verfügbare mobile Plattform zur Anwendungsentwicklung für Entwickler und Systemintegratoren, die umfassende Funktionen für mobile Entwickler bereitstellt.

Mobile Apps-Entwickler können mit den folgenden Arbeitsablauf Benachrichtigungshubs verwenden:

1. PNS Gerätehandle abrufen
2. Gerät registrieren und [Vorlagen] mit Benachrichtigung über praktische Mobile Apps Client SDK registrieren API
    + Beachten Sie, dass Mobile Apps Registrierungen aus Sicherheitsgründen entfernt alle Tags entfernt. Arbeiten Sie mit Notification Hubs Backend an Geräte Tags zugeordnet.
3. Benachrichtigung von app Backend mit Benachrichtigung

Hier sind einige Vorteile Entwickler Integration gebracht:

- **Mobiler Apps Client-SDKs.** Diese Multi-Plattform-SDKs bieten einfache APIs für die Registrierung und mit Notification Hub automatisch mit der app verbunden. Entwickler müssen keine Benachrichtigungshubs Anmeldeinformationen und einen zusätzlichen Dienst arbeiten.
    + Die SDKs tag automatisch das Gerät mit Mobile Apps authentifizierten Benutzer-ID zu Benutzerszenario aktivieren.
    + Die SDKs verwenden Mobile Apps Installations-ID automatisch als GUID mit Benachrichtigung, sodass Entwickler verwalten mehrere Dienst-GUIDs registriert.
    
- **Installation-Modell.** Mobile Apps arbeitet Notification Hubs neuesten Push-Modell alle Push Eigenschaften mit einem Gerät in einer JSON-Installation, die Push Notification Services ausgerichtet und benutzerfreundlich darstellen.

- **Flexibilität.** Entwickler können immer direkt mit Benachrichtigung auch an Integration funktioniert.

- **Integrierte Lösung in [Azure-Portal].** Drücken Sie eine Mobile Apps visuell dargestellt wird, und Entwickler können problemlos mit dem zugehörigen Hub durch Mobile Apps.



##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen über die Benachrichtigungshubs finden in diesen Themen Sie:

+ **[Wie Kunden Benachrichtigungshubs verwenden]**

+ **[Benachrichtigung Hubs Lernprogramme und Hilfslinien]**

+ **Benachrichtigung Hubs Schnellstart-Lernprogramme** ([iOS], [Android], [Windows Universal] [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android])

Die entsprechenden verwalteten API-Referenzen für Pushbenachrichtigungen hier befinden:

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [Wie Kunden Benachrichtigungshubs verwenden]: http://azure.microsoft.com/services/notification-hubs
  [Benachrichtigung Hubs Lernprogramme und Hilfslinien]: http://azure.microsoft.com/documentation/services/notification-hubs
  [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
  [Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
  [Universal Windows]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
  [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
  [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
  [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
  [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
  [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
  [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
  [App Service mobiler Apps]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
  [Vorlagen]: notification-hubs-templates-cross-platform-push-messages.md
  [Azure-portal]: https://portal.azure.com
  [Tags]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
