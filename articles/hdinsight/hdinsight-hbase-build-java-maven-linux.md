<properties
    pageTitle="Maven mit Java HBase Anwendung erstellen und dann auf Linux-basierten HDInsight | Microsoft Azure"
    description="Dazu verwenden Sie Apache Maven um eine Apache HBase Java-basierte Anwendung auf Linux-basierten HDInsight in der Azure-Cloud bereitstellen."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-linux-based-hdinsight-hadoop"></a>Mit der Java-Anwendung erstellen, die auf Linux basierende HDInsight (Hadoop) HBase mit Maven

Informationen Sie zum Erstellen und [Apache HBase](http://hbase.apache.org/) Anwendung in Java mit Apache Maven erstellen. Verwenden Sie die Anwendung dann mit einer Linux-basierten HDInsight.

[Maven](http://maven.apache.org/) ist eine Software-Projektmanagement und Verständnis Tool, Software, Dokumentation und Berichte für Java-Projekte erstellen können. In diesem Artikel lernen, um eine grundlegende Java-Anwendung erstellen, erstellt, Abfragen verwenden und eine HBase-Tabelle auf einem Linux-basierten HDInsight Cluster löscht.

> [AZURE.NOTE] Die Schritte in diesem Dokument wird mit einem HDInsight Linux-basierten Cluster. Informationen über Windows-basierte HDInsight-Cluster finden Sie unter [Verwenden Maven Java-Anwendung erstellen, die HBase mit Windows-basierten HDInsight verwenden](hdinsight-hbase-build-java-maven.md)

##<a name="requirements"></a>Vorschriften

* [Java-Plattform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 oder höher

* [Maven](http://maven.apache.org/)

* [Ein Linux-basiertes Azure HDInsight Cluster mit HBase](../hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

    > [AZURE.NOTE] Die Schritte in diesem Dokument wurden mit HDInsight Cluster Versionen 3.2, 3.3 und 3.4 getestet. Die Standardwerte in den Beispielen sind für HDInsight 3.4 Cluster.

* **Vertrautheit mit SSH und SCP**. Weitere Informationen zur Verwendung von SSH und SCP mit HDInsight finden Sie unter:

    * **Linux, Unix oder OS X Clients**: Siehe [Verwendet SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-Clients**: Siehe [Verwendet SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

##<a name="create-the-project"></a>Erstellen Sie das Projekt

1. Über die Befehlszeile in Umgebung, wechseln Sie in den Speicherort zum Erstellen des Projekts `cd code/hdinsight`.

2. Verwenden Sie den Befehl __Mvn__ mit Maven installiert ist, das Gerüst für das Projekt generiert.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Dies erstellt ein neues Verzeichnis im aktuellen Verzeichnis mit dem Namen angegebene __ArtifactID__ Parameter (**Hbaseapp** in diesem Beispiel). Dieses Verzeichnis enthält die folgenden Elemente:

    * __pom.XML__: das Project-Objektmodell ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) enthält Informationen und Details zum Erstellen des Projekts verwendet.

    * __Src__: das Verzeichnis, das Verzeichnis __Main/Java/com/Microsoft/Beispiele__ enthält, Sie die Anwendung erstellen.

3. Löschen Sie die Datei __src/test/java/com/microsoft/examples/apptest.java__ , da sie in diesem Beispiel nicht verwendet werden.

##<a name="update-the-project-object-model"></a>Aktualisieren Sie Project-Objektmodell

1. Bearbeiten Sie die Datei __pom.xml__ , und fügen Sie folgenden Code in die `<dependencies>` Abschnitt:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Dies weist Maven, dass das Projekt __Hbase-Client__ Version __1.1.2__erfordert. Zur Kompilierzeit werden diese aus dem Standard Maven Repository heruntergeladen. [Maven zentralen Repository suchen](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) können Sie weitere Informationen über diese Abhängigkeit.

    > [AZURE.IMPORTANT] Die Versionsnummer muss die Version von HBase, die mit HDInsight Cluster bereitgestellt. Anhand der folgenden Tabelle finden Sie die richtige Versionsnummer.

  	| HDInsight Cluster-version | HBase-Version verwenden |
  	| ----- | ----- |
  	| 3.2 | 0.98.4-hadoop2 |
  	| 3.3 und 3.4 | 1.1.2 |

    Weitere Informationen zu Versionen von HDInsight und Komponenten finden Sie unter [verschiedenen Hadoop Komponenten mit HDInsight](hdinsight-component-versioning.md).

2. Wenn Sie ein HDInsight 3.3 oder 3.4 Cluster verwenden, müssen Sie auch Folgendes hinzufügen der `<dependencies>` Abschnitt:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Dadurch lädt Phoenix-Core Komponenten die Hbase Version 1.1.x.

2. Fügen Sie folgenden Code in die Datei __pom.xml__ . Muss in der `<project>...</project>` Tags in der Datei, z. B. zwischen `</dependencies>` und `</project>`.

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
            <resource>
              <directory>${basedir}/conf</directory>
              <filtering>false</filtering>
              <includes>
                <include>hbase-site.xml</include>
              </includes>
            </resource>
          </resources>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
                        <version>3.3</version>
              <configuration>
                <source>1.7</source>
                <target>1.7</target>
              </configuration>
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

    Dadurch wird eine Ressource (__Hbase/Conf-site.xml__,), die Konfigurationsinformationen für HBase konfiguriert.

    > [AZURE.NOTE] Sie können auch Werte über Code festlegen. Finden Sie in der __CreateTable__ Beispiel für die Vorgehensweise.

    Dadurch wird konfiguriert [Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) und [Maven Schatten Plugin](http://maven.apache.org/plugins/maven-shade-plugin/). Compiler-Plug-in wird zum Kompilieren der Topologie. Schatten-Plug-in wird zum Lizenz Verdoppelung in JAR-Paket zu verhindern, die von Maven erstellt. Der verwendet deshalb doppelte Lizenzdateien ein Fehler zur Laufzeit auf dem HDInsight-Cluster. Mit Maven-Schatten-Plugin mit der `ApacheLicenseResourceTransformer` Umsetzung dieser Fehler verhindert.

    Maven-Schatten-Plugin wird auch ein Uber Glas (oder fat Jar), enthält die Anwendung konfiguriert.

3. Speichern Sie die Datei __pom.xml__ .

4. Erstellen Sie ein neues Verzeichnis __Conf__ im Verzeichnis __Hbaseapp__ . Hiermit werden Konfigurationsinformationen für die Verbindung mit HBase.

5. Verwenden Sie den folgenden Befehl HBase Konfiguration vom Server HDInsight __Conf__ -Verzeichnis kopieren. **Benutzernamen** ersetzen die die SSH-Benutzername. Der Clustername HDInsight ersetzen Sie **CLUSTERNAME** :

        scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml

    > [AZURE.NOTE] Wenn Sie ein Kennwort für Ihr Konto SSH verwendet, werden Sie aufgefordert, das Kennwort einzugeben. SSH-Schlüssel mit dem Konto verwendet wird, müssen mit den `-i` Parameter, um den Pfad zur Schlüsseldatei. Das folgende Beispiel lädt den privaten Schlüssel aus `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml`

##<a name="create-the-application"></a>Erstellen der Anwendung

1. Wechseln Sie zum Verzeichnis __Hbaseapp/Src/Main/Java/com/Microsoft/Beispiele__ , und benennen Sie die Datei app.java in __CreateTable.java__.

2. Öffnen Sie die Datei __CreateTable.java__ , und Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            //
            //NOTE: Actual zookeeper host names can be found using Ambari:
            //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"
            
            //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
            config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    Dies ist die __CreateTable__ -Klasse, die Tabelle __Benutzer__ erstellen und füllen Sie es mit vordefinierten Benutzern.

3. Speichern Sie die Datei __CreateTable.java__ .

4. Erstellen Sie eine neue Datei namens __SearchByEmail.java__im Verzeichnis __Hbaseapp/Src/Main/Java/com/Microsoft/Beispiele__ . Verwenden Sie die folgenden Inhalt dieser Datei:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    __SearchByEmail__ -Klasse verwenden zum Abfragen von Zeilen nach e-Mail-Adresse. Da Filter regulären Ausdruck verwendet, können Sie eine Zeichenfolge oder ein regulärer Ausdruck die Klasse nutzen können.

5. Speichern Sie die Datei __SearchByEmail.java__ .

6. Erstellen Sie eine neue Datei namens __DeleteTable.java__im Verzeichnis __Hbaseapp/Src/Main/Hava/com/Microsoft/Beispiele__ . Verwenden Sie die folgenden Inhalt dieser Datei:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    Diese Klasse ist für dieses Beispiel deaktivieren und Löschen von der __CreateTable__ -Klasse erstellte Tabelle bereinigen.

7. Speichern Sie die Datei __DeleteTable.java__ .

##<a name="build-and-package-the-application"></a>Erstellen Sie und Verpacken Sie die Anwendung

2. Verwenden Sie den folgenden Befehl zu einer JAR-Datei, die die Anwendung enthält, aus dem Verzeichnis __Hbaseapp__ :

        mvn clean package

    Bereinigt alle vorherigen Buildartefakte, downloads Abhängigkeiten noch nicht installiert, dann erstellt und die Anwendung verpackt.

3. Bei Abschluss des Befehls enthält das __Hbaseapp-Ziel__ -Verzeichnis eine Datei namens __Hbaseapp-1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] Die Datei __Hbaseapp-1.0-SNAPSHOT.jar__ ist ein Uber Glas (fat JAR-Datei bezeichnet) enthält die zum Ausführen der Anwendung erforderliche Abhängigkeit.

##<a name="upload-the-jar-file-and-run-jobs"></a>Die JAR-Datei hochladen und Aufträgen

1. Anhand der folgenden HDInsight-Cluster die JAR-Datei hinzufügen. **Benutzernamen** ersetzen die die SSH-Benutzername. Der Clustername HDInsight ersetzen Sie **CLUSTERNAME** :

        scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Das Hochladen der Datei auf das Basisverzeichnis für das Benutzerkonto SSH.

    > [AZURE.NOTE] Wenn Sie ein Kennwort für Ihr Konto SSH verwendet, werden Sie aufgefordert, das Kennwort einzugeben. SSH-Schlüssel mit dem Konto verwendet wird, müssen mit den `-i` Parameter, um den Pfad zur Schlüsseldatei. Das folgende Beispiel lädt den privaten Schlüssel aus `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

2. SSH Verbindung HDInsight Cluster verwenden. **Benutzernamen** ersetzen die die SSH-Benutzername. Der Clustername HDInsight ersetzen Sie **CLUSTERNAME** :

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] Wenn Sie ein Kennwort für Ihr Konto SSH verwendet, werden Sie aufgefordert, das Kennwort einzugeben. SSH-Schlüssel mit dem Konto verwendet wird, müssen mit den `-i` Parameter, um den Pfad zur Schlüsseldatei. Das folgende Beispiel lädt den privaten Schlüssel aus `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Nachdem die Verbindung hergestellt ist, verwenden Sie folgende zum Erstellen einer neuen Anwendung Java HBase-Tabelle:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable

    Diese neue HBase Tabelle __Personen__erstellen und mit Daten zu füllen.

4. Anschließend anhand der folgenden e-Mail-Adressen in der Tabelle gespeicherte Suchen:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com

    Sie erhalten die folgenden Ergebnisse:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

##<a name="delete-the-table"></a>Löschen der Tabelle

Wenn Sie das Beispiel fertig, verwenden Sie folgenden Befehl Azure PowerShell-Sitzung verwendeten __Benutzer__ -Tabelle löschen:

    hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable

