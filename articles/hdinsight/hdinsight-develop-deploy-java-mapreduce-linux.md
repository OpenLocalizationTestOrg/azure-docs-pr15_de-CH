<properties
    pageTitle="Entwickeln von Java MapReduce Programme für Linux-basierte HDInsight | Microsoft Azure"
    description="Informationen Sie zu Java MapReduce Programme für Linux-basierte HDInsight bereitstellen."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="Blackmist"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight-linux"></a>Java MapReduce Programme für Hadoop auf HDInsight Linux Entwicklung

Diese Dokumente führt Sie durch Erstellen einer MapReduce-Anwendung bereitstellen und Ausführen auf einem Linux-basierten Hadoop auf HDInsight Cluster mit Apache Maven.

##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 oder höher (oder eine entsprechende wie OpenJDK)

- [Apache Maven](http://maven.apache.org/)

- **Ein Azure-Abonnement**

- **Azure CLI**

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Die folgenden Umgebungsvariablen können beim Installieren von Java, und JDK festgelegt. Jedoch sollten Sie überprüfen, dass sie vorhanden und enthalten die Werte für Ihr System.

* **JAVA_HOME** - darauf zu dem Verzeichnis, in dem Java Runtime Environment (JRE) installiert. Beispielsweise in einem Mac OS, Unix oder Linux-System muss einen Wert wie `/usr/lib/jvm/java-7-oracle`. In Windows müsste einen Wert wie`c:\Program Files (x86)\Java\jre1.7`

* **Pfad** – sollte die folgenden Pfade:

    * **JAVA_HOME** (oder den entsprechenden Pfad)

    * **JAVA_HOME\bin** (oder den entsprechenden Pfad)

    * Das Verzeichnis, in dem Maven installiert ist

##<a name="create-a-new-maven-project"></a>Erstellen eines neuen Projekts Maven

1. Wechseln Sie von einer terminal Sitzung oder Befehlszeile Umgebung an diesem Projekt speichern möchten.

3. Verwenden Sie den Befehl __Mvn__ mit Maven installiert ist, das Gerüst für das Projekt generiert.

        mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Ein neues Verzeichnis dadurch im aktuellen Verzeichnis mit dem Namen angegebene __ArtifactID__ Parameter (**Wordcountjava** in diesem Beispiel.) Dieses Verzeichnis enthält die folgenden Elemente:

    * __pom.xml__ - [Projekt Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) , die zum Erstellen des Projekts verwendete Informationen und Details enthält.

    * __Src__ - Verzeichnis, das Verzeichnis __Main/Java/Org/Apache/Hadoop/Beispiele__ enthält, in dem Sie die Anwendung erstellen.

3. Löschen Sie die Datei __src/test/java/org/apache/hadoop/examples/apptest.java__ , wie sie in diesem Beispiel nicht verwendet werden.

##<a name="add-dependencies"></a>Abhängigkeit hinzufügen

1. Bearbeiten Sie die Datei __pom.xml__ , und fügen Sie Folgendes in die `<dependencies>` Abschnitt:

        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-examples</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-client-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>

    Diese Maven teilt mit, dass das Projekt die Bibliotheken erfordert (innerhalb &lt;ArtifactId\>) mit einer bestimmten Version (innerhalb &lt;Version\>). Zur Kompilierzeit werden diese aus dem Standard Maven Repository heruntergeladen. [Maven Repository-Suche](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) können Sie mehr.

    Die `<scope>provided</scope>` weist Maven, dass diese Abhängigkeit nicht mit der Anwendung verpackt werden soll als HDInsight Cluster zur Laufzeit bereitgestellt werden.

2. Fügen Sie folgenden __pom.xml__ Datei. Dies muss in der `<project>...</project>` -Tags in die Datei. zwischen `</dependencies>` und `</project>`.

        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                    <goals>
                      <goal>shade</goal>
                    </goals>
                </execution>
              </executions>
            </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
               <source>1.7</source>
               <target>1.7</target>
              </configuration>
            </plugin>
          </plugins>
        </build>

    Das erste Plug-in konfiguriert [Maven Schatten Plug-in](http://maven.apache.org/plugins/maven-shade-plugin/), das verwendet wird, eine Uberjar (bezeichnet einen Fatjar) abhängig von der Anwendung benötigte enthält. Duplizierung von Lizenzen innerhalb des Jar verursachen Probleme bei einigen Systemen können verhindert.

    Das zweite Plug-in konfiguriert den Maven-Compiler verwendet wird, um die Version der Java HDInsight-Cluster verwendete Version der Anwendung erforderlich.

3. Speichern Sie die Datei __pom.xml__ .

##<a name="create-the-mapreduce-application"></a>Die MapReduce-Anwendung erstellen

1. Wechseln Sie zum Verzeichnis __Wordcountjava/Src/Main/Java/Org/Apache/Hadoop/Beispiele__ , und benennen Sie die Datei __App.java__ in __WordCount.java__.

2. Öffnen Sie die Datei __WordCount.java__ in einem Texteditor, und Ersetzen Sie den Inhalt durch Folgendes:

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

    Der Paketname ist **org.apache.hadoop.examples** und der Klassenname ist **WordCount**. Sie verwenden diesen Namen, wenn MapReduce-Auftrag senden.

3. Speichern Sie die Datei.

##<a name="build-the-application"></a>Erstellen Sie die Anwendung

1. Wechseln Sie zum Verzeichnis __Wordcountjava__ , wenn Sie nicht bereits vorhanden sind.

2. Verwenden Sie den folgenden Befehl zum Erstellen einer JAR-Datei mit der Anwendung:

        mvn clean package

    Alle vorherigen Build Elemente säubern, Abhängigkeiten, die nicht bereits installiert wurden, erstellen und verpacken die Anwendung herunterladen.

3. Nachdem der Befehl abgeschlossen ist, enthält das Verzeichnis __Wordcountjava-Ziel__ eine Datei namens __Wordcountjava-1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] Die Datei __Wordcountjava-1.0-SNAPSHOT.jar__ ist ein Uberjar enthält nicht nur die WordCount-Auftrag, sondern auch Faktoren, die der Auftrag zur Laufzeit erfordert.


##<a id="upload"></a>Die JAR-Datei hochladen

Verwenden Sie den folgenden Befehl zum HDInsight Hauptknoten JAR-Datei hochladen:

    scp wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

Dies kopiert die Dateien aus dem lokalen System auf dem Head-Knoten.

> [AZURE.NOTE] Wenn Sie ein Kennwort zum Schützen von SSH-Konto verwendet, werden Sie aufgefordert, das Kennwort anzugeben. Verwendet einen SSH-Schlüssel möglicherweise mit den `-i` Parameter und den Pfad zum privaten Schlüssel. Z. B. `scp -i /path/to/private/key wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

##<a name="run"></a>Die Stapelverarbeitung MapReduce

1. Verbinden Sie mit HDInsight über SSH, wie in den folgenden Artikeln beschrieben:

    - [Verwenden Sie SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix und Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Verwenden Sie über SSH-Sitzung den folgenden Befehl zum Ausführen der Anwendung MapReduce:

        yarn jar wordcountjava.jar org.apache.hadoop.examples.WordCount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/wordcountout

    Diese Anwendung WordCount MapReduce zählen der Wörter in der Datei davinci.txt, und die Ergebnisse zu __Wasbs: / / / Beispiel/Daten/Wordcountout__. Die Eingabe- und Ausgabe werden auf den Standardspeicher für den Cluster gespeichert.

3. Sobald der Auftrag abgeschlossen ist, verwenden Sie folgende Ergebnisse anzeigen:

        hdfs dfs -cat wasbs:///example/data/wordcountout/*

    Sie erhalten eine Liste von Wörtern und Zahlen mit Werten, die der folgenden ähnelt:

        zeal    1
        zelus   1
        zenith  2

##<a id="nextsteps"></a>Nächste Schritte

In diesem Dokument haben Sie Java MapReduce Projekt entwickeln. Anzeigen Sie die folgenden Dokumente für andere Methoden zum Arbeiten mit HDInsight

- [Struktur mit HDInsight verwenden][hdinsight-use-hive]
- [Verwenden Sie Schwein mit HDInsight][hdinsight-use-pig]
- [Verwenden Sie MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

Weitere Informationen finden Sie auch [Java Developer Center](https://azure.microsoft.com/develop/java/).

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

