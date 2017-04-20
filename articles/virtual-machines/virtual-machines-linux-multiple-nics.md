<properties
   pageTitle="Linux VM mit mehreren NICs erstellen | Microsoft Azure"
   description="Erfahren Sie, wie Linux VM mit mehreren NICs verknüpft den Azure-CLI oder Ressourcen-Manager Vorlagen erstellen."
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
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-linux-vm-with-multiple-nics"></a>Linux VM erstellen mit mehreren NICs
Eine virtuellen Maschine (VM) kann in Azure erstellen, die mehrere virtuelle Netzwerkkarten (NICs) zugeordnet. Ein häufiges Szenario wäre Subnetzen für Front-End- und Back-End-Verbindung oder ein Netzwerk zu überwachen oder backup-Lösung. Dieser Artikel bietet Schnellzugriff erstellen einen virtuellen Computer mit mehreren NICs angefügt. Ausführliche Informationen, wie mehrere Netzwerkkarten mit eigener Skripts Bash erstellen mehr zum [Bereitstellen von Multi-NIC VMs](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Verschiedene [Größen VM](virtual-machines-linux-sizes.md) unterstützen eine unterschiedliche Anzahl von Netzwerkkarten, so entsprechend Größe Ihrer VM.

>[AZURE.WARNING] Beim Erstellen einer VM - Sie können eine vorhandene VM Netzwerkkarten hinzufügen, müssen Sie mehrere NICs anfügen. Erstellen [eine VM basierend auf den ursprünglichen virtuellen Laufwerken](virtual-machines-linux-copy-vm.md) und mehrere Netzwerkkarten VM bereitstellen.

## <a name="quick-commands"></a>Schnellzugriff
Achten Sie [Azure CLI](../xplat-cli-install.md) angemeldet und Ressourcen-Manager-Modus:

```bash
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Parameternamen wird durch Ihre eigenen Werte. Beispiel-Parameternamen enthalten `myResourceGroup`, `mystorageaccount`, und `myVM`.

Erstellen Sie zunächst eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUS` Speicherort:

```bash
azure group create myResourceGroup -l WestUS
```

Erstellen Sie ein Speicherkonto Ihrer virtuellen Computer enthält. Im folgenden Beispiel wird ein Speicherkonto namens `mystorageaccount`:

```bash
azure storage account create mystorageaccount -g myResourceGroup \
    -l WestUS --kind Storage --sku-name PLRS
```

Erstellen Sie ein virtuelles Netzwerk anschließen der VMs. Das folgende Beispiel erstellt ein virtuelles Netzwerk mit dem Namen `myVnet` mit einem Adresspräfix `192.168.0.0/16`:

```bash
azure network vnet create -g myResourceGroup -l WestUS \
    -n myVnet -a 192.168.0.0/16
```

Erstellen Sie zwei virtuelle Netzwerk-Subnetze - Verkehr Front-End-und Back-End-Datenverkehr. Das folgende Beispiel erstellt zwei Subnetzen mit dem Namen `mySubnetFrontEnd` und `mySubnetBackEnd`:

```bash
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetFrontEnd -a 192.168.1.0/24
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetBackEnd -a 192.168.2.0/24
```

Erstellen Sie zwei Netzwerkkarten im Back-End-Subnetz eine Netzwerkkarte mit dem Front-End-Subnetz und eine Netzwerkkarte zuweisen. Das folgende Beispiel erstellt zwei Netzwerkkarten mit dem Namen `myNic1` und `myNic2`, und fügt sie an die Subnetze:

```bash
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic1 -m myVnet -k mySubnetFrontEnd
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic2 -m myVnet -k mySubnetBackEnd
```

Erstellen Sie die VM befestigen die beiden Karten, die Sie zuvor erstellt haben. Im folgenden Beispiel wird einen virtueller Computer mit dem Namen `myVM`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-azure-cli"></a>Erstellen mehrere Netzwerkkarten mit Azure-CLI
Wenn einen virtueller Computer mithilfe der Azure-CLI zuvor erstellt haben, sollten den Schnellzugriff vertraut sein. Der Vorgang entspricht eine NIC oder mehreren NICs erstellen. Lesen Sie mehr über [mehrere Netzwerkkarten mit CLI Azure bereitstellen](../virtual-network/virtual-network-deploy-multinic-arm-cli.md), wie des Prozess durchlaufen alle Netzwerkkarten zu erstellen.

Das folgende Beispiel erstellt zwei Netzwerkkarten mit dem Namen `myNic1` und `myNic2`, mit einer NIC mit jedem Subnetz:

```bash
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic1 --subnet-vnet-name myVnet --subnet-name mySubnetFrontEnd
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic2 --subnet-vnet-name myVnet --subnet-name mySubnetBackEnd
```

Normalerweise erstellen Sie auch eine [Netzwerk-Sicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) oder [Lastenausgleich](../load-balancer/load-balancer-overview.md) und Datenverkehr auf die VMs verteilen. Auch stimmen die Befehle beim Arbeiten mit mehreren NICs. Im folgenden Beispiel wird eine Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location WestUS \
    --name myNetworkSecurityGroup
```

Binden Sie Ihre Netzwerkkarten mit der Netzwerk-Sicherheitsgruppe `azure network nic set`. Im folgenden Beispiel wird `myNic1` und `myNic2` mit `myNetworkSecurityGroup`:

```bashazure 
azure network nic set --resource-group myResourceGroup --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set --resource-group myResourceGroup --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

Wenn die VM zu erstellen, geben Sie jetzt mehrere Netzwerkkarten. Vielmehr `--nic-name` zu einer einzelnen NIC Sie stattdessen `--nic-names` und eine kommagetrennte Liste der Netzwerkkarten. Außerdem müssen vorsichtig die VM-Größe auswählen. Gibt es Grenzwerte für die Gesamtzahl der Netzwerkkarten eine VM hinzugefügt werden können. Weitere Informationen über [Linux VM-Größen](virtual-machines-linux-sizes.md). Im folgenden Codebeispiel wird veranschaulicht, wie mehrere Netzwerkkarten und eine VM-Größe unterstützt die Verwendung mehrerer NICs (`Standard_DS2_v2`):

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Mehrere Netzwerkkarten mit Ressourcen-Manager Vorlagen erstellen
Azure Schablonen Ressourcenmanager declarative JSON-Dateien Ihrer Umgebung definieren. Lesen Sie eine [Übersicht der Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md). Ressourcen-Manager Vorlagen bieten eine Möglichkeit, mehrere Instanzen einer Ressource während der Bereitstellung wie erstellen mehrere Netzwerkkarten erstellen. *Kopie* verwenden an die Anzahl der Instanzen zu:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Weitere Informationen zum [Erstellen mehrerer Instanzen *Kopie*](../resource-group-create-multiple.md). 

Können Sie auch eine `copyIndex()` dann einen Ressourcennamen angefügt erstellen ermöglicht eine Reihe `myNic1`, `myNic2`usw.. Die folgenden zeigt ein Beispiel den Indexwert anzufügen:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Lesen Sie ein vollständiges Beispiel für [mehrere Netzwerkkarten mit Ressourcen-Manager Vorlagen erstellen](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie [Linux VM Größen](virtual-machines-linux-sizes.md) beim Erstellen eines virtuellen Computers mit mehreren Netzwerkkarten. Achten Sie auf maximale Anzahl Netzwerkkarten jede VM-Größe unterstützt. 

Beachten Sie, dass die vorhandenen VM keine zusätzliche Netzwerkkarten hinzufügen, müssen Sie alle Netzwerkkarten erstellen, wenn Sie den virtuellen Computer bereitstellen. Achten Sie bei der Planung der Bereitstellung sicherstellen, dass die erforderliche Netzwerkkonnektivität von Anfang an haben.