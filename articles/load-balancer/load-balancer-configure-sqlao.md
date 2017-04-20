<properties
   pageTitle="Lastenausgleich für SQL immer auf Konfigurieren | Microsoft Azure"
   description="Konfigurieren Sie Lastenausgleich mit SQL immer arbeiten und wie Powershell erstellen Lastenausgleich für die SQL-Implementierung"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-load-balancer-for-sql-always-on"></a>Konfigurieren von Lastenausgleich für SQL immer auf

SQL Server AlwaysOn Availability Gruppen kann nun mit ILB ausgeführt werden. Availability Group ist SQL Server führenden Lösung für hohe Verfügbarkeit und Disaster Recovery. Der Verfügbarkeitsgruppenlistener ermöglicht Clientanwendungen nahtlose Verbindung primäres Replikat unabhängig von der Anzahl der Replikate in der Konfiguration.

Der Listener (DNS) zugeordnet ist, eine IP-Adresse mit Lastenausgleich und Azure Lastenausgleich leitet den Datenverkehr an den primären Server in der Replikatgruppe.

ILB-Unterstützung können für SQL Server AlwaysOn (Listener) Endpunkte. Sie steuern den Zugriff auf den Listener und können die Netzwerklastenausgleich-IP-Adresse aus einem bestimmten Subnetz im virtuellen Netzwerk (VNet).

Mit ILB auf dem Listener, der SQL Server-Endpunkt (z.B. Server = Tcp:ListenerName, 1433; Datenbank = DatabaseName) kann nur zugegriffen werden:

- Dienste und virtuelle Computer im gleichen virtuellen Netzwerk
- Dienste und VMs lokal verbundenen Netzwerk
- Dienste und VMs aus miteinander verbundenen VNets

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Abbildung 1: SQL AlwaysOn mit Internetzugriff Lastenausgleich konfiguriert

## <a name="add-internal-load-balancer-to-the-service"></a>Der Dienst internen Lastenausgleich hinzufügen

1. Im folgenden Beispiel konfigurieren wir einem virtuellen Netzwerk, ein Subnetz bezeichnet "Subnetz 1" enthält:

        Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc

2. Lastenausgleich Endpunkte für ILB auf jedem virtuellen Computer hinzufügen

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –
        DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Im obigen Beispiel 2 VM namens "sqlsvc1" und "sqlsvc2" ausgeführt haben in der Cloud service "Sqlsvc". Nach dem Erstellen der ILB mit "DirectServerReturn", wird der ILB zu SQL die Listener für die Verfügbarkeitsgruppen konfigurieren Lastenausgleich Endpunkte hinzufügen.

Weitere Informationen zu SQL AlwaysOn finden Sie unter [Konfigurieren einer internen Lastenausgleich für eine AlwaysOn Availability Group in Azure](../virtual-machines/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Siehe auch

[Erste Schritte mit Lastenausgleich ein Internetzugriff konfigurieren](load-balancer-get-started-internet-arm-ps.md)

[Beginnen Sie einen internen Lastenausgleich konfigurieren](load-balancer-get-started-ilb-arm-ps.md)

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)
