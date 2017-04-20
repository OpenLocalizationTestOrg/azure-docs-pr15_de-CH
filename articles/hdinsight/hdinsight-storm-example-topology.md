<properties
 pageTitle="Apache Storm Topologien auf HDInsight | Microsoft Azure"
 description="Ein Sturm Topologien erstellt und getestet mit Apache auf HDInsight einschließlich basic C# und Java-Topologien und Arbeiten mit Event Hubs."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
    tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="08/23/2016"
 ms.author="larryfr"/>

# <a name="example-storm-toplogies-and-components-for-apache-storm-on-hdinsight"></a>Beispiel Sturm Toplogies und Komponenten für Apache Storm auf HDInsight

Folgendes ist eine Liste von Beispielen erstellt und verwaltet von Microsoft für die Verwendung mit Apache auf HDInsight. Beispiele decken eine Vielfalt von Themen grundlegende C# und Java Topologien mit Azure Services wie Ereignis-Hubs DocumentDB Power BI, SQL-Datenbank, HBase HDInsight und Azure-Speicher zu erstellen. Beispiele veranschaulichen auch nicht Azure oder sogar nicht-Microsoft-Technologien wie SignalR und Socket.IO arbeiten

| Beschreibung                                                                                             | Zeigt                                         | Sprache-Framework         |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------|:---------------------------|
| [Schreiben Sie in Azure See Datenspeicher von Apache Sturm](hdinsight-storm-write-data-lake-store.md) | Schreiben in Azure See Datenspeicher | Java |
| [Ereignis-Hub Auslauf und Bolzen Quelle](https://github.com/apache/storm/tree/master/external/storm-eventhubs) | Quelle für das Ereignis Hub Schnauze und Schraube | Java |
| [Entwickeln Sie Java-basierte Topologien für Apache Sturm HDInsight][5797064f]                                 | Maven                                                | Java                       |
| [Entwickeln von C# Topologien für Apache Storm auf HDInsight mit Visual Studio][16fce2d1]                     | HDInsight-Tools für Visual Studio                    | C#, Java                   |
| [Erstellen Sie mehrere Datenströme in C# Storm-Topologie][ec5a4064]                                         | Mehrere Datenströme                                     | C#                         |
| [Bestimmen von Trends mit HDInsight auf Twitter][3c86c7c8]                                   | Trident                                              | Java Trident              |
| [Verarbeitung von Ereignissen von Azure Ereignis Hubs mit auf HDInsight (C#)][844d1d81]                                | Ereignis-Hubs                                           | C# und Java                |
| [Verarbeitung von Ereignissen von Azure Ereignis Hubs mit auf HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md) | Ereignis-Hubs | Java |
| [Verwenden Sie Power Bi, um Daten aus einem Storm-Topologie][94d15238]                              | Power BI                                             | C#                         |
| [Analysieren von Daten mit Sturm und HBase in HDInsight][ab894747]                                     | Ereignis Hubs, HBase, Socket.IO Cockpit          | C#, Java, JavaScript, HTML |
| [Prozessdaten Sie Fahrzeug Sensor Event Hubs mit Sturm HDInsight][246ee964]                        | Ereignis Hubs, DocumentDb, Azure-Speicher Blob (WASB)    | C#, Java                   |
| [Extrahieren, Transformieren und laden (ETL) von Azure Ereignis HBase HDInsight über Sturm][b4b68194] | Ereignis-Hubs, HBase                                    | C#                         |
| [Vorlagenprojekt C# Storm-Topologie für die Arbeit mit Azure auf HDInsight][ce0c02a2]  | Ereignis Hubs, DocumentDb SQL-Datenbank, HBase, SignalR | C#, Java                   |
| [Lesen von Azure Ereignis HDInsight Sturm über Skalierbarkeit benchmarks][d6c540e3]           | Nachrichtendurchsatz, Ereignis-Hubs SQL-Datenbank         | C#, Java                   |
| [Korreliert Events über Sturm und HBase HDInsight](hdinsight-storm-correlation-topology.md) | HBase | C# |
| [Verwenden Sie Python mit auf HDInsight](hdinsight-storm-develop-python-topology.md) | Python-Komponenten mit Java und Makrosystem basierten Sturm Topologien | Python |

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Apache auf HDInsight][2b8c3488]

* [Informationen Sie zum Bereitstellen und Verwalten von Storm Topologien mit auf HDInsight][6eb0d3b8]

  [2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Informationen Sie zu erstellen eines Sturms HDInsight Cluster Storm-Dashboard Topologien bereitstellen."
  [6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Informationen Sie zum Bereitstellen und Verwalten von Topologien mit Web-basierter Storm Dashboard und Storm-Benutzeroberfläche oder HDInsight-Tools für Visual Studio."
  [16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Informationen Sie zu C# Sturm Topologien mit HDInsight-Tools für Visual Studio."
  [5797064f]: hdinsight-storm-develop-java-topology.md "Informationen Sie zum Erstellen von Storm Topologien in Java mit Maven, erstellen eine grundlegende Wordcount-Topologie."
  [94d15238]: hdinsight-storm-power-bi-topology.md "Veranschaulicht das Schreiben von Daten in Power BI aus C#-Topologie und Erstellen eines Diagramms und Dashboard aus den Daten."
  [ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Zeigt eine grundlegende Storm-Topologie, die führt einen Wordcount in C# implementiert. Dadurch wird das Erstellen mehrerer Datenströme in einer C#-Topologie veranschaulicht."
  [844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Informationen Sie zum Lesen und Schreiben von Daten von Azure Ereignis Hubs mit auf HDInsight."
  [ab894747]: hdinsight-storm-sensor-data-analysis.md "Informationen Sie zum Apache Storm HDInsight verwenden, um Daten von Azure-Ereignis verarbeiten, D3.js visualisieren und (optional) HBase speichern."
  [3c86c7c8]: hdinsight-storm-twitter-trending.md "Erfahren Sie, wie mit Trident Storm-Topologie, die Trends (basierend auf Hashtags,) bestimmt auf Twitter."
  [246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "Erfahren Sie, wie mit einem Storm-Topologie Azure Ereignis Hubs Nachrichten lesen, lesen Sie Dokumente von Azure DocumentDB zum Verweisen auf Daten und speichern Daten in Azure-Speicher."
  [d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Mehrere Topologien Durchsatz bei Azure Ereignis Hubs lesen und Speichern von SQL-Datenbank mit Apache Storm auf HDInsight veranschaulichen."
  [b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Informationen Sie zum Lesen von Daten aus Azure Ereignis Hubs, aggregate und Transformieren der Daten und HBase auf HDInsight speichern."
  [ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Dieses Projekt enthält Vorlagen für Ausläufe, Bolzen und Topologien mit verschiedenen Azure Ereignis Hubs, DocumentDB und SQL Datenbank interagieren."
 
