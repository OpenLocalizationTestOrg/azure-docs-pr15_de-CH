<properties
   pageTitle="Berechnen Kontextoptionen R Server HDInsight (Vorschau) | Microsoft Azure"
   description="Erfahren Sie mehr über andere Compute Kontext verfügbaren Optionen für Benutzer mit R Server HDInsight (Vorschau)"
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="jhubbard"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="10/18/2016"
   ms.author="jeffstok"
/>

# <a name="compute-context-options-for-r-server-on-hdinsight-preview"></a>Berechnen Sie Kontextoptionen R Server HDInsight (Vorschau)

Microsoft R Server auf Azure HDInsight (Preview) bietet die neuesten Funktionen für R-basierte Analysen. BIETET in einem Container Ihres Speicherkontos [Azure Blob](../storage/storage-introduction.md "Azure BLOB-Speicher") oder dem lokalen Linux-Dateisystem gespeicherten Daten verwendet. Da open Source F R Server aufbaut, können R-basierte Anwendung erstellen 8000 + open-Source-R Pakete nutzen. Sie können auch die Routinen [ScaleR](http://www.revolutionanalytics.com/revolution-r-enterprise-scaler "Revolution Analytics ScaleR"), Microsoft big Data Analytics Paket nutzen, die mit R Server enthalten ist.  

Der edgeknoten eines Clusters Premium bietet eine bequeme Möglichkeit zum Cluster und R Skripts ausführen. Mit einen Kantenknoten haben Sie die Möglichkeit des ScaleR parallelisierten verteilten Funktionen über Kerne Knoten Edgeserver. Sie haben die Möglichkeit, mithilfe des ScaleR Hadoop Map Reduce Knoten des Clusters ausgeführt oder Funken berechnen Kontexte.

## <a name="compute-contexts-for-an-edge-node"></a>Kontexte für einen Kantenknoten berechnen

Im Allgemeinen wird ein Skript R, R-Server auf dem edgeknoten ausgeführt wird, innerhalb der R-Interpreter auf diesem Knoten ausgeführt. Alle Schritte, die ein ScaleR Funktionsaufruf gilt. ScaleR Aufrufe ausführen in einer Compute-Umgebung ScaleR Compute Kontext festlegen bestimmt wird.  Mögliche Werte für Compute-Kontext beim Ausführen von R-Skript von eines lokalen sequenzielle ('local'), sind lokale Parallel ('Localpar') Karte reduzieren und Spark.

Die Optionen 'local' und 'Localpar' unterscheiden sich nur wie RxExec Aufrufe ausgeführt werden. Beide Aufrufe ausgeführt werden andere Rx-Funktion auf parallele Weise über alle verfügbaren Kerne – sofern nicht anderweitig durch die ScaleR NumCoresToUse Möglichkeit, z.B. rxOptions(numCoresToUse=6). Im folgenden wird zusammengefasst Compute Kontextoptionen

| Berechnen von Kontext  | Festlegen                      | Ausführungskontext                                                                     |
|------------------|---------------------------------|---------------------------------------------------------------------------------------|
| Lokale fortlaufende | rxSetComputeContext('local')    | Parallelisierte Ausführung über die Kerne die Knoten Edgeserver RxExec Ruft die seriell ausgeführt werden |
| Lokale parallel   | rxSetComputeContext('localpar') | Parallelisierte Ausführung über Kerne Knoten Edgeserver                                 |
| Funken            | RxSpark()                       | Parallelisiert verteilte Ausführung über Spark Knoten des Clusters HDI      |
| Karte reduzieren       | RxHadoopMR()                    | Parallelisiert verteilte Ausführung über Map Reduce Knoten des Clusters HDI |


Sofern parallelisierte Ausführung Zwecken Leistung möchten, sind drei Optionen verfügbar. Welche Option Sie wählen, hängt von der Art Ihrer Arbeit Analytics und die Größe und Position der Daten ab.

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Richtlinien für die Entscheidung für einen compute

Derzeit ist keine Formel, der wird zu verwendende Kontext zu berechnen. Es gibt jedoch einige Leitlinien, mit die Hilfe die richtige Wahl, oder zumindest Ihre Auswahl einzugrenzen, bevor Sie eine Richtlinie ausführen. Diese Grundsätze sind:

1.  Das lokale Dateisystem von Linux ist schneller als bietet.
2.  Wiederholte Analysen sind schneller, die Daten lokal sind und ist in XDF.
3.  Es wird empfohlen, kleine Mengen von Daten aus einer Datenquelle Text übertragen; Wenn die Datenmenge größer ist, konvertieren sie XDF vor der Analyse.
4.  Der Aufwand für das Kopieren oder Datenströme zu Kantenknoten Analyse wird für große Datenmengen verwaltet.
5.  Spark ist schneller als Zuordnung für Analyse Hadoop verringern.

Diese Prinzipien sind einige Faustregeln für Compute-Kontext auswählen:

### <a name="local"></a>Lokale

- Wenn die Datenmenge zu analysieren klein ist und keine wiederholten Analyse erfordert Streaming direkt in die Analyse-Routine und verwenden 'local' oder 'Localpar'.
- Wenn die Datenmenge analysieren kleinen oder mittleren und wiederholten Analyse erfordert, dann auf das lokale Dateisystem kopieren Sie XDF importieren Sie und analysieren Sie 'local' oder 'Localpar'.

### <a name="hadoop-spark"></a>Hadoop Funken

- Wenn die Datenmenge analysieren groß ist, dann in importieren XDF in bietet (außer wenn Speicherplatz ein Problem ist), und über 'Spark'.

### <a name="hadoop-map-reduce"></a>Hadoop Karte reduzieren

- Nur verwenden Sie, wenn UNLÖSBARES Problem mit Spark Compute Kontext auftreten, da im Allgemeinen langsamer werden.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Inline-Hilfe rxSetComputeContext

Finden Sie weitere Informationen und Beispiele für ScaleR Compute Kontexte Inline r RxSetComputeContext Methode beispielsweise helfen:

    > ?rxSetComputeContext

Außerdem finden Sie im "ScaleR Distributed Computing Guide" [R Server MSDN](https://msdn.microsoft.com/library/mt674634.aspx "R Server auf der MSDN-") Bibliothek verfügbar ist.


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie einen neuen HDInsight-Cluster erstellen, der r-Server enthält. Außerdem haben Sie gelernt, die Grundlagen der Verwendung von R-Konsole aus einer SSH-Sitzung. Jetzt können Sie andere Wege mit R Server HDInsight der folgenden Artikel lesen:

- [Übersicht über Server für Hadoop R](hdinsight-hadoop-r-server-overview.md)
- [Erste Schritte mit R Server für Hadoop](hdinsight-hadoop-r-server-get-started.md)
- [HDInsight Premium RStudio Server hinzufügen](hdinsight-hadoop-r-server-install-r-studio.md)
- [Azure Speicheroptionen für R Server HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
