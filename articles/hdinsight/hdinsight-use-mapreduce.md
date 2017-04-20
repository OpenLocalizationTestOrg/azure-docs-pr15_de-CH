<properties
   pageTitle="MapReduce Hadoop auf HDInsight | Microsoft Azure"
   description="Informationen Sie zum MapReduce Aufträge für Hadoop in HDInsight-Cluster ausgeführt. Eine grundlegende Word Count Operation als Java MapReduce-Job laufen."
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

# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>Hadoop auf HDInsight MapReduce verwenden

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In diesem Artikel erfahren Sie, wie MapReduce-Jobs auf Hadoop in HDInsight-Cluster ausgeführt. Wir führen eine einfache Word Count Operation als Auftrag Java MapReduce implementiert.

##<a id="whatis"></a>Was ist MapReduce?

Hadoop MapReduce ist ein Softwareframework für das Schreiben von Aufträgen, die große Datenmengen verarbeiten. Eingabedaten ist in unabhängige Stücke unterteilt die Knoten im Cluster dann parallel verarbeitet werden. Ein Auftrag MapReduce bestehen aus zwei Funktionen:

* **Mapper**: Eingabedaten verbraucht (normalerweise mit Filtern und Sortieren von Vorgängen) analysiert und gibt Tupel (Schlüssel-Wert-Paare)
* **Reduzierung**: verbraucht Tupel Mapper ausgegeben und eine Zusammenfassung durchführt, die eine kleinere, kombinierte Ergebnis Mapper Daten erstellt

Eine grundlegende Word Count MapReduce Job-Beispiel ist im folgenden Diagramm dargestellt:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

Die Ausgabe dieses Projekts ist die Anzahl der wie oft jedes Wort im Text aufgetreten, das analysiert wurde.

* Zuordnung jeder Zeile aus dem Eingabetext als Eingabe akzeptiert und bricht Wörter. Gibt einen Schlüssel-Wert-Paar jeweils ein Wort das Wort kommt gefolgt von 1. Vor dem Senden an Reduzierstücks ist die Ausgabe sortiert.

* Des Reduzierstücks fasst diese einzelnen Zähler für jedes Wort und gibt eine einzelne Schlüssel-Wert-Paar mit dem Wort gefolgt von der Summe der vorkommen.

MapReduce kann in einer Vielzahl von Sprachen implementiert werden. Java ist die gebräuchlichste Implementierung und dient zur Veranschaulichung in diesem Dokument.

### <a name="hadoop-streaming"></a>Hadoop Streaming

Sprachen oder Frameworks, die auf Java und Java Virtual Machine (z. B. Scalding oder Cascading,) können ausgeführt werden direkt als MapReduce Auftrag einer Java-Anwendung ähnelt. Andere, verwenden wie C# oder Python oder eigenständige ausführbare Hadoop streaming.

Hadoop streaming kommuniziert mit Mapper und Reduzierstücks STDIN und STDOUT - Mapper und Reduzierstücks aus STDIN Daten zeilenweise Lesen und Ausgabe an STDOUT. Das Format des Schlüssel-Wert-Paar, getrennt durch ein Charaacter Registerkarte muss zeilenweise Lesen oder Mapper und Reduzierstücks ausgegeben:

    [key]/t[value]

Weitere Informationen finden Sie unter [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html).

Beispiele für die Verwendung mit HDInsight Hadoop finden Sie unter:

* [Python MapReduce Arbeitsplätze](hdinsight-hadoop-streaming-python.md)

##<a id="data"></a>Zu den Beispieldaten

In diesem Beispiel verwenden für Beispieldaten, die Leonardo Da Vinci-Notebooks Sie die als Textdokument im HDInsight Cluster bereitgestellt werden.

Beispieldaten werden in Azure BLOB-Speicher gespeichert, die HDInsight als das Standarddateisystem für Hadoop-Cluster verwendet. HDInsight kann mit dem Präfix **Wasb** im BLOB-Speicher gespeicherte Dateien zugreifen. Zugriff auf die Datei sample.log verwenden Sie z. B. die folgende Syntax:

    wasbs:///example/data/gutenberg/davinci.txt

Da Azure BLOB-Speicher der Standardspeicher für HDInsight ist, können Sie auch die Datei mit **/example/data/gutenberg/davinci.txt**zugreifen.

> [AZURE.NOTE] In der vorherigen Syntax **Wasbs: / /** wird verwendet, um Zugriff auf Dateien im Speicher Standardcontainer für HDInsight Cluster gespeichert sind. Wenn zusätzliche Speicherkonten wird angegeben, wenn Sie Cluster bereitgestellt und Dateien auf diese Konten zugreifen möchten, können Sie Daten über die Container und Kontoadresse zugreifen. Z. B. **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/gutenberg/davinci.txt**.

##<a id="job"></a>Zum Beispiel MapReduce

MapReduce-Auftrag, der in diesem Beispiel verwendet wird, ist am **wasbs://example/jars/hadoop-mapreduce-examples.jar**und mit HDInsight Cluster bereitgestellt. Enthält Word Count-Beispiel, das gegen **davinci.txt**ausgeführt wird.

> [AZURE.NOTE] In Clustern HDInsight 2.1 ist der Speicherort **wasbs:///example/jars/hadoop-examples.jar**.

Zur Referenz wird folgende Java-Code dafür MapReduce Word Count:

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.util.StringTokenizer;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

      public static class TokenizerMapper
           extends Mapper<Object, Text, Text, IntWritable>{

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                        ) throws IOException, InterruptedException {
          StringTokenizer itr = new StringTokenizer(value.toString());
          while (itr.hasMoreTokens()) {
            word.set(itr.nextToken());
            context.write(word, one);
          }
        }
      }

      public static class IntSumReducer
           extends Reducer<Text,IntWritable,Text,IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                           Context context
                           ) throws IOException, InterruptedException {
          int sum = 0;
          for (IntWritable val : values) {
            sum += val.get();
          }
          result.set(sum);
          context.write(key, result);
        }
      }

      public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        if (otherArgs.length != 2) {
          System.err.println("Usage: wordcount <in> <out>");
          System.exit(2);
        }
        Job job = new Job(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
      }
    }

Informationen zum Schreiben eigener MapReduce-Job, [entwickeln Java MapReduce](hdinsight-develop-deploy-java-mapreduce-linux.md)Programme für HDInsight.

##<a id="run"></a>Die MapReduce ausführen

HDInsight kann mithilfe verschiedener Methoden HiveQL Aufträge ausführen. Verwenden Sie in der folgenden Tabelle entscheiden, welche Methode Sie, und folgen Sie dem Link eine exemplarische Vorgehensweise.

| **Verwenden Sie diese**...                                                    | **... dazu zu**                                       | ... mit diesem **Cluster-Betriebssystem** | ... aus dem **Client-Betriebssystem** |
|:-------------------------------------------------------------------|:--------------------------------------------------------|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md)                       | Verwenden Sie den Befehl Hadoop über **SSH**                  | Linux                                     | Linux, Unix, Mac OS X oder Windows        |
| [Aufrollen](hdinsight-hadoop-use-mapreduce-curl.md)                     | Senden Sie den Auftrag Remote mithilfe von **REST**               | Linux oder Windows                          | Linux, Unix, Mac OS X oder Windows        |
| [Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) | Senden Sie den Auftrag Remote mithilfe von **Windows PowerShell** | Linux oder Windows                          | Windows                                  |
| [Remote Desktop](hdinsight-hadoop-use-mapreduce-remote-desktop)    | Verwenden Sie den Befehl Hadoop über **Remotedesktop**       | Windows                                   | Windows                                  |

##<a id="nextsteps"></a>Nächste Schritte

Zwar MapReduce diagnostische Fähigkeiten, kann es etwas schwer zu meistern. Es gibt mehrere Java-basierten Frameworks, die MapReduce Applications, sowie Technologien wie Schwein und Struktur, die eine einfachere Möglichkeit, Daten in HDInsight erleichtern. Weitere finden Sie in folgenden Artikeln:

* [Entwickeln von Java MapReduce Programme für HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Entwickeln von Python streaming MapReduce Programme für HDInsight](hdinsight-hadoop-streaming-python.md)

* [Verbrennung MapReduce Arbeitsplätze mit Apache Hadoop auf HDInsight](hdinsight-hadoop-mapreduce-scalding.md)

* [Struktur mit HDInsight verwenden][hdinsight-use-hive]

* [Verwenden Sie Schwein mit HDInsight][hdinsight-use-pig]

* [HDInsight Beispiele ausführen][hdinsight-samples]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[powershell-install-configure]: ../powershell-install-configure.md

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
