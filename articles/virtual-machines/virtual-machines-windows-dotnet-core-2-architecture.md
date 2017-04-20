<properties
   pageTitle="Bereitstellen von Datenverarbeitungsressourcen Azure Ressourcenmanager Vorlagen | Microsoft Azure"
   description="Azure Virtual Machine DotNet Core-Lernprogramm"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-architecture-with-azure-resource-manager-templates"></a>Anwendungsarchitektur mit Azure-Ressourcen-Manager-Vorlagen

Bei einer Bereitstellung Azure Ressourcenmanager compute-Anforderungen Azure Ressourcen und Services zugeordnet werden. Besteht eine Anwendung mehrere HTTP-Endpunkte, einer Datenbank und einem Dienst das Zwischenspeichern von Daten, Azure Ressourcen, Host muss jede dieser Komponenten rationalisiert werden. Beispiel Musik Store-Anwendung enthält beispielsweise eine Anwendung, die auf einem virtuellen Computer gehostet wird und eine SQL-Datenbank in SQL Azure-Datenbank gehostet wird. 

Dieses Dokument beschreibt die Konfiguration von Datenverarbeitungsressourcen Musik Speicher in der Azure-Ressourcen-Manager aus. Alle Abhängigkeiten und eindeutige Konfigurationen werden hervorgehoben. Für optimale Ergebnisse bereits eine Instanz der Lösung der Azure-Abonnement und zusammen mit der Azure-Ressourcen-Manager bereitgestellt. Die vollständige Vorlage – [Musik Shop-Bereitstellung unter Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)finden Sie hier.

## <a name="virtual-machine"></a>Virtual Machine

Musik Store-Anwendung enthält eine Webanwendung, Kunden können durchsuchen und Musik. Gibt verschiedene Azure Services, die ASP.NET-Webanwendungen beispielsweise hosten können wird eine virtuelle Maschine verwendet. Musik Shop Vorlage verwenden, ein virtuellen Computer bereitgestellt einen Webserver installieren und laden Musik-Website installiert und konfiguriert. Für diesen Artikel wird nur die VM-Bereitstellung beschrieben. Die Konfiguration des Web-Server und die Anwendung wird in einem späteren Artikel ausführlich beschrieben.

Ein virtuellen Computer können einer Vorlage mit Assistenten für Visual Studio neue Ressource hinzufügen oder die Bereitstellungsvorlage gültigen JSON einfügen hinzugefügt werden. Wenn Sie einen virtuellen Computer bereitstellen, sind mehrere verwandte Ressourcen erforderlich. Wenn Visual Studio die Vorlage erstellen, werden diese Ressourcen für Sie erstellt. Wenn die Vorlage manuell zu erstellen, müssen diese Ressourcen und konfiguriert sein.

Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager-Vorlage – [Virtual Machine JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).

```none
{
  {
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
  ........<truncated>  
}
```

Nach der Bereitstellung können die Eigenschaften im Azure-Portal angezeigt werden.

![Virtual Machine](./media/virtual-machines-windows-dotnet-core/vm-win.png)

## <a name="storage-account"></a>Konto

Speicherkonten verfügen viele Optionen und Funktionen. Kontext Azure virtuelle Computer enthält ein Speicherkonto virtuellen Festplatten der virtuellen Computer und zusätzliche Datenträger. Musik-Store Sample enthält ein Speicherkonto virtuelle Festplatte der einzelnen virtuellen Computer in der Bereitstellung enthalten. 

Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager-Vorlage- [Konto](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

Ein Speicherkonto ist mit einem virtuellen Computer in der Ressourcen-Manager Vorlagendeklaration des virtuellen Computers. 

Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager-Vorlage – [virtueller Computer und Speicher-Konto-Zuordnung](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).

```none
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

Nach der Bereitstellung kann das Speicherkonto in Azure-Portal angezeigt werden.

![Konto](./media/virtual-machines-windows-dotnet-core/storacct-win.png)

Klicken in den Speicher Konto BLOB-Container, die virtuelle Festplatte Datei für jeden virtuellen Computer mit der Vorlage bereitgestellt sehen.

![Virtuelle Festplatten](./media/virtual-machines-windows-dotnet-core/vhd-win.png)

Weitere Informationen zu Azure Storage Dokumentation [Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/).

## <a name="virtual-network"></a>Virtuelles Netzwerk

Eine virtuelle Maschine interne Netzwerke wie die Fähigkeit zur Kommunikation mit anderen virtuellen Computern und Azure-Ressourcen benötigt, ist ein virtuelles Azure-Netzwerk erforderlich.  Ein virtuelles Netzwerk nicht den virtuellen Computer über das Internet zugänglich. Öffentliche Verbindung erfordert eine öffentliche IP-Adresse in dieser Serie ausführlich beschrieben wird.

Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager-Vorlage – [virtuelles Netzwerk und Subnetze](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
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
    ]
  }
}
```

Azure-Portal sieht im folgenden Bild das virtuelle Netzwerk. Beachten Sie, dass alle virtuellen Maschinen mit der Vorlage bereitgestellt an das virtuelle Netzwerk angeschlossen sind.

![Virtuelles Netzwerk](./media/virtual-machines-windows-dotnet-core/vnet-win.png)

## <a name="network-interface"></a>Netzwerkschnittstelle

 Eine Schnittstelle verbindet einen virtuellen Computer zu einem virtuellen Netzwerk genauer mit einem Subnetz, die im virtuellen Netzwerk definiert. 
 
 Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager-Vorlage- [Schnittstelle](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).
 
```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceName'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'RDP-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[variables('subnetRef')]"
          },
          "loadBalancerBackendAddressPools": [
            {
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

Jede VM-Ressource enthält ein Netzwerkprofil. Die Netzwerkschnittstelle ist der virtuelle Computer in diesem Profil zugeordnet.  

Folgen Sie diesem Link um JSON-Beispiel innerhalb der Ressourcen-Manager-Vorlage – [Virtual Machine Netzwerkprofil](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330)anzuzeigen.


```none
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

Azure-Portal sieht aus wie in der folgenden Abbildung die Netzwerkschnittstelle. Die interne IP-Adresse und die Zuordnung virtuellen Computer auf die Netzwerkschnittstellenressource ersichtlich.

![Netzwerkschnittstelle](./media/virtual-machines-windows-dotnet-core/nic-win.png)

Weitere Informationen zu virtuellen Netzwerken Azure Dokumentation [Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>SQL Azure-Datenbank

Neben einer virtuellen Maschine Musik Store-Website-hosting hosten die Musik-Datenbank eine SQL Azure-Datenbank eingesetzt. Der Vorteil von Azure SQL-Datenbank hier also eine zweite Gruppe von virtuellen Computern ist nicht erforderlich und Skalierung und Verfügbarkeit in den Dienst integriert.

Mit dem Visual Studio neue Ressource hinzufügen Assistenten oder gültigen JSON in eine Vorlage einfügen kann eine Azure SQL-Datenbank hinzugefügt werden. Die SQL Server-Ressource enthält einen Benutzernamen und ein Kennwort ein, das über Administratorrechte für die SQL-Instanz erhält. Außerdem wird eine SQL-Firewall Ressource hinzugefügt. Standardmäßig sind Anwendung in Azure SQL-Instanz herstellen. Um externe Anwendung muss so SQL Server Management Studio Verbindung mit der SQL-Instanz, die Firewall konfiguriert werden. Um Musik Store Demo ist die Standardkonfiguration in Ordnung. 

Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager-Vorlage – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).


```none
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicstoresqlName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('adminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicstoresqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Eine Ansicht von SQL Server und Musikladen Datenbank wie im Azure-Portal.

![SQL Server](./media/virtual-machines-windows-dotnet-core/sql-win.png)

Weitere Informationen zum Bereitstellen von Azure SQL-Datenbank Dokumentation [Azure SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Nächstes

<hr>

[Schritt 2: Zugriff und Sicherheit von Azure Ressourcenmanager Vorlagen](./virtual-machines-windows-dotnet-core-3-access-security.md)
