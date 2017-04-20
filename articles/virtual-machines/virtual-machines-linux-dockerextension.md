<properties
   pageTitle="Mit der Erweiterung Azure Andockfenster VM | Microsoft Azure"
   description="Informationen Sie zum Andockfenster VM-Erweiterung verwenden, um schnell und sicher eine Andockfenster Umgebung in Azure Ressourcenmanager Vorlagen bereitstellen."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/25/2016"
   ms.author="iainfou"/>

# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Erstellen einer Umgebung Andockfenster in Azure Andockfenster VM-Erweiterung verwenden
Andockfenster ist eine beliebte Container Management und imaging-Plattform, die schnell mit Linux (und Windows) ermöglicht. In Azure sind verschieden Andockfenster bedarfsgerecht bereitstellen können. Dieser Artikel konzentriert sich auf die Andockfenster VM-Erweiterung und Azure Ressourcenmanager Vorlagen. 

Weitere Informationen zu den verschiedenen Bereitstellungsmethoden einschließlich Andockfenster Computer und Azure Containerdienste finden Sie in folgenden Artikeln:

- Um schnell Prototyp einer Anwendung können Sie einen einzelnen Andockfenster Host [Andockfenster](./virtual-machines-linux-docker-machine.md)Computer erstellen.
- Für größere und stabilere Umgebung können Sie die Erweiterung Azure Andockfenster VM unterstützt konsistente Container Bereitstellung generiert [Andockfenster erstellen](https://docs.docker.com/compose/overview/) verwenden. Dieser Artikeldetails Azure Andockfenster VM-Erweiterung verwenden.
- Um Produktion, skalierbare Umgebung erstellen, die weitere Planung und Management-Tools bereitstellen, können Sie einen [Schwarm Andockfenster Cluster Azure Containerdienste](../container-service/container-service-deployment.md)bereitstellen.


## <a name="azure-docker-vm-extension-overview"></a>Azure Andockfenster VM-Erweiterung (Übersicht)
Azure Andockfenster VM-Erweiterung installiert und konfiguriert die Andockfenster Daemon Andockfenster Client und Andockfenster erstellen in der virtuellen Linux-Maschine (VM). Mithilfe der Azure Andockfenster VM-Erweiterung haben Sie mehr Kontrolle und Funktionen als Andockfenster Computer oder Host Andockfenster selbst erstellen. Diese zusätzlichen Funktionen wie [Andockfenster erstellen](https://docs.docker.com/compose/overview/), stellen geeignete für stabilere Entwickler oder Produktion Azure Andockfenster VM-Erweiterung.

Azure Ressourcenmanager Vorlagen definieren die gesamte Struktur der Umgebung. Vorlagen können Sie Ressourcen wie Andockfenster Host VMs, Speicher, rollenbasierte Zugriffskontrolle (RBAC) und Diagnose erstellen und konfigurieren. Sie können diese Vorlagen erstellen zusätzliche Bereitstellung konsistent verwenden. Weitere Informationen zu Azure-Ressourcen-Manager und Vorlagen finden Sie unter [Übersicht über Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md). 


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Bereitstellen einer Vorlage mit der Azure Andockfenster VM-Erweiterung
Wir verwenden eine Quickstart-Vorlage eine Ubuntu VM erstellen, Azure Andockfenster VM-Erweiterung installieren und Konfigurieren des Andockfenster Hosts verwendet. Sie können die Vorlage anzeigen: [einfache Bereitstellung ein Ubuntu VM mit Andockfenster](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Sie benötigen die [Neueste Azure-CLI](../xplat-cli-install.md) installiert und mit den Ressourcen-Manager-Modus wie folgt angemeldet:

```
azure config mode arm
```

Bereitstellen der Vorlage mithilfe der Azure-CLI Vorlage URI anzugeben. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUS` Ort. Verwenden Sie Ihre eigenen Namen und Speicherort wie folgt:

```
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Fordert das Speicherkonto, geben einen Benutzernamen und ein Kennwort und einen DNS-Namen zu beantworten. Die Ausgabe ist ähnlich dem folgenden Beispiel:

```
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: ops
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicip
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK

```

Der Azure-CLI gibt Sie auf Aufforderung nach nur wenigen Sekunden, sondern die Andockfenster Host noch erstellt und von der Azure Andockfenster VM-Erweiterung konfiguriert. Es dauert einige Minuten für die Bereitstellung abgeschlossen. Können Details über die Andockfenster Host mit den `azure vm show` Befehl.

Das folgende Beispiel überprüft den Status der Bezeichnung VM `myDockerVM` (aus der Vorlage - nicht ändern dieser Name) in der Ressourcengruppe mit dem Namen `myResourceGroup`. Geben Sie den Namen der Ressource, die Sie im vorherigen Schritt erstellt haben:

```bash
azure vm show -g myResourceGroup -n myDockerVM
```

Die Ausgabe der `azure vm show` Befehl ähnelt dem folgenden Beispiel:

```
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicip.westus.cloudapp.azure.com]
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

Im oberen Teil der Ausgabe finden Sie die `ProvisioningState` der VM. Wenn hier `Succeeded`, die Bereitstellung beendet, und Sie können SSH für die VM.

Ende der Ausgabe `FQDN` zeigt den vollqualifizierten Domänennamen des Hosts Andockfenster. Dieser FQDN wird SSH auf den Andockfenster in den verbleibenden Schritten.


## <a name="deploy-your-first-nginx-container"></a>Den ersten Nginx Container bereitstellen
Abschluss die Bereitstellung, SSH auf den neuen Andockfenster vom lokalen Computer. Geben Sie Ihren eigenen Benutzernamen und FQDN:

```bash
ssh ops@mypublicip.westus.cloudapp.azure.com
```

Nach der Anmeldung an den Host Andockfenster führen wir einen Nginx-Container:

```
sudo docker run -d -p 80:80 nginx
```

Die Ausgabe ähnelt dabei Nginx Bild heruntergeladen und Container:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Überprüfen Sie den Status der Container auf Ihrem Host Andockfenster wie folgt ausgeführt:

```
sudo docker ps
```

Die Ausgabe ähnelt dem folgenden Beispiel und zeigen, dass Nginx-Container ausgeführt wird und TCP-Ports 80 und 443 weitergeleitet:

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Um den Container in Aktion zu sehen, öffnen Sie einen Webbrowser, und geben Sie den Vollqualifizierten Domänennamen des Hosts Andockfenster:

![Ausgeführte Ngnix container](./media/virtual-machines-linux-dockerextension/nginxrunning.png)


## <a name="azure-docker-vm-extension-template-reference"></a>Azure Andockfenster VM Erweiterung Vorlage Verweis
Im vorherige Beispiel verwendet eine Quickstart-Vorlage. Sie können auch Azure Andockfenster VM-Erweiterung mit Ressourcen-Manager-Vorlagen bereitstellen. Hierzu fügen Sie Folgendes zu der Ressourcen-Manager-Vorlagen definieren die `vmName` Ihrer VM entsprechend:

```
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

Ausführliche Anleitung Schablonen Ressourcenmanager lesen [Übersicht über Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)finden.


## <a name="next-steps"></a>Nächste Schritte
Sie können [Andockfenster Daemon TCP-Port konfigurieren](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket)möchten [Andockfenster Sicherheit](https://docs.docker.com/engine/security/security/)verstehen oder Bereitstellen von Containern [Andockfenster erstellen](https://docs.docker.com/compose/overview/). Weitere Informationen zu Azure Andockfenster VM Erweiterung selbst finden Sie unter [GitHub-Projekt](https://github.com/Azure/azure-docker-extension/).

Informationen Sie weitere zu zusätzlichen Andockfenster Bereitstellungsoptionen in Azure:

- [Andockfenster Computer Azure Treiber verwenden](./virtual-machines-linux-docker-machine.md)  
- [Andockfenster mit verfassen definieren und Ausführen einer Anwendung mehrere Container Azure virtuellen Computer beginnen](virtual-machines-linux-docker-compose-quickstart.md).
- [Bereitstellen eines Clusters Azure Container Service](../container-service/container-service-deployment.md)
