<properties
    pageTitle="Erste Schritte mit Event-Hubs in C# | Microsoft Azure"
    description="Dieses Lernprogramm Azure Ereignis Hubs mit C# und der EventProcessorHost beginnen."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/02/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Erste Schritte mit Event Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Einführung

Ereignis-Hubs ist ein Dienst, der große Datenmengen Ereignis (Telemetrie) von angeschlossenen Geräten und Applikationen verarbeitet. Nach Ereignis Hubs erfassen Sie speichern Speicher ein Cluster oder Transformieren eines Echtzeitanalysen. Umfangreiche Ereignis Sammlung und Verarbeitung Dies ist eine wichtige Komponente der modernen Architekturen, einschließlich das Internet der Dinge (IoT).

Dieses Lernprogramm zeigt, wie mit klassischen Azure-Portal einen Ereignis-Hub. Sie zeigt auch in ein Ereignis mit einem Konsolenanwendungsprojekt in C# geschriebenen Nachrichten sammeln, und wie sie parallel mit C# [Prozessor Veranstalter][] Bibliothek abgerufen.

Um dieses Lernprogramm benötigen Sie Folgendes:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Ein aktives Azure-Konto. Wenn Sie eine haben, können Sie ein kostenloses Konto in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/free/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt können Sie die Anwendung ausführen.

1. Öffnen Sie in Visual Studio **Empfänger** -Projekt, das Sie zuvor erstellt haben.

2. Maustaste Lösung **Empfänger** klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Vorhandenes Projekt**.
 
3. Die vorhandene Sender.csproj-Datei doppelklicken Sie darauf, um es der Projektmappe hinzufügen.
 
4. Erneut mit der rechten Maustaste der **Receiver** -Lösung und anschließend auf **Eigenschaften**. **Empfänger** -Eigenschaftenseite wird angezeigt.

5. **Startprojekt**, klicken auf die Schaltfläche **mehrere Startprojekte** . **Feld für den **Empfänger** und **Absender** ** **zu**legen.

    ![][19]

6. Klicken Sie auf **Project Dependencies**. Klicken Sie im Feld **Projekte** auf **Absender**. Im Feld **hängt** unbedingt **Empfänger** aktiviert ist.

    ![][20]

7. Klicken Sie auf **OK** , um das Dialogfeld **Eigenschaften** zu schließen.

1.  Drücken Sie F5, um das **Empfänger** -Projekt in Visual Studio ausführen und die Empfänger für die Partitionen zu warten.

    ![][21]

2.  **Absender** -Projekt wird automatisch ausgeführt. Drücken Sie die **EINGABETASTE** im Konsolenfenster, und die Ereignisse im empfängerfenster angezeigt.

    ![][22]

Drücken Sie **STRG + C** im Fenster **Absender** den Absender Anwendung und drücken die **EINGABETASTE** im Fenster Empfänger diese Anwendung beenden.

## <a name="next-steps"></a>Nächste Schritte

Da Sie eine funktionierende Anwendung, die ein Ereignis Hub erstellt erstellt haben und sendet und empfängt, können Sie auf folgenden Szenarien verschieben:

- Eine vollständige [Anwendung mit Event Hubs][].
- Das Beispiel [Dezentrales Ereignis mit Ereignis verarbeitet][] .
- [Übersicht über Hubs][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Veranstalter-Prozessor]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Übersicht über Hubs]: event-hubs-overview.md
[beispielanwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Ereignis mit Ereignis verarbeitet skalieren]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
