<properties
    pageTitle="Installieren von Active Directory-Gesamtstruktur in Azure virtual Network | Microsoft Azure"
    description="Ein Lernprogramm, das Erstellen einer neuen Active Directory-Gesamtstruktur auf einer virtuellen Maschine (VM) in einem virtuellen Netzwerk Azure erläutert."
    services="active-directory, virtual-network"
    keywords="Active Directory virtuellen Computer installieren active Directory-Gesamtstruktur Azure active Directory videos "
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    tags=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Installieren einer neuen Active Directory-Gesamtstruktur in Azure virtual network

In diesem Thema zeigt eine [Azure virtual Network](../virtual-network/virtual-networks-overview.md)erstellen Sie eine neue Windows Server Active Directory-Umgebung auf ein Azure virtuelles Netzwerk auf einer virtuellen Maschine (VM). In diesem Fall ist Azure virtuelle Netzwerk nicht mit einem lokalen Netzwerk verbunden.

Sie können auch diese Themen interessieren:

- Ein Video folgendermaßen, finden Sie unter [neue Active Directory-Gesamtstruktur in Azure virtual Network installieren](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
- Sie können optional [eine Standort-zu-Standort-VPN konfigurieren](../vpn-gateway/vpn-gateway-site-to-site-create.md) und installieren Sie dann eine neue Gesamtstruktur oder eine lokalen Gesamtstruktur Azure virtuelle Netzwerk. Diese Schritte finden Sie unter [ein Replikat Active Directory-Domänencontroller in einem virtuellen Netzwerk Azure installieren](../active-directory/active-directory-install-replica-active-directory-domain-controller.md).
-  Grundlegende Hinweise zum Installieren von Active Directory-Domänendienste (AD DS) in Azure virtual Network finden Sie unter [Richtlinien für die Bereitstellung von Windows Server Active Directory in Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Szenario-Diagramm

In diesem Szenario müssen externe Benutzern Zugriff auf Server Domäne ausgeführt Applications. Die VMs, die der Anwendungsserver ausgeführt und VMs, die Domänencontroller sind installiert in eigenen Clouddienst Azure virtual Network. Sie sind auch in eine Verfügbarkeit für Verbesserte Fehlertoleranz festgelegt.

![Active Directory-Gesamtstruktur auf einem virtuellen Computer in Azure Virtual Network ][1] 7
## <a name="how-does-this-differ-from-on-premises"></a>Wie unterscheidet sich dies von lokalen?

Es ist nicht viel Unterschied im Vergleich zu lokalen Domänencontroller auf Azure installieren. In der folgenden Tabelle werden die wichtigsten Unterschiede aufgeführt.

Konfigurieren...  | Vor Ort  | Azure virtual network
------------- | -------------  | ------------
**IP-Adresse für den Domänencontroller**  | Zuweisen von statischen IP-Adresse für den Netzwerkadapter-Eigenschaften   | Führen Sie das Cmdlet Set-AzureStaticVNetIP eine statische IP-Adresse zuweisen
**DNS-Clientauflösung**  | Bevorzugte und alternative DNS-Server-Adresse im Netzwerk Adaptereigenschaften Domänenmitglieder festgelegt.   | Festlegen von DNS-Serveradresse auf die Eigenschaften der virtuellen
**Active Directory-Datenbank-Speicherung**  | Optional ändern Sie den Standardspeicherort von C:\  | Sie müssen C:\ Standardspeicherort ändern



## <a name="create-an-azure-virtual-network"></a>Ein Azure virtuelles Netzwerk erstellen

1. Mit klassischen Azure-Portal anmelden.
2. Erstellen Sie ein virtuelles Netzwerk. Klicken Sie auf **Netzwerke** > **Erstellen eines virtuellen Netzwerks**. Verwenden Sie die Werte in der folgenden Tabelle, um den Assistenten abzuschließen.

    Auf dieser Seite des Assistenten...  | Geben Sie diese Werte
    ------------- | -------------
    **Virtuelles Netzwerk-Details**  | <p>Name: Geben Sie einen Namen für das virtuelle Netzwerk</p><p>Region: Wählen Sie den nächsten Bereich</p>
    **DNS und VPN**  | <p>DNS-Server leer</p><p>Wählen Sie entweder VPN-option</p>
    **Virtuelles Netzwerk-Adressräume**  | <p>Subnet-Name: Geben Sie einen Namen für das Subnetz</p><p>Start-IP-: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p>



## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>Führen Sie die Domänencontroller und DNS-Server virtuelle Computer erstellen

Wiederholen Sie folgende VMs Hosten von Domänencontrollern nach Bedarf erstellen. Sie sollten mindestens zwei virtuellen DCs Fehlertoleranz und Redundanz bereitstellen. Enthält das Azure virtuelle Netzwerk mindestens zwei Domänencontroller, die entsprechend konfiguriert sind (, sind beide GCs zur DNS-Server und enthält weder eine FSMO-Rolle usw.) setzen Sie die VMs, die die DCs eine verbesserte Fehlertoleranz festgelegt Verfügbarkeit ausgeführt.

Die VMs anstelle der Benutzeroberfläche mithilfe von Windows PowerShell finden Sie [Mit Azure PowerShell zu Windows-basierten virtuellen Maschinen vorkonfigurieren](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. Im klassischen Portal klicken Sie auf **neu** > **berechnen** > **virtuellen** > **Aus Galerie**. Verwenden Sie folgende Werte, um den Assistenten abzuschließen. Übernehmen Sie den Standardwert für eine Einstellung, wenn ein anderer Wert empfohlen oder erforderlich ist.

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

## <a name="install-windows-server-active-directory"></a>Installieren Sie Windows Server Active Directory

Verwenden Sie die gleiche Routine, die Verwendung von lokalen [Installation von AD DS](https://technet.microsoft.com/library/jj574166.aspx) (d. h. können die Benutzeroberfläche einer Antwortdatei oder Windows PowerShell). Sie müssen Administrator angeben, um eine neue Gesamtstruktur installieren. Um den Speicherort für die Active Directory-Datenbank, Protokollen und SYSVOL anzugeben, den Standardspeicherort aus dem Betriebssystem-Laufwerk zu wechseln zusätzliche Datenträger, den die die VM zugeordnet

Nach Abschluss der Installation DC Verbindung zur VM erneut, und melden Sie sich an den DC. Beachten Sie Anmeldeinformationen angeben.

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>Den DNS-Server für das virtuelle Netzwerk Azure zurücksetzen

1. Setzen Sie die Einstellung DNS-Weiterleitung auf dem neuen Domänencontroller und DNS-Server zurück.
  1. Klicken Sie im Server-Manager auf **Extras** > **DNS**.
  2. Namen Sie im **DNS-Manager**den des DNS-Servers, und klicken Sie auf **Eigenschaften**.
  3. Klicken Sie auf der Registerkarte **Weiterleitung** auf die IP-Adresse der Weiterleitung, und klicken Sie auf **Bearbeiten**.  Die IP-Adresse, und klicken Sie auf **Löschen**.
  4. Klicken Sie auf **OK** , um den Editor zu schließen und **Ok** , um die Eigenschaften zu schließen.
2. Die DNS-Einstellung für das virtuelle Netzwerk zu aktualisieren.
  1. Klicken Sie auf **Virtuelle Netzwerke** > doppelklicken Sie auf das virtuelle Netzwerk erstellen > **Konfigurieren** > **DNS-Server**Geben Sie den Namen und DIP eines VMs, die Domänencontroller und DNS-Serverrolle ausgeführt wird und klicken Sie auf **Speichern**.
  2. Wählen Sie die VMs aus und auf **Starten** zum Auslösen der VM DNS-Auflösungsdienst die IP-Adresse des neuen DNS-Servers konfigurieren.


## <a name="create-vms-for-domain-members"></a>Erstellen von VMs für Domänenmitglieder

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


## <a name="see-also"></a>Siehe auch

-  [Installieren eine neue Active Directory-Gesamtstruktur in Azure virtual network](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
-  [Richtlinien für die Bereitstellung von Windows Server Active Directory in Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)

-  [Konfigurieren eines Standort-zu-Standort-VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)
-  [Installieren von Active Directory Active Directory in Azure virtual network](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
-  [Microsoft Azure IT Pro IaaS: (01) Virtual Machine-Grundlagen](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) erstellen virtuelle Netzwerke und standortübergreifende Konnektivität](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Virtuelles Netzwerk (Übersicht)](../virtual-network/virtual-networks-overview.md)
-  [Zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure-Cmdlet-Referenz](https://msdn.microsoft.com/library/azure/jj554330.aspx)
-  [Azure VM statische IP-Adresse](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
-  [Azure VM statische IP-Adresse zuweisen](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
-  [Installieren Sie eine neue Active Directory-Gesamtstruktur](https://technet.microsoft.com/library/jj574166.aspx)
-  [Einführung in Active Directory Domain Services (AD DS) Virtualisierung (Level 100)](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
