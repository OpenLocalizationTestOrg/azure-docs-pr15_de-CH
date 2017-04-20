<properties 
   pageTitle="Ein Punkt-zu-Standort-VPN-Gateway mit virtuellen Netzwerk Ressourcen-Manager-Bereitstellungsmodell konfigurieren | Microsoft Azure"
   description="Sichere Verbindung mit dem virtuellen Netzwerk Azure eine Punkt-zu-Standort-VPN-Gateway Verbindung."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc" />

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Konfigurieren einer Punkt-zu-Standort-Verbindung zu einer VNet mithilfe von PowerShell

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Classic - Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Eine Punkt-zu-Standort (P2S)-Konfiguration können Sie eine sichere Verbindung von einem Clientcomputer zu einem virtuellen Netzwerk erstellen. P2S Verbindung ist nützlich, wenn Sie möchten das VNet von einem Remotestandort aus wie von zu Hause oder einer Konferenz oder wenn nur wenige Clients, die Verbindung zu einem virtuellen Netzwerk. 

Punkt-zu-Standort-Verbindung erfordern keine VPN-Gerät oder eine öffentliche IP-Adresse zu. Eine VPN-Verbindung erfolgt durch die Client-Computer die Verbindung ab. Weitere Informationen über Punkt-zu-Standort-Verbindung finden Sie unter [VPN-Gateway FAQ](vpn-gateway-vpn-faq.md#point-to-site-connections) und [Planung und Entwicklung](vpn-gateway-plan-design.md). 

Dieser Artikel führt Sie durch die Erstellung einer VNet mit einer Punkt-zu-Standort-Verbindung in der Ressourcen-Manager-Bereitstellungsmodell mit PowerShell.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Bereitstellung und-Modelle P2S Verbindung

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Die folgende Tabelle enthält zwei Bereitstellungsmodelle und verfügbaren Bereitstellungsmethoden für P2S Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir direkt aus dieser Tabelle.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Grundlegende workflow 

![Punkt-zu-Standort-Diagramm] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "Punkt-zu-Standort")

In diesem Szenario erstellen Sie ein virtuelles Netzwerk über eine Punkt-zu-Standort-Verbindung. Die Anleitung hilft auch Zertifikate, die für diese Konfiguration generiert. P2S Verbindung besteht aus folgenden Elementen: ein VNet mit einem VPN-Gateway, Datei eines Stammzertifikats CER (öffentlicher Schlüssel) ein Clientzertifikat und die VPN-Konfiguration auf dem Client. 

Wir verwenden die folgenden Werte für diese Konfiguration. Wir setzen Sie die Variablen in Abschnitt [1](#declare) des Artikels. Sie gehen als eine Anleitung und Werte zu verwenden, oder ändern sie entsprechend Ihrer Umgebung. 

### <a name="example"></a>Beispielwerte

- **Name: VNet1**
- **Adressraum: 192.168.0.0/16** und **10.254.0.0/16**<br>In diesem Beispiel verwenden wir mehrere Adressraum um zu veranschaulichen, dass diese Konfiguration mit mehreren Adressräume. Mehrere Adressräume sind jedoch nicht erforderlich für diese Konfiguration.
- **Subnet-Name: Front-End**
    - **Subnetzadressbereichs: 192.168.1.0/24**
- **Subnet-Name: Back-End**
    - **Subnetzadressbereichs: 10.254.1.0/24**
- **Subnet-Name: Subnetzname**<br>Subnetz ist *Subnetzname* für das VPN-Gateway arbeiten.
    - **Subnetzadressbereichs: 192.168.200.0/24** 
- **VPN-Client-Adresspool: 172.16.201.0/24**<br>Verbindung mit dieser Punkt-zu-Standort-Verbindung VNet VPN-Clients erhalten eine IP-Adresse aus dem Adresspool der VPN-Client.
- **Abonnement:** Haben Sie mehrere Abonnements überprüfen, ob Sie richtig verwenden.
- **Ressourcengruppe: TestRG**
- **Ort: USA OST**
- **DNS-Server: IP-Adresse** DNS-Server, die für die namensauflösung verwendet werden soll.
- **GW-Name: Vnet1GW**
- **Öffentliche IP-Name: VNet1GWPIP**
- **VpnType: RouteBased**

## <a name="before-beginning"></a>Vor Beginn

- Überprüfen Sie die Azure-Abonnement. Wenn Sie bereits ein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder Zeichen, für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)aktivieren.
    
- Installieren Sie die neueste Version von Azure Ressourcenmanager PowerShell-Cmdlets. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets. Beim Arbeiten mit PowerShell für diese Konfiguration stellen Sie sicher, dass Sie als Administrator ausgeführt werden. 

## <a name="declare"></a>Teil 1 - Protokoll und festgelegten Variablen

In diesem Abschnitt anmelden und die Werte für diese Konfiguration verwendete deklarieren. Die Werte werden in den Beispielskripts verwendet. Ändern Sie die Werte entsprechend Ihrer Umgebung. Oder verwenden Sie die angegebenen Werte und durchlaufen die Schritte als Übung.

1. Melden Sie sich in der PowerShell-Konsole Azure-Konto an. Dieses Cmdlet aufgefordert, die Anmeldeinformationen für Ihr Konto Azure. Nach der Anmeldung gedownloadet Ihrer Konteneinstellungen, damit sie in Azure PowerShell verfügbar sind.

        Login-AzureRmAccount 

2. Eine Liste der Azure-Abonnements abrufen.

        Get-AzureRmSubscription

3. Geben Sie das Abonnement, das Sie verwenden möchten. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Deklarieren Sie die Variablen, die Sie verwenden möchten. Verwenden Sie im folgende Beispiel die Werte für Ihren eigenen bei Bedarf.

        $VNetName  = "VNet1"
        $FESubName = "FrontEnd"
        $BESubName = "Backend"
        $GWSubName = "GatewaySubnet"
        $VNetPrefix1 = "192.168.0.0/16"
        $VNetPrefix2 = "10.254.0.0/16"
        $FESubPrefix = "192.168.1.0/24"
        $BESubPrefix = "10.254.1.0/24"
        $GWSubPrefix = "192.168.200.0/26"
        $VPNClientAddressPool = "172.16.201.0/24"
        $RG = "TestRG"
        $Location = "East US"
        $DNS = "8.8.8.8"
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"
        
## <a name="ConfigureVNet"></a>Teil 2 – eine VNet konfigurieren 

1. Erstellen Sie eine Ressourcengruppe.

        New-AzureRmResourceGroup -Name $RG -Location $Location

2. Erstellen Sie Subnetz Konfigurationen für das virtuelle Netzwerk benennen, *Front-End*, *Back-End-*und *Subnetzname*. Diese Präfixe müssen Adressraum VNet gehören, die Sie deklariert.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

3. Erstellen Sie virtuelle Netzwerk. Der DNS-Server sollte einen DNS-Server, der die Namen der Ressourcen auflösen kann, eine Verbindung herstellen. In diesem Beispiel verwendet haben wir eine öffentliche IP-Adresse. Achten Sie darauf, dass Ihre eigenen Werte verwenden.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

4. Geben Sie die Variablen für das virtuelle Netzwerk erstellen.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

5. Eine dynamisch zugewiesene öffentliche IP-Adresse anfordern. Diese IP-Adresse muss das Gateway funktioniert. Später verbinden Sie das Gateway, Gateway-IP-Konfiguration.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## <a name="Certificates"></a>Teil 3 - Zertifikate

Zertifikate werden von Azure zur Authentifizierung von VPN-Clients für Punkt-zu-Standort-VPNs. Öffentliche Daten (nicht der private Schlüssel) wird als eine Base64-x. 509-CER-Datei aus einem Stammzertifikat eine Lösung für Großunternehmen Zertifikat generiert oder ein selbstsigniertes Zertifikat codierte exportieren. Importieren Sie dann die öffentlichen Daten aus dem Stammzertifikat in Azure. Außerdem müssen Sie ein Client-Zertifikat aus dem Stammzertifikat für Clients generieren. Jeder Client, der mit dem virtuellen Netzwerk über eine P2S-Verbindung eine Verbindung herstellen möchte, müssen ein Clientzertifikat installiert, der aus dem Stammzertifikat generiert wurde.

### <a name="cer"></a>1. erhalten Sie die CER-Datei für das Stammzertifikat

Sie müssen die öffentlichen Daten für das Stammzertifikat erhalten, die Sie verwenden möchten.

- Wenn ein Zertifikat Unternehmenssystem arbeiten, erhalten Sie die CER-Datei für das gewünschte Stammzertifikat. 

- Wenn Sie eine Lösung für Großunternehmen Zertifikat nicht verwenden, müssen Sie ein selbstsigniertes Zertifikat zu generieren. Windows 10 Schritte finden Sie mit [selbst signierten Stammzertifikaten für Punkt-zu-Standort-Konfigurationen](vpn-gateway-certificates-point-to-site.md).


1. Eine CER-Datei aus einem Zertifikat öffnen Sie **certmgr.msc** und suchen Sie das Stammzertifikat. Maustaste selbstsigniertes Zertifikat, klicken Sie auf **Alle Tasks**und dann auf **Exportieren**. Der **Zertifikatsexport-Assistent**wird geöffnet.

2. **Klicken**Sie im Assistenten klicken Sie auf **Weiter**und wählen Sie **Nein, privaten Schlüssel nicht exportieren**.

3. Auf der Seite **Exportdateiformat** wählen **Base-64-codiert x. 509 (. CER).** Klicken Sie dann auf **Weiter**. 

4. Auf die **zu exportierende Datei**, **Suchen** Sie das Zertifikat exportieren möchten an. **Dateiname**Dateinamen Sie den Zertifikat aus. Klicken Sie auf **Weiter**.

5. Klicken Sie auf **Fertig stellen** , um das Zertifikat exportieren.

### <a name="generate"></a>2. das Client-Zertifikat generieren

Als Nächstes generieren Sie Clientzertifikate. Sie können entweder ein eindeutiges Zertifikat für jeden Client eine Verbindung herstellt, oder können Sie dasselbe Zertifikat auf mehreren Clients. Generieren eindeutige Clientzertifikate Vorteil ist die Fähigkeit, ggf. ein Zertifikat widerrufen. Wenn alle das gleiche Clientzertifikat und finden, Sie das Zertifikat für einen Client Sperren müssen, müssen Sie zum Generieren und installieren neue Zertifikate für alle Clients, die das Zertifikat zur Authentifizierung verwenden. Clientzertifikate werden später in dieser Übung auf jedem Client installiert.

- Wenn Sie eine unternehmenszertifikatlösung verwenden, generieren ein Clientzertifikat mit common Name-Wert-Format 'name@yourdomain.com', anstatt NetBIOS "Domäne\Benutzername" formatieren. 

- Wenn Sie ein selbstsigniertes Zertifikat verwenden, finden Sie unter [Arbeiten mit selbst signierten Stammzertifikaten für Punkt-zu-Standort-Konfigurationen](vpn-gateway-certificates-point-to-site.md) ein Client-Zertifikat generieren.

### <a name="exportclientcert"></a>3. das Client-Zertifikat exportieren

Ein Client-Zertifikat ist zur Authentifizierung erforderlich. Nach dem Generieren von Client-Zertifikat exportieren. Das Client-Zertifikat exportieren wird später auf jedem Clientcomputer installiert.

1. Um ein Clientzertifikat zu exportieren, können Sie *certmgr.msc*. Klicken Sie auf das Clientzertifikat zu exportieren, klicken Sie auf **Alle Tasks**und klicken Sie dann auf **Exportieren**.
2. Exportieren Sie das Client-Zertifikat mit dem privaten Schlüssel. Dies ist eine *PFX* -Datei. Vergewissern Sie sich, und das Kennwort (Schlüssel), der für dieses Zertifikat festgelegt.

### <a name="upload"></a>4 Root Zertifikat CER-Datei hochladen

Deklarieren Sie die Variable für die Zertifikatnamen Wert durch Ihren eigenen ersetzen:

        $P2SRootCertName = "Mycertificatename.cer"

Die öffentlichen Daten für das Stammzertifikat in Azure hinzufügen. Sie können bis zu 20 Stammzertifikate Dateien hochladen. Sie nicht den privaten Schlüssel für das Stammzertifikat in Azure hochladen. Nachdem die CER-Datei hochgeladen, von Azure Clients authentifizieren, die mit dem virtuellen Netzwerk verwendet. 

Ersetzen Sie den Pfad durch Ihren eigenen, und führen Sie die Cmdlets.
    
        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="creategateway"></a>Teil 4 - VPN-Gateway erstellen

Konfigurieren Sie und erstellen Sie das virtuelle Netzwerkgateway für das VNet. *-GatewayType* muss **Vpn** und *-VpnType* muss **RouteBased**sein. Dies kann bis zu 45 Minuten dauern.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="clientconfig"></a>Teil 5 - Paket VPN-Client-Konfiguration

Mit Azure mit P2S muss ein Client-Zertifikat und ein VPN-Client-Konfigurationspaket installiert. VPN-Client-Konfigurationspakete stehen für Windows-Clients. VPN-Client-Paket enthält Informationen zur VPN-Client-Software konfigurieren, die in Windows integriert und ist spezifisch für das VPN herstellen möchten. Paket wird keine zusätzlichen Software installiert. Finden Sie das [VPN-Gateway FAQ](vpn-gateway-vpn-faq.md#point-to-site-connections) Weitere Informationen.

1. Nachdem das Gateway erstellt wurde, können Sie das Konfigurationspaket Client. In diesem Beispiel lädt das Paket für 64-Bit-Clients. Wenn Sie den 32-Bit-Client herunterladen möchten, ersetzen Sie "Amd64" mit 'X86'. VPN-Clients können mithilfe des Azure-Portals.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. Das PowerShell-Cmdlet gibt einen URL-Link. Dies ist ein Beispiel wie die zurückgegebene URL aussieht:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Kopieren Sie und fügen Sie den Link wieder einen Webbrowser, um das Paket herunterzuladen. Installieren Sie das Paket auf dem Clientcomputer. SmartScreen-Popup abzurufen, klicken Sie auf **Weitere Informationen**zum Installieren des Pakets **trotzdem ausgeführt** .

4. Auf dem Clientcomputer zu **Network Settings** und klicken Sie auf **VPN**. Sie sehen die Verbindung aufgeführt. Es zeigt den Namen des virtuellen Netzwerks eine Verbindung herstellt und sieht ähnlich wie dieses Beispiel: 

    ![VPN-client] (./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "VPN-client")


## <a name="clientcertificate"></a>Teil 6: Clientzertifikat installieren

Jeder Clientcomputer muss ein Client-Zertifikat zur Authentifizierung. Wenn das Clientzertifikat installieren, benötigen Sie das Kennwort, das erstellt wurde, wenn das Clientzertifikat exportiert wurde.

1. Kopieren Sie die PFX-Datei auf dem Clientcomputer.
2. Doppelklicken Sie auf die PFX-Datei, um sie zu installieren. Ändern Sie den Installationspfad.

## <a name="connect"></a>Teil 7: Azure verbinden

1. Verbindung mit Ihrem VNet auf dem Clientcomputer zu VPN-Verbindungen und suchen Sie die VPN-Verbindung, die Sie erstellt haben. Er wird den gleichen Namen wie das virtuelle Netzwerk bezeichnet. Klicken Sie auf **Verbinden**. Eine Popup-Meldung erscheint, die mit dem Zertifikat verweist. In diesem Fall klicken Sie auf **Weiter** , um erhöhte Rechte. 

2. Klicken Sie auf der Statusseite **Verbindung** auf **Verbinden** , um die Verbindung zu starten. Wenn Sie ein **Zertifikat auswählen** angezeigt, überprüfen Sie, ob das Client-Zertifikat angezeigt wird, die Sie herstellen möchten. Nicht verwenden Sie den Dropdown Pfeil, um das richtige Zertifikat auszuwählen und klicken Sie dann auf **OK**.

    ![VPN-Client-Verbindung] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN-Client-Verbindung")

3. Die Verbindung sollte nun hergestellt.

    ![Verbindung hergestellt] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Verbindung hergestellt")

## <a name="verify"></a>Teil 8: Überprüfen Sie die Verbindung

1. Um sicherzustellen, dass die VPN-Verbindung aktiv ist, öffnen Sie ein Eingabeaufforderungsfenster mit erhöhten Rechten, und führen Sie *Ipconfig/All*.

2. Die Ergebnisse anzeigen. Beachten Sie, dass die IP-Adresse erhalten Sie eine Adresse in der Punkt-zu-Standort-VPN-Client-Adresspool, der in der Konfiguration angegeben. Das Ergebnis sollte etwa so:
    
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="addremovecert"></a>Hinzufügen oder Entfernen eines vertrauenswürdigen Stammzertifikats

Zertifikate werden zur Authentifizierung von VPN-Clients für Punkt-zu-Standort-VPNs. Die folgenden Schritte führen Sie durch Hinzufügen und Entfernen von Stammzertifikaten. Beim Hinzufügen eine Base64-codierte x. 509 (.) in Azure erzählen Sie Azure, das Stammzertifikat zu vertrauen, das die Datei darstellt. 

Sie können hinzufügen oder Entfernen von vertrauenswürdigen Stammzertifikaten mithilfe von PowerShell oder im Azure-Portal. Wenn Sie hierzu Azure-Portal verwenden möchten, fahren Sie mit Ihrem **virtuellen Netzwerk-Gateway > Settings > Punkt-zu-Standort-Konfiguration > Stammzertifikate**. Die folgenden Schritte durchlaufen diese Aufgaben mithilfe von PowerShell. 

### <a name="add-a-trusted-root-certificate"></a>Hinzufügen eines vertrauenswürdigen Stammzertifikats

Sie können bis zu 20 vertrauenswürdiges Zertifikat CER-Dateien in Azure hinzufügen. Gehen Sie folgendermaßen vor, um ein Stammzertifikat hinzufügen.

1. Erstellen Sie und bereiten Sie das neuen zu Azure hinzufügen Stammzertifikat vor. Öffentlichen Schlüssel exportieren, wie eine Base64-x. 509 codierte (. CER) und mit einem beliebigen Texteditor öffnen. Kopieren Sie nur Abschnitt unten. 
 
    Kopieren Sie die Werte wie im folgenden Beispiel gezeigt:

    ![Zertifikat] (./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "Zertifikat")
    
2. Name des Zertifikats und Schlüsselinformationen als Variable angeben. Ersetzen Sie die Informationen mit eigenen, wie im folgenden Beispiel:

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Fügen Sie das neue Root-Zertifikat. Sie können jeweils nur ein Zertifikat hinzufügen.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. Sie können überprüfen, dass das neue Zertifikat korrekt mit dem folgenden Cmdlet hinzugefügt wurde.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Ein vertrauenswürdiges Zertifikat entfernen

Sie können vertrauenswürdige Stammzertifikat von Azure entfernen. Beim Entfernen eines vertrauenswürdigen Zertifikats werden die Clientzertifikate, die aus dem Stammzertifikat erstellt wurden mehr Azure über Punkt-zu-Standort-Verbindung sein. Wenn Clients eine Verbindung herstellen möchten, müssen sie ein Client-Zertifikat zu installieren, das von einem Zertifikat generiert wird, der in Azure vertraut.

1. Ändern Sie zum Entfernen eines vertrauenswürdigen Stammzertifikats im folgende Beispiel:

    Deklarieren Sie die Variablen.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Entfernen Sie das Zertifikat.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Verwenden Sie das folgende Cmdlet zu überprüfen, ob das Zertifikat erfolgreich entfernt wurde. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="revoke"></a>Verwalten die Liste gesperrte Clientzertifikate

Sie können Client-Zertifikate widerrufen. Die Zertifikatssperrliste können Sie selektiv basierend auf einzelnen Clientzertifikate Punkt-zu-Standort-Verbindung verweigern. Entfernen einer CER Root Zertifikat von Azure Widerruft den Zugriff für alle Client-Zertifikate generiert/signiert von gesperrten Stammzertifikat. Möchten Sie ein bestimmtes Clientzertifikat nicht den Stamm aufheben möchten. Auf diese Weise werden die Zertifikate, die aus dem Stammzertifikat erstellt wurden noch sein gültig.

Meist ist mit dem Stammzertifikat Zugriff auf Team oder die Organisation Ebenen mit aufgehobener Clientzertifikate für detaillierte Zugriffskontrolle für einzelne Benutzer verwalten.

### <a name="revoke-a-client-certificate"></a>Ein Clientzertifikat widerrufen

1. Rufen Sie den Fingerabdruck des Clientzertifikats widerrufen.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Die Liste der gesperrten Fingerabdruck des Fingerabdrucks hinzufügen.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Stellen Sie sicher, dass der Fingerabdruck der Sperrliste hinzugefügt wurde. Sie müssen einen Fingerabdruck gleichzeitig hinzufügen.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Reaktivieren eines Clientzertifikats

Sie können ein Clientzertifikat durch Entfernen des Fingerabdrucks aus der Liste der gesperrten Clientzertifikate wiederherstellen.

1.  Entfernen des Fingerabdrucks des Clientzertifikats widerrufen.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Überprüfen Sie, ob der Fingerabdruck aus gesperrten Liste entfernt wird.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Nächste Schritte

Sie können einen virtuellen Computer mit dem virtuellen Netzwerk hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .