<properties
   pageTitle="Verfügbarkeit und Skalierung in Azure Ressourcenmanager Vorlagen | Microsoft Azure"
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

# <a name="availability-and-scale-in-azure-resource-manager-templates"></a>Verfügbarkeit und Skalierung in Azure-Ressourcen-Manager-Vorlagen

Verfügbarkeit und Skalierung verweisen Betriebszeit und Nachfrage zu. Wenn eine Anwendung 99,9 % der Zeit sein muss, ist eine Architektur verfügen über mehrere gleichzeitige Compute-Ressourcen. Anstatt eine Website enthält beispielsweise eine Konfiguration mit einer höheren Verfügbarkeit mehrere Instanzen derselben Site mit Technologie vor. In dieser Konfiguration kann eine Instanz der Anwendung zu Wartungszwecken abgeschaltet werden während der verbleibenden weiterhin. Skala bezieht sich auf Applikationen Fähigkeit auf Nachfrage. Ausgewogene Anwendung hinzufügen oder Entfernen von Instanzen aus dem Pool ermöglicht mit einer Anwendung bei Bedarf zu skalieren.

Dieses Dokument beschreibt, wie Musik Shop beispielbereitstellung für Verfügbarkeit und Skalierung konfiguriert ist. Alle Abhängigkeiten und eindeutige Konfigurationen werden hervorgehoben. Für optimale Ergebnisse bereits eine Instanz der Lösung der Azure-Abonnement und zusammen mit der Azure-Ressourcen-Manager bereitgestellt. Die vollständige Vorlage – [Bereitstellung von Musik Store on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux)finden Sie hier.

## <a name="availability-set"></a>Festlegen der Verfügbarkeit

Verfügbarkeit festlegen erstreckt sich logisch Azure Virtual Machines auf physischen Hosts und andere Infrastrukturkomponenten wie Netzteile und physischen Netzwerkhardware. Legt Verfügbarkeit sicherstellen, dass während der Wartung, Gerätefehler oder andere Ausfall nicht alle virtuellen Computer erfolgt. Verfügbarkeit festlegen kann eine Vorlage Azure-Ressourcen-Manager mithilfe der Visual Studio-Ressource Assistent oder gültigen JSON in eine Vorlage einfügen hinzugefügt werden.

Folgen Sie diesem Link um JSON-Beispiel in der Ressourcen-Manager-Vorlage – [Verfügbarkeit](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387)anzuzeigen.


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "avalibility-set"
  },
  "properties": {
    "platformUpdateDomainCount": 5,
    "platformFaultDomainCount": 3
  }
}
```

Verfügbarkeit festlegen als Eigenschaft der VM-Ressource deklariert. 

Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager-Vorlage – [Verfügbarkeit Zuordnung virtuellen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313).


```none
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
Die Verfügbarkeit von Azure-Portal festgelegt. Jede virtuelle Maschine und Informationen über die Konfiguration werden hier erläutert.

![Festlegen der Verfügbarkeit](./media/virtual-machines-linux-dotnet-core/aset.png)

Ausführliche Informationen auf Verfügbarkeit finden Sie unter [Manage Verfügbarkeit virtueller Maschinen](./virtual-machines-linux-manage-availability.md). 

## <a name="network-load-balancer"></a>Netzwerklastenausgleich

Während eine Verfügbarkeit Anwendung Fehlertoleranz bietet, Verfügung Lastenausgleichsmodul viele Instanzen der Anwendung eine einzige Netzwerkadresse. Viele virtuelle Computer jeweils einen Lastenausgleich verbundenen können mehrere Instanzen einer Anwendung gehostet. Wie die Anwendung erfolgt leitet Lastenausgleich die eingehende Anforderung angeschlossenen Mitglieder. Ein Lastenausgleich mit der Visual Studio-Ressource Assistent hinzugefügt werden oder einfügen ordnungsgemäß formatiert JSON-Ressource in der Azure-Ressourcen-Manager-Vorlage.

Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager-Vorlage- [Netzwerklastenausgleich](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-front"
  },
  ........<truncated>
}
```

Beispiel-Anwendung mit einer öffentlichen IP-Adresse im Internet ausgesetzt, ist diese Adresse mit Lastenausgleich. 

Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager-Vorlage – [Netzwerklastenausgleich öffentliche IP-Adresse zugeordnet](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).

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

Von Azure-Portal zeigt Network Load Balancer (Übersicht) die Zuordnung mit der öffentlichen IP-Adresse.

![Netzwerklastenausgleich](./media/virtual-machines-linux-dotnet-core/nlb.png)

## <a name="load-balancer-rule"></a>Load Balancer Regel

Wenn ein Lastenausgleich verwenden, sind Regeln konfiguriert, die steuern, wie Verkehr vorgesehenen Ressourcen verteilt. Anwendung laden Musik Beispiel Datenverkehr auf Port 80 der öffentlichen IP-Adresse eingeht und auf Port 80 aller virtuellen Computer verteilt. 

Folgen Sie diesem Link zum Beispiel JSON innerhalb der Ressourcen-Manager-Vorlage- [Load Balancer Regel](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270)anzuzeigen.


```none
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

Eine Ansicht der Network Load Balancer Regel vom Portal aus.

![Network Load Balancer Regel](./media/virtual-machines-linux-dotnet-core/lbrule.png)

## <a name="load-balancer-probe"></a>Load Balancer Prüfpunkt

Lastenausgleich muss auch auf jedem virtuellen Computer überwachen, sodass Anfragen nur für Systeme bereitgestellt werden. Diese Überwachung erfolgt konstant Überprüfung eines vordefinierten Ports. Musik Shop-Bereitstellung konfiguriert Prüfpunkt Port 80 auf allen virtuellen Computern enthalten. 

Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager-Vorlage- [Load Balancer Prüfpunkt](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257).


```none
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

Load Balancer Prüfpunkts in Azure-Portal angezeigt.

![Network Load Balancer Prüfpunkt](./media/virtual-machines-linux-dotnet-core/lbprobe.png)

## <a name="inbound-nat-rules"></a>Eingehende NAT-Regeln

Wenn Sie einen Lastenausgleich verwenden, müssen Regeln umgesetzt werden, die nicht Load balanced Zugriff auf jedem virtuellen Computer. Beispielsweise beim Erstellen einer SSH-Verbindungs mit jedem virtuellen Computer sollte Datenverkehr nicht Lastenausgleich, sondern ein vordefinierter Pfad konfiguriert werden sollte. vorgegebenen Pfade werden über eine NAT-Regel eingehende Ressource konfiguriert. Diese Ressource verwenden, kann eingehende Kommunikation einzelner virtueller Maschinen zugeordnet werden. 

Musik-Store-Anwendung ist ein Port 5000 ab Port 22 auf jedem virtuellen Computer für SSH-Zugriff zugewiesen. Die `copyindex()` Funktion erhöht den eingehenden Port, der zweite virtuelle Computer einen eingehenden Port 5001 dritten 5002 erhält und so weiter.

Folgen Sie diesem Link das JSON Beispiel innerhalb der Ressourcen-Manager-Vorlage – [Eingehende NAT-Regeln](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270). 

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'SSH-VM', copyIndex())]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 22,
    "enableFloatingIP": false
  }
}
```

Ein Beispiel eingehende NAT-Regel, wie im Azure-Portal. SSH NAT-Regel wird für jeden virtuellen Computer in der Bereitstellung erstellt.

![Eingehende NAT-Regel](./media/virtual-machines-linux-dotnet-core/natrule.png)

Ausführliche Informationen zu Azure Netzwerklastenausgleichs finden Sie unter [Netzwerklastenausgleich für Azure Infrastrukturdienste](./virtual-machines-linux-load-balance.md).

## <a name="deploy-multiple-vms"></a>Bereitstellen mehrerer virtueller Computer

Schließlich müssen eine Verfügbarkeit oder Lastenausgleich funktionieren, mehrere virtuelle Computer. Mit der Funktion Azure Ressourcenmanager Vorlage kopieren können mehrere VMs bereitgestellt werden. Mithilfe der Kopierfunktion ist nicht erforderlich, eine begrenzte Anzahl von virtuellen Computern definieren, eher Wert kann dynamisch erfolgen zum Zeitpunkt der Bereitstellung. Die Kopierfunktion nimmt die Anzahl der Instanzen erstellt und behandelt die richtige Anzahl von virtuellen Maschinen und die zugeordneten Ressourcen bereitstellen.

Musik Shop Vorlage wird ein Parameter definiert, die eine instanzanzahl akzeptiert. Diese Nummer wird beim Erstellen von virtuellen Maschinen und zugehörige Ressourcen in der Vorlage verwendet.

```none
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 1,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
}
```

Auf die VM-Ressource erhält die kopierschleife einen Namen und die Anzahl der Instanzenparameter verwendet, um die Anzahl der Kopien.

Folgen Sie diesem Link zum Beispiel JSON innerhalb der Ressourcen-Manager-Vorlage – [Virtual Machine-Kopierfunktion](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300)anzuzeigen. 


```none
"apiVersion": "2015-06-15",
"type": "Microsoft.Compute/virtualMachines",
"name": "[concat(variables('vmName'),copyindex())]",
"location": "[resourceGroup().location]",
"copy": {
  "name": "virtualMachineLoop",
  "count": "[parameters('numberOfInstances')]"
}
```

Die aktuelle Iteration die Kopierfunktion erfolgt mit der `copyIndex()` Funktion. Der Wert der Funktion Index kopieren kann virtuelle Maschinen und anderen Ressourcen verwendet werden. Beispielsweise sollten zwei Instanzen von einem virtuellen Computer bereitgestellt werden, sie unterschiedliche Namen. Die `copyIndex()` Funktion kann einen eindeutigen Namen als Teil der Name des virtuellen Computers verwendet werden. Ein Beispiel für die `copyindex()` Funktion Benennung in die VM-Ressource angezeigt werden. Hier die Computername ist eine Verkettung der `vmName` Parameter und `copyIndex()` Funktion. 

Folgen Sie diesem Link zum Beispiel JSON innerhalb der Ressourcen-Manager-Vorlage- [Kopierfunktion Index](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319)anzuzeigen. 


```none
"osProfile": {
  "computerName": "[concat(parameters('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "linuxConfiguration": {
    "disablePasswordAuthentication": "true",
    "ssh": {
      "publicKeys": [
        {
          "path": "[variables('sshKeyPath')]",
          "keyData": "[parameters('sshKeyData')]"
        }
      ]
    }
  }
}
```

Die `copyIndex` -Funktion wird mehrmals in der Musik Shop aus. Ressourcen und Funktionen mit `copyIndex` für eine einzelne Instanz des virtuellen Computers wie Netzwerkschnittstelle Load Balancer Regeln enthalten und alle Funktionen abhängig. 

Weitere Informationen zu der Kopierfunktion finden Sie unter [mehrere Instanzen von Ressourcen in Azure-Ressourcen-Manager erstellen](../resource-group-create-multiple.md).

## <a name="next-step"></a>Nächstes

<hr>

[Schritt 4 - Bereitstellung mit Azure Ressourcenmanager Vorlagen](./virtual-machines-linux-dotnet-core-5-app-deployment.md)