<properties
pageTitle="DataFu Schwein auf HDInsight verwenden"
description="DataFu ist eine Sammlung von Bibliotheken mit Hadoop. Erfahren Sie DataFu Schwein auf HDInsight Cluster Verwendung."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="use-datafu-with-pig-on-hdinsight"></a>DataFu Schwein auf HDInsight verwenden

DataFu ist eine Sammlung von Open Source-Bibliotheken mit Hadoop. In diesem Dokument erfahren Sie, wie Sie DataFu auf dem HDInsight-Cluster und wie Schwein DataFu benutzerdefinierte Funktion (UDF) mit.

##<a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure-Abonnement.

* Ein Azure HDInsight Cluster (Linux oder Windows-basierte)

* Vertraut mit [Schwein auf HDInsight](hdinsight-use-pig.md)

##<a name="install-datafu-on-linux-based-hdinsight"></a>Installieren Sie DataFu auf Linux-basierten HDInsight

> [AZURE.NOTE] DataFu wird auf Linux-basierten Clustern Version 3.3 und Windows-basierten Clustern installiert. Es ist nicht vor 3.3 auf Linux-Cluster installiert.
>
> Wenn Sie eine Linux-basierten Cluster-Version 3.3 oder höher oder Windows-basierten Cluster verwenden, können Sie diesen Abschnitt überspringen.

DataFu kann heruntergeladen und Maven Repository installiert. Gehen Sie folgendermaßen vor, HDInsight Cluster DataFu hinzu:

1. Verbinden Sie mit HDInsight Linux-basierten Cluster über SSH. Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Dokumenten:

    * [SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X und Unix verwenden](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)
    
2. Verwenden Sie den folgenden Befehl DataFu JAR-Datei mit dem Dienstprogramm Wget herunterladen oder kopieren Sie und fügen Sie den Link in Ihren Browser mit dem Download beginnen.

        wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar

3. Laden Sie die Datei anschließend auf Standardspeicher für Ihren HDInsight. Dadurch wird die Datei verfügbar auf allen Knoten im Cluster und die Datei bleibt im Speicher, auch wenn Sie löschen und neu des Clusters erstellen.

        hdfs dfs -put datafu-1.2.0.jar /example/jars
    
    > [AZURE.NOTE] Im obige Beispiel speichert die JAR-Datei in `wasbs:///example/jars` da dieses Verzeichnis auf den Clusterspeicher bereits. Sie können überall auf HDInsight Clusterspeicher.

##<a name="use-datafu-with-pig"></a>DataFu Schwein verwenden

Die Schritte in diesem Abschnitt wird HDInsight Schwein mit kennen, und nur die Pig Latin-Anweisungen nicht die Schritte in Verbindung mit dem Cluster. Weitere Informationen zur Verwendung von Schwein mit HDInsight finden Sie unter [Verwenden Schwein mit HDInsight](hdinsight-use-pig.md).

> [AZURE.IMPORTANT] Wenn DataFu aus auf einem Linux-basierten HDInsight-Cluster verwenden, müssen Sie die JAR-Datei mit der folgenden Anweisung Pig Latin registrieren:
>
> ```register wasbs:///example/jars/datafu-1.2.0.jar```
>
> DataFu ist standardmäßig auf Windows-basierten HDInsight Cluster registriert.

Sie definieren in der Regel einen Alias für DataFu Funktionen. Zum Beispiel:

    DEFINE SHA datafu.pig.hash.SHA();
    
Definiert einen alias mit dem Namen `SHA` für die Hashfunktion SHA. Dann können diese in ein Schwein lateinischen einen Hash für die Eingabedaten generieren. Beispielsweise ersetzt die folgenden Namen in den Eingabedaten mit einem Hashwert:

    raw = LOAD '/data/raw/' USING PigStorage(',') AS  
        (name:chararray, 
        int1:int, 
        int2:int,
        int3:int); 
    mask = FOREACH raw GENERATE SHA(name), int1, int2, int3; 
    DUMP mask;

Mit der folgenden Eingabedaten verwendet:

    Lana Zemljaric,5,9,1
    Qiong Zhong,9,3,6
    Sandor Harsanyi,0,7,3
    Roko Petkovic,2,6,2
    Tibor Rozsa,8,0,0
    Lea Hrastovsek,6,3,6
    Regina Toth,2,1,2
    Eva Makay,8,9,2
    Shi Liao,4,6,0
    Tjasa Zemljaric,0,2,5
    
Es wird die folgende Ausgabe generiert:

    (c1a743b0f34d349cfc2ce00ef98369bdc3dba1565fec92b4159a9cd5de186347,5,9,1)
    (713d030d621ab69aa3737c8ea37a2c7c724a01cd0657a370e103d8cdecac6f99,9,3,6)
    (7a5f5abdd281f68168199319d98a1a662535f988d1443b3a3c497010937bac89,0,7,3)
    (a94818e93807e12079c4b35f8f3c8c8ef8e8acd1954e7f0476bc1a3a86fc96a9,2,6,2)
    (894ead4f48af91df7e088241218a23157bede7c52115272e417e95c046d48902,8,0,0)
    (6f99f163af3448fda672087db306f363e27a98a9e49c1f274a0860e303f8aec4,6,3,6)
    (a03de92a28be3c6a984c7a153fa9ed81c0413f76a9401955b5f7e04a5dd0ab9f,2,1,2)
    (6ceab977c8fb48d9ad0dc413e6bc646cabd89f22e7ab97a6b0133f3d225c6013,8,9,2)
    (fa9c436469096ff1bd297e182831f460501b826272ae97e921f5f6e3f54747e8,4,6,0)
    (bc22db7c238b86c37af79a62c78f61a304b35143f6087eb99c34040325865654,0,2,5)

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu DataFu oder Schwein finden Sie in folgenden Dokumenten:

* [Apache DataFu Schwein Guide](http://datafu.incubator.apache.org/docs/datafu/guide.html).

* [Verwenden Sie Schwein mit HDInsight](hdinsight-use-pig.md)
