<properties
    pageTitle="Verwenden Sie leere Randknoten in HDInsight | Microsoft Azure"
    description="Wie einen Kantenknoten Ampty HDInsight Cluster hinzufügen, der als Client verwendet werden kann und wie Test-Host HDInsight Anwendung."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="use-empty-edge-nodes-in-hdinsight"></a>Verwenden Sie leere Randknoten in HDInsight

Erfahren Sie, wie ein Linux-basiertes HDInsight-Cluster eine leere Kantenknoten hinzu. Ein leere Kantenknoten ist einer virtuellen Linux-Maschine dieselbe Clienttools installiert und wie die Headnodes konfiguriert, aber keine Hadoop-Dienste ausgeführt. Edgeknoten können für den Zugriff auf den Cluster, Clientanwendungen testen und Hosten von Clientanwendungen. 

Hinzufügen einen leeren Kantenknoten vorhandenen HDInsight-Cluster zu einem neuen Cluster beim Erstellen des Clusters. Hinzufügen einen leeren Kantenknoten erfolgt mit Azure-Ressourcen-Manager-Vorlage.  Das folgende Beispiel veranschaulicht, wie dies erfolgt mithilfe einer Vorlage:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "[variables('clusterApiVersion')]",
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "[parameters('edgeNodeSize')]"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Wie im Beispiel gezeigt, können Sie optional eine [Skriptaktion](hdinsight-hadoop-customize-cluster-linux.md) zusätzliche Konfiguration, wie [Apache Farbton](hdinsight-hadoop-hue-linux.md) im Rand Knoten installieren aufrufen.

Nach der Erstellung eines können Sie Verbindung über SSH Kantenknoten und Clienttools Hadoop Cluster in HDInsight Zugriff auf ausführen.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Hinzufügen eines zu einem vorhandenen cluster

In diesem Abschnitt verwenden Sie Ressourcenmanager Vorlage eines vorhandenen HDInsight-Cluster hinzufügen.  Die Ressourcen-Manager-Vorlage finden in [GitHub](https://github.com/hdinsight/Iaas-Applications/tree/master/EmptyNode). Die Ressourcen-Manager-Vorlage Ruft ein Skript Aktion Skript unter https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Das Skript wird keine Aktionen durchführen.  Dies ist aufrufende Skriptaktion aus einer Vorlage Ressourcenmanager.

**Hinzufügen einen leeren Kantenknoten zu einem vorhandenen cluster**

1. Erstellen Sie HDInsight-Cluster, wenn Sie noch keine haben.  Siehe [Hadoop Lernprogramm: Erste Schritte mit Linux-basierten Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klicken Sie auf das folgende Bild Azure anmelden und öffnen die Vorlage Azure Resource Manager im Azure-Portal. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FEmptyNode%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Konfigurieren Sie die folgenden Eigenschaften:

    - CLUSTERNAME: Geben Sie den Namen eines vorhandenen Clusters HDInsight.
    - EDGENODESIZE: Wählen Sie eine VM-Größen.
    - EDGENODEPREFIX: Der Standardwert ist **neu**.  Den Standardwert ist, der Knotenname Rand **neue Edgenode**.  Sie können das Präfix vom Portal aus. Sie können auch den vollständigen Namen der Vorlage.


4. Klicken Sie auf **OK** , um die Informationen zu speichern.
5. In **Ressourcengruppe**wählen.
6. Klicken Sie auf **Einkauf**auf **rechtlich überprüfen**
7. Wählen Sie **Dashboard**, und klicken Sie auf **Erstellen**.

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Hinzufügen eines ein Cluster

In diesem Abschnitt verwenden Sie eine Ressourcen-Manager-Vorlage HDInsight-Cluster mit einem Knoten Rand erstellen.  Die Ressourcen-Manager-Vorlage finden in einem öffentlichen [Azure Blob-Speicher-Container](http://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json). Die Ressourcen-Manager-Vorlage Ruft ein Skript Aktion Skript unter https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Das Skript wird keine Aktionen durchführen.  Dies ist aufrufende Skriptaktion aus einer Vorlage Ressourcenmanager.

**Hinzufügen einen leeren Kantenknoten zu einem vorhandenen cluster**

1. Erstellen Sie HDInsight-Cluster, wenn Sie noch keine haben.  Siehe [Hadoop Lernprogramm: Erste Schritte mit Linux-basierten Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klicken Sie auf das folgende Bild Azure anmelden und öffnen die Vorlage Azure Resource Manager im Azure-Portal. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Konfigurieren Sie die folgenden Eigenschaften:
        
    - CLUSTERNAME: Geben Sie einen Namen für den neuen Cluster erstellen.
    - CLUSTERLOGINUSERNAME: Geben Sie den Benutzernamen Hadoop HTTP.  Der Standardname lautet **Admin**.
    - CLUSTERLOGINPASSWORD: Geben Sie das Benutzerkennwort Hadoop HTTP.
    - SSHUSERNAME: Geben Sie den SSH-Benutzernamen. Der Standardname lautet **Sshuser**.
    - SSHPASSWORD: Geben Sie das Benutzerkennwort SSH.
    - Speicherort: Der Name der Ressourcengruppe, Cluster und Storage-Standardkonto geben.
    - CLUSTERTYPE: Der Standardwert ist **Hadoop**.
    - CLUSTERWORKERNODECOUNT: Der Standardwert ist 2.
    - EDGENODESIZE: Wählen Sie eine VM-Größen.
    - EDGENODEPREFIX: Der Standardwert ist **neu**.  Den Standardwert ist, der Knotenname Rand **neue Edgenode**.  Sie können das Präfix vom Portal aus. Sie können auch den vollständigen Namen der Vorlage.

4. Klicken Sie auf **OK** , um die Informationen zu speichern.
5. **Ressourcengruppe**Geben Sie einen neuen Ressourcengruppe ein.
6. Klicken Sie auf **Einkauf**auf **rechtlich überprüfen**
7. Wählen Sie **Dashboard**, und klicken Sie auf **Erstellen**. 


## <a name="access-an-edge-node"></a>Zugriff auf einen Kantenknoten

Die Kantenknoten ssh Endpunkt ist <EdgeNodeName>. <ClusterName>-ssh.azurehdinsight.net:22.  Z. B. neue-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Edgeknoten wird als Anwendung auf Azure-Portal angezeigt.  Das Portal informiert über SSH Kantenknoten zugreifen.

**Überprüfen Sie den Rand Knoten SSH-Endpunkt**

1. Melden Sie sich auf der [Azure-Portal](https://portal.azure.com).
2. Öffnen eines HDInsight-Cluster.
3. Klicken Sie auf **Applikationen** aus dem Cluster-Blade. Sie sehen den edgeknoten.  Der Standardname ist **neue Edgenode**.
4. Klicken Sie auf den Rand Knoten. Sie sehen den SSH-Endpunkt.

**Struktur auf dem edgeknoten verwenden**

5. SSH Verbindung mit edgeknoten verwenden.  Finden Sie unter [Verwendung SSH mit Linux-basierten Hadoop auf Linux, Unix oder OS X HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) oder [SSH mit Linux-basierten Hadoop auf HDInsight von Windows verwenden](hdinsight-hadoop-linux-use-ssh-windows.md).
6. Nachdem Sie über SSH Kantenknoten verbunden haben, verwenden Sie folgenden Befehl zum Öffnen der Konsole Struktur:

        hive
7. Führen Sie den folgenden Befehl im Cluster Hive-Tabellen angezeigt:

        show tables;

## <a name="delete-an-edge-node"></a>Löschen eines

Sie können eines von Azure-Portal.

**Auf einen Kantenknoten**

1. Melden Sie sich auf der [Azure-Portal](https://portal.azure.com).
2. Öffnen eines HDInsight-Cluster.
3. Klicken Sie auf **Applikationen** aus dem Cluster-Blade. Eine Liste der Randknoten sorgen.  
4. Maustaste Kantenknoten löschen möchten, und klicken Sie auf **Löschen**.
5. Klicken Sie auf **Ja** zu bestätigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie einen Kantenknoten hinzufügen und auf den edgeknoten. Weitere finden Sie in folgenden Artikeln:

- [Installieren HDInsight Applikationen](hdinsight-apps-install-applications.md): erfahren Sie, wie eine Anwendung HDInsight Cluster installieren.
- [Benutzerdefinierte HDInsight Anwendung installieren](hdinsight-apps-install-custom-applications.md): Weitere Informationen zum Bereitstellen von unveröffentlichten HDInsight Anwendung HDInsight.
- [HDInsight veröffentlichen Applications](hdinsight-apps-publish-applications.md): erfahren Sie, wie Ihre benutzerdefinierte Anwendung HDInsight Azure Marketplace veröffentlichen.
- [MSDN: Installieren einer Anwendung HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Informationen zum HDInsight Anwendung definieren.
- [Anpassen von Linux-basierten HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster-linux.md): Informationen zum Skript-Aktion verwenden, um zusätzliche Applikationen zu installieren.
- [Hadoop erstellen Linux-basierten Clustern in HDInsight Ressourcenmanager Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md): erfahren Sie, wie Vorlagen erstellen HDInsight-Cluster Resource Manager aufrufen.

