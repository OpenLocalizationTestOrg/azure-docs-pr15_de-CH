<properties
   pageTitle="Öffnen von Ports und Endpunkte einer Linux VM | Microsoft Azure"
   description="So öffnen Sie einen Port / Endpunkt der Azure-Ressourcen-Manager-Bereitstellungsmodell mit CLI Azure Linux VM erstellen"
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
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure"></a>Öffnen von Ports und Endpunkte einer Linux VM in Azure
Sie einen Port öffnen oder einen Endpunkt einer virtuellen Maschine (VM) in Azure durch Erstellen eines Netzwerk-Filters in einem Subnetz oder VM-Schnittstelle erstellen. Sie setzen diese Filter eingehende und ausgehende Datenverkehr zu steuern, einer Netzwerk-Sicherheitsgruppe an der Ressource, die den Datenverkehr empfängt. Wir verwenden ein allgemeines Beispiel für Web-Datenverkehr an Port 80.

## <a name="quick-commands"></a>Schnellzugriff
So erstellen Sie eine Sicherheitsgruppe Netzwerk und Regeln müssen [Der Azure-CLI](../xplat-cli-install.md) installiert und Ressourcen-Manager-Modus

```bash
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Parameternamen wird durch Ihre eigenen Werte. Beispiel-Parameternamen enthalten `myResourceGroup`, `myNetworkSecurityGroup`, und `myVnet`.

Erstellen Sie Ihr Netzwerk Sicherheitsgruppe entsprechend Eingabe von Namen und Speicherort. Im folgenden Beispiel wird eine Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup` in der `WestUS` Speicherort:

```
azure network nsg create --resource-group myResourceGroup --location westus \
    --name myNetworkSecurityGroup
```

Hinzufügen einer Regel zum Zulassen von HTTP-Verkehr auf Ihren Webserver (oder passen Sie Ihr eigenes Szenario wie SSH-Zugriff oder Datenbank-Konnektivität). Das folgende Beispiel erstellt eine Regel namens `myNetworkSecurityGroupRule` zu TCP-Datenverkehr an Port 80:

```
azure network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow
```

Die VM-Schnittstelle (NIC) Netzwerk-Sicherheitsgruppe zugeordnet. Im folgende Beispiel ordnet eine vorhandene Netzwerkkarte mit dem Namen `myNic` mit der Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup`:

```
azure network nic set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup --name myNic
```

Alternativ können Sie die Netzwerk-Sicherheitsgruppe ein virtuelles Netzwerk-Subnetz nicht nur mit der Netzwerkschnittstelle auf einer einzelnen VM zuordnen. Im folgende Beispiel ordnet ein bestehendes Subnetz mit dem Namen `mySubnet` in der `myVnet` virtuelles Netzwerk mit der Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup`:

```
azure network vnet subnet set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Weitere Informationen zu Netzwerk-Sicherheitsgruppen
Die schnelle Befehle ermöglichen mit der VM-Datenverkehr zu kommen. Netzwerk-Sicherheitsgruppen bieten viele hervorragende Funktionen und Granularität zum Steuern des Zugriffs auf Ressourcen. Erfahren Sie mehr über das [Erstellen einer Sicherheitsgruppe Netzwerk und ACL Regeln](../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Als Teil der Azure-Ressourcen-Manager-Vorlagen können Sie Netzwerk-Sicherheitsgruppen und ACL-Regeln definieren. Mehr über das [Erstellen von Sicherheitsgruppen mit Vorlagen Netzwerk](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Verwenden Sie müssen verwenden Port Weiterleitung zu einem internen Anschluss VM einen eindeutigen externen Anschluss zuordnen, ein Lastenausgleich und Network Address Translation (NAT) Regeln. Beispielsweise möchten Sie TCP-Port 8080 extern verfügbar machen und Datenverkehr an TCP-Port 80 auf einer VM geleitet. Sie erfahren [erstellen ein Lastenausgleich Internetzugriff](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel wird eine einfache Regel HTTP-Datenverkehr erstellt. Finden Sie Informationen zum Erstellen von detaillierte Umgebung in den folgenden Artikeln:

- [Azure-Ressourcen-Manager (Übersicht)](../azure-resource-manager/resource-group-overview.md)
- [Was ist Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure Ressourcenmanager Übersicht für Lastenausgleich](../load-balancer2    /load-balancer-arm.md)