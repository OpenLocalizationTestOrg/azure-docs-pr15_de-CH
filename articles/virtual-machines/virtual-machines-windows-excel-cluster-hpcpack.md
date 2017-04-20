<properties
 pageTitle="HPC Pack Cluster für Excel und SOA | Microsoft Azure"
 description="Einstieg umfangreiche Excel und SOA-Arbeitslasten auf einem Cluster HPC Pack in Azure ausgeführt"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/25/2016"
 ms.author="danlep"/>

# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Erste Schritte mit Excel und SOA Arbeitslasten auf einem Cluster HPC Pack in Azure

Dieser Artikel veranschaulicht das Bereitstellen eines Clusters Microsoft HPC Pack Azure virtuelle Computer mithilfe einer Vorlage Azure Schnellstart oder optional ein Bereitstellungsskript Azure PowerShell. Der Cluster verwendet Azure Marketplace VM Bildern, Microsoft Excel oder Service-orientierte Architektur (SOA) Arbeitslasten mit HPC Pack auszuführen. Cluster können Sie einfache Excel HPC und SOA-Dienste von einem lokalen Client-Computer ausführen. Excel HPC-Diensten gehören Excel Arbeitsmappe Entlastung und benutzerdefinierte Funktionen von Excel oder UDF.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Das folgende Diagramm zeigt eine hoch HPC Pack Cluster erstellen.

![HPC-Cluster mit Knoten Excel Arbeitslasten][scenario]

## <a name="prerequisites"></a>Erforderliche Komponenten

*   **Client-Computer** – Sie benötigen ein Windows-Clientcomputer Beispiel Excel und SOA Aufträge zum Cluster senden. Außerdem benötigen Sie einen Computer Windows Azure PowerShell Cluster Bereitstellungsskript ausgeführt (Wenn Sie diese Bereitstellungsmethode auswählen) und

*   **Azure-Abonnement** - Wenn Sie Azure-Abonnement haben, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) in wenigen Minuten erstellen.

*   **Kerne Quota** - eventuell erhöhen Sie das Kontingent der Kerne, besonders wenn mehrere Clusterknoten mit multicore VM bereitstellen. Bei Verwendung eine Vorlage Azure Quickstart ist Kerne Kontingent im Ressourcenmanager pro Azure. In diesem Fall müssen Sie das Kontingent in einer bestimmten Region zu erhöhen. [Azure-Abonnement Grenzen, Kontingente und Einschränkungen](../azure-subscription-service-limits.md)anzeigen Kostenlos [Öffnen einer Anfrage](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) Kontingent zu erhöhen.

*   **Microsoft Office-Lizenz** - Bereitstellung von Compute-Knoten mit einem Markt HPC Pack VM Image mit Microsoft Excel eine 30-Tage-Evaluierungsversion von Microsoft Excel Professional Plus 2013 ist installiert. Nach der Evaluierungsphase müssen Sie eine gültige Microsoft Office-Lizenz aktivieren Excel weiterhin Arbeitslasten bereitzustellen. Finden Sie unter [Excel-Aktivierung](#excel-activation) in diesem Artikel. 


## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Schritt 1. Einrichten eines Clusters HPC Pack in Azure

Wir zeigen zwei Optionen Cluster einrichten: erste Azure Schnellstart-Vorlage mit der Azure-Portal; und Zweitens ein Bereitstellungsskript Azure PowerShell verwenden.


### <a name="option-1-use-a-quickstart-template"></a>Option 1. Schnellstart-Vorlage verwenden
Verwenden einer Vorlage Azure Schnellstart, schnell und einfach einen HPC Pack Cluster im Azure-Portal bereitstellen. Beim Öffnen der Vorlage im Portal erhalten Sie eine einfache Benutzeroberfläche, in dem Sie die für Ihren Einstellungen. Folgen die Schritten. 

>[AZURE.TIP]Verwenden Sie eine [Azure Marketplace-Vorlage](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) , die einen ähnlichen Cluster speziell für Excel-Arbeitslasten erstellt. Die Schritte unterscheiden sich geringfügig von der folgenden.

1.  Besuchen Sie die [Vorlagenseite auf GitHub HPC-Cluster erstellen](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Wenn Sie möchten, Informationen Sie über die Vorlage und den Quellcode

2.  Klicken Sie zum start einer Bereitstellung mit der Vorlage im Azure-Portal **Bereitstellen in Azure** .

    ![Vorlage für Azure bereitstellen][github]

3.  Folgendermaßen Sie im Portal vor, um die Parameter für die HPC-Cluster-Vorlage eingeben.

    ein. Geben Sie auf der Seite **Parameter** ein oder ändern Sie die Werte für die Vorlagenparameter. (Klicken Sie auf neben jeder Einstellung für Hilfe.) Werte werden im folgenden Bildschirm angezeigt. Dieses Beispiel erstellt einen Cluster mit dem Namen *hpc01* im Bereich Head-Knoten aus *hpc.local* und 2 compute-Knoten. Compute-Knoten werden aus einem Bild HPC Pack VM erstellt Microsoft Excel.

    ![Geben Sie Parameter][parameters]

    >[AZURE.NOTE]Der Head-Knoten VM wird automatisch vom [aktuellen Markt Bild](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) HPC Pack 2012 R2 Windows Server 2012 R2 erstellt. Derzeit das Bild HPC Pack 2012 R2 Update 3 basiert.
    >
    >Compute-Knoten VMs aus dem aktuellen Bild der ausgewählten Compute Node-Familie erstellt werden. Die Option **ComputeNodeWithExcel** für die neuesten HPC Pack Compute Knoten Image, das eine Evaluierungsversion von Microsoft Excel Professional Plus 2013 enthält. Um einen Cluster für allgemeine SOA-Sessions oder Excel UDF Verschiebung bereitzustellen, wählen Sie **ComputeNode** (ohne Excel installiert).

    b. Wählen Sie das Abonnement.

    c. Erstellen Sie eine Ressourcengruppe für den Cluster wie *hpc01RG*.

    d. Wählen Sie einen Speicherort für die Ressourcengruppe wie USA.

    e. Überprüfen Sie auf der Seite **rechtlich** Begriffe. Wenn Sie zustimmen, klicken Sie auf **kaufen**. Wenn Sie die Werte für die Vorlage festgelegt haben, klicken Sie **Erstellen**.

4.  Wenn die Bereitstellung abgeschlossen ist (in der Regel dauert ca. 30 Minuten), exportieren Cluster Zertifikat aus dem Head-Knoten des Clusters. In einem späteren Schritt importieren Sie das öffentliche Zertifikat der Clientcomputer die serverseitige Authentifizierung für sicheren HTTP-Bindung.

    ein. Verbinden Sie mit dem Head-Knoten von Remotedesktop von Azure-Portal.

     ![Verbinden Sie mit dem Head-Knoten][connect]

    b. Verwenden Sie Standardverfahren in Certificate Manager das Head-Knoten-Zertifikat (befindet sich unter Cert: \LocalMachine\My) ohne den privaten Schlüssel exportieren. In diesem Beispiel exportieren *CN = hpc01.eastus.cloudapp.azure.com*.

    ![Exportieren Sie das Zertifikat][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>Option 2. HPC Pack IaaS Bereitstellungsskript verwenden

Das Bereitstellungsskript HPC Pack IaaS ermöglicht anderen vielseitige HPC Pack-Cluster bereitstellen. Es erstellt einen Cluster im klassischen Bereitstellungsmodell, während die Vorlage Bereitstellungsmodell Azure-Ressourcen-Manager verwendet. Außerdem ist das Skript mit einem Abonnement der Azure oder Global Azure China Service kompatibel.

**Zusätzliche Komponenten**

* **Azure PowerShell** - [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) (Version 0.8.10 oder höher) auf dem Clientcomputer.

* **Bereitstellungsskript HPC Pack IaaS** - herunterladen und Entpacken Sie die neueste Version des Skripts im [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Überprüfen Sie die Version des Skripts mit `New-HPCIaaSCluster.ps1 –Version`. Dieser Artikel basiert auf Version 4.5.0 oder höher des Skripts.

**Erstellen der Konfigurationsdatei**

 Das Bereitstellungsskript HPC Pack IaaS verwendet XML-Konfigurationsdatei als Eingabe, die die Infrastruktur des HPC-Cluster. Ersetzen Sie bereitzustellende einen Cluster mit Head-Knoten und 18 Compute-Knoten aus dem Compute-Knotenbild, die Microsoft Excel erstellt Werte für Ihre Umgebung in der folgenden Beispiel-Konfigurationsdatei. Informationen zur Konfigurationsdatei finden Sie unter der Manual.rtf-Datei im Skriptordner und [erstellen eine HPC-Cluster mit HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Hinweise zur Konfigurationsdatei**

* **VMName** des Head-Knotens **muss** der **ServiceName**identisch sein oder SOA Aufträge nicht ausführen.

* Stellen Sie sicher, dass Sie **EnableWebPortal** angeben, sodass das Head-Knoten-Zertifikat generiert und exportiert wird.

* Die Datei gibt eine nach der Powershellskript PostConfig.ps1 auf dem Head-Knoten ausgeführt wird. Das folgende Beispielskript Verbindungszeichenfolge Azure-Speicher konfiguriert, den Headknoten Compute-Knoten-Rolle entfernt und bringt alle Knoten online bereitgestellt werden. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Führen Sie das Skript**

1.  Öffnen Sie PowerShell auf dem Clientcomputer als Administrator.

2.  Wechseln Sie zu der Skriptordner (in diesem Beispiel E:\IaaSClusterScript).

    ```
    cd E:\IaaSClusterScript
    ```
    
3.  Führen Sie den folgenden Befehl zum Bereitstellen des Clusters HPC Pack. Es wird angenommen, dass die Konfigurationsdatei im E:\HPCDemoConfig.xml befindet.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

Das Bereitstellungsskript HPC Pack wird für einige Zeit ausgeführt. Was das Skript wird zu exportieren und das Cluster-Zertifikat im Dokumente-Ordner des Benutzers auf dem Clientcomputer gespeichert. Das Skript generiert eine Meldung ähnlich der folgenden. Im folgenden Schritt importieren Sie das Zertifikat im entsprechenden Zertifikatspeicher.    
    
    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Schritt 2. Verschieben von Excel-Arbeitsmappen und UDF-Dateien von einem lokalen Client ausführen

### <a name="excel-activation"></a>Excel-Aktivierung

Beim ComputeNodeWithExcel VM-Image Produktionsarbeitslasten müssen Sie einen gültigen Lizenzschlüssel von Microsoft Office Excel auf den Compute-Knoten aktivieren bereitstellen. Andernfalls wird die Evaluierungsversion von Excel nach 30 Tagen und mit Excel-Arbeitsmappen mit COMException (0x800AC472) fehl. 

Rearm Excel weitere 30 Tage Evaluierung Zeit: Anmelden Head-Knoten und Clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` auf alle Excel compute-Knoten über HPC Cluster Manager. Sie können maximal zwei rearm. Danach müssen Sie einen gültigen Lizenzschlüssel Office bereitstellen.

Office Professional Plus 2013 auf VM-Image installiert ist eine Edition mit einer generischen Volume License Key (GVLK). Sie können es über Key Management Service (KMS) / Active Directory-Based-Aktivierung (AD-BA) oder mehrere Activation Key (MAK). 

    * AD/KMS-BA, vorhandenen KMS Server verwenden oder eine neue Microsoft Office 2013 Volume Lizenzpaket mit eingerichtet. (Wenn Sie möchten, richten Sie Server auf dem Head-Knoten.) Aktivieren Sie den KMS-Hostschlüssel per Internet oder Telefon. Dann Clusrun `ospp.vbs` km-Server und Port und aktivieren Sie alle Office Excel compute-Knoten. 
    
    * MAK, erste Clusrun mit `ospp.vbs` den Schlüssel und aktivieren Sie alle Excel compute-Knoten per Internet oder Telefon. 

>[AZURE.NOTE]Retail Product Keys für Office Professional Plus 2013 können mit diesem virtuellen Computer verwendet werden. Haben Sie gültigen Schlüssel und Installationsmedien für Office oder Excel-Versionen als Ausgabe dieser Office Professional Plus 2013, können Sie diese stattdessen verwenden. Zuerst deinstallieren Sie dieser Ausgabe und installieren Sie Ausgabe, die Sie. Neu installierte Excel Compute-Knoten kann als benutzerdefinierte VM Bild mithilfe einer Bereitstellung Ebene erfasst werden.

### <a name="offload-excel-workbooks"></a>Verschieben von Excel-Arbeitsmappen

Führen Sie diese Schritte, um eine Excel-Arbeitsmappe verschieben, damit auf dem HPC Pack Cluster in Azure ausgeführt wird. Hierzu muss Excel 2010 oder 2013 auf dem Clientcomputer installiert sein.

1. Verwenden Sie eine der Optionen in Schritt 1 einen HPC Pack Cluster mit Excel bereitstellen berechnen Knotenbild. Cluster-Zertifikatdatei (CER) und Cluster Benutzername und Kennwort zu erhalten.

2. Importieren Sie auf dem Clientcomputer unter Cert: \CurrentUser\Root Cluster-Zertifikat.

3. Stellen Sie sicher, dass Excel installiert ist. Erstellen Sie eine Excel.exe.config-Datei mit folgendem Inhalt im gleichen Ordner wie Excel.exe auf dem Clientcomputer. Dadurch wird sichergestellt, dass das HPC Pack 2012 R2 Excel-add-in wurde erfolgreich geladen.

    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
    
4.  Einrichten des Clients Aufträge HPC Pack Cluster senden. Eine Möglichkeit ist die vollständige [Installation HPC Pack 2012 R2 Update 3](http://www.microsoft.com/download/details.aspx?id=49922) herunterladen und Installieren des HPC Pack-Clients. Alternativ herunter, und installieren Sie die [Clientdienstprogramme HPC Pack 2012 R2 Update 3](https://www.microsoft.com/download/details.aspx?id=49923) und die entsprechenden Visual C++ 2010 redistributable Computers ([X64](http://www.microsoft.com/download/details.aspx?id=14632), [X86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.  In diesem Beispiel verwenden wir ein Beispiel-Arbeitsblatt mit dem Namen ConvertiblePricing_Complete.xlsb. Sie können [hier](https://www.microsoft.com/en-us/download/details.aspx?id=2939).

6.  Kopieren Sie die Excel-Arbeitsmappe in einen Ordner wie D:\Excel\Run.

7.  Öffnen Sie die Excel-Arbeitsmappe. Auf der Multifunktionsleiste **entwickeln** auf **COM-Add-Ins** und bestätigen Sie, dass das HPC Pack Excel-add-in erfolgreich geladen wurde.

    ![Excel-add-in für HPC Pack][addin]

8.  Bearbeiten der VBA-Makros in Excel HPCControlMacros kommentierten Zeilen ändern, wie im folgenden Skript gezeigt. Ersetzen durch die entsprechenden Werte für Ihre Umgebung.

    ![Excel-Makro für HPC Pack][macro]

    ```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"

    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles

    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
```

9.  Kopieren Sie die Excel-Arbeitsmappe ein Upload-Verzeichnis wie D:\Excel\Upload. Dieses Verzeichnis wird in der HPC_DependsFiles-Konstante im VBA-Makro angegeben.

10. Führen Sie die Arbeitsmappe auf dem Cluster in Azure klicken Sie **Cluster** auf dem Arbeitsblatt.

### <a name="run-excel-udfs"></a>Führen Sie Excel UDFs

So Excel UDFs führen Sie die vorherigen Schritte 1 bis 3 auf dem Clientcomputer einrichten. Für Excel UDFs müssen Sie die Excel-Anwendung auf Compute-Knoten installiert haben. Also beim Erstellen des Clusters compute-Knoten, können Sie eine normale anstelle der Compute Knotenbild mit Excel berechnet.

>[AZURE.NOTE] Gibt maximal 34 Zeichen in Excel 2010 und 2013 Cluster Connector-Dialogfeld. Sie verwenden dieses Dialogfeld Cluster an, der die UDF ausgeführt wird. Ist der vollständige Clustername länger (z. B. hpcexcelhn01.southeastasia.cloudapp.azure.com) nicht in das Dialogfeld passen. Die Lösung ist zum Festlegen einer computerweiten Variablen wie *CCP_IAASHN* mit dem Wert des long Clusternamen. Geben Sie *% CCP_IAASHN %* im Dialogfeld als Clustername Head-Knoten. 

Nach erfolgreicher Cluster Bereitstellung weiterhin mit den folgenden Schritten eine integrierte Beispiel ausführen Excel UDF. Benutzerdefinierte Excel UDFs finden Sie [Ressourcen](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) erstellen die XLLs und IaaS-Cluster bereitstellen.

1.  Öffnen Sie eine neue Excel-Arbeitsmappe. Klicken Sie auf der Multifunktionsleiste **entwickeln** auf **Add-Ins**. Klicken Sie im Dialogfeld auf **Durchsuchen**, navigieren Sie zum Ordner %CCP_HOME%Bin\XLL32 und Stichprobe ClusterUDF32.xll. Wenn die ClusterUDF32 auf dem Clientcomputer vorhanden ist, kopieren Sie sie aus dem Ordner %CCP_HOME%Bin\XLL32 auf dem Head-Knoten.

    ![Wählen Sie das UDF][udf]

2.  Klicken Sie auf **Datei** > **Optionen** > **Erweitert**. Überprüfen Sie **Formeln** **Zulassen XLL Funktionen Compute-Cluster ausgeführt**. Klicken Sie auf **Optionen** und geben Sie vollständige Cluster **Headknoten Clustername**. (Wie bereits erwähnt ist zuvor dieses Eingabefeld auf 34 Zeichen so lange Clusternamen nicht passen. Sie können hier eine Computer-Variable für lange Clusternamen verwenden.)

    ![Konfigurieren der UDF-Datei][options]

3.  Führen Sie die UDF-Berechnung auf dem Cluster klicken Sie auf die Zelle mit dem Wert =XllGetComputerNameC() und drücken Sie die EINGABETASTE. Die Funktion ruft einfach den Namen der Computerknoten, auf dem der UDF-Datei ausgeführt wird. Für die erste Ausführung aufgefordert, ein Dialogfeld Anmeldeinformationen Benutzername und Kennwort für die Verbindung zum Cluster IaaS.

    ![UDF ausführen][run]

    Gibt es viele Zellen zu berechnen, drücken Sie Alt-Umschalt-Strg + F9 die Berechnung auf alle Zellen ausgeführt.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Schritt 3. Eine SOA-Arbeitslast von einem lokalen Client ausführen

Führen Sie allgemeine SOA-Applikationen HPC Pack IaaS-Cluster zunächst mit einer der Methoden in Schritt 1 Cluster bereitstellen. Geben Sie ein generisches Knoten in diesem Fall berechnen an, da Excel auf den Compute-Knoten brauchen. Gehen Sie dann folgendermaßen vor.

1. Nach dem Abrufen von Cluster-Zertifikat auf dem Clientcomputer unter Cert: \CurrentUser\Root importieren.

2. Installieren Sie [HPC Pack 2012 R2-3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) und [HPC Pack 2012 R2 Update 3-Clienttools](https://www.microsoft.com/download/details.aspx?id=49923). Diese Tools ermöglichen das Entwickeln und Ausführen von SOA-Clientanwendungen.

3. HelloWorldR2- [Beispielcode](https://www.microsoft.com/download/details.aspx?id=41633)herunterladen Öffnen Sie die HelloWorldR2.sln in Visual Studio 2010 oder 2012.

4. Erstellen Sie zuerst das Projekt EchoService. Bereitstellen des Diensts dann IaaS Cluster genauso zu einem lokalen Cluster bereitstellen. Ausführliche Schritte finden Sie in der Infodatei im HelloWordR2. Ändern Sie und erstellen Sie die HellWorldR2 und andere Projekte wie im folgenden Abschnitt beschrieben SOA-Clientanwendungen generieren, die in Azure IaaS-Cluster ausgeführt.

### <a name="use-http-binding-with-azure-storage-queue"></a>Verwenden Sie HTTP-Bindung mit Azure-Speicher-Warteschlange

Um HTTP-Bindung mit einer Warteschlange Azure-Speicher verwenden, ändern Sie einige des Beispielcodes.

* Aktualisieren Sie den Namen des Clusters.

    ```
// Before
const string headnode = "[headnode]";
// After e.g.
const string headnode = "hpc01.eastus.cloudapp.azure.com";
or
const string headnode = "hpc01.cloudapp.net";
```

* Optional, verwenden Sie den TransportScheme in SessionStartInfo oder explizit auf Http.

```
    info.TransportScheme = TransportScheme.Http;
```

* Verwenden Sie für die BrokerClient verwendet.

    ```
// Before
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
// After
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
```

    Oder explizit mit BasicHttpBinding.

    ```
BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
```

* Festlegen Sie das UseAzureQueue-Flag auf True SessionStartInfo optional. Wenn nicht festgelegt, es wird festgelegt, wenn der Clustername Azure-Suffixe und die TransportScheme Http ist standardmäßig auf true.

    ```
    info.UseAzureQueue = true;
```

###<a name="use-http-binding-without-azure-storage-queue"></a>HTTP-Bindung ohne Warteschlange Azure-Speicher

HTTP-Bindung ohne eine Warteschlange Azure-Speicher explizit verwendet festgelegt das UseAzureQueue-Flag auf False in der SessionStartInfo.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>NetTcp Bindung

Um NetTcp Bindung verwenden, ist die Konfiguration mit einem lokalen Cluster ähnlich. Sie müssen einige Endpunkte auf dem Head-Knoten VM öffnen. Wenn Sie das Bereitstellungsskript HPC Pack IaaS Erstellen des Clusters verwendet, z. B. Legen Sie die Endpunkte klassischen Azure-Portal wie folgt.


1. Beenden Sie den virtuellen Computer.

2. Fügen Sie die TCP-Ports 9090, 9087, 9091, 9094 für die Sitzung Broker, Broker Arbeitskraft und Datendienste

    ![Endpunkte konfigurieren][endpoint]

3. Starten Sie VM.

Die SOA-Clientanwendung erfordert keine Änderungen außer Head Name den vollständigen IaaS Clusternamen ändern.

## <a name="next-steps"></a>Nächste Schritte

* [Diese Ressourcen](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) Weitere Informationen zu Excel-Arbeitslasten mit HPC Pack angezeigt.

* Weitere [SOA-Dienste verwalten, Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) zum Bereitstellen und Verwalten von SOA-Dienste mit HPC Pack.

<!--Image references-->
[scenario]: ./media/virtual-machines-windows-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-windows-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-windows-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-windows-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-windows-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-windows-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-windows-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-windows-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-windows-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-windows-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-windows-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-windows-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-windows-excel-cluster-hpcpack/udf.png
