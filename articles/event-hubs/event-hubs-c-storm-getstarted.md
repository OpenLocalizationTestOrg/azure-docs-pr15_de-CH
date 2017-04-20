<properties
    pageTitle="Erste Schritte mit Ereignis Hubs mit C und Apache Storm | Microsoft Azure"
    description="Dieses Lernprogramm den Einstieg in Azure Ereignis Hubs; Senden von Ereignissen in C# und in einem Apache Storm-Cluster empfangen."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="java"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Erste Schritte mit Event Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Einführung

Ereignis-Hubs wird eine hochskalierbare Einnahme die Aufnahme Millionen Ereignisse pro Sekunde durch eine Anwendung verarbeitet und analysiert große Mengen von Daten von den Geräten und Applikationen erzeugt. Nach Ereignis Hubs, können transformiert und Datenspeicher mit Anbieter Echtzeitanalysen oder Speicherung Cluster.

Weitere Informationen finden Sie unter [Übersicht über Event Hubs].

In diesem Lernprogramm erfahren Sie, wie Nachrichten in ein Ereignis über eine in C aufnehmen und parallel mit Apache Storm abrufen.

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Eine C-Development-Umgebung. Für dieses Lernprogramm wird den Stapel Gcc eine [Azure Linux VM](../virtual-machines/virtual-machines-linux-quick-create-cli.md) mit Ubuntu 14.04 angenommen. Für andere Unternehmen werden in externen Anweisungen.

+ Eine Umgebung von Java Development [Maven](http://maven.apache.org/)konfiguriert. Für dieses Lernprogramm wird [Eclipse](https://www.eclipse.org/)angenommen.

+ Ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Konto in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt können Sie die Anwendung ausführen.

1.  Führen Sie die **LogTopology** -Klasse von Eclipse und der Empfänger für die Partitionen zu warten.

2.  Führen Sie **Absender** -Programm aus und die Ereignisse im empfängerfenster angezeigt.

    ![][23]

> [AZURE.NOTE] Verwenden Sie in diesem Lernprogramm nur Sturm im lokalen Modus für Entwicklungszwecke. Finden Sie in [HDInsight Sturm Übersicht] und der offiziellen [Apache Storm] Dokumentation Sturm Bereitstellung und Muster.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Ressourcen stehen für Anwendungsentwicklung Ereignis Hubs und Sturm integrieren.

- [Analysieren von Daten mit Sturm und HDInsight][] ist ein vollständige Szenario Tutorial mit Event Hubs, Sturm und HBase Daten in einem Cluster Hadoop aufnehmen.
- [Entwickeln Streaminganwendungen Datenverarbeitung mit SCP.NET C# Sturm und HDInsight][] ist eine Anleitung zum Sturm Rohrleitungen C# schreiben.

<!-- Images. -->
[23]: ./media/event-hubs-c-storm-getstarted/receive-storm3.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Übersicht über Hubs]: event-hubs-overview.md

[Apache Sturm]: https://storm.incubator.apache.org
[Übersicht über die HDInsight Sturm]: ../hdinsight/hdinsight-storm-overview.md/
[Analysieren von Daten mit Sturm und HDInsight]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Entwickeln von Streaminganwendungen Datenverarbeitung mit SCP.NET C# Sturm und HDInsight]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
