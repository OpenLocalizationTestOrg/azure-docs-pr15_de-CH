
<properties
   pageTitle="Erstellen eine vollständige Linux Umgebung CLI Azure | Microsoft Azure"
   description="Erstellen Sie Speicher, Linux VM ein virtuelles Netzwerk und Subnetz, ein Lastenausgleich, eine Netzwerkkarte, eine öffentliche IP-Adresse und einer Netzwerk-Sicherheitsgruppe, von Grund auf mit der Azure-CLI."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-complete-linux-environment-by-using-the-azure-cli"></a>Mithilfe der Azure-CLI eine komplette Linux-Umgebung

In diesem Artikel erstellen wir ein einfaches Netzwerk mit einem Lastenausgleich und VMs für Entwicklung und einfache Datenverarbeitung. Wir durchlaufen des Prozesses vom Befehl, bis zwei arbeiten, sichere Linux VMs haben, Sie von überall im Internet verbinden können. Dann können Sie komplexe Netzwerke und Umgebung übergehen.

Dabei lernen Sie die Abhängigkeitshierarchie, dass Ressourcen-Manager-Bereitstellungsmodell bietet, und wie viel Strom. Wenn Sie sehen, wie das System aufgebaut ist, können Sie es mithilfe von [Azure-Ressourcen-Manager Vorlagen](../resource-group-authoring-templates.md)schneller wiederherstellen. Auch nachdem Sie lernen, wie die Teile der Umgebung zusammen, einfacher Vorlagen zu automatisieren.

Die Umgebung enthält:

- Zwei VMs innerhalb einer Verfügbarkeit.
- Ein Lastenausgleich mit Lastenausgleich auf Port 80.
- Gruppe (NSG) Netzwerksicherheitsregeln VM von unerwünschtem Datenverkehr zu schützen.

![Basic-Umgebung (Übersicht)](./media/virtual-machines-linux-create-cli-complete/environment_overview.png)

Zum Erstellen dieses benutzerdefinierten Umgebung benötigen Sie die neueste [Azure CLI](../xplat-cli-install.md) im Ressourcen-Manager-Modus (`azure config mode arm`). Außerdem benötigen Sie ein Tool analysiert JSON. Dieses Beispiel verwendet [Jq](https://stedolan.github.io/jq/).

## <a name="quick-commands"></a>Schnellzugriff
Möchten Sie schnell, die folgenden Abschnitt Details die Base Aufgabe Befehle eine VM in Azure hochladen. Detaillierte Informationen für jeden Schritt im Rest des Dokuments finden ab [hier](#detailed-walkthrough).

Achten Sie [Azure-CLI](../xplat-cli-install.md) angemeldet und Ressourcen-Manager-Modus:

```bash
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Parameternamen wird durch Ihre eigenen Werte. Beispiel-Parameternamen enthalten `myResourceGroup`, `mystorageaccount`, und `myVM`.

Erstellen Sie die Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `westeurope` Speicherort:

```bash
azure group create -n myResourceGroup -l westeurope
```

Überprüfen Sie die Ressourcengruppe mit JSON-Parser:

```bash
azure group show myResourceGroup --json | jq '.'
```

Speicher-Konto zu erstellen. Im folgenden Beispiel wird ein Speicherkonto namens `mystorageaccount` (Speicher-Kontoname muss eindeutig sein, so geben Ihre eigenen eindeutigen Namen):

```bash
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Überprüfen Sie das Speicherkonto mithilfe des JSON-Parsers:

```bash
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Erstellen Sie virtuelle Netzwerk. Das folgende Beispiel erstellt ein virtuelles Netzwerk mit dem Namen `myVnet`:

```bash
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Erstellen Sie ein Subnetz. Im folgenden Beispiel wird ein Subnetz mit dem Namen `mySubnet`"

```bash
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Überprüfen Sie virtuelles Netzwerk und Subnetz mithilfe des JSON-Parsers:


```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Erstellen Sie eine öffentliche IP-Adresse. Im folgenden Beispiel wird eine öffentliche IP-Adresse mit dem Namen `myPublicIP` mit dem DNS-Namen des `mypublicdns` (der DNS-Name muss eindeutig sein, so geben Ihre eigenen eindeutigen Namen):

```bash
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Lastenausgleich zu erstellen. Im folgenden Beispiel wird ein Lastenausgleich mit dem Namen `myLoadBalancer`:

```bash
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Erstellen eines Front-End-IP-Adresspools für Lastenausgleich und öffentliche IP-Adresse zuordnen. Das folgende Beispiel erstellt einen Front-End-IP-Pool namens `mySubnetPool`:

```bash
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool 
```

Erstellen Sie den Back-End-IP-Adresspool für Lastenausgleich. Das folgende Beispiel erstellt einen Back-End-IP-Pool namens `myBackEndPool`:

```bash
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Erstellen Sie Adresse (Netzwerkadressübersetzung) Regeln für Lastenausgleich eingehenden SSH-Netzwerk. Das folgende Beispiel erstellt zwei Load Balancer Regeln `myLoadBalancerRuleSSH1` und `myLoadBalancerRuleSSH2`:

```bash
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Erstellen Sie die eingehende NAT-Regeln für den Lastenausgleich. Das folgende Beispiel erstellt eine Load Balancer Regel`myLoadBalancerRuleWeb`

```bash
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Erstellen Sie Load Balancer-Integritätstest. Das folgende Beispiel erstellt einen TCP-Prüfpunkt namens `myHealthProbe`:

```bash
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Überprüfen Sie mithilfe des JSON-Parsers Lastenausgleich, IP-Pools und NAT-Regeln:

```bash
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Erstellen der ersten Netzwerkschnittstellenkarte (NIC). Ersetzen Sie die `#####-###-###` mit eigenen Azure-Abonnement-ID. Ihr Abonnement ID in der Ausgabe von ist `jq` Ressourcen Prüfung erstellen. Außerdem können Sie Ihre Abonnenten-ID mit `azure account list`. 

Das folgende Beispiel erstellt eine Netzwerkkarte mit dem Namen `myNic1`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Erstellen Sie die zweite Netzwerkkarte. Das folgende Beispiel erstellt eine Netzwerkkarte mit dem Namen `myNic2`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Zwei NICs mithilfe des JSON-Parsers überprüfen:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Die Netzwerk-Sicherheitsgruppe erstellt. Im folgenden Beispiel wird eine Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup`:

```bash
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Fügen Sie zwei eingehende Regeln für die Netzwerk-Sicherheitsgruppe hinzu. Im folgende Beispiel werden zwei Regeln erstellt `myNetworkSecurityGroupRuleSSH` und `myNetworkSecurityGroupRuleHTTP`:

```bash
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Überprüfen Sie die Network Security und eingehende Regeln mithilfe des JSON-Parsers:

```bash
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

Binden Sie Netzwerk-Sicherheitsgruppe an zwei NICs:

```bash
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Erstellen Sie auf Verfügbarkeit. Das folgende Beispiel erstellt eine Verfügbarkeit legen Sie benannte `myAvailabilitySet`:

```bash
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

Erstellen der ersten Linux VM. Im folgenden Beispiel wird einen virtueller Computer mit dem Namen `myVM1`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Erstellen Sie die zweite Linux VM. Im folgenden Beispiel wird einen virtueller Computer mit dem Namen `myVM2`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Verwenden des JSON-Parsers zu überprüfen, ob alles erstellt wurde:

```bash
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Exportieren der neuen Umgebung in eine Vorlage neue Instanzen neu zu erstellen:

```bash
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Ausführliche Anleitung
Die detaillierten Schritte erläutern jeden Befehl tun wird Ihre Umgebung erstellen. Diese Konzepte sind hilfreich, wenn Sie Ihre eigenen benutzerdefinierten Umgebung für die Entwicklung oder Produktion erstellen.

Achten Sie [Azure-CLI](../xplat-cli-install.md) angemeldet und Ressourcen-Manager-Modus:

```bash
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Parameternamen wird durch Ihre eigenen Werte. Beispiel-Parameternamen enthalten `myResourceGroup`, `mystorageaccount`, und `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Erstellen Sie Ressourcengruppen und wählen Deployment-Speicherort aus

Azure Ressourcengruppen sind logische Bereitstellung Entitäten, die Konfigurationsinformationen und Metadaten ermöglichen die logische Verwaltung der Ressource Bereitstellung enthalten. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `westeurope` Speicherort:

```bash
azure group create --name myResourceGroup --location westeurope
```

Ausgabe:

```bash                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Ein Speicherkonto erstellen

Sie benötigen Speicherkonten für die VM-Festplatten und alle zusätzlichen Datenträger, die Sie hinzufügen möchten. Sie erstellen Speicherkonten fast sofort nach dem Erstellen von Ressourcengruppen.

Hier verwenden wir die `azure storage account create` Befehl übergeben, der Position des Kontos der Ressourcengruppe und den Typ der gewünschten Unterstützung steuert. Im folgenden Beispiel wird ein Speicherkonto namens `mystorageaccount`:

```bash
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Ausgabe:

```bash
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Zu unseren Ressourcengruppe mit der `azure group show` Befehl, verwenden wir mit [Jq](https://stedolan.github.io/jq/) der `--json` Azure CLI-Option. (Sie können **Jsawk** oder jede Bibliothek JSON analysiert werden sollen.)

```bash
azure group show myResourceGroup --json | jq '.'
```

Ausgabe:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Untersuchen das Speicherkonto mit CLI, müssen Sie den Namen und Schlüssel festlegen. Ersetzen Sie den Namen des Speicherkontos im folgenden Beispiel mit Namen wählen:

```
AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Dann können Sie problemlos die Speicherinformationen anzeigen:

```bash
azure storage container list
```

Ausgabe:

```bash
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Erstellen Sie ein virtuelles Netzwerk und Subnetz

Anschließend gehen Sie erstellen ein virtuelles Netzwerks in Azure und ein Subnetz, in dem Sie Ihre virtuellen Computer erstellen können. Das folgende Beispiel erstellt ein virtuelles Netzwerk mit dem Namen `myVnet` mit der `192.168.0.0/16` -Adresspräfix:

```bash
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16 
```

Ausgabe:

```bash
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Wir verwenden wieder die Json - Option `azure group show` und **Jq** zu sehen, wie wir unsere Ressourcen erstellen. Wir haben jetzt eine `storageAccounts` Ressourcen und `virtualNetworks` Ressource.  

```bash
azure group show myResourceGroup --json | jq '.'
```

Ausgabe:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Jetzt erstellen Sie ein Subnetz in der `myVnet` virtuellen Netzwerk in der VMs bereitgestellt werden. Wir verwenden die `azure network vnet subnet create` Befehl, und wir bereits haben Ressourcen: die `myResourceGroup` Ressourcengruppe und `myVnet` virtuelles Netzwerk. Im folgenden Beispiel fügen wir das Subnetz mit dem Namen `mySubnet` mit der Subnetzmaske Adresspräfix `192.168.1.0/24`:

```bash
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Ausgabe:

```bash
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Ist das Subnetz logisch in das virtuelle Netzwerk, suchen wir Subnetzinformationen mit einem etwas anderen Befehl. Der Befehl verwendet `azure network vnet show`, aber wir untersuchen die JSON-Ausgabe mit **Jq**.

```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Ausgabe:

```bash
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address-pip"></a>Erstellen Sie eine öffentliche IP-Adresse (BIB)

Nun erstellen Sie die öffentliche IP-Adresse (PIP), die wir der Lastenausgleich zuweisen. Können Sie Ihre VMs aus dem Internet herstellen der `azure network public-ip create` Befehl. Ist die Standardadresse dynamische wir mithilfe ein benanntes DNS-Eintrags in der Domäne **cloudapp.azure.com** der `--domain-name-label` Option. Im folgenden Beispiel wird eine öffentliche IP-Adresse mit dem Namen `myPublicIP` mit dem DNS-Namen des `mypublicdns`. Wie der DNS-Name eindeutig sein muss, bieten Sie Ihre eigenen eindeutigen DNS-Namen:

```bash
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Ausgabe:

```bash
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

Die öffentliche IP-Adresse ist auch eine Ressource der obersten Ebene mit sehen `azure group show`.

```bash
azure group show myResourceGroup --json | jq '.'
```

Ausgabe:

```bash
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

Ressourcendetails einschließlich vollqualifizierten Domänennamen (FQDN) der Domäne mit der vollständigen untersuchen `azure network public-ip show` Befehl. Die öffentliche IP-Adressenressource logisch zugeordnet wurde, jedoch eine bestimmte Adresse noch nicht zugewiesen wurde. Zum Abrufen einer IP-Adresse benötigen Sie einen Lastenausgleich brauchen wir noch nicht erstellt haben.

```bash
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Ausgabe:

```bash
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Erstellen Sie ein System zum Lastenausgleich und IP-pools
Beim Erstellen eines Lastenausgleichsmoduls können Sie Datenverkehr auf mehrere VMs verteilen. Darüber hinaus Redundanz Ihrer Anwendung mit mehreren VMs auf Benutzeranfragen Wartung oder Lasten. Im folgenden Beispiel wird ein Lastenausgleich mit dem Namen `myLoadBalancer`:

```bash
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Ausgabe:

```bash
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Unsere Lastverteiler ist ziemlich leer lassen einige IP-Adresspools erstellen. Wir möchten für unsere Lastenausgleich - für front-End und Back-End für zwei IP-Adresspools erstellen. Der Front-End-IP-Adresspool ist öffentlich sichtbar. Es ist auch den Speicherort, den wir PIP zuweisen, die wir zuvor erstellt haben. Dann verwenden wir Verbindung zum Back-End-Pool als Speicherort für unsere VMs. Auf diese Weise kann der Datenverkehr über den Lastenausgleich auf die VMs fließen.

Zunächst erstellen wir unsere Front-End-IP-Adresspool. Das folgende Beispiel erstellt einen Front-End-Pool namens `myFrontEndPool`:

```bash
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool 
```

Ausgabe:

```bash
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Wie wir verwendet die `--public-ip-name` Switch übergeben der `myPublicIP` , die wir zuvor erstellt haben. Lastenausgleich die öffentliche IP-Adresse zuweisen, können Sie Ihre virtuellen Computer über das Internet erreichen.

Als Nächstes erstellen wir unsere zweite IP-Adresspool diesmal für unser Back-End-Datenverkehr. Das folgende Beispiel erstellt einen Back-End-Pool namens `myBackEndPool`:

```bash
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Ausgabe:

```bash
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Sehen wir, wie unsere Lastenausgleich geht mit `azure network lb show` und die JSON-Ausgabe:

```bash
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Ausgabe:

```bash
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Lastenausgleich NAT-Regeln erstellen
Um Datenverkehr über unsere Lastenausgleich zu erhalten, müssen Netzwerkadresse (Netzwerkadressübersetzung) Regeln erstellen, die eingehende oder ausgehende Aktionen angeben. Sie können das zu verwendende Protokoll angeben dann interne Ports nach Bedarf externe Anschlüsse zuordnen. Unsere Umgebung wir einige Regeln erstellen, die unsere Lastenausgleich unsere VMs SSH passieren. Wir richten Sie TCP-Ports 4222 und 4223, an TCP-Port 22 auf unsere VMs (das später noch erstellt). Das folgende Beispiel erstellt eine Regel namens `myLoadBalancerRuleSSH1` Anschluss 22 TCP-Port 4222 zuordnen:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Ausgabe:

```bash
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Wiederholen der zweiten NAT-Regel für SSH. Das folgende Beispiel erstellt eine Regel namens `myLoadBalancerRuleSSH2` Anschluss 22 TCP-Port 4223 zuordnen:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Wir nun auch eine NAT-Regel für TCP-Port 80 für Webverkehr Einhängen der Regel bis zu unserem IP-Adresspools erstellen. Wenn wir die Regel IP-Pool anstatt die Regel unserer VMs, einbinden einbinden können wir hinzufügen oder Entfernen von VMs aus dem IP-Adresspool. Lastenausgleich passt automatisch den Fluss des Datenverkehrs. Das folgende Beispiel erstellt eine Regel namens `myLoadBalancerRuleWeb` Port 80 TCP-Port 80 zuzuordnen:

```bash
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Ausgabe:

```bash
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Erstellen einer Load Balancer Integritätstest

Ein Prüfpunkt in regelmäßigen Abständen überprüft auf VMs, die hinter unserer Lastenausgleich sicherstellen sie sind und reagieren auf Anfragen definiert sind. Andernfalls sind sie um sicherzustellen, dass Benutzer diese weitergeleitet werden nicht entfernt. Sie können benutzerdefinierte überprüft Integritätstest Werte und Intervalle definieren. Weitere Informationen zu Health Prüfpunkte finden Sie in der [Load-Balancer Prüfpunkte](../load-balancer/load-balancer-custom-probe-overview.md). Das folgende Beispiel erstellt einen TCP Gesundheit geprüft benannten `myHealthProbe`:

```bash
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Ausgabe:

```bash
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Hier haben Sie 15 Sekunden für unsere Gesundheitskontrolle angegeben. Wir vermissen maximal vier Prüfpunkte (eine Minute) bevor Lastenausgleich der Auffassung, dass der Host nicht mehr funktionsfähig ist.

## <a name="verify-the-load-balancer"></a>Überprüfen Sie das System zum Lastenausgleich
Load Balancer Konfiguration wird jetzt durchgeführt. Hier werden die ausgeführten Schritten:

1. Sie erstellt ein Lastenausgleich.
2. Sie erstellt einen Front-End-IP-Pool und eine öffentliche IP-Adresse zugewiesen.
3. Anschließend erstellt Sie einen Back-End-IP-Adresspool, dem VMs herstellen können.
4. Anschließend erstellt Sie NAT-Regeln, mit denen SSH auf die VMs für die Verwaltung einer Regel, die TCP-Port 80 für WebApp ermöglicht.
5. Zuletzt hinzugefügt Integritätstest VMs regelmäßig überprüfen. Dieser Prüfpunkt Gesundheit sichergestellt, dass Benutzer nicht versuchen, einen virtuellen Computer zugreifen, die nicht mehr funktioniert oder Inhalte.

Betrachten wir die Load-Balancer jetzt sieht:

```bash
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Ausgabe:

```bash
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Erstellen Sie eine Netzwerkkarte mit Linux VM

NICs sind programmgesteuert verfügbar, da deren Verwendung Regeln können. Sie können auch mehrere. Im folgenden `azure network nic create` Befehl Einbinden der NIC laden Back-End-IP-Pool und ordnen die NAT-Regel SSH-Datenverkehr zulassen.
 
Ersetzen Sie die `#####-###-###` mit eigenen Azure-Abonnement-ID. Ihr Abonnement ID in der Ausgabe von ist `jq` Ressourcen Prüfung erstellen. Außerdem können Sie Ihre Abonnenten-ID mit `azure account list`. 

Das folgende Beispiel erstellt eine Netzwerkkarte mit dem Namen `myNic1`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Ausgabe:

```bash
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Sie sehen die Details anhand der Ressource direkt. Untersuchen Sie die Ressource mithilfe der `azure network nic show` Befehl:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Ausgabe:

```bash
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Erstellen Sie jetzt die zweite Netzwerkkarte wieder zu unserem Back-End-IP-Adresspool verknüpfen. Diese Zeit NAT-Regel erlaubt SSH-Verkehr. Das folgende Beispiel erstellt eine Netzwerkkarte mit dem Namen `myNic2`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Erstellen einer Network Security und Regeln

Erstellen Sie jetzt eine Netzwerk-Sicherheitsgruppe und der eingehenden Regeln auf der Netzwerkkarte Eine Netzwerk-Sicherheitsgruppe können einer Netzwerkkarte oder einem Subnetz zugewiesen werden. Regeln zur Steuerung des Datenverkehrs und Ihre VMs definieren. Im folgenden Beispiel wird eine Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Fügen Sie die eingehende Regel NSG eingehende Verbindung auf Port 22 (SSH unterstützen). Das folgende Beispiel erstellt eine Regel namens `myNetworkSecurityGroupRuleSSH` zu TCP-Port 22:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Nun fügen Sie eingehende Regel für die NSG eingehende Verbindungen auf Port 80 (zur Unterstützung von Web-Datenverkehr) zulassen. Das folgende Beispiel erstellt eine Regel namens `myNetworkSecurityGroupRuleHTTP` zu TCP-Port 80:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [AZURE.NOTE] Eingehende Regel ist ein Filter für eingehende Netzwerkanschlüsse. In diesem Beispiel binden wir die NSG an die VMs virtuelle NIC, sodass jede Anforderung an Port 22 an der Netzwerkkarte auf unsere VM übergeben wird. Eingehende Regel ist eine Netzwerk Verbindung, und nicht über einen Endpunkt der zum klassischen Deployments wäre es. Um einen Port öffnen, lassen Sie die `--source-port-range` soll "\*" (der Standardwert) akzeptieren eingehende Anfragen von **jedem** Port anfordert. Ports sind in der Regel dynamisch.

## <a name="bind-to-the-nic"></a>An die Netzwerkkarte gebunden

Binden Sie die NSG mit den NICs. Wir müssen unser NICs mit unserem Netzwerk-Sicherheitsgruppe zu verbinden. Führen Sie beide Befehle sowohl unsere NICs einbinden:

```bash
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```bash
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Erstellen einer Verfügbarkeit
Verfügbarkeit wird Hilfe Verbreitung der VMs auf Fehler und Upgrade-Domänen. Erstellen Sie eine Verfügbarkeit für Ihre virtuellen Computer festgelegt. Das folgende Beispiel erstellt eine Verfügbarkeit legen Sie benannte `myAvailabilitySet`:

```bash
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Fehlerdomänen definieren eine Gruppe von virtuellen Computern, die einen gemeinsamen Quelle und Netzwerk Netzschalter freigeben. Standardmäßig werden die virtuellen Computer, die innerhalb der Verfügbarkeit konfiguriert sind auf bis zu drei Fehlerdomänen getrennt. Die Idee ist, dass ein Hardwareproblem eines dieser Fehlerdomänen nicht jede VM, der Ihre Anwendung ausgeführt wird. Azure verteilt VMs domänenübergreifend Fehler, wenn sie in einem Verfügbarkeit.

Upgrade Domänen geben Gruppen von virtuellen Maschinen und zugrunde liegende physische Hardware gleichzeitig neu gestartet werden kann. Die Reihenfolge der Aktualisierung Domänen neu gestartet werden möglicherweise nicht sequenzielle geplante Wartungsarbeiten, aber nur ein Upgrade zu einem Zeitpunkt neu gestartet wird. Erneut verteilt Azure automatisch Ihre VMs auf Upgrade Domänen platzieren in einer Site Verfügbarkeit.

Weitere Informationen über [die Verfügbarkeit von virtuellen Maschinen](./virtual-machines-linux-manage-availability.md).

## <a name="create-the-linux-vms"></a>Erstellen Sie die Linux-VMs

Sie haben die Speicher- und Netzwerkressourcen unterstützen Internet zugänglichen VMs erstellt. Jetzt wir die virtuelle Computer erstellen und mit einem SSH-Schlüssel, das Kennwort nicht. In diesem Fall werden wir ein Ubuntu VM basierend auf der aktuellen LTS erstellen. Wir suchen, Bildinformationen `azure vm image list`, wie [Azure VM Bilder](virtual-machines-linux-cli-ps-findimage.md)suchen.

Wir ein Bild mit dem Befehl ausgewählte `azure vm image list westeurope canonical | grep LTS`. In diesem Fall verwenden wir `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Für das letzte Feld übergeben `latest` so dass zukünftig immer den neuesten Build. (Die Zeichenfolge wir verwenden `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Diesen Schritt kennt, wer bereits eine ssh Rsa öffentlichen und privaten Schlüsselpaares unter Linux oder Mac mit **ssh-Keygen-t Rsa -b 2048**. Haben Sie keine Schlüsselpaare Zertifikat der `~/.ssh` Verzeichnis erstellen:

- Automatisch, durch das `azure vm create --generate-ssh-keys` Option.
- Manuell, indem [die Schritte selbst erstellen](virtual-machines-linux-mac-create-ssh-keys.md).

Alternativ können Sie die `--admin-password` -Methode die SSH-Verbindung zu authentifizieren, nachdem die VM erstellt wird. Diese Methode ist in der Regel weniger sicher.

Die VM erstellen wir alle unsere Ressourcen und Informationen zusammen mit der `azure vm create` Befehl:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Ausgabe:

```bash
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Sie können die VM sofort Verbinden mit Ihrem Standard SSH-Schlüssel. Stellen Sie sicher, den Anschluss anzugeben, da wir über den Lastenausgleich übergeben. (Für unsere erste VM stellen wir die NAT-Regelliste Port 4222 unserer VM weiterleiten):

```bash
 ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Ausgabe:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Weiter und zweiten VM auf die gleiche Weise erstellen:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Und Sie können jetzt das `azure vm show myResourceGroup myVM1` Befehl zu überprüfen. Zu diesem Zeitpunkt laufen die Ubuntu VMs hinter einem Lastenausgleich in Azure Sie, die Sie sich nur mit Ihrem SSH-Schlüsselpaar anmelden können (da Kennwörter deaktiviert sind). Installieren Sie Nginx oder Httpd, eine Webanwendung bereitstellen und finden Sie den Datenverkehr Datenfluss über den Lastenausgleich auf VMs.

```bash
azure vm show --resource-group myResourceGroup --name myVM1
```

Ausgabe:

```bash
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a>Die Umgebung als Vorlage exportieren
Nun, diese Umgebung erstellt haben, möchten wenn zusätzliche schaffen mit Parametern oder einer produktiven Umgebung, die es entspricht? Ressourcen-Manager verwendet JSON-Vorlagen, die die Parameter für Ihre Umgebung zu definieren. Sie erstellen gesamte Umgebungen JSON-Vorlage verweisen. Sie können [JSON-Vorlagen manuell erstellen](../resource-group-authoring-templates.md) oder eine vorhandene Umgebung um JSON-Vorlage erstellen:

```bash
azure group export --name myResourceGroup
```

Dieser Befehl erstellt die `myResourceGroup.json` Datei im aktuellen Arbeitsverzeichnis. Wenn Sie eine Umgebung aus dieser Vorlage erstellen, werden Sie aufgefordert, alle Ressourcennamen einschließlich Lastenausgleich, Netzwerkschnittstellen oder VMs Diese Namen in der Vorlagendatei füllen, indem Sie Hinzufügen der `-p` oder `--includeParameterDefaultValue` Parameter für die `azure group export` Befehl, der zuvor gezeigt wurde. Bearbeiten Sie die Vorlage JSON Ressourcennamen bzw. [Erstellen Sie eine parameters.json-Datei](../resource-group-authoring-templates.md#parameters) , in der Namen angeben.

So erstellen Sie eine Umgebung aus der Vorlage

```bash
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Sie möchten [Weitere Informationen zur Bereitstellung von Vorlagen](../resource-group-template-deploy-cli.md)lesen. Weitere Informationen Sie über inkrementell Umgebungen aktualisieren, verwenden Sie die Datei und einem Lagerort Vorlagen zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Sie können nun mehrere Netzwerkkomponenten und VMs arbeiten. Dieser beispielumgebung können Sie hier die Kernkomponenten eingeführt mit Ihrer Anwendung erstellen.
