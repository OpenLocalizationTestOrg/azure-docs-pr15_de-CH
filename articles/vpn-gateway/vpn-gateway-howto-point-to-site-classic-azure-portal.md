<properties
   pageTitle="Ein Punkt-zu-Standort-VPN-Gateway mit einem Azure virtuelle Netzwerk Azure-Portal konfigurieren | Microsoft Azure"
   description="Sichere Verbindung mit dem virtuellen Netzwerk Azure eine Punkt-zu-Standort-VPN-Gateway-Verbindung Azure-Portal erstellen."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurieren Sie eine Punkt-zu-Standort-Verbindung mit einem VNet über das Azure-portal

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Classic - Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Dieser Artikel führt Sie durch das Erstellen einer VNet mit einer Punkt-zu-Standort-Verbindung im klassischen Bereitstellungsmodell Azure-Portal. Eine Punkt-zu-Standort (P2S)-Konfiguration können Sie eine sichere Verbindung von einem Clientcomputer zu einem virtuellen Netzwerk erstellen. P2S Verbindung ist nützlich, wenn Sie das VNet von einem Remotestandort aus wie von zu Hause oder einer Konferenz möchten. Oder, wenn Sie nur einige Clients, die Verbindung zu einem virtuellen Netzwerk benötigen.

Punkt-zu-Standort-Verbindung erfordern keine VPN-Gerät oder eine öffentliche IP-Adresse zu. Eine VPN-Verbindung erfolgt durch die Client-Computer die Verbindung ab. Weitere Informationen über Punkt-zu-Standort-Verbindung finden Sie unter [VPN-Gateway FAQ](vpn-gateway-vpn-faq.md#point-to-site-connections) und [Über VPN-Gateway](vpn-gateway-about-vpngateways.md#point-to-site).

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Bereitstellung und-Modelle P2S Verbindung

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Die folgende Tabelle enthält zwei Bereitstellungsmodelle und verfügbaren Bereitstellungsmethoden für P2S Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir direkt aus dieser Tabelle.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Grundlegende workflow 

![Punkt-zu-Standort-Diagramm] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "Punkt-zu-Standort")


In den folgenden Abschnitten gehen Sie durch die Schritte zum sichere Punkt-zu-Standort-Verbindung zu einem virtuellen Netzwerk. 

1. Erstellen Sie ein virtuelles Netzwerk und VPN-gateway
2. Zertifikate
3. Die CER-Datei hochladen
4. Das VPN-Client-Konfigurationspaket generieren
5. Konfigurieren des Clientcomputers
6. Verbinden mit Azure

### <a name="example-settings"></a>Konfigurieren

Sie können Folgendes Beispiel:

- **Name: VNet1**
- **Adressraum: 192.168.0.0/16**
- **Subnet-Name: Front-End**
- **Subnetzadressbereichs: 192.168.1.0/24**
- **Abonnement:** Haben Sie mehrere Abonnements überprüfen, ob Sie richtig verwenden.
- **Ressourcengruppe: TestRG**
- **Ort: USA OST**
- **Verbindungstyp: Punkt-zu-Standort**
- **Client-Adressraum: 172.16.201.0/24**. Verbindung mit dieser Punkt-zu-Standort-Verbindung VNet VPN-Clients erhalten eine IP-Adresse aus dem angegebenen Pool.
- **Subnetzname: 192.168.200.0/24**. Gateway-Subnetz muss den Namen "Subnetzname" verwenden.
- **Größe:** Wählen Sie den Gateway-SKU, die Sie verwenden möchten.
- **Routingtyp: dynamische**

## <a name="vnetvpn"></a>Abschnitt 1: Erstellen eines virtuellen Netzwerks und ein VPN-Gateway

### <a name="createvnet"></a>Teil 1: Erstellen eines virtuelles Netzwerks

Haben Sie bereits ein virtuelles Netzwerk, erstellen. Screenshots werden als Beispiele bereitgestellt. Achten Sie darauf, dass Sie die Werte ändern. Erstellen einer VNet mithilfe von Azure Portal gehen Sie folgendermaßen vor: 

1. In einem Browser navigieren Sie zum [Azure-Portal](http://portal.azure.com) und melden Sie sich ggf. mit Ihrem Azure-Konto.

2. **Klicken Sie auf.** Geben Sie in das Suchfeld **am Markt** "Virtuelle Netzwerk". Suchen Sie **Virtuelles Netzwerk** aus der zurückgegebenen Liste, und klicken Sie zum Öffnen von **Virtual Network** -Blade.

    ![Virtuelles Netzwerk Blade suchen] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png "Virtuelles Netzwerk Blade suchen")

3. Unteren Blade virtuelles Netzwerk aus der Liste **Wählen Sie ein Bereitstellungsmodell** wählen Sie **klassische aus**und dann auf **Erstellen**.

    ![Modell auswählen] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png "Modell auswählen")

4. Konfigurieren Sie auf VNet,-Blade **virtuelles Netzwerk erstellen** . Dieses Blatt fügen Sie Ihre erste Adressraum und einen Adressbereich des Subnetzes. Nach dem Erstellen des Vnets, können Sie zurückgehen und Weitere Subnetze und Adressräume hinzufügen.

    ![Erstellen virtuelle Netzwerk blade] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png "Erstellen virtuelle Netzwerk blade")

5. Überprüfen Sie, ob das **Abonnement** korrekt ist. Mithilfe der Dropdownliste können Sie Abonnements ändern.

6. Klicken Sie auf **Ressource** und entweder eine vorhandene Ressourcengruppe auswählen oder einen neuen erstellen einen Namen für die neue Ressourcengruppe eingeben. Beim Erstellen einer neuen Gruppe name der Ressourcengruppe nach geplanten Konfigurationswerte. Weitere Informationen zu Ressourcengruppen Überblick [Azure Ressourcen-Manager](azure-resource-manager/resource-group-overview.md#resource-groups).

7. Als nächstes wählen Sie das **Speicherort** für das VNet. Die Position bestimmen die Bereitstellung dieser VNet Ressourcen befinden werden.

8. Wählen Sie **Pin Dashboard** , möchten Sie möglicherweise finden das VNet im Schaltpult, und klicken Sie auf **Erstellen**.
    
    ![PIN zum dashboard] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png "PIN zum dashboard")


9. Nach dem Klicken auf erstellen, sehen Sie eine Kachel auf dem Dashboard, die den Status der VNet widerspiegeln. Die Kachel ändert das VNet erstellt wird.

    ![Virtuelles Netzwerk erstellen nebeneinander] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Virtuelles Netzwerk erstellen nebeneinander")

10. Nach dem Erstellen des virtuellen Netzwerks können Sie die IP-Adresse eines DNS-Servers hinzufügen, um die Auflösung zu behandeln. Standardeinstellungen für das virtuelle Netzwerk zu öffnen, klicken Sie auf DNS-Servern und die IP-Adresse des DNS-Servers, die Sie verwenden möchten. Diese Einstellung erstellt keine neuen DNS-Servers. Achten Sie darauf, dass Sie einen DNS-Server hinzufügen, dem Ihre Ressourcen kommunizieren können.

Erstellte virtuelle Netzwerk sehen Sie auf der Seite Netzwerk im klassischen Azure-Portal **erstellt** unter **Status** aufgeführt.

### <a name="gateway"></a>Teil 2: Gatewaysubnetz und dynamische routing-Gateway erstellen

In diesem Schritt erstellen Sie ein und dynamische routing-Gateway. In Azure-Portal für klassische Bereitstellungsmodell können gatewaysubnetz und Gateway dieselbe Konfiguration Karten erstellt werden.

1. Navigieren Sie im Portal auf das virtuelle Netzwerk, für das Sie ein Gateway erstellen möchten.

2. Klicken Sie auf die für das virtuelle Netzwerk-Blade **Overview** im Abschnitt VPN-Verbindungen auf **Gateway**.

    ![Klicken Sie hier, um ein Gateway erstellen] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png "Klicken Sie hier, um ein Gateway erstellen")


3. Auf die **VPN-Verbindung** , wählen **Punkt-zu-Standort-**.

    ![P2S-Verbindungstyp] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png "P2S-Verbindungstyp")

4. **Client-Adressraum**fügen Sie den IP-Adressbereich hinzu Dies ist die VPN-Clients eine IP-Adresse beim Verbinden erhält. Automatisch gefüllten Bereich löschen und eigene hinzufügen.

    ![Client-Adressraum] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png "Client-Adressraum")

5. Aktivieren Sie das Kontrollkästchen **Gateway sofort erstellen** .

    ![Gateway sofort erstellen] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png "Gateway sofort erstellen")

6. Klicken Sie auf **Optional Gateway-Konfiguration** öffnen Blade **Gateway-Konfiguration** .

    ![Klicken Sie auf optional Gateway-Konfiguration] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png "Klicken Sie auf optional Gateway-Konfiguration")

7. Klicken Sie auf Hinzufügen **gatewaysubnetz** **Subnetz konfigurieren Einstellungen erforderlich** . Es ist, zwar möglich, ein /29 möglichst klein erstellen empfiehlt sich die größeren Subnetzes erstellen, die mehrere Adressen enthält mindestens /28 oder /27. Damit wird genügend Adressen, die in Zukunft möglicherweise zusätzliche Konfigurationen aufzunehmen.

    >[AZURE.IMPORTANT] Beim Arbeiten mit Gateway vermeiden Sie eine Netzwerk-Sicherheitsgruppe (NSG) Gateway Subnetz zuordnen. Zuordnen einer Netzwerk-Sicherheitsgruppe zu diesem Subnetz möglicherweise das VPN-Gateway nicht mehr wie erwartet funktioniert. Weitere Informationen zu Netzwerk-Sicherheitsgruppen finden Sie unter [Was ist eine Netzwerk-Sicherheitsgruppe?](../articles/virtual-network/virtual-networks-nsg.md)

    ![Subnetzname hinzufügen] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png "Subnetzname hinzufügen")

8. Wählen Sie das Gateway **Größe**. Dies ist das Gateway SKU, mit denen Sie Ihr virtuelles Netzwerk-Gateway erstellen. Das Portal ist Standard SKU **grundlegende**. Weitere Informationen über Gateway SKUs finden Sie [Über VPN-Gateway Settings](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![Gateway-Größe](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)

9. Wählen Sie den **Routingtyp** für Ihr Gateway. P2S Konfigurationen erfordern ein **Dynamisches** routing. Klicken Sie auf **OK** , wenn Sie diese Blade konfiguriert haben.

    ![Routingtyp konfigurieren] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png "Routingtyp konfigurieren")

10. Klicken Sie auf die **VPN-Verbindung** auf **OK** unten das Blade Schlüsselaufgaben virtuelles Netzwerk erstellen. Dies kann bis zu 45 Minuten dauern. 


## <a name="generatecerts"></a>Abschnitt 2 - Zertifikate

Zertifikate werden von Azure zur Authentifizierung von VPN-Clients für Punkt-zu-Standort-VPNs. Öffentliche Daten (nicht der private Schlüssel) wird als eine Base64-x. 509-CER-Datei aus einem Stammzertifikat eine Lösung für Großunternehmen Zertifikat generiert oder ein selbstsigniertes Zertifikat codierte exportieren. Importieren Sie dann die öffentlichen Daten aus dem Stammzertifikat in Azure. Außerdem müssen Sie ein Client-Zertifikat aus dem Stammzertifikat für Clients generieren. Jeder Client, der mit dem virtuellen Netzwerk über eine P2S-Verbindung eine Verbindung herstellen möchte, müssen ein Clientzertifikat installiert, der aus dem Stammzertifikat generiert wurde.

### <a name="cer"></a>Teil 1: Abrufen die CER-Datei für das Stammzertifikat


Bei Verwendung eine Enterprise-Lösung können Sie Ihre vorhandenen Zertifikatkette. Wenn Sie ein Unternehmen CA-Lösung verwenden, können Sie ein selbstsigniertes Zertifikat erstellen. Eine Methode zum Erstellen einer selbst signierten Zertifikat ist Makecert.

- Wenn ein Zertifikat Unternehmenssystem arbeiten, erhalten Sie die CER-Datei für das gewünschte Stammzertifikat. 

- Wenn Sie eine Lösung für Großunternehmen Zertifikat nicht verwenden, müssen Sie ein selbstsigniertes Zertifikat zu generieren. Windows 10 Schritte finden Sie mit [selbst signierten Stammzertifikaten für Punkt-zu-Standort-Konfigurationen](vpn-gateway-certificates-point-to-site.md).

1. Eine CER-Datei aus einem Zertifikat öffnen Sie **certmgr.msc** und suchen Sie das Stammzertifikat. Maustaste selbstsigniertes Zertifikat, klicken Sie auf **Alle Tasks**und dann auf **Exportieren**. Der **Zertifikatsexport-Assistent**wird geöffnet.

2. **Klicken**Sie im Assistenten klicken Sie auf **Weiter**und wählen Sie **Nein, privaten Schlüssel nicht exportieren**.

3. Auf der Seite **Exportdateiformat** wählen **Base-64-codiert x. 509 (. CER).** Klicken Sie dann auf **Weiter**. 

4. Auf die **zu exportierende Datei**, **Suchen** Sie das Zertifikat exportieren möchten an. **Dateiname**Dateinamen Sie den Zertifikat aus. Klicken Sie auf **Weiter**.

5. Klicken Sie auf **Fertig stellen** , um das Zertifikat exportieren.


### <a name="genclientcert"></a>Teil 2: Erstellen Sie ein Clientzertifikat

Sie können entweder ein eindeutiges Zertifikat für jeden Client eine Verbindung herstellt, oder können Sie dasselbe Zertifikat auf mehreren Clients. Generieren eindeutige Clientzertifikate Vorteil ist die Fähigkeit, ggf. ein Zertifikat widerrufen. Wenn alle das gleiche Clientzertifikat und finden, Sie das Zertifikat für einen Client Sperren müssen, müssen Sie zum Generieren und installieren neue Zertifikate für alle Clients, die das Zertifikat zur Authentifizierung verwenden.

- Wenn Sie eine unternehmenszertifikatlösung verwenden, generieren ein Clientzertifikat mit common Name-Wert-Format 'name@yourdomain.com', das Format 'Domänenname\Benutzername'. 

- Wenn Sie ein selbstsigniertes Zertifikat verwenden, finden Sie unter [Arbeiten mit selbst signierten Stammzertifikaten für Punkt-zu-Standort-Konfigurationen](vpn-gateway-certificates-point-to-site.md) ein Client-Zertifikat generieren.

### <a name="exportclientcert"></a>Teil 3: Exportieren das Clientzertifikat

Installieren Sie ein Clientzertifikat auf jedem Computer, der mit dem virtuellen Netzwerk verbinden möchten. Ein Client-Zertifikat ist zur Authentifizierung erforderlich. Automatisieren Sie das Clientzertifikat installieren oder manuell installieren. Die folgenden Schritte führen Sie durch Exportieren und das Client-Zertifikat manuell installieren.

1. Um ein Clientzertifikat zu exportieren, können Sie *certmgr.msc*. Klicken Sie auf das Clientzertifikat zu exportieren, klicken Sie auf **Alle Tasks**und klicken Sie dann auf **Exportieren**.
2. Exportieren Sie das Client-Zertifikat mit dem privaten Schlüssel. Dies ist eine *PFX* -Datei. Vergewissern Sie sich, und das Kennwort (Schlüssel), der für dieses Zertifikat festgelegt.

## <a name="upload"></a>Abschnitt 3 - Root Zertifikat CER-Datei hochladen

Nach dem Erstellen das Gateway können Sie die CER-Datei für ein vertrauenswürdiges Zertifikat in Azure hochladen. Sie können bis zu 20 Stammzertifikate Dateien hochladen. Sie nicht den privaten Schlüssel für das Stammzertifikat in Azure hochladen. Nachdem die CER-Datei hochgeladen, von Azure Clients authentifizieren, die mit dem virtuellen Netzwerk verwendet.

1. Klicken Sie auf Abschnitt **VPN-Verbindungen** des Blades für das VNet auf die **Clients** Grafik Öffnen der **Punkt-zu-Standort-VPN-Verbindung** Blade.

    ![Clients] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png "Clients")

2. Auf der **Punkt-zu-Standort-Verbindung** Blade, klicken Sie auf **Zertifikate verwalten** **Zertifikate** Blade öffnen.<br>

    ![Zertifikate-blade](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png "Certificates blade")<br><br>


3. Blade **Zertifikate** klicken Sie auf **Hochladen** , um Blade **Hochladen Zertifikat** öffnen.<br>

    ![Upload Zertifikate blade](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png "Upload certificates blade")<br>


4. Klicken Sie auf den Ordner Grafik CER-Datei suchen. Die Datei, klicken Sie auf **OK**. Aktualisieren Sie die Seite um hochgeladene Zertifikat auf die **Zertifikate** anzuzeigen.

    ![Zertifikat hochladen](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png "Upload certificate")<br>
    

## <a name="vpnclientconfig"></a>Abschnitt 4 - VPN-Client-Konfigurationspaket generieren

Um das virtuelle Netzwerk zu verbinden, müssen Sie auch einen VPN-Client. Der Clientcomputer benötigt ein Client-Zertifikat und dem entsprechenden VPN-Konfiguration Clientpaket Verbindung.

Das VPN-Client-Paket enthält die in Windows integrierte VPN-Clientsoftware konfigurieren. Paket wird keine zusätzlichen Software installiert. Die Einstellungen für das virtuelle Netzwerk zu verbinden. Die Liste der unterstützten Clientbetriebssystemen finden Sie unter [Punkt-zu-Standort-Verbindung](vpn-gateway-vpn-faq.md#point-to-site-connections) VPN-Gateway FAQ-Abschnitt. 

### <a name="to-generate-the-vpn-client-configuration-package"></a>Das VPN-Client-Konfigurationspaket generieren

1. Azure-Portal Blatt **Übersicht** für das VNet **VPN-Verbindungen**klicken Sie auf die Grafik Client Öffnen der **Punkt-zu-Standort-VPN-Verbindung** Blade.
2. Am Anfang von der **Punkt-zu-Standort-VPN-Verbindung** Blade, klicken Sie auf das Downloadpaket Clientbetriebssystem entspricht, auf dem er installiert:

 - Wählen Sie **VPN-Client (64 Bit)**für 64-Bit-Clients.
 - Wählen Sie **VPN-Client (32 Bit)**für 32-Bit-Clients.

    ![Paket für VPN-Client herunterladen](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png "Download VPN client configuration package")<br>


3. Sie sehen eine Meldung Azure VPN Client-Konfigurationspaket für das virtuelle Netzwerk generiert. Nach einigen Minuten das Paket generiert, und Sie sehen eine Meldung auf Ihrem lokalen Computer, dass das Paket. Paket-Konfigurationsdatei zu speichern. Sie installiert diese auf jedem Clientcomputer, die mit dem virtuellen Netzwerk P2S verbunden wird.


## <a name="clientconfiguration"></a>Abschnitt 5: konfigurieren den Clientcomputer

### <a name="part-1-install-the-client-certificate"></a>Teil 1: Clientzertifikat installieren

Jeder Clientcomputer muss ein Client-Zertifikat zur Authentifizierung. Wenn das Clientzertifikat installieren, benötigen Sie das Kennwort, das erstellt wurde, wenn das Clientzertifikat exportiert wurde.

1. Kopieren Sie die PFX-Datei auf dem Clientcomputer.
2. Doppelklicken Sie auf die PFX-Datei, um sie zu installieren. Ändern Sie den Installationspfad.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>Teil 2: Installieren der VPN-Client-Konfigurationspaket

VPN-Client-Konfiguration Packung können auf jedem Clientcomputer, die Version die Architektur für den Client entspricht.

1. Kopieren Sie die Konfigurationsdatei lokal auf dem Computer zu verbinden mit dem virtuellen Netzwerk, und doppelklicken Sie auf die .exe-Datei. 

2. Wenn das Paket installiert wurde, können Sie die VPN-Verbindung starten. Das Paket wird nicht von Microsoft signiert. Sie können das Paket Signaturdienst Ihres Unternehmens anmelden oder mit [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327)signieren. Es ist das Paket ohne verwenden. Wenn das Paket ist nicht signiert, wird eine Warnung jedoch bei der Installation des Pakets.

3. Auf dem Clientcomputer zu **Network Settings** und klicken Sie auf **VPN**. Sie sehen die Verbindung aufgeführt. Es wird der Name des virtuellen Netzwerks Verbinden mit und sieht etwa wie folgt: 

    ![VPN-client] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png "VNet VPN-client")

## <a name="connect"></a>Abschnitt 6: Azure verbinden

### <a name="connect-to-your-vnet"></a>Verbinden mit Ihrem VNet

1. Verbindung mit Ihrem VNet auf dem Clientcomputer zu VPN-Verbindungen und suchen Sie die VPN-Verbindung, die Sie erstellt haben. Er wird den gleichen Namen wie das virtuelle Netzwerk bezeichnet. Klicken Sie auf **Verbinden**. Eine Popup-Meldung erscheint, die mit dem Zertifikat verweist. In diesem Fall klicken Sie auf **Weiter** , um erhöhte Rechte. 

2. Klicken Sie auf der Statusseite **Verbindung** auf **Verbinden** , um die Verbindung zu starten. Wenn Sie ein **Zertifikat auswählen** angezeigt, überprüfen Sie, ob das Client-Zertifikat angezeigt wird, die Sie herstellen möchten. Nicht verwenden Sie den Dropdown Pfeil, um das richtige Zertifikat auszuwählen und klicken Sie dann auf **OK**.

    ![Verbinden] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png "VPN-Client-Verbindung")

3. Die Verbindung sollte nun hergestellt.

    ![Verbindung hergestellt] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png "Verbindung hergestellt")

### <a name="verify-the-vpn-connection"></a>Die VPN-Verbindung

1. Um sicherzustellen, dass die VPN-Verbindung aktiv ist, öffnen Sie ein Eingabeaufforderungsfenster mit erhöhten Rechten, und führen Sie *Ipconfig/All*.
2. Die Ergebnisse anzeigen. Beachten Sie, dass die IP-Adresse erhalten Sie eine Adresse im Adressbereich der Punkt-zu-Standort-Verbindung beim Erstellen des Vnets angegeben. Das Ergebnis sollte etwa so:

Beispiel:



    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="next-steps"></a>Nächste Schritte

Sie können virtuelle Computer mit dem virtuellen Netzwerk hinzufügen. Finden Sie unter [Erstellen eines benutzerdefinierten virtuellen Computers](../virtual-machines/virtual-machines-windows-classic-createportal.md).