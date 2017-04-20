<properties
    pageTitle="Hadoop, HBase oder Sturm Cluster unter Linux in HDInsight mit CLI Azure plattformübergreifende erstellen | Microsoft Azure"
    description="Erfahren Sie, wie Linux-basierten HDInsight Cluster mit CLI Azure plattformübergreifende, Azure-Ressourcen-Manager Vorlagen und Azure REST API erstellen. Sie können geben den Cluster (Hadoop, HBase oder Sturm) oder mithilfe von Skripts für angepasste Komponenten..."
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
    ms.date="09/20/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-cli"></a>Erstellen von Linux-basierten Clustern in HDInsight mit der Azure-CLI

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure-CLI ist eine plattformübergreifende Befehlszeilenprogramm Azure Services verwalten. Es kann, zusammen mit Azure Ressource-Vorlagen verwendet werden HDInsight-Cluster mit zugeordneten Speicherkonten und anderen Diensten.

Azure Ressourcenmanagement Vorlagen sind JSON Standarddokumenten, die zur Beschreibung einer __Ressourcengruppe__ und alle Ressourcen (z. B. HDInsight.) Diese Vorlage basierenden Ansatz können Sie die Ressourcen definieren, die in einer Vorlage für HDInsight erforderlich. Es können Sie verwalten die Gruppe als Ganzes durch __Bereitstellung__der für die gesamte Gruppe zu verwenden.

In diesem Dokument Schritte durch den Prozess der Erstellung eines neuen HDInsight-Clusters Azure-CLI mit einer Vorlage.

> [AZURE.IMPORTANT] Die Schritte in diesem Dokument verwenden Sie die Standardanzahl von workerknoten (4) für einen HDInsight-Cluster. Wenn Sie mehr als 32 Arbeitskraft Knoten (während der Erstellung des Clusters oder Skalierung des Clusters) planen, müssen Sie eine Headknoten Größe 14 GB Ram mit mindestens 8 Kernen auswählen.
>
> Weitere Informationen zu Knoten Größen und Kosten finden Sie unter [HDInsight Preisgestaltung](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Azure CLI__. Die Schritte in diesem Dokument wurden zuletzt mit Azure CLI Version 0.10.1 getestet.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 


### <a name="access-control-requirements"></a>Steuerelement erforderlich

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="log-in-to-your-azure-subscription"></a>Melden Sie sich bei Ihrem Azure-Abonnement

Folgen Sie den Schritten in [Verbindung mit einer Azure-Abonnement aus der Azure-Befehlszeilenschnittstelle (CLI Azure)](../xplat-cli-connect.md) dokumentiert und Ihr Abonnement mit der __Login__ -Methode an.

##<a name="create-a-cluster"></a>Erstellen eines Clusters

Die folgenden Schritte sollte nach der Installation und Konfiguration von Azure-CLI Eingabeaufforderungsfenster, Schale oder terminal Sitzung ausgeführt werden.

1. Verwenden Sie folgenden Befehl Azure-Abonnement zu authentifizieren:

        azure login

    Sie werden aufgefordert, Ihren Namen und Kennwort. Haben Sie mehrere Azure-Abonnements verwenden `azure account set <subscriptionname>` Abonnement festlegen, Azure-CLI-Befehle verwenden.

3. Wechseln Sie zur Azure-Ressourcen-Manager-Modus mit dem folgenden Befehl:

        azure config mode arm

4. Erstellen Sie eine Ressourcengruppe. Diese Ressourcengruppe enthält HDInsight-Cluster und zugeordnete Konto.

        azure group create groupname location
        
    * Ersetzen Sie durch einen eindeutigen Namen für die Gruppe __Gruppenname__ . 
    * Ersetzen Sie __Speicherort__ mit dem geografischen Bereich die Gruppe erstellen möchten. 
    
        Eine Liste der gültigen Speicherorte, die `azure location list` Befehl, und verwenden Sie eines der in der Spalte __Name__ .

5. Erstellen Sie ein Speicherkonto. Dieses Speicherkonto wird als der Standardspeicher für HDInsight-Cluster verwendet werden.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename
        
     * Der Name des im vorherigen Schritt erstellte Gruppe ersetzen Sie __Groupname__ .
     * Am gleichen Speicherort im vorherigen Schritt, ersetzen Sie __Speicherort__ . 
     * Ersetzen Sie __Storagename__ durch einen eindeutigen Namen für das Speicherkonto.
     
     > [AZURE.NOTE] Weitere Informationen zu diesem Befehl verwendeten Parameter verwenden `azure storage account create -h` zum Anzeigen der Hilfe für diesen Befehl.

5. Rufen Sie den Schlüssel zum Zugriff auf das Speicherkonto ab.

        azure storage account keys list -g groupname storagename
        
    * Der Ressourcengruppenname ersetzen Sie __Groupname__ .
    * Ersetzen Sie __Storagename__ durch den Namen des Speicherkontos.
    
    In den zurückgegebenen Daten speichern Sie __Schlüsselwert für __key1____ .

6. Erstellen Sie einen HDInsight-Cluster.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Der Ressourcengruppenname ersetzen Sie __Groupname__ .

    * Ersetzen Sie __Hadoop__ mit dem Cluster, den Sie erstellen möchten. Beispielsweise `Hadoop`, `HBase`, `Storm` oder `Spark`.

        > [AZURE.IMPORTANT] HDInsight Cluster sind in verschiedenen Typen entsprechen den Arbeitslast oder Technologie Cluster abgestimmt ist. Es ist keine unterstützte Methode in einem Cluster, der mehrere Typen wie Sturm und HBase auf einem Cluster kombiniert. 

    * Am gleichen Speicherort, in den vorherigen Schritten ersetzen Sie __Speicherort__ .

    * Der Kontoname Storage ersetzen Sie __Storagename__ .

    * Der Schlüssel im vorherigen Schritt ersetzen Sie __Storagekey__ . 

    * Für die `--defaultStorageContainer` Parameter und denselben Namen wie für den Cluster verwenden.

    * Ersetzen Sie __Admin__ und __Httppassword__ mit dem Namen und Kennwort verwenden, wenn den Cluster über HTTPS zugreifen möchten.

    * Ersetzen Sie __Sshuser__ und __Sshuserpassword__ mit dem Benutzernamen und Kennwort verwenden, wenn den Cluster über SSH zugreifen möchten

    Der Clustererstellung beendet einige Minuten dauern. In der Regel etwa 15.

##<a name="next-steps"></a>Nächste Schritte

Nun, einen HDInsight-Cluster mithilfe der Azure-CLI erfolgreich erstellt haben, verwenden Sie folgende Informationen zum Cluster arbeiten:

###<a name="hadoop-clusters"></a>Hadoop-Cluster

* [Struktur mit HDInsight verwenden](hdinsight-use-hive.md)
* [Verwenden Sie Schwein mit HDInsight](hdinsight-use-pig.md)
* [Verwenden Sie MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase-Cluster

* [Erste Schritte mit HBase auf HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Java-Anwendungsentwicklung für HBase auf HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Storm-Cluster

* [Entwickeln von Java-Topologien für auf HDInsight](hdinsight-storm-develop-java-topology.md)
* [Python-Komponenten in auf HDInsight verwenden](hdinsight-storm-develop-python-topology.md)
* [Bereitstellen und Überwachen von Topologien mit auf HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
