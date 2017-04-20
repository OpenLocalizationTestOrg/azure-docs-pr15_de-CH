<properties 
    pageTitle="Azure Notification Hubs - Diagnose-Richtlinien" 
    description="Richtlinien zum Diagnostizieren allgemeiner Probleme bei Azure Notification Hubs." 
    services="notification-hubs" 
    documentationCenter="Mobile" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure Notification Hubs - Diagnose-Richtlinien

##<a name="overview"></a>Übersicht

Eine der häufigsten Fragen von Azure Notification Hubs Kunden hören ist wie herauszufinden, warum sie eine Mitteilung über ihre Anwendung Backend nicht angezeigt, Clientgerät - wo und warum Benachrichtigung gelöscht wurden und dieses Problem zu beheben. In diesem Artikel gehen wir über verschiedene Gründe Benachrichtigungen möglicherweise verloren oder Ende nicht auf den Geräten. Wir sehen auch Wege in denen können Sie analysieren und die Ursache herauszufinden. 

Zunächst ist es wichtig, zu verstehen, wie Azure Notification Hubs Benachrichtigungen zu den Geräten legt.
![][0]

In einem Benachrichtigung normalerweise senden wird die Nachricht aus dem **Back-End-Anwendung** auf **Azure Notification Hub (NH)** gesendet die wiederum verarbeiten alle Registrierungen Berücksichtigung konfigurierten Tags und tagausdrücke bestimmen "Ziele", d. h. alle Registrierungen, die Push-Benachrichtigung erhalten. Diese Registrierung können über eine oder alle unterstützten Plattformen - iOS, Google, Windows, Windows Phone umfassen Kindle und Baidu für China Android. Sobald die Ziele eingerichtet wurden, NH dann Push Benachrichtigungen, verteilt mehrere Stapel von Einträgen auf der Plattform des bestimmten **Push Notification Service (PNS)** - APN für Apple beispielsweise GCM Google usw.. NH authentifiziert mit entsprechenden PNS basierend auf den Azure-Verwaltungsportal auf Konfigurieren Notification Hub festgelegten Anmeldeinformationen. Der PNS leitet dann Nachrichten an die entsprechenden **Client-Geräte**. Dies ist die empfohlene Methode Pushbenachrichtigungen und beachten, dass der letzte Teil Benachrichtigungsübermittlung zwischen der PNS-Plattform und das Gerät Plattform. Daher haben wir vier Hauptkomponenten - *Client*, *Back-End-Anwendung*, kann *Azure Notification Hubs (NH)* und *Push Notification Services (PNS)* und diese Benachrichtigung verworfen. Weitere Informationen zu dieser Architektur ist auf [Benachrichtigung Hubs]verfügbar.

Fehler Benachrichtigungen vielleicht während der ersten Test/Bereitstellung phase der ein Konfigurationsproblem hinweisen oder Vorkommen in der Produktion, alle oder einige der Benachrichtigung verworfenen, dass eine tiefere Anwendung oder messaging Muster Problem geworden ist. Im Abschnitt betrachten unten wir Verworfene Benachrichtigungen Szenarien von gemeinsamen seltener Art, die Sie offensichtlich finden und andere nicht. 

##<a name="azure-notifications-hub-mis-configuration"></a>Azure Benachrichtigungen Hub Fehlkonfiguration 

Azure Notification Hubs muss sich im Kontext der Anwendung des Entwicklers zu entsprechenden PNS erfolgreich Benachrichtigungen zu authentifizieren. Dies ermöglicht der Entwickler ein Entwicklerkonto mit der jeweiligen Plattform (Google, Apple, Windows usw.) erstellen und registrieren ihre Anwendung erhalten sie Anmeldeinformationen im Portal unter Benachrichtigungshubs Konfigurationsabschnitt konfiguriert werden müssen. Wenn keine Benachrichtigung über sind, sollte zunächst sicher, dass die richtigen Anmeldeinformationen im Notification Hub mit der Anwendung unter ihren spezifischen Plattformentwickler-Konto erstellt werden. Unsere [Erste Schritte Lernprogramme] finden für diesen Prozess einen Schritt gehen Sie. Hier sind einige häufige Fehlkonfigurationen:

1. **Allgemein**
 
    (a) stellen Sie sicher, dass der Hubnamen (ohne Tippfehler) identisch:

    - Wo Sie vom Client registrieren, 
    - Wo Benachrichtigung vom Backend senden  
    - Wo Sie PNS Anmeldeinformationen konfiguriert haben und 
    - Deren SAS-Anmeldeinformationen der Client und der Back-End konfiguriert. 
        
    (b) stellen sicher, dass Sie richtigen SAS Konfigurationszeichenfolgen Client und Backend-Anwendung verwenden. Als Faustregel gilt müssen Sie **DefaultListenSharedAccessSignature** auf dem Client und **DefaultFullSharedAccessSignature** Anwendung Backend verwenden (was Berechtigung zu NH Benachrichtigung)

2. **Apple Push Notification Service (APN) Konfiguration**
 
    Sie müssen zwei verschiedene Hubs - für Produktion und weitere Tests Zweck. Dies bedeutet hochladen Zertifikat Sandbox-Umgebung mit einem separaten Hub verwendet werden und die Produktion an einen eigenen Hub verwendet werden. Versuchen Sie nicht, verschiedene Arten von Zertifikaten an denselben Hub hochladen Benachrichtigung Fehler zu verursachen. Wenn Sie sich in einer Position finden, in dem Sie verschiedene Zertifikat versehentlich an denselben Hub hochgeladen haben, wird empfohlen Hub löschen und neu zu beginnen. Wenn aus irgendeinem Grund Sie nicht den Hub dann mindestens löschen können, müssen Sie alle vorhandenen Einträge vom Hub löschen. 

3. **Google Cloud Messaging (GCM) Konfiguration** 

    (a) stellen Sie sicher, dass Sie "Google Cloud Messaging für Android" unter dem Cloud-Projekt aktivieren. 
    
    ![][2]
    
    (b) stellen Sie sicher, dass "Server Key" erstellen, während den Anmeldeinformationen erhalten die NH GCM Authentifizierung verwenden. 
    
    ![][3]
    
    (c) stellen sicher, dass Sie "Projekt-ID" auf dem Client darauf konfiguriert haben, vollständig numerische Entität ist, die aus dem Dashboard zu erhalten:
    
    ![][1]

##<a name="application-issues"></a>Probleme

1) **Tags / Tag Ausdrücke**

Wenn Sie Tags und tagausdrücke für die Zielgruppe segment verwenden, kann es immer beim Senden der Benachrichtigung ist kein Ziel basierend auf Tags-Tag Ausdrücke senden Aufruf angeben gefunden. Es empfiehlt sich, Ihrer Registrierung, um sicherzustellen, dass bei Benachrichtigung senden und überprüfen Sie die Benachrichtigung Lieferung nur von Clients mit diesen Tags zu überprüfen. Z.b. mit Ihrer Registrierung mit NH getan wurden Tag "Politik" sagen, Sie senden eine Benachrichtigung mit "Sport" wird sie keines Gerät gesendet. Komplexer Fall könnte tagausdrücke, in denen Sie nur, mit "A" "Tag B" beim Senden von Benachrichtigungen registriert, Zielen "Tag A & & Tag B". Im folgenden Tipps Selbstdiagnose-Abschnitt werden Methoden mit Ihrer Registrierung mit Tags überprüfen können Sie 

2) **Probleme mit Dokumentvorlagen**

Wenn Sie Vorlagen verwenden und sicherstellen, dass die [Vorlage Anleitung]beschriebenen Richtlinien folgen. 

3) **Ungültige Registrierung**

Wenn Notification Hub richtig konfiguriert wurde und alle Tags-Tag Ausdrücke wurden verwendet, richtig suchen gültige Ziele, die Benachrichtigungen gesendet werden müssen, werden mehrere Verarbeitung Batches parallel - jeden Batch Senden von Nachrichten an einen Satz von Einträgen NH feuert. 

> [AZURE.NOTE] Da wir parallel verarbeiten, garantiert nicht die Reihenfolge in der Mitteilung zugestellt werden. 

Azure Benachrichtigungen Hub ist jetzt für eine Nachricht "am meisten einmal" Modell optimiert. Daher versuchen wir eine Deduplizierung, damit keine Benachrichtigung mehr als einmal an ein Gerät übermittelt werden. Zum dafür über die Registrierung und stellen Sie sicher, dass nur eine Nachricht pro Gerät-ID gesendet wird, bevor die Nachricht tatsächlich an die PNS. Jeder Batch an den PNS gesendet wird akzeptiert und die Registrierung überprüfen, kann der PNS erkennt Fehler mit der Registrierung eines Stapels Azure NH ein Fehler zurückgegeben und damit vollständig löschen, Batch beendet. Dies gilt insbesondere für APN ein Streaming-Protokoll TCP verwendet. Auch wenn wir am meisten werden nach Lieferung in diesem Fall wir fehlerhaften Registrierung aus unserer Datenbank entfernen, und wiederholen Sie die Benachrichtigungsübermittlung für den Rest der Geräte in diesem Batch.

Fehlerinformationen für die fehlgeschlagenen Versuch gegen eine Registrierung Azure Notification Hubs REST APIs abrufen: [pro Nachricht Telemetrie: Get Notification Message Telemetrie](https://msdn.microsoft.com/library/azure/mt608135.aspx) und [PNS Feedback](https://msdn.microsoft.com/library/azure/mt705560.aspx). [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) Code anzeigen

##<a name="pns-issues"></a>PNS Probleme

Die Nachricht vom jeweiligen PNS eingegangen ist es die Verantwortung zum Übermitteln der Benachrichtigung an das Gerät. Azure Notification Hubs ist hier und kann nicht steuern, wenn oder wenn die Benachrichtigung an das Gerät übermittelt werden. Da Notification Services Plattform ziemlich stabil sind, neigen Benachrichtigungen PNS Geräte in wenigen Sekunden erreichen. Wenn PNS jedoch eingeschränkt ist dann Azure Notification Hubs gilt eine exponentielle Backoff-Strategie und bleibt PNS 30 Minuten nicht erreichbar müssen wir eine Richtlinie abläuft und die Nachrichten permanent löschen. 

Wenn ein PNS versucht, eine Benachrichtigung zu senden, aber das Gerät offline ist, die Benachrichtigung vom PNS für einen begrenzten Zeitraum gespeichert und an das Gerät übermittelt, wenn verfügbar. Nur eine letzte Benachrichtigung für eine bestimmte Anwendung gespeichert. Wenn mehrere Benachrichtigungen gesendet werden, während das Gerät offline ist, wird jede neue Benachrichtigung Vorankündigung verworfen werden. Dadurch halten die neueste Benachrichtigung wird genannt zusammenfügenden Benachrichtigungen in APN und GCM (die Reduzierung Schlüsselverwendung) reduzieren. Wenn das Gerät längere Zeit offline bleibt, werden keine Benachrichtigungen für sie gespeichert wurden verworfen. Quelle - [APN Anleitung] & [GCM-Hinweise]

Mit Azure Notification - kann Sie zusammenfügen Schlüssel über einen HTTP-Header mit den generischen `SendNotification` API (z.B. .NET SDK- `SendNotificationAsync`) die auch HTTP-Header als übergeben ist an den jeweiligen PNS. 

##<a name="self-diagnose-tips"></a>Selbstdiagnose-Tipps

Hier prüfen wir die verschiedenen Wege zur diagnose und Stamm keine Benachrichtigungshub Probleme verursachen:

###<a name="verify-credentials"></a>Anmeldeinformationen überprüfen

1. **PNS-Entwicklerportal**

    Überprüfen sie die jeweiligen PNS Developer Portal (APN GCM, WNS usw.) über unsere [Erste Schritte Lernprogramme].

2. **Azure Classic-portal**

    Wechseln Sie zur Registerkarte konfigurieren zu überprüfen und die PNS-Entwicklerportal Anmeldeinformationen entsprechen. 

    ![][4]

###<a name="verify-registrations"></a>Überprüfen der Registrierung

1. **Visual Studio**

    Verwenden Sie Visual Studio für die Entwicklung können Sie Microsoft Azure und verbinden und verwalten eine Reihe von Azure Services einschließlich Benachrichtigungen Hub aus dem "Server Explorer". Dies ist in erster Linie für die Entwicklung/Test-Umgebung. 

    ![][9]

    Sie können anzeigen und verwalten alle Einträge der Hub die native Plattform gut geordnet oder Registrierung der Dokumentvorlage, Tags PNS Bezeichner, Identifikationsnummer und das Ablaufdatum. Sie können auch eine Registrierung - dynamisch bearbeiten ist beispielsweise Tags bearbeiten möchten. 

    ![][8]
 
    > [AZURE.NOTE] Visual Studio-Funktionen zum Bearbeiten der Registrierung sollte nur während der Test-mit begrenzten Anzahl von Einträgen verwendet werden. Tritt in Massen Ihrer Registrierung beheben müssen, sollten Sie mithilfe der Registrierung Funktionalität Export/Import- [Export/Import-Registrierung] (https://msdn.microsoft.com/library/dn790624.aspx) beschrieben.

2. **Service Bus-explorer**

    Viele Kunden nutzen ServiceBus Explorer- [ServiceBus-Explorer] zum Anzeigen und Verwalten ihrer Notification Hub beschrieben. Es ist ein open-Source-Projekt code.microsoft.com - [ServiceBus Explorer-code]

###<a name="verify-message-notifications"></a>Benachrichtigung überprüfen

1. **Azure-Verwaltungsportal**

    Sie gelangen zur Registerkarte "Debug" Test für Ihre Clients Benachrichtigungen benötigen alle Service-Backend einrichten und ausführen. 

    ![][7]

2. **Visual Studio**

    Testbenachrichtigungen senden Sie aus Visual Studio Komfort:

    ![][10]

    Erfahren Sie mehr über die Visual Studio Notification Hub Azure Explorer Funktionen- 
    
    - [Übersicht über die VS Server-Explorer]
    - [VS Server Explorer Blogbeitrag - 1]
    - [VS Server Explorer Blogbeitrag - 2]

###<a name="debug-failed-notifications-review-notification-outcome"></a>Fehlgeschlagene Benachrichtigung Debug / Benachrichtigung Ergebnis überprüfen

**EnableTestSend-Eigenschaft**

Beim Senden einer Benachrichtigung über das Notification Hubs zunächst es nur wird in die Warteschlange für NH Verarbeitung um herauszufinden, ihre Ziele und schließlich NH sendet es an den PNS. Dies bedeutet, dass bei Verwendung von REST-API oder eine Client-SDK erfolgreiche Rückgabe des Aufrufs senden bedeutet, dass mit Benachrichtigung erfolgreich die Nachricht eingereiht wurde hat. Es Einblick nicht in was passiert, wenn NH schließlich Nachricht PNS. Wenn die Benachrichtigung nicht auf dem Clientgerät eintreffen, besteht die Möglichkeit, als NH übermitteln der Nachricht an PNS, Fehler z.B. die Nutzlastgröße die zulässige PNS überschritten in NH konfigurierten Anmeldeinformationen sind ungültig usw.. Um einen Einblick in die PNS Fehler erhalten, haben wir eine Eigenschaft namens [EnableTestSend Funktion]eingeführt. Diese Eigenschaft wird automatisch aktiviert, wenn Sie Testnachrichten Portal oder Visual Studio-Client senden und somit ausführliche Debuginformationen angezeigt. Sie können dies über APIs Beispiel jetzt wird .NET SDK und werden schließlich an alle Client-SDKs hinzugefügt werden. Zur Verwendung mit den übrigen Aufruf fügen Sie einen Abfragezeichenfolgeparameter aufgerufen, "test" am Ende des Aufrufs senden z.B. 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Beispiel (.NET SDK)*
 
Angenommen Sie, Sie verwenden .NET SDK native Popupbenachrichtigung senden:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
 
`result.State`wird einfach State `Enqueued` am Ende der Ausführung ohne Einsicht in Ihre Push passierte. Jetzt können Sie die `EnableTestSend` boolesche Eigenschaft beim Initialisieren der `NotificationHubClient` und erhalten detaillierte Statusinformationen PNS-Fehler beim Senden der Benachrichtigung. Aufruf senden hier erfordert zusätzliche Zeit zurück, da es nur zurück nach NH PNS das Ergebnis bestimmt die Benachrichtigung übermittelt hat. 
 
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);
    
    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);
    
    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Ausgabe*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong
 
Diese Meldung zeigt eine ungültigen Anmeldeinformationen in Notification Hub oder ein Problem mit der Registrierung auf dem Hub konfiguriert sind und empfohlen wäre diese Registrierung löschen und den Client vor dem Senden der Nachricht erstellen. 
 
> [AZURE.NOTE] Beachten Sie die Verwendung dieser Eigenschaft stark eingeschränkt, so muss nur Hiermit in Test-/Umgebung mit begrenzten Anzahl von Einträgen. Wir Benachrichtigung nur Debug 10 Geräte. Es können maximal Verarbeitung sendet Debug 10 pro Minute. 

###<a name="review-telemetry"></a>Telemetrie überprüfen 

1. **Verwenden von Azure-Verwaltungsportal**

    Das Portal ermöglicht einen schnellen Überblick über alle Aktivitäten auf Ihrem Haupt-Benachrichtigung erhalten. 
    
    (a) auf der Registerkarte "Dashboard" können Sie eine zusammengesetzte Ansicht der Registrierung, Benachrichtigung und die Fehler pro Plattform anzeigen. 
    
    ![][5]
    
    (b) Sie können auch viele andere Plattform Metriken hinzufügen, auf der Registerkarte "Überwachen" zu besonders einmal PNS bestimmte Fehler beim NH versucht die Benachrichtigung an den PNS zurückgegeben. 
    
    ![][6]
    
    (c) Sie sollten beginnen Sie mit **Eingehenden Nachrichten** **Registrierung Operationen** **Erfolgreich Benachrichtigungen** und fahren pro Plattform Registerkarte PNS Fehler überprüfen. 
    
    (d) haben Sie Notification Hub mit Authentifizierung falsch, Authentifizierungsfehler PNS wird angezeigt. Dies ist eine gute PNS Anmeldeinformationen zu überprüfen. 

2) **Programmgesteuerter Zugriff**

Mehr- 

- [Programmgesteuerte Telemetrie Zugriff]
- [Zugriff über APIs Beispiel Telemetrie] 

> [AZURE.NOTE] Mehrere Telemetrie bezogene Funktionen wie **Export/Import-Registrierungen** **Telemetrie Zugriff über APIs** usw. stehen nur in Standard-Stufe. Wenn Sie versuchen, diese Funktionen zu verwenden, wenn Sie frei oder grundlegenden Ebene sind erhalten zu diesem Effekt Sie während der Verwendung des SDK und einen HTTP 403 (Verboten) Wenn direkt von REST-APIs. Stellen Sie sicher, daß Sie Standard Stufen über Azure-Verwaltungsportal.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png
 
<!-- LINKS -->
[Übersicht über Notification Hubs]: notification-hubs-push-notification-overview.md
[Erste Schritte-Lernprogramme]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Vorlage-Hinweise]: https://msdn.microsoft.com/library/dn530748.aspx 
[APN Anleitung]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM-Hinweise]: http://developer.android.com/google/gcm/adv.html
[Importieren Sie Registrierung/exportieren]: http://msdn.microsoft.com/library/dn790624.aspx
[ServiceBus-Explorer]: http://msdn.microsoft.com/library/dn530751.aspx
[ServiceBus Explorer-code]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Übersicht über die VS Server-Explorer]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[VS Server Explorer Blogbeitrag - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[VS Server Explorer Blogbeitrag - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend-Funktion]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Programmgesteuerte Telemetrie Zugriff]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Zugriff über APIs Beispiel Telemetrie]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

 