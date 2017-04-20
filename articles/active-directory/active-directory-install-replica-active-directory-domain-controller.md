<properties
    pageTitle="Installieren von Active Directory Active Directory in Azure | Microsoft Azure"
    description="Ein Lernprogramm, das Installieren eines Domänencontrollers von lokalen Active Directory-Gesamtstruktur auf einem virtuellen Computer Azure erläutert."
    services="virtual-network"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Installieren von Active Directory Active Directory in Azure virtual network

In diesem Thema veranschaulicht Installieren zusätzlicher Domänencontroller (auch Replikat Domänencontroller) für lokale Active Directory-Domäne auf Azure virtuelle Maschinen (VMs) in Azure virtual Network.

Sie können auch diese Themen interessieren:

-  Sie können optional eine neue Active Directory-Gesamtstruktur in Azure virtual Network. Diese Schritte finden Sie unter [neue Active Directory-Gesamtstruktur in Azure virtual Network installieren](../active-directory/active-directory-new-forest-virtual-machine.md).
-  Grundlegende Hinweise zum Installieren von Active Directory-Domänendienste (AD DS) in Azure virtual Network finden Sie unter [Richtlinien für die Bereitstellung von Windows Server Active Directory in Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx).


## <a name="scenario-diagram"></a>Szenario-Diagramm

In diesem Szenario müssen externe Benutzern Zugriff auf Server Domäne ausgeführt Applications. VMs Application Server ausführen und die Replikat-Domänencontroller in Azure virtual Network installiert sind. Das virtuelle Netzwerk angeschlossen werden auf das lokale Netzwerk durch eine [Standort-zu-Standort-VPN-](../vpn-gateway/vpn-gateway-site-to-site-create.md) Verbindung wie in der folgenden Abbildung dargestellt oder [ExpressRoute](../expressroute/expressroute-locations-providers.md) für schnellere Verbindung verwenden.

Der Anwendungsserver und Domänencontroller werden in separaten Clouddienste Compute Verarbeitung verteilen und [Verfügbarkeit legt](../virtual-machines/virtual-machines-windows-manage-availability.md) für Verbesserte Fehlertoleranz bereitgestellt.
Domänencontroller replizieren untereinander und mit lokalen Domänencontrollern mithilfe von Active Directory-Replikation. Keine Synchronisierung Werkzeuge erforderlich.

![Diagramm Pf Replikat Active Directory-Domänencontroller Azure vnet][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Erstellen einer Active Directory-Standort für Azure virtuelle Netzwerk

In diesem Zusammenhang empfiehlt es sich, eine Site in Active Directory erstellen, die entsprechende Netzwerk-Bereich an das virtuelle Netzwerk darstellt. Das Optimieren Authentifizierung, Replikation und anderer Operationen DC Speicherort. Die folgenden Schritte erläutern zum Erstellen einer Website und Weitere Hintergrundinformationen finden Sie unter [Hinzufügen einer neuen Site](https://technet.microsoft.com/library/cc781496.aspx).

1. Öffnen Sie Active Directory-Standorte und-Dienste: **Server Manager** > **Tools** > **Active Directory-Standorte und -Dienste**.
2. Erstellen Sie eine Website für den Bereich dar, in dem ein Azure virtuelles Netzwerk erstellt: Klicken Sie auf **Sites** > **Aktion** > **neue Website** > Geben Sie den Namen des neuen Standorts wie Azure US West > Website klicken > **OK**.
3. Erstellen Sie ein Subnetz und ordnen Sie die neue Website: Doppelklicken Sie auf **Websites** > Maustaste **Subnetze** > **Neues Subnetz** > Geben Sie den IP-Adressbereich des virtuellen Netzwerks (z. B. 10.1.0.0/16 im Diagramm Szenario) > Wählen Sie die neue Azure Site > **OK**.

## <a name="create-an-azure-virtual-network"></a>Ein Azure virtuelles Netzwerk erstellen

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) **neu** > **Netzwerkdienste** > **Virtual Network** > **Benutzerdefinierte** und verwenden die folgenden Werte des Assistenten.

    Auf dieser Seite des Assistenten...  | Geben Sie diese Werte
    ------------- | -------------
    **Virtuelles Netzwerk-Details**  | <p>Name: Geben Sie einen Namen für das virtuelle Netzwerk wie WestUSVNet.</p><p>Region: Wählen Sie den nächsten Bereich.</p>
    **DNS und VPN-Konnektivität**  | <p>DNS-Server: Geben Sie den Namen und die IP-Adresse eine oder mehrere lokale DNS-Server.</p><p>-Konnektivität: **Konfigurieren einer Standort-zu-Standort-VPN**auswählen</p><p>Lokalen Netzwerk: Geben Sie ein neues lokales Netzwerk.</p><p>Bei Verwendung von ExpressRoute anstelle eines VPN finden Sie unter [Konfigurieren einer ExpressRoute-Verbindung über einen Exchange-Anbieter](../expressroute/expressroute-locations-providers.md).</p>
    **Standort-zu-Standort-Verbindung**  | <p>Name: Geben Sie einen Namen für das lokale Netzwerk.</p><p>VPN-Geräte-IP-Adresse: Geben Sie die öffentliche IP-Adresse des Geräts, das mit dem virtuellen Netzwerk verbinden. Das VPN-Gerät kann nicht hinter einem NAT befinden</p><p>: Adresse die Adressbereiche für das lokale Netzwerk (z. B. 192.168.0.0/16 im Diagramm Szenario).</p>
    **Virtuelles Netzwerk-Adressräume**  | <p>Adressraum: Geben Sie den IP-Adressbereich für VMs in Azure virtuellen Netzwerk (z. B. 10.1.0.0/16 im Diagramm Szenario) ausgeführt werden soll. Dieser Adressbereich kann mit der Adresse des lokalen Netzwerks nicht überlappen.</p><p>Subnetze: Geben Sie einen Namen und eine Adresse für ein Subnetz für den Anwendungsserver (wie Front-End, 10.1.1.0/24) und die DCS (wie Back-End, 10.1.2.0/24).</p><p>Klicken Sie auf **gatewaysubnetz**.</p>

2. Anschließend konfigurieren Sie das virtuelle Netzwerkgateway sichere Standort-zu-Standort-VPN-Verbindung. Die Anleitung finden Sie unter [konfigurieren ein virtuelles Netzwerk-Gateway](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .
3. Erstellen Sie die Standort-zu-Standort-VPN-Verbindung zwischen neues virtuelles Netzwerk und lokalen VPN-Gerät. Die Anleitung finden Sie unter [konfigurieren ein virtuelles Netzwerk-Gateway](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .


## <a name="create-azure-vms-for-the-dc-roles"></a>Erstellen von Azure VMs für DC-Rollen

Wiederholen Sie folgende VMs Hosten von Domänencontrollern nach Bedarf erstellen. Sie sollten mindestens zwei virtuellen DCs Fehlertoleranz und Redundanz bereitstellen. Enthält das Azure virtuelle Netzwerk mindestens zwei Domänencontroller, die entsprechend konfiguriert sind (, sind beide GCs zur DNS-Server und enthält weder eine FSMO-Rolle usw.) setzen Sie die VMs, die die DCs eine verbesserte Fehlertoleranz festgelegt Verfügbarkeit ausgeführt.
Die VMs anstelle der Benutzeroberfläche mithilfe von Windows PowerShell finden Sie [Mit Azure PowerShell zu Windows-basierten virtuellen Maschinen vorkonfigurieren](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) **neu** > **berechnen** > **virtuellen** > **Aus Galerie**. Verwenden Sie folgende Werte, um den Assistenten abzuschließen. Übernehmen Sie den Standardwert für eine Einstellung, wenn ein anderer Wert empfohlen oder erforderlich ist.

    Auf dieser Seite des Assistenten...  | Geben Sie diese Werte
    ------------- | -------------
    **Wählen Sie ein Bild**  | Windows Server 2012 R2 Datacenter
    **Konfiguration des virtuellen Computers**  | <p>Virtual Machine-Name: Geben Sie ein einzelnes Etikett (z. B. AzureDC1).</p><p>Neuer Benutzername: Geben Sie den Namen eines Benutzers. Dieser Benutzer wird Mitglied der lokalen Administratorengruppe auf dem virtuellen Computer sein. Sie benötigen diesen Namen für die VM zum ersten Mal anmelden. Das integrierte Konto Administrator funktioniert nicht.</p><p>Neues und Bestätigtes Kennwort: Geben Sie ein Kennwort</p>
    **Konfiguration des virtuellen Computers**  | <p>Cloud-Dienst: <b>Erstellen einen neuen Cloud-Dienst</b> für den ersten virtuellen Computer wählen Sie aus und wählen Sie dieser Cloud Service Name mehr VMs erstellen, das Domänencontrollern gehostet wird.</p><p>Cloud-Dienst DNS-Name: Geben Sie einen global eindeutigen Namen</p><p>Bereich Gruppe/Virtual Network: Geben Sie den Namen des virtuellen Netzwerks (z. B. WestUSVNet).</p><p>Speicherkonto: Wählen Sie <b>verwenden automatisch erzeugte Speicherkonto</b> für den ersten virtuellen Computer und wählen Sie mehr VMs erstellen, das Domänencontrollern gehostet wird, denselben Kontonamen Storage.</p><p>: Verfügbarkeit auswählen <b>einer Verfügbarkeit erstellen</b>.</p><p>Verfügbarkeit festlegen Name: Geben Sie einen Namen für die Verfügbarkeit festgelegt, wenn Sie den ersten virtuellen Computer erstellen und dann, denselben Namen, wenn Sie weitere virtuelle Computer erstellen.</p>
    **Konfiguration des virtuellen Computers**  | <p>Wählen Sie <b>den VM-Agent installieren</b> und Erweiterungen benötigten.</p>
2. Fügen Sie einen Datenträger für jede VM, die DC-Serverrolle ausgeführt wird. AD-Datenbank, Protokollen und SYSVOL gespeichert ist zusätzliche Speicherplatz erforderlich. Geben Sie eine Größe für den Datenträger (z. B. 10 GB), und lassen Sie die **Host-Cache-Einstellung** auf **None**festgelegt. Die Schritte finden Sie unter [Legen Sie einen Datenträger auf einem Windows-Computer](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Nachdem Sie zuerst die VM anmelden, öffnen Sie **Server-Manager** > **Datei und** zum Erstellen eines Datenträgers auf diesem Datenträger mit NTFS.
4. Reservieren Sie eine statische IP-Adresse für VMs, die Domänencontrollern ausgeführt werden. Reservieren Sie eine statische IP-Adresse herunterladen Sie Microsoft-Webplattform-Installer und [Azure PowerShell installieren](../powershell-install-configure.md) , und führen Sie das Cmdlet "Set-AzureStaticVNetIP". Zum Beispiel:

    "Get-AzureVM - ServiceName AzureDC1-Namen AzureDC1 | Set-AzureStaticVNetIP - IP-Adresse 10.0.0.4 | AzureVM aktualisieren

Weitere Informationen zum Festlegen einer statischen IP-Adresse finden Sie unter [Konfigurieren einer statischen interne IP-Adresse für einen virtuellen Computer](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Installieren von AD DS auf Azure VMs

Melden Sie an, um einen virtuellen Computer, und überprüfen Sie die Verbindung über die Standort-zu-Standort-VPN oder ExpressRoute Verbindung auf Ressourcen im lokalen Netzwerk. Installieren Sie AD DS auf Azure-VMs. Dabei können, mit denen Sie einen zusätzlichen Domänencontroller in Ihrem lokalen Netzwerk (Benutzeroberfläche, Windows PowerShell oder eine Antwortdatei) installieren. Installieren von AD DS stellen Sie sicher, dass Sie das neue Volume den Speicherort der AD-Datenbank, Protokollen und SYSVOL. Sie aktualisieren auf AD DS-Installation finden Sie unter [Installieren Active Directory Domain Services (Level 100)](https://technet.microsoft.com/library/hh472162.aspx) oder [Installieren Sie Windows Server 2012 Active Directory in einer vorhandenen Domäne (Level 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>DNS-Server für das virtuelle Netzwerk neu konfigurieren

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)auf den Namen des virtuellen Netzwerks, und klicken Sie dann auf die Registerkarte **Konfigurieren** , verwenden Sie die statischen IP-Adressen des Replikats DCs anstelle einer lokalen DNS-Server die IP-Adressen [die DNS-Server IP-Adressen für Ihr virtuelles Netzwerk konfigurieren](../virtual-network/virtual-networks-manage-dns-in-vnet.md) .

2. Um sicherzustellen, dass alle Replica DC VMs auf das virtuelle Netzwerk mit Verwendung der DNS-Server auf dem virtuellen Netzwerk konfiguriert sind, klicken Sie auf **virtuellen Maschinen**klicken Sie auf die Statusspalte für jeden virtuellen Computer und klicken Sie dann auf **neu starten**. Warten Sie, bis die VM Zustand **ausgeführt** wird, bevor Sie sich wieder anmelden.

## <a name="create-vms-for-application-servers"></a>Erstellen von VMs für Anwendungsserver

1. Wiederholen Sie die folgenden Schritte erstellen Sie virtuelle Computer als Anwendungsserver ausgeführt. Übernehmen Sie den Standardwert für eine Einstellung, wenn ein anderer Wert empfohlen oder erforderlich ist.

    Auf dieser Seite des Assistenten...  | Geben Sie diese Werte
    ------------- | -------------
    **Wählen Sie ein Bild**  | Windows Server 2012 R2 Datacenter
    **Konfiguration des virtuellen Computers**  | <p>Virtual Machine-Name: Geben Sie ein einzelnes Etikett (z. B. AppServer1).</p><p>Neuer Benutzername: Geben Sie den Namen eines Benutzers. Dieser Benutzer wird Mitglied der lokalen Administratorengruppe auf dem virtuellen Computer sein. Sie benötigen diesen Namen für die VM zum ersten Mal anmelden. Das integrierte Konto Administrator funktioniert nicht.</p><p>Neues und Bestätigtes Kennwort: Geben Sie ein Kennwort</p>
    **Konfiguration des virtuellen Computers**  | <p>Cloud-Dienst: **Erstellen einen neuen Cloud-Dienst** für den ersten virtuellen Computer wählen Sie aus und wählen Sie demselben Cloud Service Namen aus, wenn Sie weitere virtuelle Computer erstellen, der die Anwendung hostet.</p><p>Cloud-Dienst DNS-Name: Geben Sie einen global eindeutigen Namen</p><p>Bereich Gruppe/Virtual Network: Geben Sie den Namen des virtuellen Netzwerks (z. B. WestUSVNet).</p><p>Speicherkonto: Wählen Sie **verwenden eine automatisch generierte Speicherkonto** für die erste VM und wählen Sie demselben Speicher Kontonamen, wenn Sie weitere virtuelle Computer erstellen, der die Anwendung hostet.</p><p>: Verfügbarkeit auswählen **einer Verfügbarkeit erstellen**.</p><p>Verfügbarkeit festlegen Name: Geben Sie einen Namen für die Verfügbarkeit festgelegt, wenn Sie den ersten virtuellen Computer erstellen und dann, denselben Namen, wenn Sie weitere virtuelle Computer erstellen.</p>
    **Konfiguration des virtuellen Computers**  | <p>Wählen Sie <b>den VM-Agent installieren</b> und Erweiterungen benötigten.</p>

2. Nach jeder VM bereitgestellt wird, anmelden und der Domäne beitreten. Klicken Sie im **Server-Manager**auf **Lokale Server** > **Arbeitsgruppe** > **ändern...** Wählen Sie **Domäne aus** und geben Sie den Namen der lokalen Domäne. Anmeldeinformationen Sie eines Domänenbenutzers, und starten Sie die VM vervollständigen das Beitreten zu einer Domäne.

Die VMs anstelle der Benutzeroberfläche mithilfe von Windows PowerShell finden Sie [Mit Azure PowerShell zu Windows-basierten virtuellen Maschinen vorkonfigurieren](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

Finden Sie weitere Informationen zur Verwendung von Windows PowerShell [mit Azure-Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx) und [Azure Cmdlet Verweis](https://msdn.microsoft.com/library/azure/jj554330.aspx).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

-  [Richtlinien für die Bereitstellung von Windows Server Active Directory in Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)
-  [Lokale Hyper-V Domänencontroller in Azure mithilfe von Azure PowerShell uploaden](http://support.microsoft.com/kb/2904015)
-  [Installieren einer neuen Active Directory-Gesamtstruktur in Azure virtual network](../active-directory/active-directory-new-forest-virtual-machine.md)
-  [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
-  [Microsoft Azure IT Pro IaaS: (01) Virtual Machine-Grundlagen](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) erstellen virtuelle Netzwerke und standortübergreifende Konnektivität](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure Management Cmdlets](https://msdn.microsoft.com/library/azure/jj152841)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
