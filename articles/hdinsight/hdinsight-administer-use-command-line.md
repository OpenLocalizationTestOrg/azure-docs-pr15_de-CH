<properties
    pageTitle="Hadoop Cluster mithilfe von Azure CLI verwalten | Microsoft Azure"
    description="Verwendung der Azure-CLI Hadoop Cluster HDIsight verwalten"
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
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Verwalten Sie Hadoop in Azure-CLI mit HDInsight Cluster

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Erfahren Sie, wie mit der [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) in Azure HDInsight Hadoop Cluster verwalten. Node.js ist der Azure-CLI implementiert. Sie können auf jeder Plattform verwendet werden, Node.js, einschließlich Windows, Mac und Linux unterstützt.

Dieser Artikel behandelt nur HDInsight mit der Azure-CLI. Eine allgemeine Anleitung zur Verwendung von Azure CLI finden Sie [Installieren und Konfigurieren von Azure CLI][azure-command-line-tools].

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="prerequisites"></a>Erforderliche Komponenten

Vor diesem Artikel benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure-CLI** - Siehe [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md) für Installations-und Konfigurationsinformationen.
- **Mit Azure**mithilfe des folgenden Befehls:

        azure login

    Weitere Informationen zu Authentifizierung mit einem Arbeits- oder Schulcomputer Konto finden Sie unter [mit Azure-Abonnement von Azure-CLI](xplat-cli-connect.md).
    
- **Wechsel zum Azure-Ressourcen-Manager-Modus**, mit dem folgenden Befehl:

        azure config mode arm

Um Hilfe zu erhalten, verwenden Sie **h -** Switch.  Zum Beispiel:

    azure hdinsight cluster create -h
    
##<a name="create-clusters"></a>Erstellen von Clustern

Finden Sie unter [Erstellen von Linux-basierten Clustern in Azure-CLI mit HDInsight](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

##<a name="list-and-show-cluster-details"></a>Auflisten und Cluster-Details anzeigen
Verwenden Sie die folgenden Befehle und Cluster-Details anzeigen:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![HDI. CLIListCluster][image-cli-clusterlisting]


##<a name="delete-clusters"></a>Cluster löschen

Verwenden Sie den folgenden Befehl, um einen Cluster zu löschen:

    azure hdinsight cluster delete <Cluster Name>

Sie können einen Cluster auch durch Löschen der Ressourcengruppe, die den Cluster enthält. Beachten Sie, dass alle Ressourcen in der Gruppe standardspeicherkonto einschließlich Löschvorgang.

    azure group delete <Resource Group Name>

##<a name="scale-clusters"></a>Clustern

Die Clustergröße Hadoop ändern:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>HTTP-Zugriff für einen Cluster aktivieren

    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>RDP-Zugriff für einen Cluster aktivieren

    azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
    azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##<a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie HDInsight Cluster administrative Aufgaben ausführen. Weitere finden Sie in folgenden Artikeln:

* [Verwalten Sie HDInsight mithilfe der Azure-Portal] [hdinsight-admin-portal]
* [Verwalten von HDInsight mithilfe von Azure PowerShell] [hdinsight-admin-powershell]
* [Erste Schritte mit Azure HDInsight] [hdinsight-get-started]
* [Verwendung der Azure-CLI] [azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli-install.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "Auflisten und Anzeigen von Clustern"
