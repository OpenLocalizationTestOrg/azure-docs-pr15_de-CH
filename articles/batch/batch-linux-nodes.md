<properties
    pageTitle="Linux-Nodes in Azure Batch Pools | Microsoft Azure"
    description="Erfahren Sie, wie die parallele Berechnung Arbeitslasten auf Pools virtuelle Linux-Computer in Azure Batch verarbeitet."
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="marsma" />

# <a name="provision-linux-compute-nodes-in-azure-batch-pools"></a>Bereitstellung von Linux Computeknoten in Azure Batch-pools

Azure Batch können parallel Computing Auslastung auf Linux und Windows virtuelle Maschinen ausführen. Dieser Artikel enthält Informationen zum Erstellen von Pools von Linux Compute-Knoten im Batch-Dienst sowohl die [Stapelverarbeitung Python] mit[ py_batch_package] und [Batch.NET] [ api_net] Client-Bibliotheken.

> [AZURE.NOTE] [Pakete](batch-application-packages.md) werden derzeit nicht unterstützt auf Linux Compute-Knoten.

## <a name="virtual-machine-configuration"></a>Konfiguration des virtuellen Computers

Beim Erstellen eines Pools von Compute-Knoten im Stapel haben zwei Optionen, Knotengröße und Betriebssystem auswählen: Cloud Services-Konfiguration und Konfiguration des virtuellen Computers.

**Cloud-Services-Konfiguration** bietet Windows Knoten *nur*berechnen. Verfügbaren Compute-Knoten-Größen sind in [Größen für Cloud-Services](../cloud-services/cloud-services-sizes-specs.md)und Betriebssysteme in [Azure Gast-BS-Versionen und SDK-Kompatibilitätsmatrix](../cloud-services/cloud-services-guestos-update-matrix.md)aufgelistet. Beim Erstellen eines Pools, das Azure Cloud Services Knoten müssen Sie nur die Knotengröße und seine "OS-Produktfamilie" in den oben erwähnten Artikeln gefunden. Pools Windows Knoten berechnen, am häufigsten Cloud-Diensten verwendet.

**Konfiguration des virtuellen Computers** bietet Linux- und Windows-Abbilder für Compute-Knoten. Verfügbare Compute-Knoten Größen sind in [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) und [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows) aufgeführt. Beim Erstellen eines Pools, das Konfiguration des virtuellen Computers Knoten enthält, müssen Sie die Größe des Knoten Bildverweis virtuellen und Batch Knoten Agent SKU auf Knoten angeben.

### <a name="virtual-machine-image-reference"></a>Virtual Machine Bildverweis

Batch-Dienst verwendet [virtuelle Computer Maßstab legt fest](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) zu Linux Compute-Knoten. Betriebssystemabbilder für diese virtuellen Computer werden von [Azure Marketplace]bereitgestellt[vm_marketplace]. Beim Konfigurieren eines virtuellen Verweis Geben Sie die Eigenschaften eines Bildes virtuellen Markt. Folgenden Eigenschaften sind erforderlich, wenn Sie einen virtuellen Computer Image erstellen:

| **Eigenschaften für Image-Referenz** | **Beispiel** |
| ----------------- | ------------------------ |
| Publisher         | Kanonische                |
| Angebot             | UbuntuServer             |
| SKU               | 14.04.4-LTS              |
| Version           | neueste                   |

> [AZURE.TIP] Sie können diese Eigenschaften und wie Marketplace Bilder [Navigieren](../virtual-machines/virtual-machines-linux-cli-ps-findimage.md)und select Linux VM in Azure CLI oder PowerShell erfahren. Beachten Sie, dass nicht alle Bilder Markt derzeit mit kompatibel sind. Weitere Informationen finden Sie unter [Knoten Agents SKU](#node-agent-sku).

### <a name="node-agent-sku"></a>Knoten Agent SKU

Der Stapel Knoten Agent ist ein Programm, das wird auf jedem Knoten im Pool und die Befehl Steuerelement Schnittstelle zwischen dem Knoten und Batch-Dienst. Gibt es andere Implementierungen von Knoten-Agent genannt SKUs für verschiedene Betriebssysteme. Im Wesentlichen zur Erstellung einer Konfiguration des virtuellen Computers zuerst den virtuellen Verweis angeben und Sie Knoten Agent installieren das Bild angeben. Normalerweise ist jeder Knoten Agent SKU mit mehreren virtuellen Bildern kompatibel. Hier sind einige Beispiele für Knoten Agent SKUs:

* Batch.Node.Ubuntu 14.04
* Batch.Node.CentOS 7
* Batch.Node.Windows amd64

> [AZURE.IMPORTANT] Nicht alle auf dem Markt verfügbaren virtuellen Computer-Images sind kompatibel mit derzeit verfügbaren Batch Knoten Agents. Batch-SDKs verwenden verfügbaren Knoten Agent SKUs und Abbilder virtueller Computer mit denen kompatibel sind. [Liste der virtuellen Computer Bilder](#list-of-virtual-machine-images) in diesem Artikel Weitere Informationen anzeigen

## <a name="create-a-linux-pool-batch-python"></a>Erstellen eines Linux-Pools: Batch Python

Der folgende Codeausschnitt zeigt ein Beispiel der [Microsoft Azure Batch-Clientbibliothek für Python] [ py_batch_package] zum Erstellen eines Pools von Ubuntu Server Compute-Knoten. Referenzdokumentation für die Stapelverarbeitung Python-Modul finden Sie auf [azure.batch] [py_batch_docs] beim Lesen der Dokumentation.

Dieser Codeausschnitt erstellt ein [ImageReference] [ py_imagereference] explizit und seiner Eigenschaften (Verleger, Angebot, SKU, Version). In Produktionscode wir empfehlen jedoch die Verwendung von [List_node_agent_skus] [ py_list_skus] Methode und dem Bild und Knoten Agent SKU Kombinationen zur Laufzeit auswählen.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Wie bereits erwähnt, empfehlen wir anstatt [ImageReference] [ py_imagereference] , verwenden [List_node_agent_skus] [ py_list_skus] Methode dynamisch von aktuell unterstützten Knoten Agent-Marktplatz Bild Kombinationen auswählen. Im folgenden Codeausschnitt Python wird die Verwendung dieser Methode.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Erstellen eines Linux-Pools: Batch .NET

Der folgende Codeausschnitt zeigt ein Beispiel für Verwendung der [Stapelverarbeitung .NET] [ nuget_batch_net] -Clientbibliothek einen Pool von Ubuntu Server Compute-Knoten erstellt. Der [Batch .NET Referenzdokumentation] finden[ api_net] auf MSDN.

Der folgende Codeausschnitt verwendet [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] Methode zum Auswählen aus der Liste der derzeit unterstützten Marketplace Bild und Knoten Agent SKU Kombinationen. Diese Technik ist wünschenswert, da die Liste der unterstützten Kombinationen von Zeit zu Zeit ändern kann. Am häufigsten werden die unterstützte Kombinationen hinzugefügt.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.SkuId.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicated: nodeCount);

// Commit the pool to the Batch service
pool.Commit();
```

Codeausschnitt verwendet [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] Methode dynamisch auflisten und unterstützt Bild- und Knoten Agent SKU Kombinationen (empfohlen), Sie können auch eine [ImageReference] [ net_imagereference] explizit:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    skuId: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Liste der Images von virtuellen Maschinen

Die folgende Tabelle listet die Marketplace virtuellen Bilder mit verfügbaren Batch Knoten Agents bei diesem Artikel zuletzt aktualisiert wurde. Es ist wichtig zu beachten, dass diese Liste nicht endgültigen da Bilder und Knoten Agents hinzugefügt oder jederzeit entfernt werden. Wir empfehlen die Batch-Programme und Dienste immer [List_node_agent_skus] [ py_list_skus] (Python) und [ListNodeAgentSkus] [ net_list_skus] (Batch .NET) und die derzeit verfügbaren SKUs auswählen.

> [AZURE.WARNING] Die folgende Liste kann jederzeit ändern. Verwenden Sie immer die **Liste Knoten Agent SKU** Methoden in Batch-APIs und wählen Sie kompatible virtuelle Maschine und Knoten Agent SKUs bei der Stapelverarbeitung.

| **Publisher** | **Angebot** | **Bild SKU** | **Version** | **Knoten Agent SKU-ID** |
| ------- | ------- | ------- | ------- | ------- |
| Kanonische | UbuntuServer | 14.04.0-LTS | neueste | Batch.Node.Ubuntu 14.04 |
| Kanonische | UbuntuServer | 14.04.1-LTS | neueste | Batch.Node.Ubuntu 14.04 |
| Kanonische | UbuntuServer | 14.04.2-LTS | neueste | Batch.Node.Ubuntu 14.04 |
| Kanonische | UbuntuServer | 14.04.3-LTS | neueste | Batch.Node.Ubuntu 14.04 |
| Kanonische | UbuntuServer | 14.04.4-LTS | neueste | Batch.Node.Ubuntu 14.04 |
| Kanonische | UbuntuServer | 14.04.5-LTS | neueste | Batch.Node.Ubuntu 14.04 |
| Kanonische | UbuntuServer | 16.04.0-LTS | neueste | Batch.Node.Ubuntu 16.04 |
| Credativ | Debian | 8 | neueste | Batch.Node.Debian 8 |
| OpenLogic | CentOS | 7.0 | neueste | Batch.Node.CentOS 7 |
| OpenLogic | CentOS | 7.1 | neueste | Batch.Node.CentOS 7 |
| OpenLogic | CentOS HPC | 7.1 | neueste | Batch.Node.CentOS 7 |
| OpenLogic | CentOS | 7.2 | neueste | Batch.Node.CentOS 7 |
| Oracle | Oracle-Linux | 7.0 | neueste | Batch.Node.CentOS 7 |
| SUSE | openSUSE | 13.2 | neueste | 13.2 Batch.Node.opensuse |
| SUSE | OpenSUSE-Sprung | 42,1 | neueste | Batch.Node.opensuse 42,1 |
| SUSE | SLES HPC | 12 | neueste | Batch.Node.opensuse 42,1 |
| SUSE | SLES | 12 SP1 | neueste | Batch.Node.opensuse 42,1 |
| Microsoft-anzeigen | Standard-Daten-Science-vm | Standard-Daten-Science-vm | neueste | Batch.Node.Windows amd64 |
| Microsoft-anzeigen | Linux-Daten-Science-vm | linuxdsvm | neueste | Batch.Node.CentOS 7 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2 SP1 | neueste | Batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012 Datacenter | neueste | Batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012 R2 Datacenter | neueste | Batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Windows Server-Technical Preview | neueste | Batch.Node.Windows amd64 |

## <a name="connect-to-linux-nodes"></a>Verbinden mit Linux-Knoten

Während der Entwicklung oder während der Fehlerbehebung können Knoten im Pool bei Dateidialogfelder erforderlich. Remote Desktop Protocol (RDP) können Sie im Gegensatz zu Windows Compute-Knoten mit Linux Knoten verbunden. Stattdessen kann der Batch-Dienst SSH-Zugriff auf jedem Knoten für Remoteverbindung.

Im folgende Codeausschnitt Python erstellt einen Benutzer auf jedem Knoten in einem Pool für remote-Verbindung erforderlich ist. Die Verbindungsinformationen secure Shell (SSH) für jeden Knoten gedruckt.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Hier wird eine Beispielausgabe für den vorherigen Code für einen Pool, der vier Linux-Knoten enthält:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Beachten Sie, dass anstelle eines Kennworts Sie beim Erstellen eines Benutzers auf einem Knoten einen öffentlichen SSH-Schlüssel angeben können. Im SDK Python dies geschieht anhand des Parameters **Ssh_public_key** [ComputeNodeUser][py_computenodeuser]. In .NET wird dazu die [ComputeNodeUser][net_computenodeuser]. [SshPublicKey] [net_ssh_key] Eigenschaft.

## <a name="pricing"></a>Preisgestaltung

Azure Batch basiert auf Azure Cloud Services und Azure Virtual Machines. Der Batch-Dienst wird kostenlos angeboten, dass nur die Serverressourcen berechnet werden bedeutet, dass Stapel Projektmappen verwenden. Wenn Sie **Cloud-Services-Konfiguration**wählen, berechnet auf [Clouddienste Preise] [ cloud_services_pricing] Struktur. Bei der Auswahl der **Konfiguration des virtuellen Computers**berechnet basierend auf [virtuellen Maschinen Preise] [ vm_pricing] Struktur.

## <a name="next-steps"></a>Nächste Schritte

### <a name="batch-python-tutorial"></a>Batch Python-Lernprogramm

Einen ausführlicheren Lernprogramm zum Stapel arbeiten mit Python checken Sie [Einstieg Azure Batch Python-Client aus](batch-python-tutorial.md). Sein Begleiter [Beispielcode] [ github_samples_pyclient] enthält eine Hilfsfunktion `get_vm_config_for_distro`, zeigt, dass eine andere Technik Konfiguration einer virtuellen Maschine zu.

### <a name="batch-python-code-samples"></a>Batch Python-Codebeispiele

Sehen Sie sich die anderen [Python code Samples] [ github_samples_py] in [Azure-Batch-Beispiele] [ github_samples] Repository auf GitHub für verschiedene Skripte, die zeigen, wie Sie allgemeine Stapel Operationen wie Pool, Auftrag und Erstellung. [Infodatei] [ github_py_readme] begleitet, die Python Beispiele enthält Details über die erforderlichen Pakete installieren.

### <a name="batch-forum"></a>Batch-forum

[Azure Batch Forum] [ forum] auf MSDN eignet sich Batch und Fragen zu dem Dienst. "Stickied" hilfreiche Beiträge, und stellen Sie Ihre Fragen, wie sie beim Erstellen von Batch-Projektmappen auftreten.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
