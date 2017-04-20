<properties
    pageTitle="Azure Notification Hubs - häufig gestellte Fragen (FAQs)"
    description="Fragen und Antworten zu entwerfen/Umsetzung auf Benachrichtigungshubs"
    services="notification-hubs"
    documentationCenter="mobile"
    authors="ysxu"
    manager="erikre"
    keywords="Push-Benachrichtigung Pushbenachrichtigungen Pushbenachrichtigungen iOS, android Pushbenachrichtigungen, Ios-Push, android push"
    editor="" />

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu" />

#<a name="push-notifications-with-azure-notification-hubs---frequently-asked-questions"></a>Pushbenachrichtigungen Sie mit Azure Notification Hubs - häufig gestellte Fragen

##<a name="general"></a>Allgemein
###<a name="1---what-is-the-price-model-for-notification-hubs"></a>1. Was ist das Preismodell für Notification Hubs?
Benachrichtigungshubs wird in drei Versionen angeboten:

* **Frei** - bis zu 1 Million Push pro Abonnement pro Monat erhalten.
* **Basic** - Get 10 Millionen Push pro Abonnement pro Monat als Grundlage mit Kontingent Wachstum.
* **Standard** - Push 10 Millionen pro Abonnement pro Monat als Grundlage erhalten, erhöhen mit Optionen sowie umfangreiche Telemetrie-Funktionen.

Neuesten Informationen finden auf der Seite [Benachrichtigung Hubs Preise] . Die Preise auf Abonnementebene festgelegt und basiert auf der Anzahl Push Notification einweihungen so wie viele Namespaces oder Benachrichtigung Hubs egal in Azure-Abonnement erstellt haben.

**Frei** -Ebene wird für Entwicklungszwecken ohne SLA angeboten. Zwar diese Ebene einen guten Ausgangspunkt für diejenigen, die die Funktionen von Pushbenachrichtigungen über Azure Notification Hubs möchten möglicherweise nicht die beste Wahl für mittlere bis großen Anwendung sein.

**Grundlegende** & **Standard** Stufen Angeboten für Produktion Verwendung mit den folgenden Hauptfunktionen *Anwendungsebene für den Standard*aktiviert:

- *Rich Telemetrie* - Benachrichtigungshubs bieten eine Reihe von Funktionen zum Exportieren der Telemetriedaten sowie push Notification-Registrierungsinformationen für die offline-Anzeige und Analyse.
- *Mehrere Mandanten* - Ideal, wenn Sie eine mobile Anwendung mit Notification Hubs unterstützen mehrere Mandanten erstellen. Ermöglicht Push Notification Services (PNS) Anmeldeinformationen auf Namespaceebene Notification Hub für die Anwendung festgelegt, und dann können Sie einzelne Hubs unter diesem gemeinsamen Namespace bietet Mieter trennen. Dies ermöglicht einfache Wartung während der SAS-Tasten zum Senden und Empfangen von Pushbenachrichtigungen von Notification Hubs getrennt für jeden Mandanten nicht zwischen Mieter Überlappung sicherstellen.
- *Geplante Push* - können Sie Push-Benachrichtigung zu planen, die anschließend in die Warteschlange und versendet werden.
- *Massenimport* - können Sie Registrierungen gebündelt.

###<a name="2---what-is-the-notification-hubs-sla"></a>2. was Notification Hubs SLA ist?
**Einfache** und **standardmäßige** Benachrichtigungshubs Ebenen garantieren wir werden mindestens 99,9 % der Zeit ordnungsgemäß konfigurierten Anwendung Pushbenachrichtigungen senden oder Registrierung Management Operationen in Bezug auf ein Notification-Hub innerhalb einer unterstützten Ebene bereitgestellt. Erfahren Sie mehr über unsere SLA finden Sie auf der Seite [Benachrichtigung Hubs SLA] .

> [AZURE.NOTE] Es gibt keine Garantien SLA der Strecke zwischen den Benachrichtigungsdienst Plattform und da externe Plattformanbieter Push-Benachrichtigung an das Gerät zu Notification Hubs abhängen.

###<a name="3---which-customers-are-using-notification-hubs"></a>3. welche Kunden Benachrichtigungshubs verwenden?
Wir haben eine große Anzahl von Kunden Benachrichtigungshubs einige wichtige unten:

* Sotschi 2014 – 100 s Interessengruppen 3 Millionen Geräte, 150 Millionen Benachrichtigung in 2 Wochen verteilt. [CaseStudy - Sotschi]
* Skanska - [CaseStudy - Skanska]
* Seattle Times - [CaseStudy - Seattle Times]
* Mural.ly - [CaseStudy - Mural.ly]
* 7Digital - [CaseStudy - 7Digital]
* Bing-Apps – 10 Millionen von Geräten, 3 Millionen Benachrichtigungen pro Tag senden.

###<a name="4-how-do-i-upgrade-or-downgrade-my-notification-hubs-to-change-my-service-tier"></a>4. wie upgrade oder downgrade Meine Hubs Notification Service-Tier ändern?
Zum [Azure-Verwaltungsportal]und klicken Sie auf den Namespace des Benachrichtigung Hubs auf Service Bus. Auf der Registerkarte Skalierung werden Sie Ihrer Dienstebene Benachrichtigungshubs ändern.

![](./media/notification-hubs-faq/notification-hubs-classic-portal-scale.png)

##<a name="design--development"></a>Entwurf und Entwicklung
###<a name="1---which-server-side-platforms-do-you-support"></a>1. unterstützt welche serverseitigen Plattformen werden?
Wir bieten SDKs und [vollständige Beispiele] für .NET Java, PHP, Python, Node.js, damit eine app Back-End-Kommunikation mit Notification Hubs werden kann mit diesen Plattformen. Benachrichtigung Hubs APIs basieren auf anderen Schnittstellen direkt den stattdessen sprechen möchten Sie zusätzliche Abhängigkeit hinzufügen können. Weitere Informationen finden auf [NH - REST-APIs] .

###<a name="2---which-client-platforms-do-you-support"></a>2. unterstützt die Client-Plattformen werden?
Wir unterstützen senden Pushbenachrichtigungen [Apple iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android China (über Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) & [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Chrome-Apps](notification-hubs-chrome-push-notifications-get-started.md) und [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari) -Plattformen. Eine vollständige Liste der Einführende Lernprogramme Bekämpfung senden Pushbenachrichtigungen auf diesen Plattformen finden Sie auf unserer [NH - erste Schritte Lernprogramme] .

###<a name="3---do-you-support-smsemailweb-notifications"></a>3. e-Mail-/ SMS/Web-Benachrichtigungen unterstützen?
Benachrichtigungshubs dient hauptsächlich Benachrichtigungen mobiler apps mit den oben genannten Plattformen. Wir bieten keine noch die Möglichkeit, e-Mail oder SMS-Benachrichtigung senden; Drittanbieter-Plattformen mit diesen Funktionen können jedoch mit Notification Hubs systemeigene Pushbenachrichtigungen Senden von [Azure Mobile Apps]integriert werden.

Benachrichtigungshubs bieten auch nicht im Browser Push Notification Lieferung Service Out-von-der-ein. Kunden können mit SignalR auf serverseitige Plattformen implementieren. Wenn Sie Browser-apps in der Sandbox Chrome Benachrichtigungen auf, informieren Sie sich [Chrome-Apps-Lernprogramm].

###<a name="4---what-is-the-relation-between-azure-mobile-apps-and-azure-notification-hubs-and-when-do-i-use-what"></a>4 Was ist die Beziehung zwischen Azure Mobile Apps und Azure Notification Hubs und wann verwende ich was?
Wenn Sie eine vorhandene mobile app Backend und nur die Möglichkeit zum Senden von Pushbenachrichtigungen hinzufügen möchten, können Sie Azure Notification Hubs verwenden. Möchten Sie mobile app Backend völlig setup dann sollten Sie mit Azure Mobile Apps. Azure-Mobile-App stellt automatisch eine Benachrichtigungshub für Pushbenachrichtigungen leicht vom Back-End mobile Anwendung senden. Preise für Azure Mobile Apps enthält die Grundgebühren für Benachrichtigungshub und Zahlen Sie nur wenn enthalten Push hinausgehen. Weitere Informationen zu den Kosten stehen auf der Seite [Anwendung Preisen] .

###<a name="5---how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>5. unterstützen wie viele Geräte kann ich, wenn Senden von Pushbenachrichtigungen über Notification Hubs?
Siehe Seite [Benachrichtigung Hubs Preise] ausführliche Informationen über die Anzahl der unterstützten Geräte.

Für bestimmte Szenarien benötigen Sie Unterstützung für mehr als 10.000.000 registrierte Geräte bitte direkt [Kontakt](https://azure.microsoft.com/overview/contact-us/) und Ihre Lösung hilft Ihnen.

###<a name="6---how-many-push-notifications-can-i-send-out"></a>6. wie viele Pushbenachrichtigungen können ich senden?
Je nach der ausgewählten Ebene wird Azure automatisch basierend auf der Anzahl von Nachrichten durch das System skalieren.

>[AZURE.NOTE] Die Gesamtauslastung Kosten kann basierend auf der Anzahl von Pushbenachrichtigungen bedient steigen. Stellen Sie sicher, dass vorhandene Ebene Grenzen auf [Benachrichtigung Hubs Preise] beschrieben sind.

Benachrichtigungshubs nutzen unsere Kunden um täglich Millionen von Pushbenachrichtigungen zu senden. Sie müssen keinen etwas Besonderes Ihre Push Skalieren erreichen Benachrichtigungen als mithilfe von Azure Notification Hubs.

###<a name="7---how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>7. wie lange dauert es für Pushbenachrichtigungen zu Mein Gerät?
Azure Notification Hubs wird können mindestens **1 Mio. Push-Benachrichtigung sendet eine Minute** in einen normalen Szenario, in dem eingehende Load ziemlich konsistent und nicht spikey Natur, verwenden. Diese Rate variieren je nach Tags, Art der eingehenden sendet und externen Faktoren.

Während die geschätzte Lieferzeit des Dienstes kann der Ziele pro Plattform und Route Nachrichten in der jeweiligen Push Notification Delivery Services basierend auf Ausdrücken registrierte Tags-Tag berechnen. Hier obliegt es Push Notification Services (PNS), um die Benachrichtigung an das Gerät senden.

Ein PNS garantiert keine SLA zum Übermitteln von Benachrichtigungen. jedoch normalerweise die meisten Pushbenachrichtigungen liefern auf Zielgeräte innerhalb weniger Minuten (in der Regel im Rahmen von 10 Minuten) ab sie unsere Plattform an. Möglicherweise werden einige Ausreißer die mehr Zeit in Anspruch.

>[AZURE.NOTE] Azure Notification Hubs hat eine Push-Benachrichtigung PNS 30 Minuten zugestellt werden nicht gelöscht. Diese Verzögerung kann eine Reihe von Gründen, die häufig daran PNS Ihrer Anwendung eingeschränkt ist.

###<a name="8---is-there-any-latency-guarantee"></a>8 gibt es keine Wartezeit Garantie?
Aufgrund der Natur von Pushbenachrichtigungen (sie werden durch eine externe, plattformspezifische Push Notification Service geliefert) ist nicht Wartezeit gewährleistet. Normalerweise die meisten Pushbenachrichtigungen zugestellt in wenigen Minuten.

###<a name="9---what-are-the-considerations-i-need-to-take-into-account-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>9. Was sind beim Entwerfen einer Lösung mit Namespaces und Benachrichtigungshubs berücksichtigen müssen?

####<a name="mobile-appenvironment"></a>Mobile App-Umgebung

* Es sollte eine Benachrichtigungshub pro mobile app pro Umgebung.
* In einem Szenario mit mehreren Mandanten muss jeder einen eigenen Hub.
* Sie müssen denselben Notification Hub zwischen Tests und Produktion nie Teilen dieser Probleme zu verursachen beim Senden von Benachrichtigungen. z.B. bietet Apple Sandbox und Produktion Push Endpunkte jeweils eigenen Anmeldeinformationen.
* Standardmäßig können Sie Ihre registrierte Geräte über das Azure-Portal oder Azure integrierte Komponente in Visual Studio testbenachrichtigungen senden. Der Schwellenwert wird auf 10 Geräte festgelegt, die aus der Registrierung nach dem Zufallsprinzip ausgewählt.

>[AZURE.NOTE] Wenn Hub wurde ursprünglich mit einem Apple Sandbox Zertifikat konfiguriert und ein Apple Produktion Zertifikat konfiguriert, würde die alte Gerät Token mit dem neuen Zertifikat ungültig und Push-Vorgänge fehlschlagen. Es empfiehlt sich, trennen die Produktion Umgebung testen und anderen Hubs für verschiedene Unternehmen.

####<a name="pns-credentials"></a>PNS-Anmeldeinformationen

Beim Registrieren einer Apps in einer Plattform-Entwicklerportal (Apple oder Google usw.) erhalten Sie eine app-ID und Sicherheit ist NULL ein Backend app auf der Plattform Push Notification Services Pushbenachrichtigungen an Geräte gesendet werden muss. Diese Sicherheitstoken in Form von Zertifikaten (z. B. Apple iOS und Windows Phone) oder Sicherheitsschlüssel (Google Android, Windows usw.), können in Notification Hubs konfiguriert werden müssen. Dies geschieht normalerweise Ebene Hub Benachrichtigung jedoch auf Namespace-Ebene in einer mehrinstanzenfähigen Szenario realisiert werden kann.

####<a name="namespaces"></a>Namespaces

Namespaces können für die Bereitstellung Gruppierung verwendet werden.  Es kann auch verwendet werden, alle Notification Hubs für alle Mandanten gleichen App im Multi-Tenant-Szenario dargestellt.

####<a name="geo-distribution"></a>Geo-Verteilung

Geo-Verteilung ist nicht immer wichtig Push Notification Szenarien. Es ist zu beachten, dass verschiedene Push Notification Services (APN, GCM usw.), die Geräte schließlich Pushbenachrichtigungen liefern, nicht gleichmäßig verteilt entweder.

Wenn Sie eine Anwendung, die weltweit verwendet wird haben, können Sie mehrere Hubs in verschiedenen Namespaces nutzen die Verfügbarkeit Benachrichtigungshubs in verschiedenen Azure-Regionen weltweit erstellen.

>[AZURE.NOTE] Dadurch die Management-Kosten - insbesondere um Anmeldungen ist nicht empfehlenswert und dürfen nur ausgeführt werden, wenn explizite erforderlich ist.

###<a name="10--should-i-do-registrations-from-the-app-backend-or-directly-through-client-devices"></a>10. tun kann ich Erfassen von Backend-app oder direkt über Client Geräte?
Registrierung von app-Backend eignen Clientauthentifizierung vor die Registrierung erstellen möchten oder wenn man Tags erstellt oder geändert von app Back-End-Anwendungslogik Grundlage. Weitere Informationen Weitere Seiten [Backend-Registrierung Anleitung] und [Back-End-Registrierung Guidance - 2] .

###<a name="11--what-is-the-push-notification-delivery-security-model"></a>11 Was Push Notification Delivery-Sicherheitsmodell ist?
Azure Notification Hubs verwenden eine [Gemeinsame Zugriff Signatur (SAS)](../storage/storage-dotnet-shared-access-signature-part-1.md)-Sicherheitsmodell basiert. SAS-Token können auf der Stammebene Namespace oder Benachrichtigungshubs detailliert. Diese SAS-Token kann mit verschiedenen Autorisierungsregeln z.B. senden Nachricht Berechtigungen, Benachrichtigungsberechtigungen usw. überwachen. Weitere Informationen finden im Dokument [NH Sicherheitsmodell] .

###<a name="12--how-should-i-handle-sensitive-payload-in-the-push-notifications"></a>12. wie sollten vertrauliche Nutzlast Pushbenachrichtigungen behandeln?
Alle Anträge werden Geräte von der Plattform Push Notification Services (PNS). Absender sendet eine Benachrichtigung an Azure Notification Hubs wir verarbeiten und die jeweiligen PNS die Benachrichtigung übergeben.

Alle Verbindungen des Absenders Azure Benachrichtigung Hubs und dann PNS verwenden HTTPS.

>[AZURE.NOTE] Azure Benachrichtigung Hubs meldet die Nutzlast der Nachricht nicht in keiner Weise.

Senden vertrauliche Nutzlasten jedoch eine sichere Push Muster empfohlen, Absender 'ping' Benachrichtigung mit einer Nachricht an das Gerät ohne vertrauliche Nutzlast und erhält die Anwendung auf dem Gerät diese Nutzlast bietet, ist es eine sichere API direkt zum Abrufen der Nachrichtendetails aufrufen. Eine Anleitung zum Implementieren von des oben beschriebenen Musters steht auf [NH - Secure Push-Lernprogramm] .

##<a name="operations"></a>Vorgänge
###<a name="1---what-is-the-disaster-recovery-dr-story"></a>1. Was steckt Disaster Recovery (DR)?
Wir decken Metadaten Disaster Recovery auf unserer Seite (Notification Hub Name, Verbindungszeichenfolge und andere wichtige Informationen). Auslösen einer DR-Szenario werden die Registrierung **nur** Benachrichtigungshubs Infrastruktur verloren. Sie müssen eine Lösung, um diese Daten in Ihrem neuen Hub nach der Wiederherstellung erneut aufzufüllen implementieren.

- *Schritt 1* - Erstellen einer sekundären Notification Hub in einem anderen Rechenzentrum. Können diese dynamisch zum Zeitpunkt des Ereignisses DR oder Erstellen von Get wechseln. Es ist viel Option machen ist Notification Hub Bereitstellung relativ schnellen Vorgang in wenigen Sekunden. Eine vom Anfang wird Schild beeinträchtigen die Verwaltungsfunktionen ist die empfehlenswerte Option DR-Ereignis.

- *Schritt 2* - Octahydrat sekundären Notification Hub mit dem primären Notification Hub. Es wird nicht empfohlen, erfassen beide Hubs verwalten und versuchen, dynamisch synchronisiert wie Registrierung - kommen in der Regel, die auf PNS durch inhärente Tendenz der Registrierung abläuft funktioniert hat. Benachrichtigungshubs bereinigen sie PNS Feedback zu abgelaufenen oder ungültigen Registrierung erhalten.  

Empfehlung ist ein app-Backend entweder:

- Verwaltet einen vorgegebenen Einträge am Ende dies Bulk Insert sekundären Notification Hub bei Dr.

**ODER**

- Ruft regelmäßig Erfassungen primär-Hub als Backup sichern und anschließend eine eingefügt in sekundären NH.

>[AZURE.NOTE] Registrierung Importieren/Exportieren von Funktionen Standard Tier wird in [Registrierung Export/Import] -Dokument beschrieben.

Haben Sie einen Back-End, dann bei die Anwendung auf den Zielgeräten startet, führen sie eine neue Registrierung im sekundären Notification Hub und schließlich den sekundären Notification Hub müssen alle aktiven Geräte registriert.

Der Nachteil ist, dass ein Zeitraum Wenn Geräte, apps geöffnet haben, nicht benachrichtigt werden.

###<a name="2---is-there-any-audit-log-capability"></a>2 gibt es Audit Log-Funktion?
Alle Notification Hubs Verwaltungsvorgänge werden Protokolle die in [Azure-Verwaltungsportal]verfügbar gemacht werden.

##<a name="monitoring--troubleshooting"></a>Überwachung und Fehlerbehebung
###<a name="1---what-troubleshooting-capabilities-are-available"></a>1. welche Fehlerbehebung sind verfügbar?
Azure Notification Hubs bieten verschiedene Funktionen, die allgemeine Problembehandlung, insbesondere in den meisten Fällen um gelöschte Nachrichten führen. Finden Sie in unseren [NH - Problembehandlung] Whitepaper.

###<a name="2---what-telemetry-features-are-available"></a>2. was Telemetrie-Funktionen sind verfügbar?
Azure Notification Hubs ermöglicht das Anzeigen von Daten in [Azure-Verwaltungsportal]. Details der verfügbaren Metriken sind auf der Seite [NH - Metriken] .

>[AZURE.NOTE] Erfolgreiche Benachrichtigungen bedeuten nur, dass Pushbenachrichtigungen externe Push Notification Service (APN für Apple, GCM Google usw.) geliefert wurden. PNS zum Übermitteln der Benachrichtigung auf Zielgeräte liegt. In der Regel macht die PNS Übermittlung Metriken an Dritte.  

Wir bieten auch die Möglichkeit zum Exportieren der Telemetriedaten programmgesteuert (im **Standard** -Tier). [NH - Metriken Beispiel] Details anzeigen

[Azure-Verwaltungsportal]: https://manage.windowsazure.com
[Benachrichtigungshubs Preise]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Benachrichtigungshubs SLA]: http://azure.microsoft.com/support/legal/sla/
[CaseStudy - Sotschi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[CaseStudy - Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[CaseStudy - Seattle Zeiten]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[CaseStudy - Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[CaseStudy - 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[NH - REST-APIs]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[NH - Lernprogramme Einstieg]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Chrome Apps Lernprogramm]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[Registrierung der Back-End-Anleitung]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Back-End-Registrierung Guidance - 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[NH Sicherheitsmodell]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[NH - sichere Push-Lernprogramm]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[NH - Problembehandlung]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[NH - Metriken]: https://msdn.microsoft.com/library/dn458822.aspx
[NH - Metriken-Beispiel]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[Registrierung importieren/exportieren]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure Portal]: https://portal.azure.com
[vollständige Beispiele]: https://github.com/Azure/azure-notificationhubs-samples
[Azure mobiler Apps]: https://azure.microsoft.com/en-us/services/app-service/mobile/
[App Servicepreise]: https://azure.microsoft.com/en-us/pricing/details/app-service/
