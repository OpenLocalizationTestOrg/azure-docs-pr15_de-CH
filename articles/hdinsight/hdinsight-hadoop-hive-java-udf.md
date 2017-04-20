<properties
pageTitle="Verwenden Sie eine Java-benutzerdefinierte Funktion (UDF) mit Struktur in HDInsight | Microsoft Azure"
description="Informationen Sie zum Erstellen eine Java-benutzerdefinierte Funktion (UDF) aus und Struktur in HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/27/2016"
ms.author="larryfr"/>

#<a name="use-a-java-udf-with-hive-in-hdinsight"></a>Verwenden Sie ein Java UDF mit Struktur in HDInsight

Struktur eignet sich für die Arbeit mit Daten in HDInsight, aber manchmal brauchen Sie allgemeinen Zweck Sprache. Struktur können Sie benutzerdefinierte Funktion (UDF) mit verschiedenen Programmiersprachen erstellen. In diesem Dokument erfahren Sie, wie mit einer UDF Java aus Struktur.

## <a name="requirements"></a>Vorschriften

* Ein Azure-Abonnement

* Ein HDInsight-Cluster (Windows oder Linux-basierten)

    > [AZURE.NOTE] Die meisten Schritte in diesem Dokument werden beide Cluster bearbeiten. Allerdings sind die Schritte zum Hochladen des kompilierten UDFs zum Cluster und ausführen für Linux-basierten Clustern. Links werden Informationen bereitgestellt, die mit Windows-basierten Clustern verwendet werden kann.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 oder höher (oder eine entsprechende wie OpenJDK)

* [Apache Maven](http://maven.apache.org/)

* Ein Text-Editor oder Java IDE

    > [AZURE.IMPORTANT] Verwenden Sie einen HDInsight Linux-basierte Server jedoch die Python-Dateien auf einem Windows-Client erstellen, müssen Sie einen Editor verwenden verwendet, LF als ein Zeilenendetyp. Wenn Sie nicht sicher sind, ob der Editor LF oder CRLF verwendet, finden Sie im Abschnitt [Problembehandlung](#troubleshooting) Schritte zum Entfernen von HDInsight-Cluster über Dienstprogramme CR-Zeichen.

## <a name="create-an-example-udf"></a>Erstellen Sie ein UDF-Beispiel

1. Verwenden Sie eine Befehlszeile folgende zum Erstellen eines neuen Projekts Maven:

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    > [AZURE.NOTE] Wenn Sie PowerShell verwenden, müssen Sie die Parameter in Anführungszeichen setzen. Z. B. `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Dies erstellt ein neues Verzeichnis mit dem Namen __Exampleudf__, das Maven-Projekt enthalten.

2. Nachdem das Projekt erstellt wurde, löschen Sie im Rahmen des Projekts erstellten __Exampleudf/Src/Test__ -Verzeichnis; Es wird in diesem Beispiel nicht verwendet.

3. Öffnen Sie __exampleudf/pom.xml__, und Ersetzen Sie die vorhandene `<dependencies>` mit den folgenden Eintrag:

        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

    Diese Einträge geben die Version Hadoop und Struktur mit HDInsight 3.3 und 3.4 enthalten. Finden Sie Informationen über die Versionen von Hadoop und Struktur mit HDInsight aus dem [HDInsight Komponente Versioning](hdinsight-component-versioning.md) Dokument bereitgestellt.

    Hinzufügen einer `<build>` Abschnitt vor der `</project>` am Ende der Datei. Dieser Abschnitt enthält:

        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                            </transformer>
                        </transformers>
                        <!-- Keep us from getting a bad signature error -->
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
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
    
    Diese Einträge definieren, wie das Projekt erstellt. Insbesondere die Version vom Projekt verwendeten Java und Uberjar für die Bereitstellung auf dem Cluster erstellen.

    Speichern Sie die Datei, sobald die Änderungen vorgenommen wurden.

4. __Exampleudf/src/main/java/com/microsoft/examples/App.java__ in __ExampleUDF.java__umbenennen und die Datei dann im Editor zu öffnen.

5. Ersetzen Sie den Inhalt der Datei __ExampleUDF.java__ folgende dann speichern.

        package com.microsoft.examples;

        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;

        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }

    Implementiert eine UDF-Datei, die einen Zeichenfolgenwert und gibt die Zeichenfolge in Kleinbuchstaben.

## <a name="build-and-install-the-udf"></a>Erstellen und Installieren der UDF-Datei

1. Verwenden Sie folgenden Befehl kompilieren und das UDF-Paket:

        mvn compile package

    Dies wird erstellen und Verpacken das UDF in __exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar__.

2. Verwenden der `scp` Befehl, um die Datei mit dem HDInsight-Cluster.

        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight

    SSH-Benutzerkonto für den Cluster ersetzen Sie __Myuser__ . Der Clustername ersetzen Sie __MeinCluster__ . Wenn Sie ein Kennwort zum Schützen von SSH-Konto verwendet, werden Sie aufgefordert, das Kennwort einzugeben. Wenn ein Zertifikat verwendet, müssen mit der `-i` Parameter, um die private Schlüsseldatei.

3. Verbinden Sie mit SSH. 

        ssh myuser@mycluster-ssh.azurehdinsight.net

    Weitere Informationen über SSH mit HDInsight finden Sie in folgenden Dokumenten.

    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix und Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

4. Über SSH-Sitzung wird kopieren Sie die JAR-Datei HDInsight Speicher.

        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>Verwenden Sie das UDF aus Struktur

1. Anhand der folgenden um Luftlinie Client über SSH-Sitzung zu starten.

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Dieser Befehl setzt voraus, dass der Standardwert __Admin__ für das Anmeldekonto für den Cluster verwendet.

2. Nach in Ankunft der `jdbc:hive2://localhost:10001/>` aufgefordert, Folgendes UDF Hive und als eine Funktion verfügbar zu machen.

        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';

3. Verwenden Sie das UDF Werte aus einer Tabelle in Kleinbuchstaben Zeichenfolgen konvertieren.

        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;

    Dadurch wird die Geräteplattform (Android, Windows, iOS, etc.) aus der Tabelle auswählen in Kleinbuchstaben konvertiert und angezeigt werden. Die Ausgabe ist ähnlich der folgenden angezeigt.

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Nächste Schritte

Andere Methoden zum Arbeiten mit Struktur finden Sie unter [Struktur verwenden mit HDInsight](hdinsight-use-hive.md).

Weitere Hive User-Defined Funktionen, [Struktur und benutzerdefinierten Funktionen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) Siehe Wiki Struktur am apache.org.
