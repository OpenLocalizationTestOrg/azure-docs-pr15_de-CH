<properties
   pageTitle="Mit der HTTP-Listener Connector Logik Apps | Microsoft Azure App Service "
   description="Zum Erstellen und Konfigurieren der HTTP-Listener und HTTP-Aktion Connector oder API-app und in eine Anwendung Logik in Azure App Service verwenden"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/31/2016"
   ms.author="prkumar"/>


# <a name="get-started-with-the-http-listener-and-http-action-and-add-it-to-your-logic-app"></a>Erste Schritte mit der HTTP-Listener HTTP-Aktion und Ihre Anwendung Logik hinzufügen

> [AZURE.NOTE]Wir sind Unterstützung für diesen Connector beendet, da die Funktionalität jetzt als **Manuelle Auslösung** beim Erstellen neuer Logik apps standardmäßig ist.  Wir empfehlen Aktualisierung Ihrer Logik apps, die diese Verbindung verwenden.  
> Diese Version des Artikels gilt für Logik apps 2014-12-01-Vorschau-Schemaversion.

Verbinden Sie direktes mit HTTP-Ressourcen für HTTP-Anfragen und HTTP Webanfragen konfigurieren. Es gibt einige Szenarien direkt von HTTP-Verbindungen arbeiten muss:

1.  Um eine Logik App unterstützt, eine Web- oder mobile Benutzer interaktiv front-End.
2.  Und Daten von einem Webdienst, die nicht im Feld Anschluss.
3.  Aktionen, die bereits als Webdienst verfügbar gemachten jedoch nicht als API-Anwendung verfügbar sind.

Für diese Szenarios gibt es zwei Optionen:

1. **Http-Listener**: diesen Connector fungiert als Trigger und HTTP-Anfragen auf einem konfigurierten Endpunkt überwacht. Bei einem auf der konfigurierten Endpunkt Anruf löst eine neue Instanz des Flusses und die Daten in der Anforderung an den Fluss zur Verarbeitung übergeben. Es kann auch konfiguriert werden automatisch auf die eingehende Anforderung reagieren, wenn der Fluss gestartet oder lassen Sie eine Antwort auf Grundlage der Ausführung Flow erstellen und Senden einer Antwort an den Aufrufer.
2. **Http-Aktion**: Diese können Sie konfigurieren, eine Webanfrage zu jeder Endpunkt verfügbar im Internet wird wieder eine Antwort und kann es weitere Aktionen im Fluss nutzen.

Logik-apps auslösen basierend auf einer Vielzahl von Datenquellen und bieten Anschlüsse und Prozessdaten als Teil des Flusses. Sie können HTTP-Connector Workflow- und Prozesskontrolle Geschäftsdaten als Teil dieser Workflows innerhalb einer Anwendung Logik hinzufügen. 

## <a name="creating-an-http-listener-for-your-logic-app"></a>Erstellen eines HTTP-Listeners für Ihre Logik
Ein Connector Logik App erstellt werden oder direkt von Azure Marketplace erstellt werden. So erstellen Sie einen Connector aus dem Markt  

1. Wählen Sie in Azure Startmenü **Marketplace**.
2. Suchen Sie nach "HTTP", wählen Sie HTTP-Listener, und **Erstellen**.
3.  Konfigurieren Sie den HTTP-Listener wie folgt:  
![][1]

4.  Beim Einrichten der paketeinstellungen sehen Sie die folgende Option der Listener sollte automatisch reagieren oder müssen Sie eine explizite Antwort senden. Legen Sie diese auf **False** eigene Antwort senden:  
![][2]

5.  Klicken Sie auf **OK** , um erstellen.
6.  Erstellte API-app-Instanz öffnen Sie Einstellungen zum Konfigurieren der Sicherheits. Der HTTP-Listener unterstützt derzeit die Standardauthentifizierung. Verwenden die Option Sicherheit beim Öffnen des HTTP-Listeners können Sie dies konfigurieren:  
![][3]
  
    **Bekanntes Problem** *Der Sicherheit zeigen "None" als den Standardwert jedoch nicht definiert ist. Die Einstellung muss Basic bis keine vor dem Speichern um sicherzustellen, dass der HTTP-Listener konfiguriert.*  

7. Schließlich Einstellen der Sicherheit der API-App öffentlich (anonym) externe Clients auf den Endpunkt zugreifen. Diese Einstellung steht unter "alle Einstellungen > Einstellungen" HTTP-Listener API-App:![][10]

Danach, können Sie jetzt eine Anwendung Logik Verwendung den HTTP-Listener erstellen.

## <a name="using-the-http-listener-in-your-logic-app"></a>Mithilfe des HTTP-Listeners in Ihrer Anwendung Logik
Erstellte API-app jetzt können den HTTP-Listener als Trigger für Ihre Logik Sie. Dazu müssen Sie:

4.  Erstellen Sie eine neue Logik-App.
5.  Öffnen Sie "Trigger und Aktionen" Logik Apps Designer öffnen und Konfigurieren der Fluss. Der HTTP-Listener wird im Katalog aufgeführt. Wählen sie.
6.  Sie können nun die HTTP-Methode und die relative URL auf dem Listener den Fluss auslösen müssen festlegen:  
![][4]  
![][5]

7.  Klicken Sie den vollständigen URI verdoppeln HTTP-Listener, um die Einstellungen anzeigen und Kopieren der URL für "Host" der API-app:  
![][6]
8.  Nun können Daten in der HTTP-Anforderung in anderen Aktionen im Workflow wie folgt:  
![][7]  
![][8]
9.  Schließlich um eine Antwort zu senden, fügen Sie einen anderen HTTP-Listener hinzu, und wählen Sie die Aktion HTTP-Antwort senden. ID anfordern auf der HTTP-Listener abgerufenes RequestID und füllen Sie den Antworttext und HTTP-Status zu zurück:  
![][9]

## <a name="using-the-http-action"></a>Mithilfe der HTTP-Aktion
Die HTTP-Aktion von Logik-Apps unterstützt und erfordert eine API-app zuerst verwenden können erstellt werden. Sie können einfügen eine HTTP-Aktion jederzeit in Ihrer Anwendung Logik und URI, Header und Body für den Aufruf.
HTTP-Aktion unterstützt mehrere Optionen für die Clientsicherheit Seite. Siehe [Seite Clientsicherheitsoptionen](../scheduler/scheduler-outbound-authentication.md).

Die Ausgabe der HTTP-Aktion ist Header und Body downstream weiterverwendet werden kann im Fluss ähnlich wie andere Aktionen und Connectors verbraucht wird.

## <a name="do-more-with-your-connector"></a>Möchten Sie den Connector
Der Connector erstellt wurde, können Sie es mit einer Logik App Arbeitsabläufe hinzufügen. Siehe [welche Logik Apps?](app-service-logic-what-are-logic-apps.md).

Anzeigen der Swagger REST API Reference [Connectors und API-Apps](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Sie können auch die Leistung Statistiken und Kontrolle der Sicherheit an den Anschluss überprüfen. [Verwalten und überwachen die integrierte API-Apps und Connectors](app-service-logic-monitor-your-connectors.md)anzeigen

> [AZURE.NOTE] Wenn Sie mit Azure Logik Apps beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [versuchen Logik](https://tryappservice.azure.com/?appservice=logic)App. Sie können sofort eine kurzlebige Starter Logik-app in App Service erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

<!--Image references-->
[1]: ./media/app-service-logic-connector-http/1.png
[2]: ./media/app-service-logic-connector-http/2.png
[3]: ./media/app-service-logic-connector-http/3.png
[4]: ./media/app-service-logic-connector-http/4.png
[5]: ./media/app-service-logic-connector-http/5.png
[6]: ./media/app-service-logic-connector-http/6.png
[7]: ./media/app-service-logic-connector-http/7.png
[8]: ./media/app-service-logic-connector-http/8.png
[9]: ./media/app-service-logic-connector-http/9.png
[10]: ./media/app-service-logic-connector-http/10.png
