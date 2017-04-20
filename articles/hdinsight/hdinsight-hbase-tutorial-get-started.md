<properties
    pageTitle="HBase-Lernprogramm: Erste Schritte mit HBase in Hadoop | Microsoft Azure"
    description="Folgen Sie dieser Anleitung HBase Schritte mit Apache HBase Hadoop in HDInsight. Erstellen von Tabellen aus der Shell HBase und Abfragen können mit Struktur."
    keywords="Apache Hbase, Hbase, Hbase-Shell Hbase-Lernprogramm"
    services="hdinsight"
    documentationCenter=""
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



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-windows-based-hadoop-in-hdinsight"></a>HBase-Lernprogramm: Erste Schritte mit Windows-basierten Hadoop in HDInsight Apache HBase

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Informationen Sie zum Erstellen HBase Cluster in HDInsight, HBase Tabellen und Abfragetabellen mit Apache Struktur. HBase Informationen finden Sie unter [Übersicht HDInsight HBase][hdinsight-hbase-overview].

Die Informationen in diesem Dokument ist spezifisch für Windows-basierte HDInsight-Cluster. Informationen zu Windows-basierten Clustern mithilfe der Tabstoppauswahl oben auf der Seite.

> [AZURE.NOTE] HBase (Version 0.98.0) für Windows-basierte HDInsight ist nur zur Verwendung mit HDInsight 3.1 (basierend auf Apache Hadoop und GARN 2.4.0). Informationen zur Version finden Sie unter [neuen Hadoop Cluster Versionen von HDInsight bereitgestellten?][hdinsight-versions]

## <a name="before-you-begin"></a>Bevor Sie beginnen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Vor diesem Lernprogramm HBase benötigen Sie Folgendes:

- **Ein Microsoft Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Eine Arbeitsstation** mit Visual Studio 2013 oder höher: Anleitung finden Sie in [Visual Studio installieren](http://msdn.microsoft.com/library/e2h7fzkw.aspx).

### <a name="access-control-requirements"></a>Steuerelement erforderlich

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>HBase Cluster erstellen

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**Um ein HBase Cluster mit Azure-Portal erstellen**

1. [Azure-Portal]anmelden[azure-management-portal].
2. Klicken Sie auf **neu** oder **+** in der oberen linken Ecke, und klicken Sie auf **Daten + Analysen**, **HDInsight**.
3. Geben Sie die folgenden Werte:

    - **Cluster-Name** – Geben Sie einen Namen für diesen Cluster.
    - **Clustertyp** auswählen **HBase**.
    - **Cluster-Betriebssystem** - **Windows**auswählen.  Zum Erstellen von Linux-basierten HBase cluster, finden Sie unter [HBase Lernprogramm: Erste Schritte mit Apache HBase Hadoop in HDInsight (Linux)](hdinsight-hbase-tutorial-get-started-linux.md).
    - **Version** - wählen Sie eine HBase-Version.
    - **Abonnement** - wählen Sie Ihre Azure Abonnements für diesen Cluster erstellen.
    - **Ressourcengruppe** - neue Azure Ressourcengruppe erstellen oder eine vorhandene auswählen. Weitere Informationen finden Sie unter [Azure-Ressourcen-Manager (Übersicht)](azure-resource-manager/resource-group-overview.md)
    - **Anmeldeinformationen** - kann für Windows-basierten Cluster ein clusterbenutzer (Benutzer bzw. HTTP, HTTP-Web-Service-Benutzer) und Remote-Benutzer erstellen. Klicken Sie auf **Remotedesktop aktivieren** die desktop-Anmeldeinformationen hinzufügen. Im nächste Abschnitt muss RDP.
    - **Datenquelle** - Erstellen eines neuen Kontos Azure-Speicher oder wählen Sie ein vorhandenes Azure Storage-Konto als Standard-Dateisystem für den Cluster verwendet werden. Konto Standardspeicherort bestimmt die Position des Cluster-Speicherort. Das Standardkonto für Speicher und dem Cluster müssen im gleichen Rechenzentrum Grenzschutz.
    - **Knoten Preise Ebenen** - wählen Sie die Anzahl der Server für den Cluster HBase region

        > [AZURE.WARNING] Für hohe Verfügbarkeit HBase Services legen Sie einen Cluster mit mindestens **drei** Knoten. Dies gewährleistet, dass ein Knoten ausfällt, HBase Datenbereiche auf anderen Knoten verfügbar sind.

        > Wenn Sie HBase lernen, immer wählen Sie 1 für die Clustergröße und löschen Sie Cluster nach jeder Verwendung die Kosten zu senken.

    - **Optionale Konfiguration** - Azure konfigurieren virtuelles Netzwerk konfigurieren Skriptaktionen und zusätzlichen Speicherkonten hinzufügen.

4. Klicken Sie auf **Erstellen**.

>[AZURE.NOTE] Nach dem Löschen eines Clusters HBase können Sie einen anderen HBase Cluster mit demselben Speicher-Standardkonto und Standardcontainer Blob erstellen. HBase Tabellen holen in der ursprünglichen Cluster erstellten neue Cluster. Um Inkonsistenzen zu vermeiden, wird empfohlen, HBase Tabellen deaktivieren, bevor Sie den Cluster löschen.

## <a name="create-tables-and-insert-data"></a>Tabellen erstellen und Einfügen von Daten

Derzeit gibt es zwei-Wege-HBase auf. Dieser Abschnitt behandelt die HBase Shell. Der nächste Abschnitt behandelt mit .NET SDK.

Für die meisten Daten im Tabellenformat angezeigt:

![Hdinsight Hbase-Tabellendaten][img-hbase-sample-data-tabular]

In HBase ist eine Implementierung der BigTable sieht die gleichen Daten wie:

![Hdinsight Hbase Bigtable Daten][img-hbase-sample-data-bigtable]

Es werden mehr Sinn nach Abschluss die nächste Prozedur.  

**Die HBase-Verwaltungsshell**

1. Verwenden Sie RDP Verbindung zum Cluster HBase in HDInsight. Hinweise RDP finden Sie unter [Verwalten Hadoop Cluster in Azure-Portal mit HDInsight][hdinsight-manage-portal].
2. Die RDP-Sitzung klicken Sie auf die **Befehlszeile Hadoop** Verknüpfung auf dem Desktop befindet.
3. Öffnen Sie die HBase-Shell:

        cd %HBASE_HOME%\bin
        hbase shell

4. Erstellen Sie ein HBase mit zwei Spalten:

        create 'Contacts', 'Personal', 'Office'
        list
5. Fügen Sie Daten:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![Hdinsight Hadoop Hbase Schale][img-hbase-shell]

6. Abrufen einer Zeile

        get 'Contacts', '1000'

    Sie sehen die gleichen Ergebnisse als Befehl Scan verwenden, da nur eine Zeile vorhanden ist.

    Weitere Informationen über das Tabellenschema Hbase finden Sie unter [Introduction to HBase Schema Design][hbase-schema]. Mehr HBase Befehle finden Sie im [Referenzhandbuch Apache HBase][hbase-quick-start].


6. Beenden der shell

        exit

**Massenkopieren von Daten in HBase-Tabelle contacts**

HBase bietet mehrere Methoden zum Laden von Daten in Tabellen. Weitere Informationen finden Sie unter [Bulk laden](http://hbase.apache.org/book.html#arch.bulk.load).


Ein Blob für den öffentlichen Container eine Beispieldatendatei hochgeladen wurde wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt. Der Inhalt der Datei ist:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

Sie können eine Datei erstellen und Laden Sie die Datei auf Ihr Speicherkonto soll. Die Anleitung finden Sie unter [Daten Hadoop Aufträge in HDInsight][hdinsight-upload-data].

> [AZURE.NOTE] Dieses Verfahren verwendet die Kontakte HBase-Tabelle in der letzten Prozedur erstellten.

1. Die RDP-Sitzung klicken Sie auf die **Befehlszeile Hadoop** Verknüpfung auf dem Desktop befindet.
2. Verzeichnis ändern:

        cd %HBASE_HOME%\bin

3. Führen Sie den folgenden Befehl zum Transformieren der Datendatei bei ein relativer Pfad von Dimporttsv.bulk.output zu StoreFiles:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Führen Sie den folgenden Befehl zum Hochladen von Daten aus /example/data/storeDataFileOutput in HBase-Tabelle:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Sie können HBase-Shell öffnen und den Befehl Scan den Inhalt der Liste.



## <a name="use-hive-to-query-hbase-tables"></a>Mit der Struktur HBase Abfragetabellen

Sie können Daten in HBase Struktur mit Abfragen. In diesem Abschnitt erstellt eine Struktur-Tabelle, die ordnet der HBase-Tabelle und verwendet die Daten in der HBase-Tabelle Abfragen.

**Cluster-Dashboard öffnen**

1. Navigieren Sie zu **https://<HDInsight Cluster Name>.azurehdinsight.net/**.
5. Hadoop Kontobenutzernamen und Kennwort eingeben Der Standard-Benutzername ist **Admin** und das Kennwort wird während des Erstellungsprozesses eingegeben haben. Eine neue Registerkarte wird geöffnet.
6. Klicken Sie auf **Struktur Editor** am oberen Rand der Seite. Der Struktur sieht folgendermaßen aus:

    ![HDInsight Cluster-Dashboard.][img-hdinsight-hbase-hive-editor]

**Struktur Abfragen ausführen**

1. Geben Sie das folgende Skript HiveQL in Hive-Editor, und klicken Sie auf **Senden** , um eine Struktur erstellen, die die HBase-Tabelle zugeordnet. Stellen Sie sicher, dass Sie die Beispieltabelle, die zuvor in diesem Lernprogramm mithilfe der Shell HBase vor dem Ausführen dieser Anweisung referenziert.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

    Warten Sie, bis die **Status** -Updates **abgeschlossen**.

2. Geben Sie das folgende Skript HiveQL in Hive-Editor, und klicken Sie auf **Senden**. Die Hive-Abfrage fragt die Daten in HBase-Tabelle:

        SELECT count(*) FROM hbasecontacts;

4. Rufen Sie die Ergebnisse der Abfrage Struktur klicken Sie **Einzelheiten** im **Auftrag** Sitzungsfenster Wenn der Auftrag abgeschlossen ist. Da die HBase-Tabelle einen Datensatz abgelegt werden nur ein Auftrag Ausgabedatei vor.




**Die Ausgabedatei durchsuchen**

1. Klicken Sie in der Konsole Abfrage auf **Datei-Browser**.
2. Klicken Sie auf den Azure-Speicher, der als Standard-Dateisystem HBase-Cluster verwendet wird.
3. Klicken Sie auf den Clusternamen HBase. Standardcontainer Azure Storage-Konto verwendet den Namen des Clusters.
4. Klicken Sie auf **Benutzer**, und klicken Sie auf **Administration**. (Dies ist der Benutzername Hadoop.)
6. Klicken Sie auf Auftrag, mit der **Letzten Änderung** die Zeit entspricht bei der Ausführung der Abfrage wählen Sie Struktur.
4. Klicken Sie auf **Stdout**. Speichern Sie die Datei, und öffnen Sie die Datei mit Notepad. Es ist eine Ausgabedatei.

    ![HDInsight HBase Struktur Editor Dateibrowser][img-hdinsight-hbase-file-browser]

## <a name="use-the-net-hbase-rest-api-client-library"></a>.NET HBase REST API-Clientbibliothek verwenden

Die Clientbibliothek HBase REST-API für .NET von GitHub herunterladen, und erstellen Sie das Projekt, sodass HBase .NET SDK verwenden zu können. Das folgende Verfahren enthält Hinweise dazu.

1. Erstellen Sie eine neue C# Visual Studio Windows Desktop.
2. Öffnen Sie die Konsole NuGet Paket-Manager **auf** > **NuGet Paket-Manager** > **Paket-Manager-Konsole**.
3. Führen Sie den folgenden NuGet-Befehl in der Konsole:

        Install-Package Microsoft.HBase.Client

5. Fügen Sie am Anfang der Datei Folgendes **verwenden** :

        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

6. Ersetzen Sie die **Main** -Funktion durch Folgendes:

        static void Main(string[] args)
        {
            string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
            string hadoopUsername= "<yourHadoopUsername>";
            string hadoopUserPassword = "<yourHadoopUserPassword>";

            string hbaseTableName = "sampleHbaseTable";

            // Create a new instance of an HBase client.
            ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
            HBaseClient hbaseClient = new HBaseClient(creds);

            // Retrieve the cluster version.
            var version = hbaseClient.GetVersion();
            Console.WriteLine("The HBase cluster version is " + version);

            // Create a new HBase table.
            TableSchema testTableSchema = new TableSchema();
            testTableSchema.name = hbaseTableName;
            testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
            testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
            hbaseClient.CreateTable(testTableSchema);

            // Insert data into the HBase table.
            string testKey = "content";
            string testValue = "the force is strong in this column";
            CellSet cellSet = new CellSet();
            CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
            cellSet.rows.Add(cellSetRow);

            Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
            cellSetRow.values.Add(value);
            hbaseClient.StoreCells(hbaseTableName, cellSet);

            // Retrieve a cell by its key.
            cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
            Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
            // with the previous insert, it should yield: "the force is strong in this column"

            //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
            Scanner scanSettings = new Scanner()
            {
                batch = 10,
                startRow = BitConverter.GetBytes(25),
                endRow = BitConverter.GetBytes(35)
            };

            ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
            CellSet next = null;
            Console.WriteLine("Scan results");

            while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
            {
                foreach (CellSet.Row row in next.rows)
                {
                    Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
                }
            }

            Console.WriteLine("Press ENTER to continue ...");
            Console.ReadLine();
        }

7. Die ersten drei Variablen in der **Main** -Funktion festgelegt.
8. Drücken Sie **F5** , um die Anwendung auszuführen.

## <a name="check-cluster-status"></a>Überprüfen des Status eines Clusters

HBase in HDInsight wird mit einer Web-Benutzeroberfläche für die Überwachung von Clustern. Der Web-Benutzeroberfläche können Sie Statistiken oder Informationen zu beantragen.

Öffnen Sie die Webbenutzeroberfläche Sie RDP im Cluster müssen und dann klicken Sie auf HMaster Info-Webbenutzeroberfläche Verknüpfung auf dem Desktop oder verwenden Sie den folgenden URL in einem Webbrowser:

    http://zookeeper[0-2]:60010/master-status

In einem Cluster mit hoher Verfügbarkeit finden Sie einen Link zum aktuellen aktiven HBase master Knoten, die der Web-Benutzeroberfläche befindet.

##<a name="delete-the-cluster"></a>Cluster löschen
Um Inkonsistenzen zu vermeiden, wird empfohlen, HBase Tabellen deaktivieren, bevor Sie den Cluster löschen.
[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="whats-next"></a>Was kommt als nächstes?
In diesem Lernprogramm HBase HDInsight haben Sie einen HBase-Cluster erstellen und zum Erstellen von Tabellen und Anzeigen der Daten in diesen Tabellen aus der Shell HBase. Sie haben auch gelernt, wie eine Struktur Abfrage Daten in Tabellen HBase und Verwendung der HBase C# REST-APIs HBase Tabelle erstellen und Abrufen von Daten aus der Tabelle verwenden.

Weitere Informationen finden Sie unter:

- [Übersicht über die HDInsight HBase][hdinsight-hbase-overview].
HBase ist eine Apache Open-Source NoSQL-Datenbank basierend auf Hadoop, das RAM und starke Konsistenz für große Mengen von unstrukturierten und teilstrukturierte Daten bereitstellt.
- [HBase Cluster virtuellen Netzwerk Azure erstellen][hdinsight-hbase-provision-vnet].
Integration der virtuellen Netzwerk können HBase Cluster mit demselben virtuellen Netzwerk als Anwendung bereitgestellt, sodass Applikationen HBase direkt kommunizieren können.
- [Replikation in HDInsight HBase konfigurieren](hdinsight-hbase-geo-replication.md). Informationen Sie zum Konfigurieren der Replikation HBase über zwei Azure-Datencentern.
- [Analysieren von Twitter Stimmung mit HBase in HDInsight][hbase-twitter-sentiment].
Lernen Sie in Echtzeit [stimmungsanalyse](http://en.wikipedia.org/wiki/Sentiment_analysis) großer Datenmengen mit HBase in einem Cluster Hadoop in HDInsight.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png
