<properties
   pageTitle="Zugriff und Sicherheit in Azure Ressourcenmanager Vorlagen | Microsoft Azure" 
   description="Azure Virtual Machine DotNet Core-Lernprogramm"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="access-and-security-in-azure-resource-manager-templates"></a>Zugriff und Sicherheit von Azure-Ressourcen-Manager-Vorlagen

Anwendung in Azure wahrscheinlich müssen Zugriff über das Internet oder ein VPN / Express Route Verbindung mit Azure. Mit Anwendungsbeispiel Musik Speicher ist mit einer öffentlichen IP-Adresse der Website zur Verfügung gestellt. Zugriff eingerichtet sollten Verbindungen auf die Anwendung und den Zugriff auf Ressourcen des virtuellen Computers selbst abgesichert werden. Diese Sicherheit wird mit einer Sicherheitsgruppe Netzwerk bereitgestellt. 

Dieses Dokument beschreibt, wie die Musik Store-Anwendung in Azure Ressourcenmanager Vorlage geschützt wird. Alle Abhängigkeiten und eindeutige Konfigurationen werden hervorgehoben. Für optimale Ergebnisse bereits eine Instanz der Lösung der Azure-Abonnement und zusammen mit der Azure-Ressourcen-Manager bereitgestellt. Die vollständige Vorlage – [Bereitstellung von Musik Store on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux)finden Sie hier.


## <a name="public-ip-address"></a>Öffentliche IP-Adresse

Öffentlicher Zugriff auf Azure Ressource kann eine öffentliche IP-Adressenressource verwendet werden. Öffentliche IP-Adresse kann eine statische oder dynamische IP-Adresse konfiguriert. Wenn eine dynamische Adresse verwendet wird und die virtuelle Maschine angehalten und freigegeben, die Adressen entfernt. Wenn der Computer erneut gestartet wird, kann eine andere öffentliche IP-Adresse zugewiesen werden. Um zu verhindern, dass die IP-Adresse kann eine reservierte IP-Adresse verwendet werden. 

Eine öffentliche IP-Adresse kann ein Ressourcenmanager Azure Vorlage mithilfe der Visual Studio-Ressource Assistent oder gültigen JSON in eine Vorlage einfügen hinzugefügt werden. 

Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager-Vorlage – [Öffentliche IP-Adresse](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicipaddressName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "public-ip-front"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

Eine öffentliche IP-Adresse kann einen virtuellen Netzwerkadapter oder ein Lastenausgleich zugeordnet werden. In diesem Beispiel ist die Musik Store-Website Lastenausgleich über mehrere virtuelle Maschinen hinweg öffentliche IP-Adresse der Lastenausgleich gehört.

Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager Vorlage – [Verbindung mit Lastenausgleich öffentliche IP-Adresse](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Die öffentliche IP-Adresse von Azure-Portal. Beachten Sie, dass die öffentliche IP-Adresse ein Lastenausgleich und keiner virtuellen Maschine zugeordnet ist. Netzwerk-Lastenausgleichern werden in das nächste Dokument dieser Serie erläutert.

![Öffentliche IP-Adresse](./media/virtual-machines-linux-dotnet-core/pubip.png)

Weitere Informationen zu Azure öffentliche IP-Adressen finden Sie unter [IP-Adressen in Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Netzwerk-Sicherheitsgruppe

Nach Zugriff auf Azure Ressourcen hergestellt wurde, sollte dieser Zugriff beschränkt werden. Für Azure virtuelle Maschinen erfolgt Zugriff über eine Netzwerk-Sicherheitsgruppe. Mit Anwendungsbeispiel Musik Speicher beschränkt Zugriff auf den virtuellen Computer außer Port 80 für HTTP-Zugriff und Port 22 für SSH-Zugriff. Eine Netzwerk-Sicherheitsgruppe kann ein Ressourcenmanager Azure Vorlage mithilfe der Visual Studio-Ressource Assistent oder gültigen JSON in eine Vorlage einfügen hinzugefügt werden.

Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager-Vorlage – [Netzwerk-Sicherheitsgruppe](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).

```none
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('nsgfront')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "nsg-front"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
}
```

In diesem Beispiel wird die Netzwerk-Sicherheitsgruppe Subnetzobjekt deklariert in der virtuellen Netzwerk zugeordnet. 

Folgen Sie diesem Link zum Beispiel JSON innerhalb der Ressourcen-Manager-Vorlage – [Netzwerk-Sicherheitsgruppe Zuordnung virtuellen Netzwerk](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158)anzuzeigen.


```none
"subnets": [
  {
    "name": "[variables('subnetName')]",
    "properties": {
      "addressPrefix": "10.0.0.0/24",
      "networkSecurityGroup": {
        "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
      }
    }
  }
```

Hier sieht die Netzwerk-Sicherheitsgruppe von Azure-Portal. Hinweis eine NSG möglich ordnen Subnetz oder Netzwerk-Schnittstelle. In diesem Fall ist die NSG ein Subnetz zugeordnet. In dieser Konfiguration werden die eingehenden Regeln alle virtuellen Maschinen mit dem Subnetz verbunden.

![Netzwerk-Sicherheitsgruppe](./media/virtual-machines-linux-dotnet-core/nsg.png)

Ausführliche Informationen zu Netzwerk-Sicherheitsgruppen finden Sie unter [Network Security Group]( https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Nächstes

<hr>

[Schritt 3 - Verfügbarkeit und Skalierung in Azure Ressourcenmanager Vorlagen](./virtual-machines-linux-dotnet-core-4-availability-scale.md)
