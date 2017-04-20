<properties
   pageTitle="Konfigurieren von Punkt-zu-Standort-VPN-Gateway-Verbindung mit einem virtuellen Azure-Netzwerk mit dem Verwaltungsportal | Microsoft Azure"
   description="Sichere Verbindung mit dem virtuellen Netzwerk Azure eine Punkt-zu-Standort-VPN-Gateway Verbindung."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>Konfigurieren einer Punkt-zu-Standort-Verbindung mit einer VNet mit dem Verwaltungsportal

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Classic - Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
- [Classic - Verwaltungsportal](vpn-gateway-point-to-site-create.md)

Eine Punkt-zu-Standort (P2S)-Konfiguration können Sie eine sichere Verbindung von einem Clientcomputer zu einem virtuellen Netzwerk erstellen. P2S Verbindung ist nützlich, wenn Sie möchten das VNet von einem Remotestandort aus wie von zu Hause oder einer Konferenz oder wenn nur wenige Clients, die Verbindung zu einem virtuellen Netzwerk.

Dieser Artikel führt Sie durch die Erstellung einer VNet mit einer Punkt-zu-Standort-Verbindung im **klassischen Bereitstellungsmodell** **Verwaltungsportal**.

Punkt-zu-Standort-Verbindung erfordern keine VPN-Gerät oder eine öffentliche IP-Adresse zu. Eine VPN-Verbindung erfolgt durch die Client-Computer die Verbindung ab. Weitere Informationen über Punkt-zu-Standort-Verbindung finden Sie unter [VPN-Gateway FAQ](vpn-gateway-vpn-faq.md#point-to-site-connections) und [Planung und Entwicklung](vpn-gateway-plan-design.md).


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Bereitstellung und-Modelle P2S Verbindung

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Die folgende Tabelle enthält zwei Bereitstellungsmodelle und verfügbaren Bereitstellungsmethoden für P2S Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir direkt aus dieser Tabelle.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 



## <a name="basic-workflow"></a>Grundlegende workflow

![Punkt-zu-Standort-Diagramm] (./media/vpn-gateway-point-to-site-create/p2sclassic.png "Punkt-zu-Standort")
 
Folgenden Schritte führen Sie durch die Schritte zum sichere Punkt-zu-Standort-Verbindung zu einem virtuellen Netzwerk. 

Die Konfiguration für eine Punkt-zu-Standort-Verbindung ist in vier Abschnitte unterteilt. Die Reihenfolge in der diese Abschnitte konfigurieren ist wichtig. Keine Schritte überspringen und vorgreifen.

- **Abschnitt 1** Erstellen Sie ein virtuelles Netzwerk und VPN-Gateway.
- **Abschnitt 2** Erstellen Sie die Zertifikate für die Authentifizierung und hochladen.
- **Abschnitt 3** Exportieren und die Client-Zertifikate installieren.
- **Abschnitt 4** Konfigurieren Sie Ihren VPN-Client.

## <a name="vnetvpn"></a>Abschnitt 1: Erstellen eines virtuellen Netzwerks und ein VPN-Gateway

### <a name="part-1-create-a-virtual-network"></a>Teil 1: Erstellen eines virtuelles Netzwerks

1. Melden Sie sich im [klassischen Azure-Portal](https://manage.windowsazure.com/). Diese Schritte verwenden Verwaltungsportal nicht Azure-Portal. Derzeit können keine P2S Erstellen einer Verbindung mit Azure-Portal.

2. Unten links auf dem Bildschirm klicken Sie auf **neu**. Klicken Sie im Navigationsbereich auf **Netzwerkdienste**und klicken Sie dann auf **Virtuelles Netzwerk**. Klicken Sie auf **Benutzerdefinierte** , um den Konfigurations-Assistenten zu starten.

3. Geben Sie auf der Seite **Virtuelle Netzwerkdetails** die folgenden Informationen und klicken Sie dann auf den Pfeil rechts.
    - **Name**: Namen für das virtuelle Netzwerk. Beispielsweise 'VNet1'. Dies ist der Name, den Sie beim Bereitstellen von VMs für dieses VNet verweisen können.
    - **Position**: die Position bezieht sich direkt auf den physischen Speicherort (Region) Ressourcen (VMs) befinden. Soll die VMs, die Bereitstellung dieses virtuellen Netzwerk physisch im südostasiatischen USA befinden, wählen Sie diesen Speicherort ein. Sie können die Region mit dem virtuellen Netzwerk nach der Erstellung nicht ändern.

4. Geben Sie auf der Seite **DNS-Server und VPN-Konnektivität** die folgenden Informationen und klicken Sie dann auf den Pfeil rechts.
    - **DNS-Server**: Geben Sie den DNS-Servernamen und IP-Adresse oder einen zuvor registrierten DNS-Server aus dem Kontextmenü auswählen. Diese Einstellung wird einen DNS-Server nicht erstellt. Dadurch werden die DNS-Server angeben, die für die Auflösung von Namen für dieses virtuelle Netzwerk verwenden möchten. Wenn Sie Azure Standard-Namensauflösungsdienst verwenden möchten, lassen Sie in diesem Abschnitt
    - **Konfigurieren von Punkt-zu-Standort-VPN**: Aktivieren Sie das Kontrollkästchen.

5. Geben Sie auf **Punkt-zu-Standort-Verbindung** den IP-Adressbereich von dem VPN-Clients eine IP-Adresse bei Verbindung erhalten. Es gibt einige Regeln für die Adressbereiche, die Sie angeben können. Es ist wichtig zu überprüfen, ob der Bereich mit dem lokalen Netzwerk Bereiche nicht überlappen.

6. Geben Sie Folgendes ein, und klicken Sie auf den Pfeil.
 - **Adressraum**: die Start-IP- und CIDR (Zählung) enthalten.
 - **Adressraum hinzufügen**: Adressraum nur hinzufügen, wenn für den Netzwerkentwurf erforderlich ist.

7. Geben Sie auf der Seite **Virtuelles Netzwerk Adressräume** den Adressbereich für das virtuelle Netzwerk verwenden möchten. Dynamischen IP-Adressen (DIPS), die VMs und andere Instanzen, die in diesem virtuellen Netzwerk bereitstellen zugeordnet sind.<br><br>Es ist besonders wichtig, die nicht überlappt mit anderen Bereichen ausgewählt, die für das lokale Netzwerk verwendet werden. Sie müssen bei Ihrem Netzwerkadministrator koordinieren, die sich ein Bereich von IP-Adressen aus Ihrem lokalen Netzwerkadressbereich für das virtuelle Netzwerk verwenden.

8. Geben Sie folgende Informationen, und klicken Sie auf das Häkchen, um das virtuelle Netzwerk erstellen.
 - **Adressraum**: hinzufügen den internen IP-Adressbereich, die Sie für diese virtuelle Netzwerk einschließlich der Start-IP- und Count verwenden möchten. Es ist wichtig, die nicht überlappt mit anderen Bereichen ausgewählt, die für das lokale Netzwerk verwendet werden. 
 - **Subnetz hinzufügen**: zusätzliche Subnetze sind nicht erforderlich, aber Sie möchten ein separates Subnetz für virtuelle Computer zu erstellen, die statische DIPS. Oder Sie möchten für Ihre virtuellen Computer in einem Subnetz getrennt von der anderen Instanzen.
 - **Gateway-Subnetz hinzufügen**: das gatewaysubnetz ist für eine Punkt-zu-Standort-VPN. Klicken Sie im gatewaysubnetz. Gatewaysubnetz wird nur für das virtuelle Netzwerkgateway verwendet.

9. Erstellte virtuelle Netzwerk sehen Sie auf der Seite Netzwerk im klassischen Azure-Portal **erstellt** unter **Status** aufgeführt. Erstellte virtuelle Netzwerk können Sie das dynamische routing Gateway erstellen.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Teil 2: Erstellen von dynamischen routing-Gateway

Gatewaytyp muss als dynamisch konfiguriert werden. Statische routing Gateways funktionieren nicht mit diesem Feature.

1. Klicken Sie auf das virtuelle Netzwerk, das Sie erstellt, und die Seite **Dashboard** im klassischen Azure-Portal, auf der Seite **Netzwerk** .

2. Klicken Sie auf **Gateway erstellen**am unteren Rand der Seite **Dashboard** . Eine Meldung gefragt, **möchten Sie ein Gateway für virtuelle Netzwerk "VNet1"**. Klicken Sie auf **Ja,** um das Gateway erstellen. Es dauert ca. 15 Minuten für das Gateway erstellen.

## <a name="generate"></a>Abschnitt 2: erzeugen und Hochladen von Zertifikaten

Zertifikate werden zur Authentifizierung von VPN-Clients für Punkt-zu-Standort-VPNs. Verwenden Sie ein Stammzertifikat eine Lösung für Großunternehmen Zertifikat generiert oder ein selbstsigniertes Zertifikat verwenden. Sie können bis zu 20 Stammzertifikate in Azure hochladen. Nachdem die CER-Datei hochgeladen, können Azure enthaltene Informationen zum Authentifizieren von Clients, die ein Clientzertifikat installiert ist. Das Clientzertifikat muss das gleiche Zertifikat generiert werden, die CER-Datei darstellt.

In diesem Abschnitt wird Folgendes:

- CER-Datei für ein Stammzertifikat zu erhalten. Dies kann ein selbst signiertes Zertifikat oder Ihrem Unternehmenssystem Zertifikat verwenden.
- Hochladen Sie die CER-Datei in Azure.
- Generieren von Clientzertifikaten.

### <a name="root"></a>Teil 1: Abrufen die CER-Datei für das Stammzertifikat

Wenn ein Zertifikat Unternehmenssystem arbeiten, erhalten Sie die CER-Datei für das gewünschte Stammzertifikat. In [Teil 3](#createclientcert)generieren Sie die Clientzertifikate aus dem Stammzertifikat.

Wenn Sie eine Lösung für Großunternehmen Zertifikat nicht verwenden, müssen Sie ein selbstsigniertes Zertifikat zu generieren. Windows 10 Schritte finden Sie mit [selbst signierten Stammzertifikaten für Punkt-zu-Standort-Konfigurationen](vpn-gateway-certificates-point-to-site.md). Der Artikel führt Sie durch ein selbst signiertes Zertifikat generieren und exportieren Sie die CER-Datei Makecert mit.

### <a name="upload"></a>Teil 2: Upload Root Zertifikat CER-Datei zum klassischen Azure-portal

Hinzufügen eines vertrauenswürdigen Zertifikats in Azure. Beim Hinzufügen eine Base64-codierte x. 509 (.) in Azure erzählen Sie Azure, das Stammzertifikat zu vertrauen, das die Datei darstellt.

1. Klicken Sie auf **Uploaden Sie ein Stammzertifikat**im klassischen Azure-Portal, auf der Seite **Zertifikate** für das virtuelle Netzwerk.

2. Auf der Seite **Zertifikat hochladen** Stammzertifikat CER suchen Sie und klicken Sie dann auf das Häkchen.

### <a name="createclientcert"></a>Teil 3: Erstellen Sie ein Clientzertifikat

Als Nächstes generieren Sie Clientzertifikate. Sie können entweder ein eindeutiges Zertifikat für jeden Client eine Verbindung herstellt, oder können Sie dasselbe Zertifikat auf mehreren Clients. Generieren eindeutige Clientzertifikate Vorteil ist die Fähigkeit, ggf. ein Zertifikat widerrufen. Wenn alle das gleiche Clientzertifikat und finden, Sie das Zertifikat für einen Client Sperren müssen, müssen Sie zum Generieren und installieren neue Zertifikate für alle Clients, die das Zertifikat zur Authentifizierung verwenden.

- Wenn Sie eine unternehmenszertifikatlösung verwenden, generieren ein Clientzertifikat mit common Name-Wert-Format 'name@yourdomain.com', anstatt NetBIOS "Domäne\Benutzername" formatieren. 

- Wenn Sie ein selbstsigniertes Zertifikat verwenden, finden Sie unter [Arbeiten mit selbst signierten Stammzertifikaten für Punkt-zu-Standort-Konfigurationen](vpn-gateway-certificates-point-to-site.md) ein Client-Zertifikat generieren.

## <a name="installclientcert"></a>Abschnitt 3: exportieren und Clientzertifikat installieren

Installieren Sie ein Clientzertifikat auf jedem Computer, der mit dem virtuellen Netzwerk verbinden möchten. Ein Client-Zertifikat ist zur Authentifizierung erforderlich. Automatisieren Sie das Clientzertifikat installieren oder Sie manuell installieren können. Die folgenden Schritte führen Sie durch Exportieren und das Client-Zertifikat manuell installieren.

1. Um ein Clientzertifikat zu exportieren, können Sie *certmgr.msc*. Klicken Sie auf das Clientzertifikat zu exportieren, klicken Sie auf **Alle Tasks**und klicken Sie dann auf **Exportieren**.
2. Exportieren Sie das Client-Zertifikat mit dem privaten Schlüssel. Dies ist eine *PFX* -Datei. Vergewissern Sie sich, und das Kennwort (Schlüssel), der für dieses Zertifikat festgelegt.
3. Kopieren Sie die *PFX* -Datei auf dem Clientcomputer. Doppelklicken Sie auf dem Clientcomputer auf die *PFX* -Datei, um sie zu installieren. Geben Sie das Kennwort angefordert. Ändern Sie den Installationspfad.

## <a name="vpnclientconfig"></a>Abschnitt 4: Konfigurieren der VPN-Client

Um das virtuelle Netzwerk zu verbinden, müssen Sie auch einen VPN-Client. Der Client erfordert ein Clientzertifikat und die Konfiguration der VPN-Client um eine Verbindung herzustellen. Konfigurieren Sie einen VPN-Client führen Sie die folgenden Schritte nacheinander.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Teil 1: Erstellen das VPN-Client-Konfigurationspaket

1. Navigieren Sie im klassischen Azure-Portal, auf der Seite " **Dashboard** " für das virtuelle Netzwerk Blick Menü rechts. Die Liste der unterstützten Clientbetriebssystemen finden Sie unter [Punkt-zu-Standort-Verbindung](vpn-gateway-vpn-faq.md#point-to-site-connections) VPN-Gateway FAQ-Abschnitt. Das VPN-Client-Paket enthält die in Windows integrierte VPN-Clientsoftware konfigurieren. Paket wird keine zusätzlichen Software installiert. Die Einstellungen für das virtuelle Netzwerk zu verbinden.<br><br>Wählen Sie das Downloadpaket Clientbetriebssystem entspricht, auf dem er installiert werden:
 - Wählen Sie für 32-Bit-Clients **32-Bit-Client-VPN-Paket herunterladen**.
 - Aktivieren Sie für 64-Bit-Clients **die 64-Bit-Client VPN-Paket**.

2. Es dauert ein paar Minuten Ihr Clientpaket erstellen. Nach Abschluss des Pakets können Sie die Datei herunterladen. Die *.exe* -Datei, die Sie herunterladen kann sicher auf dem lokalen Computer gespeichert werden.

3. Generieren und Paket-VPN-Client von klassischen Azure-Portal können Sie das Client-Paket auf dem Clientcomputer installieren aus denen virtuellen Netzwerk herstellen möchten. VPN-Client auf mehreren Clientcomputern installiert werden soll, stellen Sie sicher, dass sie auch ein Clientzertifikat installiert.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Teil 2: Installieren das Paket VPN-Konfiguration auf dem client

1. Kopieren Sie die Konfigurationsdatei lokal auf dem Computer zu verbinden mit dem virtuellen Netzwerk, und doppelklicken Sie auf die .exe-Datei. 

2. Wenn das Paket installiert wurde, können Sie die VPN-Verbindung starten. Das Paket wird nicht von Microsoft signiert. Sie können das Paket Signaturdienst Ihres Unternehmens anmelden oder mit [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327)signieren. Es ist das Paket ohne verwenden. Wenn das Paket ist nicht signiert, wird eine Warnung jedoch bei der Installation des Pakets.

3. Auf dem Clientcomputer zu **Network Settings** und klicken Sie auf **VPN**. Sie sehen die Verbindung aufgeführt. Es zeigt dem Namen des virtuellen Netzwerks Verbinden mit und sieht etwa wie folgt: 

    ![VPN-client] (./media/vpn-gateway-point-to-site-create/vpn.png "VPN-client")


### <a name="part-3-connect-to-azure"></a>Teil 3: Verbinden mit Azure

1. Verbindung mit Ihrem VNet auf dem Clientcomputer zu VPN-Verbindungen und suchen Sie die VPN-Verbindung, die Sie erstellt haben. Er wird den gleichen Namen wie das virtuelle Netzwerk bezeichnet. Klicken Sie auf **Verbinden**. Eine Popup-Meldung erscheint, die mit dem Zertifikat verweist. In diesem Fall klicken Sie auf **Weiter** , um erhöhte Rechte. 

2. Klicken Sie auf der Statusseite **Verbindung** auf **Verbinden** , um die Verbindung zu starten. Wenn Sie ein **Zertifikat auswählen** angezeigt, überprüfen Sie, ob das Client-Zertifikat angezeigt wird, die Sie herstellen möchten. Nicht verwenden Sie den Dropdown Pfeil, um das richtige Zertifikat auszuwählen und klicken Sie dann auf **OK**.

    ![VPN-Client 2] (./media/vpn-gateway-point-to-site-create/clientconnect.png "VPN-Client-Verbindung")

3. Die Verbindung sollte nun hergestellt.

    ![VPN-Client 3] (./media/vpn-gateway-point-to-site-create/connected.png "VPN-Client-Verbindung 2")

### <a name="part-4-verify-the-vpn-connection"></a>Teil 4: Überprüfen Sie die VPN-Verbindung

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

Weitere Informationen über virtuelle Netzwerke, finden Sie unter Seite [Virtuelle Netzwerkdokumentation](https://azure.microsoft.com/documentation/services/virtual-network/) .
