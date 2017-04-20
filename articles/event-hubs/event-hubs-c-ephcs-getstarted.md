<properties
    pageTitle="Erste Schritte mit Event-Hubs in C und C# | Microsoft Azure"
    description="Dieses Lernprogramm den Einstieg in Azure Ereignis Hubs; Senden von Ereignissen in C# und Saum in C# mithilfe der EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="csharp"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Erste Schritte mit Event Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Einführung

Ereignis-Hubs wird eine hochskalierbare Einnahme die Aufnahme Millionen Ereignisse pro Sekunde durch eine Anwendung verarbeitet und analysiert große Mengen von Daten von den Geräten und Applikationen erzeugt. Nach Ereignis Hubs, können transformiert und Datenspeicher mit Anbieter Echtzeitanalysen oder Speicherung Cluster.

Weitere Informationen finden Sie unter [Übersicht über Hubs][].

In diesem Lernprogramm erfahren Sie, wie Nachrichten in ein Ereignis über eine in C aufnehmen und parallel mit C# [Prozessor Veranstalter][] Bibliothek abrufen.

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Eine C-Development-Umgebung. Für dieses Lernprogramm wird den Stapel Gcc eine [Azure Linux VM](../virtual-machines/virtual-machines-linux-quick-create-cli.md) mit Ubuntu 14.04 angenommen. Für andere Unternehmen werden in externen Anweisungen.

+ Microsoft Visual Studio Express für Windows

+ Ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt können Sie die Anwendung ausführen.

1.  **Empfänger** -Projekt in Visual Studio ausführen und die Empfänger für die Partitionen zu warten.

    ![][21]

2.  Führen Sie **Absender** -Programm aus und die Ereignisse im empfängerfenster angezeigt.

    ![][24]

## <a name="next-steps"></a>Nächste Schritte

Da Sie eine funktionierende Anwendung, die ein Ereignis Hub erstellt erstellt haben und sendet und empfängt, können Sie auf folgenden Szenarien verschieben:

- Eine vollständige [Anwendung mit Event Hubs][].
- Das Beispiel [Dezentrales Ereignis mit Ereignis verarbeitet][] .
- [Übersicht über Hubs][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Veranstalter-Prozessor]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Übersicht über Hubs]: event-hubs-overview.md
[beispielanwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Ereignis mit Ereignis verarbeitet skalieren]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
