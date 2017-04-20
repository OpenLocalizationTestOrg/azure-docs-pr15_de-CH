<properties
    pageTitle="Erste Schritte mit Event-Hubs in C# | Microsoft Azure"
    description="Dieses Lernprogramm den Einstieg in Azure Ereignis Hubs; Senden von Ereignissen in C# und in Java mithilfe der EventProcessorHost empfangen."
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
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Erste Schritte mit Event Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Einführung

Ereignis-Hubs ist ein Dienst, der große Datenmengen Ereignis (Telemetrie) von angeschlossenen Geräten und Applikationen verarbeitet. Nach Ereignis Hubs erfassen Sie speichern Speicher ein Cluster oder Transformieren eines Echtzeitanalysen. Umfangreiche Ereignis Sammlung und Verarbeitung Dies ist eine wichtige Komponente der modernen Architekturen, einschließlich das Internet der Dinge (IoT).

Dieses Lernprogramm zeigt, wie mit klassischen Azure-Portal einen Ereignis-Hub. Sie zeigt auch in ein Ereignis mit einem Konsolenanwendungsprojekt in C# geschriebenen Nachrichten sammeln, und wie sie parallel mit Java Prozessor Veranstalter Bibliothek abgerufen.

Um dieses Lernprogramm benötigen Sie Folgendes:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Ein aktives Azure-Konto. <br/>Wenn Sie eine haben, können Sie ein kostenloses Konto in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt können Sie die Anwendung ausführen.

1.  Führen Sie das Projekt **Empfänger** .

    ![][21]

2.  Führen Sie das **Absender** -Projekt.

    ![][22]

## <a name="next-steps"></a>Nächste Schritte

Da Sie eine funktionierende Anwendung, die ein Ereignis Hub erstellt erstellt haben und sendet und empfängt, können Sie auf folgenden Szenarien verschieben:

- Eine vollständige [Anwendung mit Event Hubs][].
- Das Beispiel [Dezentrales Ereignis mit Ereignis verarbeitet][] .
- [Übersicht über Hubs][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Übersicht über Hubs]: event-hubs-overview.md
[beispielanwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Ereignis mit Ereignis verarbeitet skalieren]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
