<properties
    pageTitle="HBase-Lernprogramm: Erste Schritte mit Linux-basierten HBase in Hadoop | Microsoft Azure"
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
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-linux-based-hadoop-in-hdinsight"></a>HBase-Lernprogramm: Erste Schritte mit Apache HBase Linux-basierten Hadoop in HDInsight 

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Informationen Sie zum Erstellen eines Clusters HBase in HDInsight, HBase Tabellen und Struktur mit Tabellen Abfragen. HBase Informationen finden Sie unter [Übersicht HDInsight HBase][hdinsight-hbase-overview].

Die Informationen in diesem Dokument ist für Linux-basierte HDInsight-Cluster. Informationen zu Windows-basierten Clustern mithilfe der Tabstoppauswahl oben auf der Seite.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="prerequisites"></a>Erforderliche Komponenten

Vor diesem Lernprogramm HBase benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- [Sichere Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
- [Aufrollen](http://curl.haxx.se/download.html).

### <a name="access-control-requirements"></a>Steuerelement erforderlich

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>HBase Cluster erstellen

Die folgende Prozedur verwendet eine Azure-Ressourcen-Manager-Vorlage eine Version 3.4 HBase Linux-basierten Cluster und die abhängigen Azure Storage-Konto erstellen. Parameter in der Prozedur und anderen Cluster Methoden finden Sie unter [Erstellen von Linux-basierten Hadoop Cluster in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Klicken Sie zum Öffnen der Vorlage im Azure-Portal auf folgende. Die Vorlage befindet sich in einem öffentlichen Blob-Container. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. **Benutzerdefinierte Bereitstellung** Blatt geben Sie Folgendes ein:

    - **Abonnement**: Wählen Sie Ihre Azure-Abonnement, die zum Erstellen des Clusters verwendet.
    - **Ressourcengruppe**: neue Azure-Ressourcenmanagement-Gruppe erstellen oder eine vorhandene verwenden.
    - **Speicherort**: Geben Sie den Speicherort der Ressourcengruppe. 
    - **ClusterName**: Geben Sie einen Namen für den HBase-Cluster, die Sie erstellen.
    - **Cluster-Benutzername und Kennwort**: der Standard-Anmeldename ist **Admin**.
    - **SSH-Benutzername und Kennwort**: der Standard-Benutzername ist **Sshuser**.  Sie können sie umbenennen.
     
    Andere Parameter sind optional.  
    
    Jeder Cluster verfügt über eine Abhängigkeit Azure BLOB-Speicher Konto. Nach dem Löschen eines Clusters behält der Daten im Speicherkonto. Cluster Storage Name des Standardkontos ist der Clustername "Store" angefügt. In Abschnitt Variablen Vorlage hartcodiert ist.
        
3. Wählen Sie **ich stimme den allgemeinen Geschäftsbedingungen genannten**und klicken Sie dann auf **kaufen**. Etwa Minuten 20 Erstellen eines Clusters.


>[AZURE.NOTE] Nach dem Löschen eines Clusters HBase können Sie einen anderen HBase Cluster mit demselben Standardcontainer Blob erstellen. HBase Tabellen holen in der ursprünglichen Cluster erstellten neue Cluster. Um Inkonsistenzen zu vermeiden, wird empfohlen, HBase Tabellen deaktivieren, bevor Sie den Cluster löschen.

## <a name="create-tables-and-insert-data"></a>Tabellen erstellen und Einfügen von Daten

Sie können SSH HBase Cluster und dann HBase Shell HBase Tabellen erstellen, legen Sie Daten und Abfragen von Daten. Informationen über SSH finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md) und [Verwendet SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md).
 

Für die meisten Daten im Tabellenformat angezeigt:

![HDInsight HBase-Tabellendaten][img-hbase-sample-data-tabular]

In HBase ist eine Implementierung der BigTable sieht die gleichen Daten wie:

![HDInsight HBase Bigtable Daten][img-hbase-sample-data-bigtable]

Es wird mehr Sinn nach Abschluss die nächste Prozedur.  


**Die HBase-Verwaltungsshell**

1. SSH führen Sie den folgenden Befehl:

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

    Weitere Informationen über das Tabellenschema HBase finden Sie unter [Introduction to HBase Schema Design][hbase-schema]. Mehr HBase Befehle finden Sie im [Referenzhandbuch Apache HBase][hbase-quick-start].

6. Beenden der shell

        exit



**Massenkopieren von Daten in HBase-Tabelle contacts**

HBase bietet mehrere Methoden zum Laden von Daten in Tabellen.  Weitere Informationen finden Sie unter [Bulk laden](http://hbase.apache.org/book.html#arch.bulk.load).


Ein Blob für den öffentlichen Container eine Beispieldatendatei hochgeladen wurde *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  Der Inhalt der Datei ist:

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

1. Von SSH, führen Sie den folgenden Befehl zum Transformieren der Datendatei bei ein relativer Pfad von Dimporttsv.bulk.output zu StoreFiles:.  Sind in HBase Shell Befehl Exit beendet.

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Führen Sie den folgenden Befehl zum Hochladen von Daten aus /example/data/storeDataFileOutput in HBase-Tabelle:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Sie können HBase-Shell öffnen und den Befehl Scan den Inhalt der Liste.



## <a name="use-hive-to-query-hbase"></a>Mit der Abfrage HBase Struktur

Sie können Daten in Tabellen HBase Struktur mit Abfragen. In diesem Abschnitt erstellt eine Struktur-Tabelle, die ordnet der HBase-Tabelle und verwendet die Daten in der HBase-Tabelle Abfragen.

1. **Kitten**öffnen und mit dem Cluster herstellen.  Finden Sie im vorherigen Verfahren.
2. Öffnen Sie Struktur-Shell.

       hive
3. Führen Sie das folgende HiveQL Skript eine Struktur erstellen, die die HBase-Tabelle zugeordnet ist. Stellen Sie sicher, dass die zuvor in diesem Lernprogramm mithilfe der Shell HBase vor dem Ausführen dieser Anweisung referenziert Beispieltabelle erstellt.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. Führen Sie das folgende HiveQL-Skript zum Abfragen von Daten in HBase-Tabelle:

        SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>Verwenden Sie HBase REST APIs mit Kringel

> [AZURE.NOTE] WebHCat locken oder andere REST-Kommunikation mit müssen Sie Anfragen durch Benutzernamen und Kennwort HDInsight Cluster-Administrator authentifizieren. Sie müssen den Clusternamen auch als Teil der der URI Uniform Resource Identifier () verwendet, um Anfragen an den Server verwenden.
>
> Befehle in diesem Abschnitt zum Cluster Authentifizierung des Benutzers ersetzen Sie **Benutzernamen** , und Ersetzen Sie **PASSWORD** durch das Kennwort für das Benutzerkonto. Der Name des Clusters ersetzen Sie **CLUSTERNAME** .
>
> Die REST-API wird über [die Standardauthentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Stellen Sie Anfragen immer, mit HTTPS (Secure HTTP) um sicherzustellen, dass Ihre Anmeldeinformationen sicher an den Server gesendet werden.

1. Verwenden Sie den folgenden Befehl um zu überprüfen, ob HDInsight Cluster hergestellt werden kann, von einer Befehlszeile aus:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    Sie erhalten eine Antwort ähnlich der folgenden:

        {"status":"ok","version":"v1"}

    In diesem Befehl verwendeten Parameter werden wie folgt:

    * **-u** - Benutzernamen und ein Kennwort zur Authentifizierung der Anforderung verwendet.
    * **-G** - gibt an, dass dies eine GET-Anforderung ist.

2. Verwenden Sie folgenden Befehl auf der vorhandenen HBase Tabellen:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. Verwenden Sie den folgenden Befehl zum Erstellen einer neuen HBase Tabelle mit zwei Spalten:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v

    Das Schema wird im JSon-Format bereitgestellt.

4. Verwenden Sie den folgenden Befehl zum Einfügen von Daten:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v

    Müssen Sie base64 codiert die Angaben in der Befehlszeilenoption -d.  Im Beispiel:

    - MTAwMA ==: 1000
    - UGVyc29uYWw6TmFtZQ ==: Personal: Name
    - Sm9obiBEb2xl: John Dole

    [False Zeilenschlüssel](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) können Sie mehrere (gespeicherten) Wert.

5. Verwenden Sie den folgenden Befehl zu einer Zeile:

        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

Weitere Informationen zu anderen HBase finden Sie unter [Apache HBase-Referenzhandbuch](https://hbase.apache.org/book.html#_rest).

## <a name="check-cluster-status"></a>Überprüfen des Status eines Clusters

HBase in HDInsight wird mit einer Web-Benutzeroberfläche für die Überwachung von Clustern. Der Web-Benutzeroberfläche können Sie Statistiken oder Informationen zu beantragen.

SSH kann auch lokale Anfragen wie Webanfragen HDInsight Cluster Tunnel verwendet werden. Die Anforderung wird dann auf die angeforderte Ressource weitergeleitet auf HDInsight Cluster-Head-Knoten stammen hatte. Weitere Informationen finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel).

**Herstellen einer SSH-Sitzung tunneling**

1. Öffnen Sie **kitten**.  
2. Wenn Sie beim Erstellen des Benutzerkontos bei der SSH-Schlüssel haben, führen Sie den folgenden Schritt den privaten Schlüssel verwenden, bei dem Cluster:

    **Kategorie** **Verbindung**erweitern, Erweitern von **SSH**und **Authentifizierung**auswählen. Abschließend klicken Sie auf **Durchsuchen** und wählen Sie die Datei .ppk, die Ihre privaten Schlüssel enthält.

3. Klicken Sie auf **Sitzung**, in **Kategorie**.
4. Grundlegende Optionen für Ihren Bildschirm PuTTY Sitzung eingeben:

    - **Hostname**: SSH-Adresse Ihr Feld HDInsight in die Host-Namen (oder IP-Adresse). Die SSH-Adresse ist der Clustername **--SSH.azurehdinsight.NET**. Z. B. *MeinCluster-SSH.azurehdinsight.NET*.
    - **Anschluss**: 22. Die ssh-Port primäre Hauptknoten ist 22.  
5. Im Abschnitt **Kategorien** links im Dialogfeld **Verbindung**erweitern, Erweitern von **SSH**und klicken Sie dann auf **Tunnel**.
6. Angaben Sie folgende auf die Optionen zum Steuern der SSH Port Weiterleitung Formular:

    - **Ursprungs-Port** - Port auf dem Client, den Sie weiterleiten möchten. Z. B. 9876.
    - **Dynamische** - ermöglicht dynamische SOCKS-Proxy weiterleiten.
7. Klicken Sie auf **Hinzufügen** , um die Einstellung hinzufügen.
8. Klicken Sie unten im Dialogfeld öffnen eine SSH-Verbindung **Öffnen** .
9. Wenn Sie aufgefordert werden, melden Sie sich des Servers mit einem SSH-Konto. Eine SSH-Sitzung einrichten wird und aktivieren den Tunnel.

**Zu den FQDN des Ambari mit lassen**

1. Suchen Sie https://<ClusterName>.azurehdinsight.net/.
2. Geben Sie Ihre Anmeldeinformationen Cluster zweimal.
3. Klicken Sie im linken Menü auf **Zookeeper**.
4. Klicken Sie auf einen der drei **ZooKeeper Server** Links Zusammenfassung aus.
5. **Hostname**zu kopieren. Zk0-CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net.

**Konfigurieren Sie ein Clientprogramm (Firefox) und Clusterstatus überprüfen**

1. Öffnen Sie Firefox.
2. Klicken Sie auf **Menü öffnen** .
3. Klicken Sie auf **Optionen**.
4. Klicken Sie auf **Erweitert**und **dann klicken Sie**auf **Netzwerk**.
5. Wählen Sie **Manual Proxy-Konfiguration**.
6. Geben Sie die folgenden Werte:

    - **SOCKS-Host**: Localhost
    - **Anschluss**: die kitten SSH Tunneling konfiguriert denselben Port verwenden.  Z. B. 9876.
    - **SOCKS-v5**: (ausgewählt)
    - **Remote-DNS**: (ausgewählt)
7. Klicken Sie auf **OK** , um die Informationen zu speichern.
8. Suchen Sie nach http://&lt;der FQDN ein ZooKeeper >: 60010, Master-Status.

In einem Cluster mit hoher Verfügbarkeit finden Sie einen Link zum aktuellen aktiven HBase master Knoten, die der Web-Benutzeroberfläche befindet.

##<a name="delete-the-cluster"></a>Cluster löschen

Um Inkonsistenzen zu vermeiden, wird empfohlen, HBase Tabellen deaktivieren, bevor Sie den Cluster löschen.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm HBase HDInsight haben Sie einen HBase-Cluster erstellen und zum Erstellen von Tabellen und Anzeigen der Daten in diesen Tabellen aus der Shell HBase. Sie haben gelernt, wie eine Struktur Query Daten in Tabellen HBase und Verwendung der HBase C# REST-APIs HBase Tabelle erstellen und Abrufen von Daten aus der Tabelle.

Weitere Informationen finden Sie unter:

- [Übersicht über die HDInsight HBase][hdinsight-hbase-overview]: HBase ist eine Apache Open-Source NoSQL-Datenbank basierend auf Hadoop, das RAM und starke Konsistenz für große Mengen von unstrukturierten und teilstrukturierte Daten bereitstellt.


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
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
