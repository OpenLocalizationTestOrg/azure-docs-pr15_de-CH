<properties 
   pageTitle="Konfigurieren einer Punkt-zu-Standort-VPN-gatewayverbindung mit virtuellen Netzwerk Bereitstellungsmodell Ressourcenmanager und Azure-Portal | Microsoft Azure"
   description="Sichere Verbindung mit dem virtuellen Netzwerk Azure eine Punkt-zu-Standort-VPN-Gateway Verbindung mit Ressourcen-Manager und Azure-Portal."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurieren Sie eine Punkt-zu-Standort-Verbindung mit einem VNet über das Azure-Portal

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Classic - Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Eine Punkt-zu-Standort (P2S)-Konfiguration können Sie eine sichere Verbindung von einem Clientcomputer zu einem virtuellen Netzwerk erstellen. P2S Verbindung ist nützlich, wenn Sie möchten das VNet von einem Remotestandort aus wie von zu Hause oder einer Konferenz oder wenn nur wenige Clients, die Verbindung zu einem virtuellen Netzwerk. 

Punkt-zu-Standort-Verbindung erfordern keine VPN-Gerät oder eine öffentliche IP-Adresse zu. Eine VPN-Verbindung erfolgt durch die Client-Computer die Verbindung ab. Weitere Informationen über Punkt-zu-Standort-Verbindung finden Sie unter [VPN-Gateway FAQ](vpn-gateway-vpn-faq.md#point-to-site-connections) und [Planung und Entwicklung](vpn-gateway-plan-design.md).

Dieser Artikel führt Sie durch das Erstellen einer VNet mit einer Punkt-zu-Standort-Verbindung im Ressourcenmanager Bereitstellungsmodell Azure-Portal.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Bereitstellung und-Modelle P2S Verbindung

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Die folgende Tabelle enthält zwei Bereitstellungsmodelle und verfügbaren Bereitstellungsmethoden für P2S Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir direkt aus dieser Tabelle.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Grundlegende workflow 

![Punkt-zu-Standort-Diagramm] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "Punkt-zu-Standort")

### <a name="example"></a>Beispielwerte

- **Name: VNet1**
- **Adressraum: 192.168.0.0/16**<br>In diesem Beispiel verwenden wir nur einen Adressraum. Sie können mehrere Adressraum für das VNet.
- **Subnet-Name: Front-End**
- **Subnetzadressbereichs: 192.168.1.0/24**
- **Abonnement:** Haben Sie mehrere Abonnements überprüfen, ob Sie richtig verwenden.
- **Ressourcengruppe: TestRG**
- **Ort: USA OST**
- **Subnetzname: 192.168.200.0/24**
- **Virtuelles Netzwerk-Gateway-Namen: VNet1GW**
- **Gateway-Typ: VPN**
- **VPN-Typ: Route basiert**
- **Öffentliche IP-Adresse: VNet1GWpip**
- **Verbindungstyp: Punkt-zu-Standort**
- **Client-Adresspool: 172.16.201.0/24**<br>Verbindung mit dieser Punkt-zu-Standort-Verbindung VNet VPN-Clients erhalten eine IP-Adresse von Client-Adresspool.

## <a name="before-beginning"></a>Vor Beginn

- Überprüfen Sie die Azure-Abonnement. Wenn Sie bereits ein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder Zeichen, für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)aktivieren.
    
## <a name="createvnet"></a>Teil 1: Erstellen eines virtuellen Netzwerks

Wenn Sie diese Konfiguration als Übung erstellen, können Sie die [Beispielwerte](#example)verweisen.

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

### <a name="2-add-additional-address-space-and-subnets"></a>2. Fügen Sie zusätzliche Adressraum und Subnetze

Sie können Hinzufügen zusätzlichen Adressraum und Subnetzen die VNet nach dem erstellen.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

### <a name="3-create-a-gateway-subnet"></a>3. erstellen Sie ein

Vor dem Gateway virtuelle Netzwerk herstellen, müssen Sie für das virtuelle Netzwerk gatewaysubnetz erstellen Sie eine Verbindung herstellen möchten. Gegebenenfalls empfiehlt es sich, ein gatewaysubnetz eines CIDR-Blocks von /28 oder /27 um genügend IP-Adressen um zusätzliche zukünftigen Konfiguration Anforderungen erstellen.

Die Screenshots in diesem Abschnitt dienen als referenzbeispiel. Achten Sie darauf, dass Subnetzname Bereich, der entspricht mit den erforderlichen Werten für die Konfiguration verwenden.

#### <a name="to-create-a-gateway-subnet"></a>Erstellen Sie ein

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="dns"></a>4. Geben Sie einen DNS-Server (optional)

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>Teil 2 – Erstellen Sie ein virtuelles Netzwerk-gateway

Punkt-zu-Standort-Verbindung ist Folgendes erforderlich:

- Gateway-Typ: VPN
- VPN-Typ: Route basiert

### <a name="to-create-a-virtual-network-gateway"></a>Erstellen Sie ein virtuelles Netzwerk-gateway

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Teil 3 - Zertifikate

Zertifikate werden von Azure zur Authentifizierung von VPN-Clients für Punkt-zu-Standort-VPNs. Öffentliche Daten (nicht der private Schlüssel) wird als eine Base64-x. 509-CER-Datei aus einem Stammzertifikat eine Lösung für Großunternehmen Zertifikat generiert oder ein selbstsigniertes Zertifikat codierte exportieren. Importieren Sie dann die öffentlichen Daten aus dem Stammzertifikat in Azure. Außerdem müssen Sie ein Client-Zertifikat aus dem Stammzertifikat für Clients generieren. Jeder Client, der mit dem virtuellen Netzwerk über eine P2S-Verbindung eine Verbindung herstellen möchte, müssen ein Clientzertifikat installiert, der aus dem Stammzertifikat generiert wurde.

### <a name="getcer"></a>1. erhalten Sie die CER-Datei für das Stammzertifikat

Bei Verwendung eine Enterprise-Lösung können Sie Ihre vorhandenen Zertifikatkette. Wenn Sie ein Unternehmen CA-Lösung verwenden, können Sie ein selbstsigniertes Zertifikat erstellen. Eine Methode zum Erstellen einer selbst signierten Zertifikat ist Makecert.

- Wenn ein Zertifikat Unternehmenssystem arbeiten, erhalten Sie die CER-Datei für das gewünschte Stammzertifikat. 

- Wenn Sie eine Lösung für Großunternehmen Zertifikat nicht verwenden, müssen Sie ein selbstsigniertes Zertifikat zu generieren. Windows 10 Schritte finden Sie mit [selbst signierten Stammzertifikaten für Punkt-zu-Standort-Konfigurationen](vpn-gateway-certificates-point-to-site.md).

1. Eine CER-Datei aus einem Zertifikat öffnen Sie **certmgr.msc** und suchen Sie das Stammzertifikat. Maustaste selbstsigniertes Zertifikat, klicken Sie auf **Alle Tasks**und dann auf **Exportieren**. Der **Zertifikatsexport-Assistent**wird geöffnet.

2. **Klicken**Sie im Assistenten klicken Sie auf **Weiter**und wählen Sie **Nein, privaten Schlüssel nicht exportieren**.

3. Auf der Seite **Exportdateiformat** wählen **Base-64-codiert x. 509 (. CER).** Klicken Sie dann auf **Weiter**. 

4. Auf die **zu exportierende Datei**, **Suchen** Sie das Zertifikat exportieren möchten an. **Dateiname**Dateinamen Sie den Zertifikat aus. Klicken Sie auf **Weiter**.

5. Klicken Sie auf **Fertig stellen** , um das Zertifikat exportieren.

### <a name="generateclientcert"></a>2. erstellen Sie ein Clientzertifikat

Sie können entweder ein eindeutiges Zertifikat für jeden Client eine Verbindung herstellt, oder können Sie dasselbe Zertifikat auf mehreren Clients. Generieren eindeutige Clientzertifikate Vorteil ist die Fähigkeit, ggf. ein Zertifikat widerrufen. Wenn alle das gleiche Clientzertifikat und finden, Sie das Zertifikat für einen Client Sperren müssen, müssen Sie zum Generieren und installieren neue Zertifikate für alle Clients, die das Zertifikat zur Authentifizierung verwenden.

- Wenn Sie eine unternehmenszertifikatlösung verwenden, generieren ein Clientzertifikat mit common Name-Wert-Format 'name@yourdomain.com', das Format 'Domänenname\Benutzername'. 

- Wenn Sie ein selbstsigniertes Zertifikat verwenden, finden Sie unter [Arbeiten mit selbst signierten Stammzertifikaten für Punkt-zu-Standort-Konfigurationen](vpn-gateway-certificates-point-to-site.md) ein Client-Zertifikat generieren.

### <a name="exportclientcert"></a>3. das Client-Zertifikat exportieren

Ein Client-Zertifikat ist zur Authentifizierung erforderlich. Nach dem Generieren von Client-Zertifikat exportieren. Das Client-Zertifikat exportieren wird später auf jedem Clientcomputer installiert.

1. Um ein Clientzertifikat zu exportieren, können Sie *certmgr.msc*. Klicken Sie auf das Clientzertifikat zu exportieren, klicken Sie auf **Alle Tasks**und klicken Sie dann auf **Exportieren**.
2. Exportieren Sie das Client-Zertifikat mit dem privaten Schlüssel. Dies ist eine *PFX* -Datei. Vergewissern Sie sich, und das Kennwort (Schlüssel), der für dieses Zertifikat festgelegt.

## <a name="addresspool"></a>Teil 4 - Client-Adresspool hinzufügen

1. Erstellte virtuelle Netzwerk-Gateway navigieren Sie zum Abschnitt **Einstellungen** des virtuellen Netzwerks Gateway Blades. Klicken Sie in **den Einstellungen** auf **Punkt-zu-Standort-Konfiguration** -Blade- **Konfiguration** zu öffnen.

    ![zeigen Sie auf Seite blade] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "zeigen Sie auf Seite blade")

2. **Adresspool** ist die IP-Adressen, die Clients verbinden eine IP-Adresse erhält. Fügen Sie den Adresspool hinzu, und klicken Sie dann auf **Speichern**.

    ![Client-Adresspool] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "Client-Adresspool")

## <a name="uploadfile"></a>Teil 5 - Root Zertifikat CER-Datei hochladen

Nach dem Erstellen das Gateway können Sie die CER-Datei für ein vertrauenswürdiges Zertifikat in Azure hochladen. Sie können bis zu 20 Stammzertifikate Dateien hochladen. Sie nicht den privaten Schlüssel für das Stammzertifikat in Azure hochladen. Nachdem die CER-Datei hochgeladen, von Azure Clients authentifizieren, die mit dem virtuellen Netzwerk verwendet.

1. Navigieren Sie zu **Punkt-zu-Standort-Konfiguration** Blade. Sie werden im Abschnitt **Stammzertifikat** dieses CER-Dateien hinzufügen.

    ![zeigen Sie auf Seite blade] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "zeigen Sie auf Seite blade")

2. Stellen Sie sicher, dass Sie das Stammzertifikat als Base-64 x. 509 (.) Datei exportiert. Sie müssen in dieses Format exportieren, damit Sie das Zertifikat mit Text-Editor öffnen können.
3. Öffnen Sie das Zertifikat mit einem Texteditor wie Notepad. Kopieren Sie den folgenden Abschnitt:

    ![Daten] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "Daten")

4. Fügen Sie Zertifikatdaten in die **Öffentlichen Daten** des Portals. Geben Sie den Namen des Zertifikats im **Namespace** , und klicken Sie dann auf **Speichern**. Sie können bis zu 20 vertrauenswürdige Stammzertifikate hinzufügen.

    ![Zertifikat laden] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "Zertifikat laden")

## <a name="clientconfig"></a>Teil 6: herunterladen und installieren das Konfigurationspaket VPN-client

Mit Azure mit P2S müssen sowohl ein Client-Zertifikat und ein VPN-Client-Konfigurationspaket installiert. VPN-Client-Konfigurationspakete stehen für Windows-Clients. 

Das VPN-Client-Paket enthält Informationen zur VPN-Client-Software konfigurieren, die in Windows integriert ist. Die Konfiguration ist das VPN herstellen möchten. Paket wird keine zusätzlichen Software installiert. Finden Sie das [VPN-Gateway FAQ](vpn-gateway-vpn-faq.md#point-to-site-connections) Weitere Informationen.

1. Auf der **Punkt-zu-Standort-Konfiguration** Blade, klicken Sie auf **Download VPN-Client** **herunterladen VPN-Client** -Blade öffnen.

    ![VPN-Client herunterladen] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "VPN-Client herunterladen")

2. Wählen Sie das richtige Paket für Ihre Kunden, und klicken Sie auf **Download**. Wählen Sie für 64-Bit-Clients **AMD64**. Wählen Sie **X86**für 32-Bit-Clients.

3. Installieren Sie das Paket auf dem Clientcomputer. SmartScreen-Popup abzurufen, klicken Sie auf **Weitere Informationen**zum Installieren des Pakets **trotzdem ausgeführt** .

4. Auf dem Clientcomputer zu **Network Settings** und klicken Sie auf **VPN**. Sie sehen die Verbindung aufgeführt. Es zeigt den Namen des virtuellen Netzwerks eine Verbindung herstellt und sieht ähnlich wie dieses Beispiel: 

    ![VPN-client] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "VPN-client")

## <a name="installclientcert"></a>Teil 7: Clientzertifikat installieren

Jeder Clientcomputer muss ein Client-Zertifikat zur Authentifizierung. Wenn das Clientzertifikat installieren, benötigen Sie das Kennwort, das erstellt wurde, wenn das Clientzertifikat exportiert wurde.

1. Kopieren Sie die PFX-Datei auf dem Clientcomputer.
2. Doppelklicken Sie auf die PFX-Datei, um sie zu installieren. Ändern Sie den Installationspfad.

## <a name="connect"></a>Teil 8: Azure verbinden

1. Verbindung mit Ihrem VNet auf dem Clientcomputer zu VPN-Verbindungen und suchen Sie die VPN-Verbindung, die Sie erstellt haben. Er wird den gleichen Namen wie das virtuelle Netzwerk bezeichnet. Klicken Sie auf **Verbinden**. Eine Popup-Meldung erscheint, die mit dem Zertifikat verweist. In diesem Fall klicken Sie auf **Weiter** , um erhöhte Rechte. 

2. Klicken Sie auf der Statusseite **Verbindung** auf **Verbinden** , um die Verbindung zu starten. Wenn Sie ein **Zertifikat auswählen** angezeigt, überprüfen Sie, ob das Client-Zertifikat angezeigt wird, die Sie herstellen möchten. Nicht verwenden Sie den Dropdown Pfeil, um das richtige Zertifikat auszuwählen und klicken Sie dann auf **OK**.

    ![VPN-Client 2] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN-Client-Verbindung")

3. Die Verbindung sollte nun hergestellt.

    ![VPN-Client 3] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "VPN-Client-Verbindung 2")

## <a name="verify"></a>Teil 9: Überprüfen Sie die Verbindung

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

## <a name="add"></a>Hinzufügen oder Entfernen von vertrauenswürdigen Stammzertifikaten

Sie können vertrauenswürdige Stammzertifikat von Azure entfernen. Beim Entfernen eines vertrauenswürdigen Zertifikats werden die Clientzertifikate, die aus dem Stammzertifikat erstellt wurden mehr Azure über Punkt-zu-Standort-Verbindung sein. Wenn Clients eine Verbindung herstellen möchten, müssen sie ein Client-Zertifikat zu installieren, das von einem Zertifikat generiert wird, der in Azure vertraut.

Die Liste der gesperrten Clientzertifikate auf der **Punkt-zu-Standort-Konfiguration verwalten** Blade. Dies ist das Blade hochladen [ein vertrauenswürdiges Zertifikat](#uploadfile)verwendet.

## <a name="revokeclient"></a>Verwalten die Liste gesperrte Clientzertifikate

Sie können Client-Zertifikate widerrufen. Die Zertifikatssperrliste können Sie selektiv basierend auf einzelnen Clientzertifikate Punkt-zu-Standort-Verbindung verweigern. Entfernen einer CER Root Zertifikat von Azure Widerruft den Zugriff für alle Client-Zertifikate generiert/signiert von gesperrten Stammzertifikat. Möchten Sie ein bestimmtes Clientzertifikat nicht den Stamm aufheben möchten. Auf diese Weise werden die Zertifikate, die aus dem Stammzertifikat erstellt wurden noch sein gültig. 

Meist ist mit dem Stammzertifikat Zugriff auf Team oder die Organisation Ebenen mit aufgehobener Clientzertifikate für detaillierte Zugriffskontrolle für einzelne Benutzer verwalten.

Die Liste der gesperrten Clientzertifikate auf der **Punkt-zu-Standort-Konfiguration verwalten** Blade. Dies ist das Blade hochladen [ein vertrauenswürdiges Zertifikat](#uploadfile)verwendet.


## <a name="next-steps"></a>Nächste Schritte

Sie können einen virtuellen Computer mit dem virtuellen Netzwerk hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .