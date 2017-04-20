<properties
    pageTitle="Verbinden mit einer SQL Server-VM (klassisch) | Microsoft Azure"
    description="Verbindung mit SQL Server auf einem virtuellen Computer in Azure erfahren. Dieses Thema verwendet das klassische Bereitstellungsmodell. Die Szenarien ist abhängig von der Netzwerkkonfiguration und der Standort des Clients."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Verbinden Sie mit einer SQL Server virtuellen Maschine auf Azure (klassische Bereitstellung)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-connect.md)
- [Classic](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Übersicht

Herstellen einer Verbindung zu Ihrer SQL Server-Instanz auf einem Azure virtuellen Computer beschrieben. Es werden einige [allgemeinen Szenarien](#connection-scenarios) behandelt und bietet [eine ausführliche Anleitung zum Konfigurieren der SQL Server-Konnektivität in Azure-VM](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Wenn Ressourcenmanager VMs lesen Sie [Verbindung zu einem SQL Server virtuelle Maschine auf Azure mit Ressourcen-Manager](virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Szenarien

Wie einer Clientverbindung zu SQL Server auf einem virtuellen Computer unterscheidet sich je nach Standort des Clients und der Computer-Netzwerk-Konfiguration. Diese Szenarien umfassen:

- [Verbinden Sie mit SQL Server im selben Cloud-Dienst](#connect-to-sql-server-in-the-same-cloud-service)
- [Verbindung zu SQL Server über das internet](#connect-to-sql-server-over-the-internet)
- [Verbinden Sie mit SQL Server im gleichen virtuellen Netzwerk](#connect-to-sql-server-in-the-same-virtual-network)

>[AZURE.NOTE] Vor dem Verbinden mit einer dieser Methoden befolgen Sie die [Schritte in diesem Artikel Verbindung konfigurieren](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Verbinden Sie mit SQL Server im selben Cloud-Dienst

Mehrere virtuelle Maschinen können im selben Cloud-Dienst erstellt werden. Diesem virtuellen Computer finden Sie unter [virtuelle Computer mit einem virtuellen Netzwerk oder Cloud verbinden](virtual-machines-windows-classic-connect-vms.md#connect-vms-in-a-standalone-cloud-service). Dieses Szenario wird ein Client auf einem virtuellen Computer versucht, SQL Server auf einem anderen virtuellen Computer im selben Cloud-Dienst herstellen.

In diesem Szenario können Sie mithilfe der **Namen** (auch als **Computer-** oder **Hostnamen** im Portal angezeigt). Dies ist der Name, der während der Erstellung der VM vorgesehen. Wenn Ihre SQL VM **Mysqlvm**benannte konnte ein Client VM im selben Cloud-Dienst folgende Verbindungszeichenfolge z. B. Verbindung:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Verbindung zu SQL Server über das Internet

Wenn Sie die SQL Server-Datenbank-Engine aus dem Internet herstellen möchten, müssen Sie einen virtuellen Endpunkt für die eingehende TCP-Kommunikation erstellen. Dieser Konfigurationsschritt Azure leitet eingehenden TCP-Port Datenverkehr an einen TCP-Port für den virtuellen Computer.

Um über das Internet zu verbinden, verwenden Sie die VM DNS-Namen und die VM Endpunkt Portnummer (in diesem Artikel konfiguriert). Zu den DNS-Namen der Azure-Portal navigieren Sie, und wählen Sie **virtuelle Computer (classic)**. Wählen Sie den virtuellen Computer. Der **DNS-Name** ist in **der Übersicht** angezeigt.

Angenommen Sie, eine klassische virtuelle Maschine namens **Mysqlvm** mit dem DNS-Namen **mysqlvm7777.cloudapp.net** und eine VM-Endpunkt eines **57500**. Ordnungsgemäß konfigurierte Verbindung angenommen, die folgende Verbindungszeichenfolge genutzt werden, den virtuellen Computer von überall im Internet:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Obwohl diese Konnektivität für Clients über das Internet ermöglicht, bedeutet aber nicht, dass jemand Ihre SQL Server-Verbindung. Clients müssen außerhalb aus den richtigen Benutzernamen und das Kennwort. Verwenden Sie für zusätzliche Sicherheit nicht den bekannten Port 1433 für den öffentlichen virtuellen Endpunkt. Und wenn möglich eine Zugriffssteuerungsliste hinzufügen auf Ihren Endpunkt Verkehr nur auf Clients eingeschränkt zuzulassen. Hinweise zur Verwendung von ACLs mit Endpunkten finden Sie unter [Verwalten der ACL für einen Endpunkt](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

>[AZURE.NOTE] Es ist wichtig zu beachten, dass bei Verwendung dieser Technik zur Kommunikation mit SQL Server alle ausgehende Daten von Azure-Rechenzentrum unter normalen [Preisen für ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Verbinden Sie mit SQL Server im gleichen virtuellen Netzwerk

[Virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md) können zusätzliche Szenarien. Sie können VMs im gleichen virtuellen Netzwerk verbinden, selbst wenn die VMs in andere Cloud-Dienste vorhanden. Und mit einem [Standort-zu-Standort-VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)erstellen eine Hybrid-Architektur, die VMs mit lokalen Netzwerken und Computern verbindet.

Virtuelle Netzwerke können Sie auch Ihre Azure-VMs zu einer Domäne hinzufügen. Dies ist die einzige Möglichkeit, Windows-Authentifizierung für SQL Server verwenden. Die andere Szenarien erfordern SQL-Authentifizierung mit Benutzernamen und Kennwörtern.

Wenn Sie einer domänenumgebung und die Windows-Authentifizierung konfigurieren möchten, müssen Sie nicht mithilfe der Schritte in diesem Artikel Konfigurieren öffentlicher Endpunkt oder SQL-Authentifizierung und Benutzernamen. In diesem Szenario können Sie Ihre SQL Server-Instanz Verbinden der Name SQL Server in der Verbindungszeichenfolge angeben. Angenommen, dass auch Windows-Authentifizierung konfiguriert wurde und der Benutzer Zugriff auf die SQL Server-Instanz erteilt wurde.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Schritte zum Konfigurieren von SQL Server-Konnektivität in Azure-VM

Die folgenden Schritte zeigen, wie über das Internet mithilfe von SQL Server Management Studio (SSMS) Verbindung zu SQL Server-Instanz. Jedoch gelten die gleichen Schritte zu den virtuellen SQL Server-Computer für die Anwendung sowohl lokal ausführen und in Azure.

Bevor die Instanz von SQL Server von einem anderen virtuellen Computer oder das Internet herstellen können, müssen die folgenden Aufgaben ausführen, wie in den Abschnitten beschrieben:

- [Erstellen Sie einen TCP-Endpunkt für den virtuellen Computer](#create-a-tcp-endpoint-for-the-virtual-machine)
- [Öffnen Sie TCP-Ports in der Windows-firewall](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Konfigurieren von SQL Server zum Abhören des TCP-Protokolls](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Konfigurieren von SQL Server für die Authentifizierung im gemischten Modus](#configure-sql-server-for-mixed-mode-authentication)
- [SQL Server-Authentifizierung Benutzernamen erstellen](#create-sql-server-authentication-logins)
- [Bestimmen der DNS-Name des virtuellen Computers](#determine-the-dns-name-of-the-virtual-machine)
- [Verbinden Sie mit der Datenbank-Engine von einem anderen computer](#connect-to-the-database-engine-from-another-computer)

Im folgenden Diagramm ist der Verbindungspfad zusammengefasst:

![Herstellen einer Verbindung mit einer SQL Server-VM](../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic Steps](../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Nächste Schritte

Falls Sie auch für hohe Verfügbarkeit und Disaster Recovery AlwaysOn Availability Gruppen verwenden, sollten Sie einen Listener implementieren. Datenbank-Clients Verbinden der Listener nicht direkt an einen SQL Server-Instanzen. Der Listener leitet Clients, primäres Replikat in der verfügbarkeitsgruppe. Weitere Informationen finden Sie unter [Konfigurieren einer ILB Listener für AlwaysOn Availability in Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Es ist wichtig, alle best Practices für die Sicherheit für SQL Server auf eine Azure virtuellen Computer überprüfen. Weitere Informationen finden Sie unter [Security Considerations for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-security.md).

[Durchsuchen der Learning Path](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) für SQL Server auf Azure virtuellen Computern. 

Weitere Themen zum Ausführen von SQL Server in Azure VMs finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
