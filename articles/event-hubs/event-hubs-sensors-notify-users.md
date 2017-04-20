<properties 
   pageTitle="Benachrichtigen der Benutzer über Daten von Sensoren oder andere Systeme | Microsoft Azure"
   description="Beschreibt das Ereignis Hubs Sensordaten Validierungsfehlern."
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="notify-users-of-data-received-from-sensors-or-other-systems"></a>Benachrichtigen der Benutzer über Daten von Sensoren oder anderen Systemen

Nehmen Sie eine Anwendung haben, die Daten in Echtzeit überwacht oder Berichte auf einen Zeitplan. Betrachten Sie die Website mit den Echtzeit-Diagramme oder Berichte angezeigt werden, können Sie etwas sehen, die Aktion erfordern. Wenn Sie müssen gewarnt zu, anstatt sich auf die Website an? Angenommen Sie, Ihnen wachsen Licht im Gewächshaus und müssen sofort wissen, ob das Licht geht. Eine Möglichkeit, die mit einem hellen Sensor im Gewächshaus anordnen eine e-Mail gesendet werden, ist die LED deaktiviert sind.

![][1]

Angenommen Sie in einem anderen Szenario, pet Internat ausführen, und Sie geringer Bestandsmenge Lieferung informiert werden müssen. Sie können z. B. anordnen SMS von Ihrem ERP-System gesendet werden, wenn der Lagerbestand Hundefutter kritischen gefallen ist. 

![][2]

Das Problem ist wie bei nicht, wenn man in beim Auschecken eines statischen Berichts Auflagen erfüllt werden, wichtige Informationen abrufen. Ein [Azure Event Hub][] [Azure IoT Hub][] Verwendung oder Daten von Geräten oder Anwendung wie [Dynamics AX][]empfangen, haben Sie mehrere Optionen zum verarbeiten. Auf einer Website anzeigen, analysieren sie können gespeichert und können diese Befehle dazu auslösen. Hierzu können Sie Werkzeuge wie [Websites Azure][], [SQL Azure][], [HDInsight][], [Cortana Intelligence Suite][], [IoT Suite][], [Logik Apps][]oder [Azure Notification Hubs][]. Aber manchmal benötigen Sie Daten mit einem Minimum an Aufwand versenden. Um anzeigen mit nur wenig Code dazu haben wir ein neues Beispiel [AppToNotifyUsers][]bereitgestellt. Optionen sind e-Mails (SMTP), SMS und Telefon.

## <a name="application-structure"></a>Anwendung

Die Anwendung ist in C# geschrieben und die Readme-Datei im Beispiel enthält alle Informationen müssen Sie erstellen, ändern und veröffentlichen Sie die Anwendung. Die folgenden Abschnitte bieten einen allgemeinen Überblick darüber, was die Anwendung macht.

Wir beginnen mit der Annahme, dass kritische Ereignisse zu einer Azure Event Hub IoT Hub abgelegt haben. Alle Hub wird, solange Sie darauf zugreifen und die Verbindungszeichenfolge.

Wenn Sie nicht bereits einen Ereignis-Hub oder IoT Hub haben, können Sie problemlos Prüfstand mit einem Arduino Schild und eine Himbeeren Pi Anweisungen im Projekt [Verbinden der Punkte](https://github.com/Azure/connectthedots) einrichten. Light Sensor auf der Abschirmung Arduino sendet Licht durch die Pi ein [Azure Event Hub][] (**Ehdevices**) ein Auftrags [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) legt Alerts an einen zweiten Ereignis Hub (**Ehalerts**) unter ein bestimmtes Niveau fällt Licht empfangen.

Beginn **AppToNotify** liest eine Konfigurationsdatei (App.config) URL und Anmeldeinformationen für Event Hub empfangen Alarme zu erhalten. Es erstellt dann einen Prozess kontinuierlich Event Hub für alle Botschaft – solange Sie den URL für die Ereignis-Hub oder IoT Hub und gültige Anmeldeinformationen möglich, dieses Ereignis Hubs Reader Code liest kontinuierlich was kommt. Beim Start liest die Anwendung auch die URL und Anmeldeinformationen für die gewünschte messaging-Dienst (e-Mail, SMS, Telefon) und Name/Adresse des Absenders und eine Liste von Empfängern.

Wenn Event Hub Monitor eine Nachricht erkennt, löst einen Prozess, der die Nachricht in der Konfigurationsdatei angegebene Methode sendet. Beachten Sie, dass jede Nachricht gesendet wird, erkennt. Wenn Sie Monitor zeigen an Event Hub, zehn Nachrichten pro Sekunde empfängt, sendet des Absenders zehn Nachrichten pro Sekunde – zehn e-Mails pro Sekunde zehn SMS-Nachrichten pro Sekunde, zehn Anrufe pro Sekunde. Deshalb unbedingt ein Hub-Ereignis zu überwachen, die nur Alarme empfängt, die aus einem Ereignis-Hub gesendet werden, die die unformatierten Daten aus der Sensoren oder Anwendung empfängt.

## <a name="applicability"></a>Anwendbarkeit

Der Code in diesem Beispiel zeigt nur wie Ereignis Hubs überwachen und externen Messagingdienste aufgerufen wird, wenn Sie diese Funktionalität Ihrer Anwendung hinzufügen möchten. Beachten Sie, dass diese Lösung BASTELN, z.B. Entwickler. Es ist kein Enterprise Anforderungen wie Redundanz, Failover-Neustart auf Fehler. Weitere umfassende und Produktion Solutions finden Sie unter:

- Mithilfe von Connectors oder Pushbenachrichtigungen [Azure Logik Apps](../app-service-logic/app-service-logic-connectors-list.md) nutzen.
- Mithilfe von [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx)wie Blog [Broadcast Pushbenachrichtigungen Millionen von Mobilgeräten Azure Notification Hubs](http://weblogs.asp.net/scottgu/broadcast-push-notifications-to-millions-of-mobile-devices-using-windows-azure-notification-hubs). 

## <a name="next-steps"></a>Nächste Schritte

Es ist einfach einen einfache Benachrichtigungsdienst erstellen, der sendet e-Mails oder Nachrichten an Empfänger oder Aufrufe, Relay-Daten von einem Ereignis Hub oder IoT Hub. Zum Bereitstellen der Projektmappe darauf basierend auf Daten von diesen Hubs Benutzer finden Sie auf [AppToNotifyUsers][].

Weitere Informationen zu diesen Hubs finden Sie in folgenden Artikeln:

- [Azure Event Hubs]
- [Azure IoT Hub]
- Erste Schritte mit einem [Ereignis Hubs Tutorial].
- Eine vollständige [Anwendung mit Event Hubs].

Besuchen Sie zum Bereitstellen der Projektmappe um Benutzer basierend auf Daten von diesen Hubs benachrichtigen:

- [AppToNotifyUsers][]

[Ereignis-Hubs Lernprogramm]: event-hubs-csharp-ephcs-getstarted.md
[Azure IoT Hub]: https://azure.microsoft.com/services/iot-hub/
[Azure Event Hubs]: https://azure.microsoft.com/services/event-hubs/
[Azure Event Hub]: https://azure.microsoft.com/services/event-hubs/
[beispielanwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[AppToNotifyUsers]: https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications
[Dynamics AX]: http://www.microsoft.com/dynamics/erp-ax-overview.aspx
[Azure Websites]: https://azure.microsoft.com/services/app-service/web/
[SQL Azure]: https://azure.microsoft.com/services/sql-database/
[HDInsight]: https://azure.microsoft.com/services/hdinsight/
[Cortana Intelligence Suite]: http://www.microsoft.com/server-cloud/cortana-analytics-suite/Overview.aspx?WT.srch=1&WT.mc_ID=SEM_lLFwOJm3&bknode=BlueKai
[IoT Suite]: https://azure.microsoft.com/solutions/iot-suite/
[Logik-Apps]: https://azure.microsoft.com/services/app-service/logic/
[Azure Notification Hubs]: https://azure.microsoft.com/services/notification-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/services/stream-analytics/
 
[1]: ./media/event-hubs-sensors-notify-users/event-hubs-sensor-alert.png
[2]: ./media/event-hubs-sensors-notify-users/event-hubs-erp-alert.png