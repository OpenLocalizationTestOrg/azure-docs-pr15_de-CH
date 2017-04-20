<properties
    pageTitle="In HDInsight Hadoop Beispiele ausführen | Microsoft Azure"
    description="Erste Schritte mit Azure HDInsight Service bereitgestellten Beispiele. Verwenden Sie PowerShell-Skripts, die auf Datencluster MapReduce Programme ausgeführt."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>

#<a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>Ausführen von Beispielen Hadoop MapReduce in Windows-basierten HDInsight

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Beispiele erhalten Schritte ausführen MapReduce-Jobs auf Hadoop Cluster mithilfe von Azure HDInsight erhalten. Diese Beispiele sind auf jedem verwalteten HDInsight Cluster zur Verfügung, die Sie erstellen. Diese Beispiele ausgeführt werden Sie vertraut mit Azure PowerShell-Cmdlets Aufträge auf Hadoop-Cluster ausgeführt.

- [**Word Count**][hdinsight-sample-wordcount]: zählt Word Vorkommen in einer Textdatei.
- [**C# streaming Wörter zählen**][hdinsight-sample-csharp-streaming]: zählt Word Vorkommen in einer Textdatei mit Hadoop Streaming-Schnittstelle.
- [**Pi Gutachter**][hdinsight-sample-pi-estimator]: verwendet ein (quasi Monte Carlo) Methode, um den Wert von Pi zu schätzen.
- [**10 GB Graysort**][hdinsight-sample-10gb-graysort]: Allgemeine GraySort 10 GB-Datei mit HDInsight ausführen. Es gibt drei Einzelvorgänge ausführen: Teragen zum Generieren von Daten Terasort zum Sortieren der Daten und Teravalidate zu bestätigen, dass die Daten ordnungsgemäß sortiert wurden.

>[AZURE.NOTE] Der Quellcode finden im Anhang. 

Viele zusätzliche Dokumentation im Web Hadoop-Technologien, wie Java-basierte MapReduce Programmierung streaming und Dokumentation über die Cmdlets in Windows PowerShell verwendet scripting. Weitere Informationen zu diesen Ressourcen finden Sie unter:

- [Entwickeln von Java MapReduce Programme für Hadoop in HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
- [Hadoop Aufträge in HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Einführung in Azure HDInsight][hdinsight-introduction]

Heute viele Leute wählen Sie Struktur und Schweine über MapReduce.  Weitere Informationen finden Sie unter:

- [Struktur in HDInsight verwenden](hdinsight-use-hive.md)
- [Schweine in HDInsight verwenden](hdinsight-use-pig.md)
 
**Komponenten**:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **ein HDInsight-Cluster**. Informationen auf verschiedene Weise mit dieser Cluster erstellt werden können, finden Sie unter [Erstellen Hadoop Cluster in HDInsight](hdinsight-provision-clusters.md).
- **Eine Workstation mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="hdinsight-sample-wordcount"></a>Word Count - Java 

Um einen MapReduce-Projekt zu senden, erstellen Sie zuerst eine Auftragsdefinition MapReduce. In der Auftragsdefinition Geben Sie MapReduce Programm JAR-Datei und den Speicherort der JAR-Datei, die **wasbs:///example/jars/hadoop-mapreduce-examples.jar**, den Klassennamen und die Argumente.  Wordcount MapReduce-Programm akzeptiert zwei Argumente: die Quelldatei, die zu zählenden Wörter und den Speicherort für die Ausgabe verwendet werden.

Der Quellcode finden im [Anhang A](#apendix-a---the-word-count-MapReduce-program-in-java).

Für die Prozedur ein Java MapReduce-Programm, siehe - [Programme entwickeln Java MapReduce für Hadoop in HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md) entwickeln
 
**Einen Word Count MapReduce-Auftrag senden**

1. Öffnen Sie **Windows PowerShell ISE**. Eine Anleitung finden Sie [Installieren und Konfigurieren von Azure PowerShell][powershell-install-configure].
2. Fügen Sie das folgende PowerShell-Skript:

        $subscriptionName = "<Azure Subscription Name>"
        $resourceGroupName = "<Resource Group Name>"
        $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name
        
        Select-AzureRmSubscription -SubscriptionName $subscriptionName
        
        # Define the MapReduce job
        $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "wordcount" `
                                    -Arguments "wasbs:///example/data/gutenberg/davinci.txt", "wasbs:///example/data/WordCountOutput1"
        
        # Submit the job and wait for job completion
        $cred = Get-Credential -Message "Enter the HDInsight cluster HTTP user credential:" 
        $mrJob = Start-AzureRmHDInsightJob `
                            -ResourceGroupName $resourceGroupName `
                            -ClusterName $clusterName `
                            -HttpCredential $cred `
                            -JobDefinition $mrJobDefinition 
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -JobId $mrJob.JobId 
        
        # Get the job output
        $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
        $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
        $defaultStorageContainer = $cluster.DefaultStorageContainer
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -DefaultStorageAccountName $defaultStorageAccount `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultContainer $defaultStorageContainer  `
            -JobId $mrJob.JobId `
            -DisplayOutputType StandardError

        # Download the job output to the workstation
        $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 
        Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force
        
        # Display the output file
        cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"

    MapReduce-Auftrags generiert eine Datei namens *Teil R 00000*, Wörter und die Werte enthält. Das Skript verwendet den Befehl **Findstr** auf alle Wörter, die *"there"*enthält.

3. Variablen Sie die ersten 3, und führen Sie das Skript.

## <a name="hdinsight-sample-csharp-streaming"></a>Word Count - C# streaming

Hadoop bietet eine streaming-API, MapReduce ermöglicht zuordnen und Funktionen in anderen Sprachen als Java reduzieren.

> [AZURE.NOTE] Die Schritte in diesem Lernprogramm gelten nur für Windows-basierte HDInsight-Cluster. Beispiel für Linux-basierte HDInsight Cluster Streaming finden Sie unter [Python entwickeln streaming Programme für HDInsight](hdinsight-hadoop-streaming-python.md).

Im Beispiel der Zuordnung und des Reduzierstücks sind ausführbare Dateien, die die Eingabe von [Stdin] lesen[ stdin-stdout-stderr] (Zeile für Zeile) und die Ausgabe an [Stdout][stdin-stdout-stderr]. Das Programm zählt alle Wörter im Text.

Für **Mapper**eine ausführbare Datei angegeben, startet jeder Aufgabe Mapper die ausführbare Datei als separater Prozess bei der Initialisierung der Zuordnung. Mapper-Task ausgeführt wird, diese Eingabe in Linien konvertiert und Zeilen [Stdin] feeds[ stdin-stdout-stderr] des Prozesses.

In der Zwischenzeit sammelt Mapper zeilenorientierten Ausgabe aus dem Stdout des Prozesses. Jede Zeile in ein Schlüssel-Wert-Paar als Ausgabe der Zuordnung werden konvertiert. Standardmäßig das Präfix einer Zeile auf der ersten Registerkarte Zeichen ist der Schlüssel und der Rest der Zeile (ausgenommen das Tabulatorzeichen) wird. Liegt keine Tabstoppzeichen in der Zeile, ganze Zeile gilt als Schlüssel und der Wert ist null.

Eine ausführbare Datei für **Reduzierstück**angegeben, startet jeder Aufgabe Reduzierstücks die ausführbaren Datei als separater Prozess bei der Initialisierung des Reduzierstücks. Reducer-Task ausgeführt wird, konvertiert die Eingabe Schlüssel-Wert-Paare in Zeilen und feeds Zeilen Sie [Stdin] [ stdin-stdout-stderr] des Prozesses.

Des Reduzierstücks sammelt in der Ausgabe zeilenorientierten [Stdout] [ stdin-stdout-stderr] des Prozesses. Jede Zeile in ein Schlüssel-Wert-Paar als Ausgabe des Reduzierstücks werden konvertiert. Standardmäßig das Präfix einer Zeile auf der ersten Registerkarte Zeichen ist der Schlüssel und der Rest der Zeile (ausgenommen das Tabulatorzeichen) wird.

Weitere Informationen zur Schnittstelle Hadoop Streaming finden Sie unter [Hadoop Streaming] [Hadoop-streaming].

**Ein C# streaming Word Count Auftrag senden**

- Gehen Sie in [Word Count - Java](#word-count-java)und Ersetzen der Auftragsdefinition mit den folgenden:

        $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                                -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                                -Mapper "cat.exe" `
                                -Reducer "wc.exe" `
                                -InputPath "/example/data/gutenberg/davinci.txt" `
                                -OutputPath "/example/data/StreamingOutput/wc.txt"  


    Die Ausgabedatei wird:
    
        example/data/StreamingOutput/wc.txt/part-00000      
                                
## <a name="hdinsight-sample-pi-estimator"></a>PI-Rechner

Pi-Rechner verwendet ein (quasi Monte Carlo) Methode, um den Wert von Pi zu schätzen. Punkten nach dem Zufallsprinzip innerhalb einer Einheit fallen Platz auch einen Kreis mit einer Wahrscheinlichkeit gleich der Fläche eines Kreises, der in das Feld eingetragen Pi/4. Der Wert von Pi werden ab der 4R, abgeschätzt R das Verhältnis der Anzahl der Punkte ist, die in den Kreis, um die Gesamtzahl der Punkte in das Quadrat. Je größer die Stichprobe der Punkte ist die bessere Schätzung.

Für dieses Beispiel bereitgestellte Skript führt einen Hadoop JAR-Einzelvorgang und soll, führen Sie mit einem Wert 16 Karten, die jeweils 10 Millionen Punkten berechnet anhand der Parameterwerte erforderlich ist. Diese Parameterwerte können geändert werden, um den geschätzten Wert von Pi zu verbessern. Zu Referenzzwecken werden die ersten 10 Dezimalstellen Pi 3.1415926535.

**Einen Pi Gutachter Auftrag senden**

- Gehen Sie in [Word Count - Java](#word-count-java)und Ersetzen der Auftragsdefinition mit den folgenden:

        $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "pi" `
                                    -Arguments "16", "10000000"

## <a name="hdinsight-sample-10gb-graysort"></a>10 GB Graysort

Dieses Beispiel verwendet eine bescheidene 10GB Daten relativ schnell ausgeführt werden kann. Entwickelt von Owen O'Malley und Arun Murthy, die jährliche allgemeine ("Daytona") Terabyte Art Benchmark 2009 mit 0,578 TB/Min. (100 TB in 173 Minuten) gewonnen MapReduce-Applikationen verwendet. Weitere Informationen zu dieser und anderen Sortierung Benchmarks finden Sie in der [Sortbenchmark](http://sortbenchmark.org/) -Website.

Dieses Beispiel verwendet drei Sätze von MapReduce Programme:

1. **TeraGen** ist ein MapReduce-Programm, mit dem Sie die Zeilen der zu sortierenden Daten generieren.
2. **TeraSort** Beispiele Eingabedaten und MapReduce zum Sortieren der Daten in einer Auftragssumme verwendet. TeraSort ist eine standardmäßige MapReduce-Funktionen außer einem benutzerdefinierten Partitionierer, der eine sortierte Liste der Stichprobe n-1-Schlüssel verwendet, das den Schlüssel für jede reduzieren definiert. Insbesondere alle Schlüssel solche, die (Beispiel) [i-1] < = Schlüssel < Beispiel [i] i zu erhalten. Dies garantiert, dass die Ausgaben i reduzieren sind kleiner als die Ausgabe i + 1 verringern.
3. **TeraValidate** ist ein MapReduce-Programm, das überprüft, dass global die Ausgabe sortiert ist. Eine Karte pro Datei im Ausgabeverzeichnis erstellt und jede Karte wird sichergestellt, dass jeder Schlüssel kleiner oder gleich vorherigen. Die Funktion generiert auch Datensätze der ersten und letzten Schlüssel jeder Datei und die Funktion wird sichergestellt, dass der erste Schlüssel der Datei i größer als der letzte Teil i-Datei 1. Probleme werden als Ausgabe reduzieren mit Schlüsseln gemeldet, die sind.

Das Eingabe- und Format, alle drei Programme liest und schreibt Text-Dateien in das richtige Format. Die Ausgabe reduzieren hat Replikation 1 anstelle des standardmäßigen 3 da Benchmark-Wettbewerb nicht erfordert, dass die Ausgabe von Daten auf mehreren Knoten repliziert werden.

Drei Aufgaben müssen im Beispiel jeweils einen MapReduce Programme beschrieben:

1. Generieren der Daten für die Sortierung **TeraGen** MapReduce Einzelvorgang ausführen.
2. Sortieren Sie die Daten **TeraSort** MapReduce Einzelvorgang ausführen.
3. Bestätigen Sie, dass die Daten richtig sortiert wurden **TeraValidate** MapReduce-Preis.

**Die Aufträge senden**

- Gehen Sie in [Word Count - Java](#word-count-java)und verwenden Sie die folgenden Auftragsdefinitionen:

    $teragen = New AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - Klassenname "Teragen" '-Argumente "-Dmapred.map.tasks=50", "100000000", "/ / Daten/10 GB-Art-Beispieleingabe"
    
    $terasort = New AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - Klassenname "Terasort" '-Argumente "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/ / Daten/10 GB-Art-Beispieleingabe", "/ / Daten/10 GB-Art-Beispielausgabe"
    
    $teravalidate = New AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - Klassenname "Teravalidate" '-Argumente "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/ / Daten/10 GB-Art-Beispielausgabe", "/ Beispiel/Daten/10 GB-Art-überprüfen"


##<a name="next-steps"></a>Nächste Schritte 

Artikel in allen Beispielen in diesem Artikel, haben Sie gelernt, HDInsight-Cluster mithilfe von Azure PowerShell enthaltenen Beispiele ausführen. Lernprogramme zu HDInsight Schwein, Struktur und MapReduce mit finden Sie unter den folgenden Themen:

* [Erste Schritte mit Hadoop Struktur in HDInsight analysieren Mobiltelefon verwenden][hdinsight-get-started]
* [Hadoop auf HDInsight Schwein verwenden][hdinsight-use-pig]
* [Hadoop auf HDInsight Struktur verwenden][hdinsight-use-hive]
* [Hadoop Aufträge in HDInsight] [hdinsight-submit-jobs]
* [Azure HDInsight SDK-Dokumentation][hdinsight-sdk-documentation]
* [Hadoop in HDInsight Debuggen: Fehlermeldungen] [hdinsight-errors]


## <a name="appendix-a---the-word-count-source-code"></a>Anhang A - Word Count-Quellcode

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


## <a name="appendix-b---the-word-count-streaming-source-code"></a>Anhang B – Wortzahl streaming-Quellcode

MapReduce-Programm verwendet die Anwendung cat.exe als Mapping-Schnittstelle zum Streamen des Texts in der Konsole und die Anwendung wc.exe Schnittstelle verringern die Anzahl der Wörter, die aus einem Dokument übertragen werden. Sowohl die Mapper Reduzierstücks Zeichen, Zeile für Zeile aus dem Standardeingabestream (Stdin) gelesen und in die Standardausgabe (Stdout) geschrieben.

    // The source code for the cat.exe (Mapper).

    using System;
    using System.IO;

    namespace cat
    {
        class cat
        {
            static void Main(string[] args)
            {
                if (args.Length > 0)
                {
                    Console.SetIn(new StreamReader(args[0]));
                }

                string line;
                while ((line = Console.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
    }



Mapper-Code in der Datei cat.cs verwendet [StreamReader] [ streamreader] Objekt lesen den Stream in den Standardausgabestream mit statischen [Console.Writeline schreibt] Zeichen auf der Konsole von eingehenden[ console-writeline] Methode.


    // The source code for wc.exe (Reducer) is:

    using System;
    using System.IO;
    using System.Linq;

    namespace wc
    {
        class wc
        {
            static void Main(string[] args)
            {
                string line;
                var count = 0;

                if (args.Length > 0){
                    Console.SetIn(new StreamReader(args[0]));
                }

                while ((line = Console.ReadLine()) != null) {
                    count += line.Count(cr => (cr == ' ' || cr == '\n'));
                }
                Console.WriteLine(count);
            }
        }
    }


Reducer-Code in der Datei wc.cs verwendet [StreamReader] [ streamreader] -Objekt zum Lesen von Zeichen aus dem Standardeingabestream, die Ausgabe von cat.exe-Mapper. Zeichen mit [Console.Writeline] liest[ console-writeline] Methode er Wörter zählen Leerzeichen und Zeilenende Zeichen am Ende jedes Wort zählt. Schreibt dann die Summe in den Standardausgabestream mit [Console.Writeline] [ console-writeline] Methode.





## <a name="appendix-c---the-pi-estimator-source-code"></a>Anhang C - Quellcode Pi Gutachter

Pi Gutachter, Java-Code, der den Mapper und Reduzierstücks Funktionen steht unter Kontrolle. Mapper-Programm generiert eine angegebene Anzahl von Punkten nach dem Zufallsprinzip in einem Einheitsquadrat und zählt dann die Anzahl der Punkte, die innerhalb des Kreises. Reducer Programm sammelt Punkte der Mapper gezählt und schätzt den Wert von Pi aus der Formel 4R, R ist das Verhältnis der Anzahl der Punkte in den Kreis, um die Gesamtzahl der Punkte in das Quadrat gezählt.

    /**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements. See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership. The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License. You may obtain a copy of the License at
    *
    * http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or   implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.math.BigDecimal;
    import java.util.Iterator;

    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.BooleanWritable;
    import org.apache.hadoop.io.LongWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Writable;
    import org.apache.hadoop.io.WritableComparable;
    import org.apache.hadoop.io.SequenceFile.CompressionType;
    import org.apache.hadoop.mapred.FileInputFormat;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.MapReduceBase;
    import org.apache.hadoop.mapred.Mapper;
    import org.apache.hadoop.mapred.OutputCollector;
    import org.apache.hadoop.mapred.Reducer;
    import org.apache.hadoop.mapred.Reporter;
    import org.apache.hadoop.mapred.SequenceFileInputFormat;
    import org.apache.hadoop.mapred.SequenceFileOutputFormat;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;


    //A Map-reduce program to estimate the value of Pi
    //using quasi-Monte Carlo method.
    //
    //Mapper:
    //Generate points in a unit square
    //and then count points inside/outside of the inscribed circle of the square.
    //
    //Reducer:
    //Accumulate points inside/outside results from the mappers.
    //Let numTotal = numInside + numOutside.
    //The fraction numInside/numTotal is a rational approximation of
    //the value (Area of the circle)/(Area of the square),
    //where the area of the inscribed circle is Pi/4
    //and the area of unit square is 1.
    //Then, Pi is estimated value to be 4(numInside/numTotal).
    //

    public class PiEstimator extends Configured implements Tool {
    //tmp directory for input/output
    static private final Path TMP_DIR = new Path(
    PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

    //2-dimensional Halton sequence {H(i)},
    //where H(i) is a 2-dimensional point and i >= 1 is the index.
    //Halton sequence is used to generate sample points for Pi estimation.
    private static class HaltonSequence {
    // Bases
    static final int[] P = {2, 3};
    //Maximum number of digits allowed
    static final int[] K = {63, 40};

    private long index;
    private double[] x;
    private double[][] q;
    private int[][] d;

    //Initialize to H(startindex),
    //so the sequence begins with H(startindex+1).
    HaltonSequence(long startindex) {
    index = startindex;
    x = new double[K.length];
    q = new double[K.length][];
    d = new int[K.length][];
    for(int i = 0; i < K.length; i++) {
    q[i] = new double[K[i]];
    d[i] = new int[K[i]];
    }

    for(int i = 0; i < K.length; i++) {
    long k = index;
    x[i] = 0;

    for(int j = 0; j < K[i]; j++) {
    q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
    d[i][j] = (int)(k % P[i]);
    k = (k - d[i][j])/P[i];
    x[i] += d[i][j] * q[i][j];
    }
    }
    }

    //Compute next point.
    //Assume the current point is H(index).
    //Compute H(index+1).
    //@return a 2-dimensional point with coordinates in [0,1)^2
    double[] nextPoint() {
    index++;
    for(int i = 0; i < K.length; i++) {
    for(int j = 0; j < K[i]; j++) {
    d[i][j]++;
    x[i] += q[i][j];
    if (d[i][j] < P[i]) {
    break;
    }
    d[i][j] = 0;
    x[i] -= (j == 0? 1.0: q[i][j-1]);
    }
    }
    return x;
    }
    }

    //Mapper class for Pi estimation.
    //Generate points in a unit square and then
    //count points inside/outside of the inscribed circle of the square.
    public static class PiMapper extends MapReduceBase
    implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

    //Map method.
    //@param offset samples starting from the (offset+1)th sample.
    //@param size the number of samples for this map
    //@param out output {ture->numInside, false->numOutside}
    //@param reporter
    public void map(LongWritable offset,
    LongWritable size,
    OutputCollector<BooleanWritable, LongWritable> out,
    Reporter reporter) throws IOException {

    final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
    long numInside = 0L;
    long numOutside = 0L;

    for(long i = 0; i < size.get(); ) {
    //generate points in a unit square
    final double[] point = haltonsequence.nextPoint();

    //count points inside/outside of the inscribed circle of the square
    final double x = point[0] - 0.5;
    final double y = point[1] - 0.5;
    if (x*x + y*y > 0.25) {
    numOutside++;
    } else {
    numInside++;
    }

    //report status
    i++;
    if (i % 1000 == 0) {
    reporter.setStatus("Generated " + i + " samples.");
    }
    }

    //output map results
    out.collect(new BooleanWritable(true), new LongWritable(numInside));
    out.collect(new BooleanWritable(false), new LongWritable(numOutside));
    }
    }


    //Reducer class for Pi estimation.
    //Accumulate points inside/outside results from the mappers.
    public static class PiReducer extends MapReduceBase
    implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

    private long numInside = 0;
    private long numOutside = 0;
    private JobConf conf; //configuration for accessing the file system

    //Store job configuration.
    @Override
    public void configure(JobConf job) {
    conf = job;
    }


    // Accumulate number of points inside/outside results from the mappers.
    // @param isInside Is the points inside?
    // @param values An iterator to a list of point counts
    // @param output dummy, not used here.
    // @param reporter

    public void reduce(BooleanWritable isInside,
    Iterator<LongWritable> values,
    OutputCollector<WritableComparable<?>, Writable> output,
    Reporter reporter) throws IOException {
    if (isInside.get()) {
    for(; values.hasNext(); numInside += values.next().get());
    } else {
    for(; values.hasNext(); numOutside += values.next().get());
    }
    }

    //Reduce task done, write output to a file.
    @Override
    public void close() throws IOException {
    //write output to a file
    Path outDir = new Path(TMP_DIR, "out");
    Path outFile = new Path(outDir, "reduce-out");
    FileSystem fileSys = FileSystem.get(conf);
    SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
    outFile, LongWritable.class, LongWritable.class,
    CompressionType.NONE);
    writer.append(new LongWritable(numInside), new LongWritable(numOutside));
    writer.close();
    }
    }

    //Run a map/reduce job for estimating Pi.
    //@return the estimated value of Pi.
    public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
    )
    throws IOException {
    //setup job conf
    jobConf.setJobName(PiEstimator.class.getSimpleName());

    jobConf.setInputFormat(SequenceFileInputFormat.class);

    jobConf.setOutputKeyClass(BooleanWritable.class);
    jobConf.setOutputValueClass(LongWritable.class);
    jobConf.setOutputFormat(SequenceFileOutputFormat.class);

    jobConf.setMapperClass(PiMapper.class);
    jobConf.setNumMapTasks(numMaps);

    jobConf.setReducerClass(PiReducer.class);
    jobConf.setNumReduceTasks(1);

    // turn off speculative execution, because DFS doesn't handle
    // multiple writers to the same file.
    jobConf.setSpeculativeExecution(false);

    //setup input/output directories
    final Path inDir = new Path(TMP_DIR, "in");
    final Path outDir = new Path(TMP_DIR, "out");
    FileInputFormat.setInputPaths(jobConf, inDir);
    FileOutputFormat.setOutputPath(jobConf, outDir);

    final FileSystem fs = FileSystem.get(jobConf);
    if (fs.exists(TMP_DIR)) {
     throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
     + " already exists. Please remove it first.");
     }
     if (!fs.mkdirs(inDir)) {
     throw new IOException("Cannot create input directory " + inDir);
     }

     //generate an input file for each map task
     try {
     for(int i=0; i < numMaps; ++i) {
     final Path file = new Path(inDir, "part"+i);
     final LongWritable offset = new LongWritable(i * numPoints);
     final LongWritable size = new LongWritable(numPoints);
     final SequenceFile.Writer writer = SequenceFile.createWriter(
     fs, jobConf, file,
     LongWritable.class, LongWritable.class, CompressionType.NONE);
     try {
     writer.append(offset, size);
     } finally {
     writer.close();
     }
     System.out.println("Wrote input for Map #"+i);
     }

     //start a map/reduce job
     System.out.println("Starting Job");
     final long startTime = System.currentTimeMillis();
     JobClient.runJob(jobConf);
     final double duration = (System.currentTimeMillis() - startTime)/1000.0;
     System.out.println("Job Finished in " + duration + " seconds");

     //read outputs
     Path inFile = new Path(outDir, "reduce-out");
     LongWritable numInside = new LongWritable();
     LongWritable numOutside = new LongWritable();
     SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
     try {
     reader.next(numInside, numOutside);
     } finally {
     reader.close();
     }

     //compute estimated value
     return BigDecimal.valueOf(4).setScale(20)
     .multiply(BigDecimal.valueOf(numInside.get()))
     .divide(BigDecimal.valueOf(numMaps))
     .divide(BigDecimal.valueOf(numPoints));
     } finally {
     fs.delete(TMP_DIR, true);
     }
     }

    //Parse arguments and then runs a map/reduce job.
    //Print output in standard out.
    //@return a non-zero if there is an error. Otherwise, return 0.
     public int run(String[] args) throws Exception {
     if (args.length != 2) {
     System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
     ToolRunner.printGenericCommandUsage(System.err);
     return -1;
     }

     final int nMaps = Integer.parseInt(args[0]);
     final long nSamples = Long.parseLong(args[1]);

     System.out.println("Number of Maps = " + nMaps);
     System.out.println("Samples per Map = " + nSamples);

     final JobConf jobConf = new JobConf(getConf(), getClass());
     System.out.println("Estimated value of Pi is "
     + estimate(nMaps, nSamples, jobConf));
     return 0;
     }

     //main method for running it as a stand alone command.
     public static void main(String[] argv) throws Exception {
     System.exit(ToolRunner.run(null, new PiEstimator(), argv));
     }
     }
     
## <a name="appendix-d---the-10gb-graysort-source-code"></a>Anhang D - Quellcode 10gb graysort

Der Code für TeraSort MapReduce-Programm ist zur Prüfung in diesem Abschnitt dargestellt.


    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     *     http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

    package org.apache.hadoop.examples.terasort;

    import java.io.IOException;
    import java.io.PrintStream;
    import java.net.URI;
    import java.util.ArrayList;
    import java.util.List;

    import org.apache.commons.logging.Log;
    import org.apache.commons.logging.LogFactory;
    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.filecache.DistributedCache;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.NullWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.Partitioner;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;

    /**
     * Generates the sampled split points, launches the job,
     * and waits for it to finish.
     * <p>
     * To run the program:
     * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
     */

    public class TeraSort extends Configured implements Tool {
      private static final Log LOG = LogFactory.getLog(TeraSort.class);

      /**
       * A partitioner that splits text keys into roughly equal
       * partitions in a global sorted order.
       */

      static class TotalOrderPartitioner implements Partitioner<Text,Text>{
        private TrieNode trie;
        private Text[] splitPoints;

        /**
         * A generic trie node
         */
        static abstract class TrieNode {
          private int level;
          TrieNode(int level) {
            this.level = level;
          }
          abstract int findPartition(Text key);
          abstract void print(PrintStream strm) throws IOException;
          int getLevel() {
            return level;
          }
        }

        /**
         * An inner trie node that contains 256 children based on the next
         * character.
         */
        static class InnerTrieNode extends TrieNode {
          private TrieNode[] child = new TrieNode[256];

          InnerTrieNode(int level) {
            super(level);
          }
          int findPartition(Text key) {
            int level = getLevel();
            if (key.getLength() <= level) {
              return child[0].findPartition(key);
            }
            return child[key.getBytes()[level]].findPartition(key);
          }
          void setChild(int idx, TrieNode child) {
            this.child[idx] = child;
          }
          void print(PrintStream strm) throws IOException {
            for(int ch=0; ch < 255; ++ch) {
              for(int i = 0; i < 2*getLevel(); ++i) {
                strm.print(' ');
              }
              strm.print(ch);
              strm.println(" ->");
              if (child[ch] != null) {
                child[ch].print(strm);
              }
            }
          }
        }

        /**
         * A leaf trie node that does string compares to figure out where the given
         * key belongs between lower..upper.
         */
        static class LeafTrieNode extends TrieNode {
          int lower;
          int upper;
          Text[] splitPoints;
          LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
            super(level);
            this.splitPoints = splitPoints;
            this.lower = lower;
            this.upper = upper;
          }
          int findPartition(Text key) {
            for(int i=lower; i<upper; ++i) {
              if (splitPoints[i].compareTo(key) >= 0) {
                return i;
              }
            }
            return upper;
          }
          void print(PrintStream strm) throws IOException {
            for(int i = 0; i < 2*getLevel(); ++i) {
              strm.print(' ');
            }
            strm.print(lower);
            strm.print(", ");
            strm.println(upper);
          }
        }


        /**
         * Read the cut points from the given sequence file.
         * @param fs the file system
         * @param p the path to read
         * @param job the job config
         * @return the strings to split the partitions on
         * @throws IOException
         */
        private static Text[] readPartitions(FileSystem fs, Path p,
                                             JobConf job) throws IOException {
          SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
          List<Text> parts = new ArrayList<Text>();
          Text key = new Text();
          NullWritable value = NullWritable.get();
          while (reader.next(key, value)) {
            parts.add(key);
            key = new Text();
          }
          reader.close();
          return parts.toArray(new Text[parts.size()]);  
        }

        /**
         * Given a sorted set of cut points, build a trie that will find the correct
         * partition quickly.
         * @param splits the list of cut points
         * @param lower the lower bound of partitions 0..numPartitions-1
         * @param upper the upper bound of partitions 0..numPartitions-1
         * @param prefix the prefix that we have already checked against
         * @param maxDepth the maximum depth we will build a trie for
         * @return the trie node that will divide the splits correctly
         */
        private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                          Text prefix, int maxDepth) {
          int depth = prefix.getLength();
          if (depth >= maxDepth || lower == upper) {
            return new LeafTrieNode(depth, splits, lower, upper);
          }
          InnerTrieNode result = new InnerTrieNode(depth);
          Text trial = new Text(prefix);
          // append an extra byte on to the prefix
          trial.append(new byte[1], 0, 1);
          int currentBound = lower;
          for(int ch = 0; ch < 255; ++ch) {
            trial.getBytes()[depth] = (byte) (ch + 1);
            lower = currentBound;
            while (currentBound < upper) {
              if (splits[currentBound].compareTo(trial) >= 0) {
                break;
              }
              currentBound += 1;
            }
            trial.getBytes()[depth] = (byte) ch;
            result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                         maxDepth);
          }
          // pick up the rest
          trial.getBytes()[depth] = 127;
          result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                        maxDepth);
          return result;
        }

        public void configure(JobConf job) {
          try {
            FileSystem fs = FileSystem.getLocal(job);
            Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
            splitPoints = readPartitions(fs, partFile, job);
            trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
          } catch (IOException ie) {
            throw new IllegalArgumentException("can't read paritions file", ie);
          }
        }

        public TotalOrderPartitioner() {
        }

        public int getPartition(Text key, Text value, int numPartitions) {
          return trie.findPartition(key);
        }

      }

      public int run(String[] args) throws Exception {
        LOG.info("starting");
        JobConf job = (JobConf) getConf();
        Path inputDir = new Path(args[0]);
        inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
        Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
        URI partitionUri = new URI(partitionFile.toString() +
                                   "#" + TeraInputFormat.PARTITION_FILENAME);
        TeraInputFormat.setInputPaths(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        job.setJobName("TeraSort");
        job.setJarByClass(TeraSort.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);
        job.setInputFormat(TeraInputFormat.class);
        job.setOutputFormat(TeraOutputFormat.class);
        job.setPartitionerClass(TotalOrderPartitioner.class);
        TeraInputFormat.writePartitionFile(job, partitionFile);
        DistributedCache.addCacheFile(partitionUri, job);
        DistributedCache.createSymlink(job);
        job.setInt("dfs.replication", 1);
        TeraOutputFormat.setFinalSync(job, true);
        JobClient.runJob(job);
        LOG.info("done");
        return 0;
      }

      /**
       * @param args
       */

      public static void main(String[] args) throws Exception {
        int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
        System.exit(res);
      }
    }














 




[hdinsight-errors]: hdinsight-debug-jobs.md

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md


[powershell-install-configure]: ../powershell-install-configure.md

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
