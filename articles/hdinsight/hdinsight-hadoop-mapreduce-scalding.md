<properties
 pageTitle="Verbrennung MapReduce Arbeitsplätze mit Maven | Microsoft Azure"
 description="Informationen Sie zum Maven Erstellen eines Auftrags Verbrennung MapReduce bereitstellen und die Stapelverarbeitung Hadoop auf HDInsight Cluster verwenden."
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
 ms.date="10/18/2016"
 ms.author="larryfr"/>

# <a name="develop-scalding-mapreduce-jobs-with-apache-hadoop-on-hdinsight"></a>Verbrennung MapReduce Arbeitsplätze mit Apache Hadoop auf HDInsight

Verbrennung ist eine Scala-Bibliothek, die Hadoop MapReduce Arbeitsplätze erleichtert. Es bietet eine kurze Syntax sowie die enge Integration mit Scala.

Erfahren Sie in diesem Dokument, wie mit Maven einen grundlegenden Word Count MapReduce Auftrag in Scalding geschrieben. Sie erfahren dann bereitstellen und Ausführen auf einem HDInsight-Cluster.

## <a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Ein Windows- oder Linux-basierten Hadoop auf HDInsight Cluster**. Weitere Informationen finden Sie unter [Bereitstellung von Linux-basierten Hadoop auf HDInsight](hdinsight-hadoop-provision-linux-clusters.md) oder [Bereitstellung Windows-basierten Hadoop auf HDInsight](hdinsight-provision-clusters.md) .

* **[Maven](http://maven.apache.org/)**

* **[Java-Plattform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 oder höher**

## <a name="create-and-build-the-project"></a>Erstellen Sie das Projekt

1. Verwenden Sie den folgenden Befehl zum Erstellen eines neuen Projekts Maven:

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    Mit diesem Befehl erstellen Sie ein neues Verzeichnis mit dem Namen **Scaldingwordcount**und Gerüstbau für eine Scala-Anwendung erstellen.

2. Im Verzeichnis **Scaldingwordcount** **pom.xml** Datei, und Ersetzen Sie den Inhalt durch Folgendes:

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <groupId>com.microsoft.example</groupId>
            <artifactId>scaldingwordcount</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>${project.artifactId}</name>
            <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
            </properties>
            <repositories>
            <repository>
                <id>conjars</id>
                <url>http://conjars.org/repo</url>
            </repository>
            <repository>
                <id>maven-central</id>
                <url>http://repo1.maven.org/maven2</url>
            </repository>
            </repositories>
            <dependencies>
            <dependency>
                <groupId>com.twitter</groupId>
                <artifactId>scalding-core_2.11</artifactId>
                <version>0.13.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-core</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
            </dependencies>
            <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
                <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                    <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                    </execution>
                </executions>
                </plugin>
                <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                    </transformers>
                    <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                        <exclude>META-INF/*.SF</exclude>
                        <exclude>META-INF/*.DSA</exclude>
                        <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                        </transformers>
                    </configuration>
                    </execution>
                </executions>
                </plugin>
            </plugins>
            </build>
        </project>

    Diese Datei beschreibt das Projekt Abhängigkeiten und Plugins. Hier sind wichtig:

    * **maven.Compiler.Source** und **maven.compiler.target**: Legt die Java-Version für dieses Projekt

    * **Repositories**: Repositories, die von dem Projekt verwendete Abhängigkeitsdateien enthalten

    * **Verbrennung core_2.11** und **Hadoop-Core**: dieses Projekt hängt von Scalding und Hadoop Core-Pakete

    * **Maven-Scala-Plugin**: Plugin Scala Anwendung kompilieren

    * **Maven-Schatten-Plugin**: Plugin erstellen schattiert (fett) Gläser. Dieses Plugin gilt Filter und Transformationen. Form:

        * **Filter**: Filter JAR-Datei enthaltene Meta-Informationen ändern. Zu signieren Ausnahmen zur Laufzeit verschiedene Dateien, die mit Abhängigkeiten ausgeschlossen.

        * **Ausführung**: die Paketkonfiguration Phase Ausführung gibt die Klasse **com.twitter.scalding.Tool** als die Hauptklasse für das Paket. Ohne diese müssten Sie com.twitter.scalding.Tool sowie die Klasse, die die Anwendungslogik enthält, wenn den Auftrag mit dem Befehl Hadoop angeben.

3. Löschen Sie das Verzeichnis **Src-Test** , wie Sie keine Tests mit diesem Beispiel erstellen.

4. Öffnen Sie die Datei **src/main/scala/com/microsoft/example/App.scala** , und Ersetzen Sie den Inhalt durch Folgendes:

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
            // 1. Read lines from the specified input location
            // 2. Extract individual words from each line
            // 3. Group words and count them
            // 4. Write output to the specified output location
            TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

            //Tokenizer to split sentance into words
            def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
            }
        }

    Implementiert einen grundlegenden Word Count Auftrag.

5. Speichern Sie und schließen Sie die Dateien.

6. Verwenden Sie den folgenden Befehl aus dem Verzeichnis **Scaldingwordcount** zum Erstellen und verpacken die Anwendung:

        mvn package

    Nach Abschluss dieses Auftrags kann das Paket mit WordCount-Anwendung am **target/scaldingwordcount-1.0-SNAPSHOT.jar**gefunden.

## <a name="run-the-job-on-a-linux-based-cluster"></a>Die Stapelverarbeitung auf einem Linux-basierten cluster

> [AZURE.NOTE] Die folgenden Schritte verwenden SSH und Hadoop-Befehl. Andere Methoden MapReduce-Jobs ausführen finden Sie unter [Verwenden MapReduce in Hadoop auf HDInsight](hdinsight-use-mapreduce.md).

1. Verwenden Sie den folgenden Befehl zum Hochladen des Pakets auf HDInsight Cluster:

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    Dies kopiert die Dateien aus dem lokalen System auf dem Head-Knoten.

    > [AZURE.NOTE] Wenn Sie ein Kennwort zum Schützen von SSH-Konto verwendet, werden Sie aufgefordert, das Kennwort anzugeben. Verwendet einen SSH-Schlüssel möglicherweise mit den `-i` Parameter und den Pfad zum privaten Schlüssel. Zum Beispiel`scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.`

2. Verwenden Sie den folgenden Befehl Verbindung mit dem Cluster-Head-Knoten:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Wenn Sie ein Kennwort zum Schützen von SSH-Konto verwendet, werden Sie aufgefordert, das Kennwort anzugeben. Verwendet einen SSH-Schlüssel möglicherweise mit den `-i` Parameter und den Pfad zum privaten Schlüssel. Zum Beispiel`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`

3. Nachdem mit dem Head-Knoten verwenden Sie den folgenden Befehl zum Ausführen des Auftrags Word count

        yarn jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasbs:///example/data/gutenberg/davinci.txt --output wasbs:///example/wordcountout

    Dies führt die WordCount-Klasse bereits implementiert. `--hdfs`weist die Aufgabe, bietet. `--input`Gibt die Eingabetextdatei während `--output` gibt den Lagerplatz an.

4. Nach dem Beenden des Auftrags anhand der folgenden Ausgabe angezeigt.

        hdfs dfs -text wasbs:///example/wordcountout/*

    Dadurch werden Informationen ähnlich der folgenden angezeigt:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="run-the-job-on-a-windows-based-cluster"></a>Die Stapelverarbeitung auf einem Windows-basierten cluster

Die folgenden Schritte verwenden Windows PowerShell. Andere Methoden MapReduce-Jobs ausführen finden Sie unter [Verwenden MapReduce in Hadoop auf HDInsight](hdinsight-use-mapreduce.md).

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

2. Starten Sie Azure PowerShell und melden in Azure-Konto. Nach Anmeldeinformationen, gibt der Befehl Informationen zu Ihrem Konto.

        Add-AzureRMAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...

3. Wenn Sie mehrere Abonnements, bieten Sie die Abonnement-Id für die Bereitstellung verwenden möchten.

        Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] Sie können `Get-AzureRMSubscription` eine Liste aller Abonnements, die Ihr Konto enthält die Abonnement-Id für jede zugeordnet.

4. Verwenden Sie das folgende Skript hochladen und die Stapelverarbeitung WordCount. Ersetzen Sie `CLUSTERNAME` mit dem Namen der HDInsight cluster und sicherstellen, dass `$fileToUpload` ist der Pfad zur Datei __Scaldingwordcount 1.0 SNAPSHOT.jar__ .

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = Read-Host -Prompt "Enter the HDInsight cluster name"
        $fileToUpload = Read-Host -Prompt "Enter the path to the scaldingwordcount-1.0-SNAPSHOT.jar file"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential -Message "Enter the login credentials for the cluster"
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasbs:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                        "--input", `
                        "wasbs:///example/data/gutenberg/davinci.txt", `
                        "--output", `
                        "wasbs:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: $job.JobId"
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     Beim Ausführen des Skripts werden Sie aufgefordert, den Administrator-Benutzernamen und das Kennwort für Ihren HDInsight eingeben. Fehler, die auftreten, während der Auftrag ausgeführt wird, werden an der Konsole angemeldet.
     
6. Sobald der Auftrag abgeschlossen ist, wird die Ausgabe der Datei __output.txt__ im aktuellen Verzeichnis heruntergeladen werden. Verwenden Sie den folgenden Befehl, um die Ergebnisse anzuzeigen.

        cat output.txt

    Die Datei sollte ähnlich der folgenden Werte enthalten:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="next-steps"></a>Nächste Schritte

Jetzt verwenden Sie mit Scalding MapReduce Aufträge für HDInsight gelernt haben, die folgenden Links zu anderen Methoden zum Arbeiten mit Azure HDInsight.

* [Struktur mit HDInsight verwenden](hdinsight-use-hive.md)

* [Verwenden Sie Schwein mit HDInsight](hdinsight-use-pig.md)

* [Verwenden Sie MapReduce Aufträge mit HDInsight](hdinsight-use-mapreduce.md)
