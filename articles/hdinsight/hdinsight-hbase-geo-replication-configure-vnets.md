<properties 
   pageTitle="VPN-Verbindung zwischen zwei virtuellen Netzwerke konfigurieren | Microsoft Azure" 
   description="Informationen zum Konfigurieren von VPN-Verbindungen und Auflösung des Domänennamens zwischen zwei Azure virtuelle Netzwerke und HBase Geo-Replikation konfigurieren." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-a-vpn-connection-between-two-azure-virtual-networks"></a>Konfigurieren einer VPN-Verbindungs zwischen zwei Azure virtuelle Netzwerke  

> [AZURE.SELECTOR]
- [Konfigurieren von VPN-Konnektivität](hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfigurieren von DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [HBase Replikation](hdinsight-hbase-geo-replication.md) 

Azure virtual-Standorten Netzwerkkonnektivität verwendet eine VPN-Gateway für einen sicheren Tunnel mit Ipsec-IKE. Die VNets können verschiedene Abonnements und Regionen. Sie können auch VNet VNet Kommunikation mit mehreren Standorten kombinieren. Es gibt mehrere Gründe für VNet VNet-Konnektivität:

- Cross-Region Geo-Redundanz und Geo-Präsenz 
- Regionale Multi-Tier-Anwendung mit starken Isolationsgrenze 
- Cross-Kommunikation zwischen Organisationen in Azure-Abonnement

Weitere Informationen finden Sie unter [VNet VNet Verbindung konfigurieren](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). 

Video anzeigen:

> [AZURE.VIDEO configure-the-vpn-connectivity-between-two-azure-virtual-networks]

Dieses Lernprogramm ist Bestandteil der [Serie] [ hdinsight-hbase-replication] auf HBase Geo-Replikation. 

- Konfigurieren einer VPN-Verbindung zwischen zwei virtuelle Netzwerke (dieses Lernprogramm)
- [Konfigurieren von DNS für virtuelle Netzwerke][hdinsight-hbase-geo-replication-dns]
- [HBase Geo Replikation][hdinsight-hbase-geo-replication]

Die folgende Abbildung zeigt zwei virtuelle Netzwerke, die Sie in diesem Lernprogramm erstellen:

![HDInsight HBase Replikation virtuelles Netzwerkdiagramm][img-vnet-diagram]
 

##<a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Eine Workstation mit Azure PowerShell**.

    Stellen Sie bevor PowerShell-Skripts sicher, dass Sie Ihre Azure-Abonnement mit dem folgenden Cmdlet verbunden sind:

        Add-AzureAccount

    Haben mehrere Azure-Abonnements mit dem folgenden Cmdlet das aktuelle Abonnement fest:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


>[AZURE.NOTE] Azure Service und virtuellen Namen müssen eindeutig sein. In diesem Lernprogramm verwendeten Namen ist Contoso [Azure Service/VM-Name]-[EU / US]. Beispielsweise ist Contoso-VNet-EU Azure virtuelles Netzwerk im Rechenzentrum Nordeuropa. Contoso-DNS-US ist der DNS-Server VM im südostasiatischen US Datencenter. Sie müssen mit Ihrem eigenen Namen kommen.
 

##<a name="create-two-azure-vnets"></a>Erstellen Sie zwei Azure-VNets



**Erstellen Sie ein virtuelles Netzwerk namens Contoso-VNet-EU in Nordeuropa**

1.  Melden Sie sich bei [Azure-Verwaltungsportal][azure-portal].
2.  Klicken Sie auf **NEW** **NETWORK SERVICES** **virtuellen Netzwerk**, **benutzerdefinierte erstellen**.
3.  Geben Sie ein:

    - **NAME**: Contoso-VNet-EU
    - **Ort**: Nordeuropa

        Diese praktische Einführung verwendet Nordeuropa und ostasiatischen US-Rechenzentren. Sie können Ihre eigenen Rechenzentren.
4.  Geben Sie ein:

    - **DNS-SERVER**: (leer lassen) 
    
        Sie benötigen einen eigenen DNS-Server für die Auflösung von Namen in virtuelle Netzwerke. Weitere Informationen zu Azure bereitgestellten namensauflösung verwenden eigene DNS-Server verwenden finden Sie unter [Auflösung DNS (Name)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Hinweise Auflösung zwischen VNets konfigurieren finden Sie unter [Konfigurieren von DNS zwischen zwei Azure virtuelle Netzwerke][hdinsight-hbase-dns].
  
    - **Konfigurieren einer Punkt-zu-Standort-VPN**: (unmarkiert)

        Punkt-zu-Standort gilt nicht für dieses Szenario.

    - **Konfigurieren einer Standort-zu-Standort-VPN**: (unmarkiert)
    
        Sie konfigurieren die Standort-zu-Standort-VPN-Verbindung Azure virtuellen Netzwerk im südostasiatischen US Datencenter.
5.  Geben Sie ein:

    -   **IP-Adresse Speicherplatz ab**: 10.1.0.0
    -   **Adresse Speicherplatz CIDR**: / 16
    -   **Subnetz 1 Starten IP**: 10.1.0.0
    -   **Subnetz 1 CIDR**: / 24

    Der Adressraum kann nicht mit US virtuelles Netzwerk überschneiden.  

**Erstellen Sie ein virtuelles Netzwerk namens Contoso-VNet-EU in Westeuropa**

- Wiederholen Sie den letzten Vorgang mit den folgenden Werten:

    - **NAME**: Contoso-VNet-US
    - **Ort**: USA OST
     
    - **DNS-SERVER**: (leer lassen)
    - **Konfigurieren einer Punkt-zu-Standort-VPN**: (unmarkiert)
    - **Konfigurieren einer Standort-zu-Standort-VPN**: (unmarkiert)
     
    - **IP-Adresse Speicherplatz ab**: 10.2.0.0
    - **Adresse Speicherplatz CIDR**: / 16
    - **Subnetz 1 Starten IP**: 10.2.0.0
    - **Subnetz 1 CIDR**: / 24

















##<a name="configure-a-vpn-connection-between-the-two-vnets"></a>Konfigurieren einer VPN-Verbindungs zwischen zwei VNets

###<a name="create-local-networks"></a>Erstellen von lokalen Netzwerken

Beim Erstellen einer VNet VNet Konfiguration müssen Sie jedes VNet um gegenseitig als LAN-Site zu identifizieren. In diesem Abschnitt Konfigurieren Sie jedes VNet als ein lokales Netzwerk. Lokale Netzwerke freigeben demselben IP-Adressbereiche der entsprechenden VNet

![Konfiguration Azure VPN-Standort-zu-Standort - Azure lokalen Netzwerken][img-vnet-lnet-diagram]


**Für ein lokales Netzwerk namens Contoso-LNet-EU entsprechen den Netzwerkadressbereich Contoso-VNet-EU**

1. Azure-Verwaltungsportal klicken Sie auf **neue** **Netzwerkdienste** **VIRTUAL NETWORK**, **Lokalen Netzwerk hinzufügen**.
3. Geben Sie ein:

    - **NAME**: Contoso-LNet-EU
    - **VPN-Geräte-IP-Adresse**: 192.168.0.1 (diese Adresse später aktualisiert wird)

        Normalerweise verwenden Sie tatsächliche externe IP-Adresse für ein VPN-Gerät. VNet VNet Konfigurationen verwenden Sie VPN-Gateway IP-Adresse. Da Sie nicht die VPN-Gateways für die beiden VNets noch erstellt haben, eine beliebige IP-Adresse und wieder zu korrigieren.
4.  Geben Sie ein:

    - **IP-Adresse Speicherplatz ab:** 10.1.0.0
    - **Adresse Speicherplatz CIDR:** / von 16.
    
    Dies muss Bereich entsprechen, den Sie zuvor für Contoso-VNet-EU angegeben.

**Für ein lokales Netzwerk namens Contoso-LNet-US mit Contoso-VNet-US Netzwerkadressbereich**

- Wiederholen Sie den letzten Vorgang mit den folgenden Parametern:

    - **NAME**: Contoso-LNet-US
    - **VPN-Geräte-IP-Adresse**: 192.168.0.1 (diese Adresse später aktualisiert wird)
     
    - **IP-Adresse Speicherplatz ab**: 10.2.0.0
    - **Adresse Speicherplatz CIDR**: / 16


###<a name="create-vpn-gateways"></a>Erstellen von VPN-gateways

Diese Konfiguration umfasst zwei Teile. Zunächst eine VNet Standort-zu-Standort-Verbindung zu einem lokalen Netzwerk konfigurieren und erstellen Sie eine dynamische routing VPN. VNet zu VNet erfordert Azure VPN-Gateways mit dynamischen routing VPNs. Azure statischen routing VPNs werden nicht unterstützt.

**Die Contoso-VNet-EU Standort-zu-Standort-Verbindung Contoso-LNet-US konfigurieren**

1.  Azure-Verwaltungsportal klicken Sie **Netzwerke** im linken Fensterbereich,
2.  Klicken Sie auf **Contoso-VNet-EU**.
3.  Klicken Sie auf **CONFIGUE** .
4.  Überprüfen Sie **mit dem lokalen Netzwerk verbinden**.
5.  Wählen Sie im **Lokalen Netzwerk** **Contoso-LNet-US**.
6.  Klicken Sie auf **Add gatewaysubnetz** in der Adresszeile Leerzeichen virtuelles Netzwerk.
7.  Klicken Sie auf **Speichern**.
8.  Klicken Sie auf **OK** bestätigen.


**Erstellen Sie eine VPN-Gateway für Contoso-VNet-EU**

1.  Azure-Verwaltungsportal klicken Sie auf die Registerkarte **DASHBOARD** .
4.  Klicken Sie auf **GATEWAY erstellen** am unteren Rand der Seite und dann auf **Dynamisches Routing**.
5.  Klicken Sie auf **Ja** zu bestätigen. Beachten Sie die Gateway-Grafik auf der Seite Gelb ändert und sagt Gateway erstellen. Es dauert ca. 15 Minuten für das Gateway erstellen.

    Beim Verbinden der Gateway-Status ändert, werden die IP-Adresse jedes Gateway im Dashboard angezeigt. Notieren Sie die IP-Adresse zu jeder VNet um zu mischen sie. Dies sind die IP-Adressen, die beim Bearbeiten der Platzhalter IP-Adressen für das VPN-Netzwerken verwendet werden.

6.  Kopieren Sie die **GATEWAY-IP-Adresse**. Es dient zum Konfigurieren von VPN-Gateway IP-Adresse für Contoso-VNet-EU im nächsten Abschnitt.

**Erstellen Sie eine VPN-Gateway für Contoso-VNet-EU**

- Wiederholen Sie die letzten beiden für Contoso-Vnet-US Contoso-VNet-US-Standorten Konnektivität für Contoso-LNet-EU und erstellen ein VPN-Gateway konfigurieren. Wenn Sie fertig sind, müssen Sie die VPN-Gateway IP-Adresse für Contoso-VNet-US.


### <a name="set-the-vpn-device-ip-addresses-for-local-networks"></a>Setzen Sie das VPN-Gerät IP-Adressen für lokale Netzwerke
Im letzten Abschnitt erstellen Sie eine VPN-Gateway für jeden der VNets. Sie haben die IP-Adressen von VPN-Gateways. Jetzt können Sie zurückgehen, IP-Adressen im lokalen Netzwerk VPN-Gerät konfiguriert.

**Konfigurieren von VPN-Gerät IP-Adresse für Contoso-LNet-EU** 

1.  Azure-Verwaltungsportal klicken Sie im linken Bereich auf **Netzwerke** .
2.  Klicken Sie oben auf **Lokalen NETZWERKEN** .
3.  Klicken Sie auf **Contoso-LNet-EU**und klicken Sie unten **Bearbeiten** .
4.  **VPN-Geräte-IP-Adresse**zu aktualisieren.  Dies ist die Adresse des Contoso-VNET-EU DASHBOARD-Registerkarte erhalten.
5.  Klicken Sie auf die rechte Maustaste.
6.  Klicken Sie auf die Schaltfläche überprüfen.

**Konfigurieren von VPN-Gerät IP-Adresse für Contoso-LNet-US** 

- Wiederholen der letzten Prozedur für Contoso-LNet-US VPN-Gerät IP-Adresse konfigurieren.

###<a name="set-vnet-gateway-keys"></a>VNet-Gateway-Schlüssel festlegen

Vnet-Gateways verwenden einen gemeinsamen Schlüssel zur Authentifizierung zwischen virtuellen Netzwerken. Der Schlüssel kann nicht aus dem Azure-Verwaltungsportal konfiguriert werden. PowerShell oder .NET SDK verwenden.

**Zum Festlegen der Schlüssel**

1. Öffnen Sie von Ihrer Arbeitsstation aus **Windows PowerShell ISE** oder Windows PowerShell-Konsole.
2. Aktualisieren Sie die Parameter in diesem Skript folgen, und führen Sie es:

        Add-AuzreAccount
        Select-AzureSubscription -[AzureSubscriptionName]
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-EU -LocalNetworkSiteName Contoso-LNet-US -SharedKey A1b2C3D4
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-US -LocalNetworkSiteName Contoso-LNet-EU -SharedKey A1b2C3D4 


##<a name="check-the-vpn-connection"></a>Überprüfen Sie die VPN-Verbindung 

Ohne VMs auf die VNets bereitgestellt können virtuellen visual Netzplandiagramm VNet Dashboardseite im klassischen Azure-Portal Sie den Verbindungsstatus prüfen:

![HDInsight HBase virtuellen Replikationsnetzwerk VPN-Verbindungsstatus][img-vpn-status]
  



##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie eine VPN-Verbindung zwischen zwei Azure virtuelle Netzwerke konfigurieren. Die beiden Artikel der Serie behandelt:

- [Zwischen zwei Azure virtuelle Netzwerke konfigurieren][hdinsight-hbase-geo-replication-dns]
- [HBase Geo Replikation][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com

[powershell-install]: ../install-configure-powershell.md



[hdinsight-hbase-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-dns]: hdinsight-hbase-geo-replication-configure-dns.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-diagram.png
[img-vnet-lnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-lnet-diagram.png
[img-vpn-status]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-status.png 
