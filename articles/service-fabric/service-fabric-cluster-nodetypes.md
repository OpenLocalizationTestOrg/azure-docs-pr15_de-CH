<properties
   pageTitle="Service Fabric Knotentypen und VM Maßstab legt | Microsoft Azure"
   description="Beschreibt, wie Service Fabric-Knotentypen VM Maßstab legt und wie remote Verbindung zu einer Instanz VM festlegen oder einem Clusterknoten."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Die Beziehung Service Fabric Knotentypen und Virtual Machine skalieren

Virtual Machine sind Skala ein Azure Compute Ressource zum Bereitstellen und Verwalten einer Auflistung von virtuellen Computern als Gruppe verwenden können. Alle Knotentypen in einem Cluster Service Fabric definiert ist als separate VM Skalierung Set eingerichtet. Jeden Knotentyp kann dann skaliert werden oder unabhängig haben unterschiedliche Ports öffnen und haben unterschiedliche Kapazitäten Metriken.

Die folgende Abbildung zeigt einen Cluster, der zwei Knoten Arten: Front-End- und Back-End.  Jeder Knoten verfügt über fünf Knoten.

![Screenshot eines Clusters, die zwei Knotentypen][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a>Knoten zuordnen VM festlegen Instanzen

Siehe oben, starten Sie die VM festlegen Instanzen Instanz 0 und geht. Die Nummerierung der Namen wider. BackEnd_0 ist z. B. Instanz 0 Backend-VM Skalierung festlegen. Diese bestimmten VM Skalierung hat fünf Instanzen mit dem Namen BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 und BackEnd_4.

Beim Skalieren von VM Skalierung festlegen, wird eine neue Instanz erstellt. Der neue Instanznamen VM festlegen werden normalerweise die nächste Instanznummer + Name VM festzulegen. In unserem Beispiel ist es BackEnd_5.


## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>Zuordnen von VM-Skala festgelegt Lastenausgleich für jeden Knoten Typ/VM Skalierungsgruppe

Wenn Ihr Cluster über das Portal bereitgestellt haben oder die Ressourcen-Manager aus, der wir dann bereitgestellt, wenn man eine Liste aller Ressourcen in einer Ressourcengruppe verwendet haben sehen Lastenausgleich für jeden VM festlegen oder Knoten Sie.

Der Name würde etwa: **LB -&lt;NodeType Name&gt;**. Z. B. LB-sfcluster4doc-0, wie in dieser Abbildung dargestellt:


![Ressourcen][Resources]


## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a>Anschließen an eine Instanz VM festlegen oder eines Knotens
Alle Knotentypen in einem Cluster definiert ist als separate VM Skalierung Set eingerichtet.  Also die Knotentypen können skaliert oder unten unabhängig und anderen VM-SKUs erfolgen. Im Gegensatz zur Instanz VMs abrufen VM festlegen Instanzen nicht selbst eine virtuelle IP-Adresse. So kann es sein eine Herausforderung beim Suchen nach einer IP-Adresse und Port, die remote mit an eine bestimmte Instanz.

Hier werden die Schritte können sie entdecken.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>Schritt 1: Suchen der virtuelle IP-Adresse für den Knotentyp und eingehende NAT-Regeln für RDP

Um das erhalten, müssen Sie eingehende NAT-Regeln Werte abzurufen, die als Teil der Ressourcendefinition für **Microsoft.Network/loadBalancers**definiert wurden.

Navigieren Sie im Portal die Load Balancer Blade und **Einstellungen**.

![LBBlade][LBBlade]


Klicken Sie auf **Einstellungen**auf **eingehende NAT-Regeln**. Nun können Sie die IP-Adresse und Port, die remote mit an zunächst VM festzulegen. Im folgenden Screenshot ist **104.42.106.156** und **3389**

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a>Schritt 2: Finden Sie heraus, mit denen Sie auf Remote Anschluss auf den bestimmten VM festlegen/Knoten

Ich Sprach weiter oben in diesem Dokument Instanzen VM festlegen wie zuordnen. Wir verwenden die genaue Portadresse herauszufinden.

Die Ports werden in aufsteigender Reihenfolge der Instanz VM festlegen zugeordnet. Damit in meinem Beispiel den Knotentyp Front-End-Ports für jede der fünf Instanzen folgen. Sie müssen nun die gleiche Zuordnung für Ihre Instanz VM Skalierung festlegen.

|**Instanz des VM skalieren**|**Anschluss**|
|-----------------------|--------------------------|
|FrontEnd_0|3389|
|FrontEnd_1|3390|
|FrontEnd_2|3391|
|FrontEnd_3|3392|
|FrontEnd_4|3393|
|FrontEnd_5|3394|


### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a>Schritt 3: Anschließen der bestimmten Instanz VM festlegen

In der Abbildung unten wird Remotedesktopverbindung zum Herstellen der FrontEnd_1 verwenden:

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>Die Bereichswerte RDP-Port ändern

### <a name="before-cluster-deployment"></a>Vor der Bereitstellung

Wenn Sie den Cluster mithilfe einer Ressourcen-Manager-Vorlage einrichten, können Sie den Bereich in der **InboundNatPools**angeben.

Die Ressourcendefinition für **Microsoft.Network/loadBalancers**werden. Darunter finden Sie die Beschreibung für **InboundNatPools**.  Ersetzen Sie die Werte *FrontendPortRangeStart* und *FrontendPortRangeEnd* .

![InboundNatPools][InboundNatPools]


### <a name="after-cluster-deployment"></a>Nach der Bereitstellung
Dies ist etwas komplizierter und möglicherweise die VMs abrufen wiederverwendet. Sie müssen jetzt neue Werte über Azure PowerShell festlegen. Stellen Sie sicher, dass Azure PowerShell 1.0 oder höher auf Ihrem Computer installiert ist. Wenn Sie dies noch nicht geschehen, empfehle ich, führen Sie die Schritte [zum Installieren und Konfigurieren von Azure PowerShell.](../powershell-install-configure.md)

Mit der Azure-Konto anmelden. PowerShell-Befehl fehlschlägt, prüfen Sie, ob Azure PowerShell ordnungsgemäß installiert ist.

```
Login-AzureRmAccount
```

Führen Sie Folgendes auf der Lastenausgleich zu Details und die Werte für die Beschreibung **InboundNatPools**:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Legen Sie jetzt *FrontendPortRangeEnd* und *FrontendPortRangeStart* auf die gewünschten Werte.

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über die Funktion "Überall bereitstellen" und einen Vergleich mit Azure verwaltet](service-fabric-deploy-anywhere.md)
- [Cluster-Sicherheit](service-fabric-cluster-security.md)
- [Service Fabric-SDK und erste Schritte](service-fabric-get-started.md)


<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
