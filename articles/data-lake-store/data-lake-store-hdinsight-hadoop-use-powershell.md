<properties
   pageTitle="HDInsight Cluster mit Azure See Datenspeicher mit PowerShell erstellen | Azure"
   description="Verwenden von Azure PowerShell zu erstellen HDInsight-Cluster mit Azure Daten"
   services="data-lake-store,hdinsight" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="nitinme"/>

# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-powershell"></a>Erstellen Sie einen HDInsight-Cluster mit See Datenspeicher Azure PowerShell

> [AZURE.SELECTOR]
- [Mithilfe von Portal](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Mithilfe von PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)
- [Mit dem Ressourcen-Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Informationen Sie zum Azure PowerShell verwenden, um einen HDInsight-Cluster (Hadoop, HBase oder Sturm) Zugriff auf Azure See Datenspeicher konfigurieren. Einige wichtige Aspekte dieser Version:

* **Für Spark-Cluster (Linux) und Hadoop-Storm-Cluster (Windows und Linux)**, See Datenspeicher können nur als zusätzlicher Speicherkonto verwendet werden. Standard-Speicherkonto für die Cluster werden Azure Storage Blobs (WASB).

* **HBase für Cluster (Windows und Linux)**, See Datenspeicher kann als Standardspeicher oder zusätzlichen Speicher verwendet werden.

> [AZURE.NOTE] Einige wichtige Punkte zu beachten. 
> 
> * Option zum Erstellen von HDInsight-Cluster mit Zugriff auf See Datenspeicher steht nur für HDInsight-Versionen 3.2 und 3.4 (für Hadoop, HBase und Sturm Cluster sowohl Windows als auch Linux). Spark-Clustern unter Linux ist diese Option nur auf HDInsight 3.4 Cluster.
>
> * Wie bereits erwähnt, ist als Standard für bestimmte Cluster (HBase) und zusätzlicher Speicher für andere Cluster (Hadoop, Funken, Sturm) See Datenspeicher verfügbar. Mit See Datenspeicher als zusätzlicher Speicherkonto beeinträchtigt nicht Leistung oder die Möglichkeit, den Speicher aus dem Cluster schreibgeschützt. In einem Szenario, See Datenspeicher als zusätzlicher Speicher verwendet, werden Cluster-Dateien (z. B. Protokolle) geschrieben Standardspeicher (Azure-Blobs), während die zu verarbeitenden Daten in einem Datenspeicher See Konto gespeichert werden können.


In diesem Artikel bereitstellen wir Hadoop Cluster mit See Datenspeicher als zusätzlichen Speicher.

Konfigurieren HDInsight Lake Datenspeicher arbeiten umfasst mit PowerShell folgende Schritte:

* Erstellen eines Datenspeichers See Azure
* Authentifizierung für den rollenbasierten Zugriff auf See Datenspeicher
* Erstellen Sie HDInsight-Cluster mit Authentifizierung See Datenspeicher
* Führen Sie die Tests des im Cluster

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

- **Azure PowerShell 1.0 oder höher**. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

- **Windows SDK**. Sie können es [hier](https://dev.windows.com/en-us/downloads). Sie können ein Zertifikat erstellen.

- **Azure Active Directory Service Principal**. Schritte in diesem Lernprogramm bereitgestellt zum Erstellen einer Dienstprinzipalnamen in Azure AD. Allerdings müssen Sie Administrator Azure AD erstellt einen Dienstprinzipal zu sein. Sind Administrator Azure AD können diese Voraussetzung überspringen und mit dem Lernprogramm fortfahren.
    
    **Sind Sie kein Administrator Azure AD**nicht die Schritte erforderlich, um einen Dienstprinzipalnamen erstellen können Sie. In diesem Fall muss Systemadministrator Azure AD einen Dienstprinzipal zunächst vor der Erstellung eines Clusters HDInsight Lake Datenspeicher. Außerdem muss die Dienstprinzipalnamen erstellt werden mithilfe eines Zertifikats beschriebenen an [Dienstprinzipal Zertifikat erstellen](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate). 


## <a name="create-an-azure-data-lake-store"></a>Erstellen eines Datenspeichers See Azure

Folgen Sie erstellen Sie einen Datenspeicher See

1. Öffnen Sie auf dem Desktop ein neues Azure PowerShell-Fenster, und geben Sie im folgenden Codeausschnitt. Aufforderung Anmelden sicherstellen Sie, dass Sie als eines der Abonnementbesitzer/Administratoren anmelden:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    >[AZURE.NOTE] Wenn Sie eine ähnliche Fehlermeldung erhalten `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` Ressourcenanbieter See Datenspeicher registriert ist möglich, dass die Subsrcription nicht auf der weißen Liste für Azure See Datenspeicher. Stellen Sie sicher, dass Ihre Azure-Abonnement für Datenspeicher See public Preview-Version dieser [Anleitung](data-lake-store-get-started-portal.md#signup)zu aktivieren.

3. Ein Azure-See Datenspeicher Konto ist Azure-Ressourcengruppe zugeordnet. Zunächst erstellen eine Ressourcengruppe Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Ein Azure-Ressourcengruppe erstellen] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Ein Azure-Ressourcengruppe erstellen")

2. Erstellen eines Kontos Azure See Datenspeicher. Der angegebene Kontoname darf nur Kleinbuchstaben und Zahlen enthalten.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Azure Data Lake Konto erstellen] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Azure Data Lake Konto erstellen")

3. Stellen Sie sicher, dass das Konto erfolgreich erstellt wurde.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Die Ausgabe für diesen sollte **True**sein.

4. Laden Sie Beispieldaten auf Azure Data Lake. Wir verwenden diese später in diesem Artikel überprüfen, ob die Daten von einem HDInsight-Cluster verfügbar sind. Wenn Sie Beispieldaten hochladen suchen, erhalten Sie von [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) **Krankenwagen** Datenordner.


        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Authentifizierung für den rollenbasierten Zugriff auf See Datenspeicher

Jede Azure-Abonnement ist Azure Active Directory zugeordnet. Benutzer und Dienste, die das Abonnement mithilfe der Azure-Verwaltungsportal oder Azure-Ressourcen-Manager-API zugreifen müssen zunächst mit Azure Active Directory authentifizieren. Services und Azure-Abonnements Zugriff durch die entsprechende Rolle in Azure Ressource zuweisen.  Für Dienste identifiziert ein Dienstprinzipal Dienst in Azure Active Directory (AAD). Dieser Abschnitt veranschaulicht, wie einen Anwendungsdienst wie HDInsight, Zugriff auf eine Azure-Ressource (die zuvor erstellten Azure See Datenspeicher-Konto) gewähren einen Dienstprinzipalnamen für die Anwendung erstellen und Zuweisen von Rollen zu, die über Azure PowerShell.

Um Active Directory-Authentifizierung für Azure Data Lake einzurichten, führen Sie die folgenden Aufgaben.

* Ein selbstsigniertes Zertifikat erstellen
* Erstellen einer Anwendung in Azure Active Directory und einen Dienstprinzipalnamen

### <a name="create-a-self-signed-certificate"></a>Ein selbstsigniertes Zertifikat erstellen

Überprüfen Sie [Windows SDK](https://dev.windows.com/en-us/downloads) installiert, bevor Sie mit den Schritten in diesem Abschnitt. Sie müssen auch ein Verzeichnis wie **C:\mycertdir**erstellt haben, in dem das Zertifikat erstellt werden.

1. Navigieren Sie im PowerShell zum Windows SDK installiert (in der Regel `C:\Program Files (x86)\Windows Kits\10\bin\x86` und [MakeCert] [ makecert] ein selbstsigniertes Zertifikat und einem privaten Schlüssel erstellt. Verwenden Sie die folgenden Befehle.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir
        $startDate = (Get-Date).ToString('MM/dd/yyyy')
        $endDate = (Get-Date).AddDays(365).ToString('MM/dd/yyyy')

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -b $startDate -e $endDate -r -len 2048

    Sie werden aufgefordert, das Kennwort für den privaten Schlüssel einzugeben. Nachdem der Befehl erfolgreich ausgeführt wird, erhalten Sie eine **CertFile.cer** und **mykey.pvk** in den angegebenen Zertifikatsspeicher.

4. Verwenden Sie die [Pvk2Pfx] [ pvk2pfx] Dienstprogramm MakeCert erstellt PVK und CER-Dateien in einer PFX-Datei konvertieren. Führen Sie den folgenden Befehl ein.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Aufforderung geben Sie das Kennwort für private Schlüssel zuvor angegeben. Der Wert für den Parameter **-po** ist das Kennwort, das der PFX-Datei zugeordnet ist. Nachdem der Befehl erfolgreich ausgeführt wurde, erhalten Sie auch eine CertFile.pfx im Zertifikatsverzeichnis angegebene.

###  <a name="create-an-azure-active-directory-and-a-service-principal"></a>Erstellen Sie Azure Active Directory und einen Dienstprinzipalnamen

In diesem Abschnitt führen Sie die Schritte zum Erstellen eines Dienstes für eine Anwendung Azure Active Directory Service principal eine Rolle zuweisen und als Service Principal durch Bereitstellen eines Zertifikats authentifiziert. Führen Sie die folgenden Befehle zum Erstellen einer Anwendung in Azure Active Directory.

1. Fügen Sie die folgenden Cmdlets im Konsolenfenster PowerShell. Stellen Sie sicher, dass für die **DisplayName -** Eigenschaft festgelegten Wert eindeutig ist. Die Werte für **- HomePage** und **IdentiferUris -** Platzhalter Werte auch nicht überprüft.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
                    -DisplayName "HDIADL" `
                    -HomePage "https://contoso.com" `
                    -IdentifierUris "https://mycontoso.com" `
                    -KeyValue $credential  `
                    -KeyType "AsymmetricX509Cert"  `
                    -KeyUsage "Verify"  `
                    -StartDate $startDate  `
                    -EndDate $endDate

        $applicationId = $application.ApplicationId

2. Erstellen Sie einen Dienstprinzipalnamen mit der Anwendung-ID.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id

3. Gewähren der Service principal See Datenspeicher Datei oder der Ordner, die Sie aus dem HDInsight-Cluster zugreifen. Der folgende Ausschnitt bietet Zugriff auf das Stammverzeichnis des Kontos See Datenspeicher.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All

    Aufgefordert werden Geben Sie **Y** zu bestätigen.

## <a name="create-an-hdinsight-cluster-with-authentication-to-data-lake-store"></a>Erstellen Sie einen HDInsight-Cluster mit Authentifizierung See Datenspeicher

In diesem Abschnitt erstellen Sie einen HDInsight Hadoop-Cluster. In dieser Version muss HDInsight-Cluster und dem Datenspeicher See am selben Speicherort (USA Osten 2).

1. Beginnen Sie mit Abonnement Mieter ID abrufen Sie benötigen, die später.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. In dieser Version kann für einen Cluster Hadoop See Datenspeicher nur als zusätzlicher Speicher für den Cluster verwendet werden. Der Standardspeicher werden Blobs Azure-Speicher (WASB). So wird das Speicherkonto und Speichercontainer für den Cluster erforderlich zunächst erstellen.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName | %{ $_.Key1 }
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext

3. HDInsight-Cluster zu erstellen. Verwenden Sie die folgenden Cmdlets.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $rdpCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.2" -RdpCredential $rdpCredentials -RdpAccessExpiry (Get-Date).AddDays(14) -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Nachdem das Cmdlet erfolgreich abgeschlossen wurde, erhalten Sie eine Ausgabe wie diese:

        Name                      : hdiadlcluster
        Id                        : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/hdiadlgroup/providers/Mi
                                    crosoft.HDInsight/clusters/hdiadlcluster
        Location                  : East US 2
        ClusterVersion            : 3.2.7.707
        OperatingSystemType       : Windows
        ClusterState              : Running
        ClusterType               : Hadoop
        CoresUsed                 : 16
        HttpEndpoint              : hdiadlcluster.azurehdinsight.net
        Error                     :
        DefaultStorageAccount     :
        DefaultStorageContainer   :
        ResourceGroup             : hdiadlgroup
        AdditionalStorageAccounts :

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Testaufträge auf HDInsight Cluster der See Datenspeicher ausgeführt

Nachdem Sie einen HDInsight-Cluster konfiguriert haben, führen Sie Testläufe im Cluster testen HDInsight Cluster See Datenspeicher zugreifen kann. Hierzu werden wir ein Beispiel Struktur Auftrag ausgeführt, der eine Tabelle mit den Beispieldaten, die Sie zuvor dem Datenspeicher See hochgeladen erstellt.

### <a name="for-a-linux-cluster"></a>Für einen Linux-cluster

In diesem Abschnitt werden Sie SSH-Cluster und Ausführen der eine Beispielabfrage Struktur. Windows bietet keine integrierten SSH-Client. Wir empfehlen, **kitten**, die von [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)heruntergeladen werden kann.

Weitere Informationen über kitten finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf Windows HDInsight ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Sobald verbunden, starten Sie die CLI Struktur mit dem folgenden Befehl:

        hive

2. Geben Sie die CLI die folgende Anweisung zum Erstellen einer neuen Tabelle mit dem Namen **Fahrzeuge** mit Beispieldaten im Datenspeicher See:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


### <a name="for-a-windows-cluster"></a>Für einen Windows-cluster

Verwenden Sie die folgenden Cmdlets die Hive-Abfrage auszuführen. In dieser Abfrage werden eine Tabelle erstellen, die Daten im Datenspeicher See und führen Sie eine select-Abfrage in der erstellten Tabelle.

    $queryString = "DROP TABLE vehicles;" + "CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://$dataLakeStoreName.azuredatalakestore.net:443/';" + "SELECT * FROM vehicles LIMIT 10;"

    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString

    $hiveJob = Start-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $httpCredentials

    Wait-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $httpCredentials

Dies hat die folgende Ausgabe. **ExitValue** 0 in der Ausgabe schlägt vor, dass der Auftrag erfolgreich abgeschlossen.

    Cluster         : hdiadlcluster.
    HttpEndpoint    : hdiadlcluster.azurehdinsight.net
    State           : SUCCEEDED
    JobId           : job_1445386885331_0012
    ParentId        :
    PercentComplete :
    ExitValue       : 0
    User            : admin
    Callback        :
    Completed       : done

Abrufen der Ausgabe aus dem Auftrag mit dem folgenden Cmdlet:

    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $hiveJob.JobId -DefaultContainer $containerName -DefaultStorageAccountName $storageAccountName -DefaultStorageAccountKey $storageAccountKey -ClusterCredential $httpCredentials

Die Job-Ausgabe ähnelt dem folgenden:

    1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
    1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
    1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
    1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
    1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
    1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
    1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
    1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
    1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
    1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


## <a name="access-data-lake-store-using-hdfs-commands"></a>Zugriff auf See Datenspeicher bietet Befehle

Nachdem Sie den HDInsight Cluster See Datenspeicher konfiguriert haben, können Sie Shellbefehle bietet, auf den Speicher zuzugreifen.

### <a name="for-a-linux-cluster"></a>Für einen Linux-cluster

In diesem Abschnitt wird SSH zum Cluster und Befehle bietet. Windows bietet keine integrierten SSH-Client. Wir empfehlen, **kitten**, die von [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)heruntergeladen werden kann.

Weitere Informationen über kitten finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf Windows HDInsight ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Sobald verbunden, Befehl folgende bietet Dateisystem Dateien im Datenspeicher See aufgelistet.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Dies sollte der Datei aufgeführt, die Sie zuvor in den Datenspeicher See hochgeladen.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Können Sie auch die `hdfs dfs -put` Befehl See Datenspeicher einige Dateien hochladen und dann `hdfs dfs -ls` überprüfen, ob die Dateien hochgeladen wurden.


### <a name="for-a-windows-cluster"></a>Für einen Windows-cluster

1. Melden Sie sich auf das neue [Azure-Portal](https://portal.azure.com).

2. Klicken Sie auf **Durchsuchen**, klicken Sie auf **HDInsight-Cluster**und klicken Sie auf den HDInsight-Cluster, den Sie erstellt.

3. Blatt Cluster auf **Remotedesktop**und ** **Remotedesktop** Blatt klicken**.

    ![Entfernte HDI Cluster] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.HDI.PS.Remote.Desktop.png "Ein Azure-Ressourcengruppe erstellen")

    Geben Sie bei Aufforderung die Anmeldeinformationen für den remote desktop Benutzer bereitgestellt.

4. Starten Sie in der Remotesitzung Windows PowerShell und Befehlen Sie bietet Dateisystem die Dateien im Datenspeicher See Azure.

        hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

    Dies sollte der Datei aufgeführt, die Sie zuvor in den Datenspeicher See hochgeladen.

        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/vehicle1_09142014.csv

    Können Sie auch die `hdfs dfs -put` Befehl See Datenspeicher einige Dateien hochladen und dann `hdfs dfs -ls` überprüfen, ob die Dateien hochgeladen wurden.

## <a name="see-also"></a>Siehe auch

* [Portal: Erstellen eines HDInsight Clusters See Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
