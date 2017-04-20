<properties
   pageTitle="Erstellen Sie Windows-basierte Hadoop Cluster in Azure CLI mit HDInsight"
    description="Informationen Sie zum Cluster mithilfe von Azure-CLI für Azure HDInsight erstellen."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-cli"></a>Erstellen Sie Windows-basierte Hadoop Cluster in Azure CLI mit HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Informationen Sie zum HDInsight-Cluster mithilfe von Azure CLI erstellen. Andere Clustererstellung Tools und Features klicken Sie auf die Registerkarte wählen oben auf dieser Seite finden Sie oder [Cluster Methoden](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Komponenten:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Vor diesem Artikel benötigen Sie Folgendes:

- **Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure CLI**.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

### <a name="access-control-requirements"></a>Steuerelement erforderlich

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="connect-to-azure"></a>Verbinden mit Azure

Verwenden Sie den folgenden Befehl zum Azure herstellen:

    azure login

Weitere Informationen zu Authentifizierung mit einem Arbeits- oder Schulcomputer Konto finden Sie unter [mit Azure-Abonnement von Azure-CLI](../xplat-cli-connect.md).

Verwenden Sie folgenden Befehl in den ARM-Modus zu wechseln:

    azure config mode arm

Um Hilfe zu erhalten, verwenden Sie **h -** Switch.  Zum Beispiel:

    azure hdinsight cluster create -h

##<a name="create-clusters"></a>Erstellen von Clustern

Benötigen Sie ein Azure-Ressourcen-Management (ARM) und ein Konto Azure BLOB-Speicher vor der Erstellung eines Clusters HDInsight. Um einen HDInsight-Cluster erstellen, müssen Sie Folgendes angeben:

- **Azure-Ressourcengruppe**: See Datenanalyse ein Konto in einer Azure-Ressourcengruppe erstellt werden muss. Azure Ressourcen-Manager können Sie die Ressourcen in der Anwendung als Gruppe arbeiten. Bereitstellen, aktualisieren, oder löschen alle Ressourcen für die Anwendung in einer einzigen koordinierten Operation.

    Listen Sie die Ressourcengruppen in Ihrem Abonnement

        azure group list

    So erstellen Sie eine neue Ressourcengruppe

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **HDInsight Clustername**

- **Lage**: eine Azure Rechenzentren, die HDInsight-Cluster unterstützt. Eine Liste der unterstützten Standorte finden Sie unter [HDInsight Preisgestaltung](https://azure.microsoft.com/pricing/details/hdinsight/).

- **Standard-Speicherkonto**: HDInsight verwendet einen Container Azure BLOB-Speicher als das Standarddateisystem. Ein Azure Storage-Konto ist erforderlich, bevor Sie einen HDInsight-Cluster erstellen können.

    So erstellen Sie ein neues Konto Azure-Speicher

        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

    > [AZURE.NOTE]Das Speicherkonto muss mit HDInsight im Rechenzentrum zusammengestellt werden.
    > Speicher-Kontotyp nicht ZRS, da ZRS Tabelle unterstützt.

    Informationen zum Erstellen von Azure Storage-Konto mithilfe der Azure-Portal finden Sie unter [erstellen, verwalten oder löschen Sie ein Speicherkonto] [Azure-erstellen-Storageaccount].

    Wenn Sie bereits ein Speicherkonto aber den Namen und das kontoschlüssel nicht kennen, können Sie die folgenden Befehle, zum Abrufen der Informationen:

        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

    Ausführliche Informationen zum Abrufen von Informationen über Azure-Portal finden Sie im Abschnitt "Verwalten Ihres Speicherkontos" [von Azure-Speicherkonten](../storage/storage-create-storage-account#manage-your-storage-account).

- **(Optional) Standard Blob-Container**: **Azure Hdinsight Cluster erstellen** Befehl erstellt den Container existiert nicht. Wenn Sie vorher den Container erstellen möchten, können Sie den folgenden Befehl:

    Azure-Speichercontainer erstellen - Namen-"<Storage Account Name>"-Konto Schlüssel <Storage Account Key> [ContainerName]

Haben Sie das Speicherkonto vorbereitet, können Sie einen Cluster erstellen:


    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


##<a name="create-clusters-using-configuration-files"></a>Erstellen von Clustern mit Konfigurationsdateien
Normalerweise einen HDInsight-Cluster erstellen, Aufträge ausgeführt und senken die Kosten durch den Cluster löschen. Die Befehlszeilenschnittstelle können Sie Konfigurationen in eine Datei speichern, damit Sie wiederverwendet werden können, wenn Sie einen Cluster erstellen.  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

Beispiel: Erstellen Sie eine Konfigurationsdatei mit Skriptaktion ein Cluster ausgeführt.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
    azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

##<a name="create-clusters-with-script-action"></a>Cluster mit Skript erstellen

Erstellen Sie eine Konfigurationsdatei mit Skriptaktion ein Cluster ausgeführt.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

Erstellen Sie einen Cluster mit einem Skript

    azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


Allgemeines Skript Aktionsinformationen finden Sie unter [Anpassen HDInsight Cluster mit Skriptaktion (Linux)](hdinsight-hadoop-customize-cluster.md).


## <a name="create-clusters-using-arm-templates"></a>Erstellen Sie Cluster mit ARM-Vorlagen

CLI können Cluster durch Aufrufen von ARM-Vorlagen erstellen. Finden Sie unter [Bereitstellen von Azure CLI](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli).

## <a name="see-also"></a>Siehe auch

- [Einstieg in Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) - Informationen von HDInsight-Cluster
- [Hadoop senden Aufträge programmgesteuert](hdinsight-submit-hadoop-jobs-programmatically.md) - erfahren Sie programmgesteuert Aufträge an HDInsight übermitteln
- [Verwalten Sie Hadoop in Azure-CLI mit HDInsight Cluster](hdinsight-administer-use-command-line.md)
- [Verwendung der Azure-CLI für Mac, Linux und Windows Azure Service Management](../virtual-machines-command-line-tools.md)
