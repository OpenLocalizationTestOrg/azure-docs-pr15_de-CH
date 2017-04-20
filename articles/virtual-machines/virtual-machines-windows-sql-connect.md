<properties
    pageTitle="Verbinden mit einer SQL Server-VM (Resource Manager) | Microsoft Azure"
    description="Verbindung mit SQL Server auf einem virtuellen Computer in Azure erfahren. Dieses Thema verwendet das klassische Bereitstellungsmodell. Die Szenarien ist abhängig von der Netzwerkkonfiguration und der Standort des Clients."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Verbinden Sie mit einer SQL Server-VM auf Azure (Ressourcen-Manager)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-connect.md)
- [Classic](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Übersicht

Herstellen einer Verbindung zu Ihrer SQL Server-Instanz auf einem Azure virtuellen Computer beschrieben. Es werden einige [allgemeinen Szenarien](#connection-scenarios) behandelt und bietet [eine ausführliche Anleitung zum Konfigurieren der SQL Server-Konnektivität in Azure-VM](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell. Die klassische Version dieses Artikels finden Sie unter [Verbindung zu einem SQL Server virtuellen Computer Azure Classic](virtual-machines-windows-classic-sql-connect.md).

Wenn Sie lieber eine vollständige Anleitung Bereitstellung und Konnektivität haben, finden Sie unter [eine SQL Server-VM auf Azure bereitgestellt](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Szenarien

Wie einer Clientverbindung zu SQL Server auf einem virtuellen Computer unterscheidet sich je nach Standort des Clients und der Computer-Netzwerk-Konfiguration. Diese Szenarien umfassen:

- [Verbindung zu SQL Server über das internet](#connect-to-sql-server-over-the-internet)
- [Verbinden Sie mit SQL Server im gleichen virtuellen Netzwerk](#connect-to-sql-server-in-the-same-virtual-network)

### <a name="connect-to-sql-server-over-the-internet"></a>Verbindung zu SQL Server über das Internet

Ggf. aus dem Internet eine Verbindung zu Ihrem SQL Server-Datenbankmodul sind mehrere Schritte erforderlich wie die Firewall konfiguriert, SQL-Authentifizierung aktivieren und konfigurieren die Netzwerk-Sicherheitsgruppe müssen Sie eine Netzwerk-Sicherheitsgruppe Regel TCP-Port 1433 ermöglichen.

Verwenden Sie das Portal mit der Ressourcen-Manager eine SQL Server-VM Bild bereitstellen, sind diese Schritte beim durchgeführt **öffentlichen** Konnektivitätsoption SQL wählen:

![Öffentliche SQL-Konnektivitätsoption während der Bereitstellung](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Wenn dies nicht während der Bereitstellung ist, können Sie manuell SQL Server und die virtuellen Computer konfigurieren anhand der [Schritte in diesem Artikel Verbindung manuell konfigurieren](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

>[AZURE.NOTE] Abbild des virtuellen PCs für SQL Server Express Edition wird nicht automatisch das TCP-Protokoll aktiviert. Express Edition müssen Sie SQL Server Configuration Manager nach dem Erstellen der VM [manuell](#configure-sql-server-to-listen-on-the-tcp-protocol) aktivieren, das TCP-Protokoll verwenden.

Danach ist können jeder Client mit Internetzugang der SQL Server-Instanz durch Angabe der öffentlichen IP-Adresse des virtuellen Computers oder der DNS-Bezeichnung, IP-Adresse zugewiesen. Wenn SQL Server Port 1433 handelt, müssen Sie nicht in der Verbindungszeichenfolge angeben.

    "Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Obwohl diese Konnektivität für Clients über das Internet ermöglicht, bedeutet aber nicht, dass jemand Ihre SQL Server-Verbindung. Clients müssen außerhalb aus den richtigen Benutzernamen und das Kennwort. Für zusätzliche Sicherheit können Sie bekannten Port 1433 vermeiden. Beispielsweise wenn Sie SQL Server zum Abhören von Port 1500 konfiguriert und angemessene Firewall und Netzwerksicherheitsregeln Gruppe eingerichtet, möglich Sie durch Anhängen die Portnummer an den Servernamen wie im folgenden Beispiel:

    "Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

>[AZURE.NOTE] Es ist wichtig zu beachten, dass bei Verwendung dieser Technik zur Kommunikation mit SQL Server alle ausgehende Daten von Azure-Rechenzentrum unter normalen [Preisen für ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Verbinden Sie mit SQL Server im gleichen virtuellen Netzwerk

[Virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md) können zusätzliche Szenarien. Selbst wenn die VMs in verschiedenen Ressourcengruppen vorhanden, können Sie virtuelle Computer im gleichen virtuellen Netzwerk verbinden. Und mit einem [Standort-zu-Standort-VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)erstellen eine Hybrid-Architektur, die VMs mit lokalen Netzwerken und Computern verbindet.

Virtuelle Netzwerke können Sie auch Ihre Azure-VMs zu einer Domäne hinzufügen. Dies ist die einzige Möglichkeit, Windows-Authentifizierung für SQL Server verwenden. Die andere Szenarien erfordern SQL-Authentifizierung mit Benutzernamen und Kennwörtern.

Verwenden Sie das Portal mit der Ressourcen-Manager eine SQL Server-VM Bild bereitstellen, sind angemessene Firewall-Regeln für die Kommunikation über das virtuelle Netzwerk einrichten, wenn Sie SQL-Konnektivitätsoption **Private** auswählen. Wenn dies nicht während der Bereitstellung ist, können Sie manuell SQL Server und die virtuellen Computer konfigurieren anhand der [Schritte in diesem Artikel Verbindung manuell konfigurieren](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm). Aber wenn Sie einer domänenumgebung und die Windows-Authentifizierung konfigurieren, müssen Sie nicht die Schritte in diesem Artikel verwenden Sie SQL-Authentifizierung und Benutzernamen. Sie müssen nicht auch Netzwerk-Sicherheitsgruppe Regeln für den Zugriff über das Internet konfigurieren.

>[AZURE.NOTE] Abbild des virtuellen PCs für SQL Server Express Edition wird nicht automatisch das TCP-Protokoll aktiviert. Express Edition müssen Sie SQL Server Configuration Manager nach dem Erstellen der VM [manuell](#configure-sql-server-to-listen-on-the-tcp-protocol) aktivieren, das TCP-Protokoll verwenden.

Sofern Sie DNS des virtuellen Netzwerks konfiguriert haben, können Sie den Computernamen des SQL Server-VM in der Verbindungszeichenfolge angeben Ihrer SQL Server-Instanz verbinden. Im folgende Beispiel wird davon ausgegangen, dass auch Windows-Authentifizierung konfiguriert wurde und der Benutzer Zugriff auf die SQL Server-Instanz erteilt wurde.

    "Server=mysqlvm;Integrated Security=true"

Beachten Sie, dass in diesem Szenario ebenfalls die IP-Adresse der VM konnte.

## <a name="steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm"></a>Schritte zur manuellen Konfiguration von SQL Server-Konnektivität in Azure-VM

Die folgenden Schritte demonstrieren die Verbindung zur Instanz von SQL Server manuell einrichten und dann optional über das Internet mithilfe von SQL Server Management Studio (SSMS). Beachten Sie, dass viele Schritte durchgeführt werden bei der Auswahl der entsprechenden SQL Server-Konnektivitätsoptionen im Portal.

Bevor die Instanz von SQL Server von einem anderen virtuellen Computer oder das Internet herstellen können, müssen die folgenden Aufgaben ausführen, wie in den Abschnitten beschrieben:

- [Öffnen Sie TCP-Ports in der Windows-firewall](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Konfigurieren von SQL Server zum Abhören des TCP-Protokolls](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Konfigurieren von SQL Server für die Authentifizierung im gemischten Modus](#configure-sql-server-for-mixed-mode-authentication)
- [SQL Server-Authentifizierung Benutzernamen erstellen](#create-sql-server-authentication-logins)
- [Konfigurieren einer eingehenden Regel Netzwerk-Sicherheitsgruppe](#configure-a-network-security-group-inbound-rule-for-the-vm)
- [Konfigurieren einer DNS-Bezeichnung für die öffentliche IP-Adresse](#configure-a-dns-label-for-the-public-ip-address)
- [Verbinden Sie mit der Datenbank-Engine von einem anderen computer](#connect-to-the-database-engine-from-another-computer)

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager-nsg-rule.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Nächste Schritte

Informationen und Schritte Konnektivität-Bereitstellung finden Sie unter [Bereitstellen einer SQL Server-VM in Azure](virtual-machines-windows-portal-sql-server-provision.md).

[Durchsuchen der Learning Path](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) für SQL Server auf Azure virtuellen Computern.

Weitere Themen zum Ausführen von SQL Server in Azure VMs finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
