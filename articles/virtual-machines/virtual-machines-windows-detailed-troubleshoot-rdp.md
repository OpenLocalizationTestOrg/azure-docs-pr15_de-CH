<properties
    pageTitle="Detaillierte Remotedesktop Problembehandlung | Microsoft Azure"
    description="Überprüfen Sie detaillierte Problembehandlungsschritte für remote desktop-Fehler, kann nicht, auf einem virtuellen Windows-Maschinen in Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"
    keywords="keine Verbindung zu Remotedesktop von Remotedesktop, Remotedesktop kann keine Verbindung hergestellt, remote desktop Fehler Remotedesktop Problembehandlung, Remotedesktopproblemen
"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Ausführliche Schritte zur Problembehandlung für Remotedesktopverbindung Probleme auf Windows-VMs in Azure

Dieser Artikel enthält detaillierte Problembehandlungsschritte zur diagnose und Behebung komplexer Remotedesktop Fehler für Windows-basierte Azure virtuelle Maschinen.

> [AZURE.IMPORTANT] Um mehr Remotedesktop-Fehler zu vermeiden, müssen Sie [Grundlegende Problembehandlung Artikel für Remotedesktop](virtual-machines-windows-troubleshoot-rdp-connection.md) vor.

Eine Remotedesktop-Fehlermeldung auftreten, die nicht in [der grundlegenden Remotedesktop](virtual-machines-windows-troubleshoot-rdp-connection.md)behandelt bestimmte Fehlermeldungen ähnlich. Gehen Sie ermitteln, warum der Remote Desktop (RDP)-Client keine Verbindung zum Dienst RDP Azure VM.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Benötigen Sie weitere Hilfe zu diesem Artikel erhalten Sie Azure-Experten auf [MSDN Azure und Foren Stapelüberlauf](https://azure.microsoft.com/support/forums/). Alternativ können Sie eine Azure Supportfall Datei. Der [Azure-Support-Website](https://azure.microsoft.com/support/options/) und klicken Sie auf **Support zu erhalten**. Informationen zur Verwendung von Azure-Unterstützung, lesen Sie den [Microsoft Azure-Fragen](https://azure.microsoft.com/support/faq/).


## <a name="components-of-a-remote-desktop-connection"></a>Remotedesktop-Verbindung

Die folgenden Komponenten spielen in eine RDP-Verbindung:

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_0.png)

Bevor Sie fortfahren, kann damit geistig überprüfen, was sich seit der letzten erfolgreichen Remotedesktopverbindung für die VM geändert hat. Zum Beispiel:

- Die öffentliche IP-Adresse der VM oder VM (so genannte virtuelle IP-Adresse [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) mit Cloud-Dienst wurde geändert. RDP-Fehler möglicherweise DNS-Clientcache noch die *alte IP-Adresse* für den DNS-Namen registriert hat. Leeren Sie den DNS-Client-Cache zu und versuchen Sie die VM erneut. Oder versuchen Sie es mit der neuen VIP.
- Eine Anwendung eines Drittanbieters verwenden Ihre Remotedesktopverbindungen anstelle von Azure-Portal generierte Verbindung verwalten. Sicher, dass die Konfiguration korrekten TCP-Port für den Remotedesktop-Datenverkehr enthält. Überprüfen dieser Port für eine klassische virtuelle Maschine im [Azure-Portal](https://portal.azure.com)klicken Sie die VM > Endpunkte.


## <a name="preliminary-steps"></a>Vorbereitende Schritte

Bevor Sie mit ausführlichen Problembehandlung

- Überprüfen Sie den Status des virtuellen Computers im klassischen Azure-Portal oder Azure-Portal offensichtliche Probleme.
- Führen Sie die [Schritte für RDP-Fehlermeldungen in der grundlegenden Quick Fix](virtual-machines-windows-troubleshoot-rdp-connection.md#quick-troubleshooting-steps).


Versuchen Sie nach diesen Schritten VM per Remote Desktop wiederherstellen.


## <a name="detailed-troubleshooting-steps"></a>Detaillierte Schritte zur Fehlerbehebung

Der Remote Desktop Client möglicherweise nicht zu den Remotedesktopdienst Azure VM durch Probleme in den folgenden Quellen:

- [Remote Desktop Client-computer](#source-1-remote-desktop-client-computer)
- [Organisation Intranet peripheres Gerät](#source-2-organization-intranet-edge-device)
- [Cloud-Endpunkt und Zugriffssteuerungsliste (ACL)](#source-3-cloud-service-endpoint-and-acl)
- [Netzwerk-Sicherheitsgruppen](#source-4-network-security-groups)
- [Windows-basierte Azure VM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>: 1 Remotedesktopclient Quellcomputer

Stellen Sie sicher, dass Ihr Computer Remotedesktopverbindungen zu einem anderen lokalen, Windows-basierten Computer.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_1.png)

Wenn nicht, überprüfen Sie Folgendes auf Ihrem Computer:

- Eine lokale Firewall-Einstellung, die Remotedesktop-Datenverkehr blockiert.
- Proxy-Clientsoftware, die Remotedesktopverbindungen verhindert lokal installiert.
- Lokal installiert Netzwerküberwachungssoftware, die Remotedesktopverbindungen verhindert wird.
- Andere Sicherheitssoftware, die Datenverkehr überwachen oder bestimmte Arten von Datenverkehr, die Remotedesktopverbindungen verhindern zulassen/nicht zulassen.

In allen diesen Fällen vorübergehend deaktiviert und auf einem lokalen Computer über Remote Desktop zugreifen. Wenn die tatsächliche Ursache auf diese Weise Sie erfahren, arbeiten Sie mit Ihrem Netzwerkadministrator Proxyeinstellungen Software um Remotedesktopverbindungen zu ermöglichen.

## <a name="source-2-organization-intranet-edge-device"></a>Quelle 2: Organisation Intranet peripheres Gerät

Stellen Sie sicher, dass ein Computer direkt mit dem Internet Remotedesktopverbindungen mit virtuellen Computers Azure.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_2.png)

Wenn Sie keinen Computer haben, der direkt mit dem Internet verbunden ist, erstellen und Testen mit einem neuen Azure virtuellen Computer in einer Gruppe oder Cloud Ressourcen. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Computers Windows in Azure ausgeführt](virtual-machines-windows-hero-tutorial.md). Sie können den virtuellen Computer und Ressourcengruppe oder Cloud-Dienst nach dem Test löschen.

Überprüfen Sie beim Erstellen von einer Remotedesktopverbindung mit einem Computer direkt mit dem Internet verbundenen Organisation Intranet Rand Geräts:

- Eine interne Firewall blockieren HTTPS-Verbindung zum Internet.
- Ein Proxyserver Remotedesktopverbindungen verhindern.
- Erkennung von Eindringversuchen oder Netzwerküberwachungssoftware auf Geräte in Ihrem Netzwerk, die Remotedesktopverbindungen verhindert wird.

Arbeiten Sie mit Ihrem Netzwerkadministrator die Proxyeinstellungen des Geräts Rand Intranet Organisation um HTTPS-basierten Remotedesktop-Verbindung zum Internet zu ermöglichen.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Quelle 3: Cloud-Endpunkt und ACL

VMs mit klassischen Bereitstellungsmodell erstellt stellen Sie sicher, einen anderen im selben Cloud-Dienst oder virtuelles Netzwerk Azure-VM Remotedesktopverbindungen Azure-VM zu können.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_3.png)

> [AZURE.NOTE] Ressourcen-Manager erstellte virtuelle Maschinen, fahren Sie mit [Quelle 4: Netzwerksicherheitsgruppen](#source-4-network-security-groups).

Wenn Sie keinen anderen virtuellen Computer im selben Cloud-Dienst oder virtuelles Netzwerk erstellen. Gehen Sie in [Windows in Azure Erstellen eines virtuellen Computers ausgeführt](virtual-machines-windows-hero-tutorial.md). Löschen Sie Test-VM nach Abschluss des Tests

Wenn Sie über Remotedesktop mit einem virtuellen Computer in der gleichen Cloud-Dienst oder virtuelles Netzwerk verbinden können, überprüfen Sie diese Einstellung:

- Die Endpunktkonfiguration für Remotedesktop-Datenverkehr auf dem Ziel-VM: private TCP-Port des Endpunkts muss TCP-Port auf dem die VM Remotedesktopdienst überwacht (Standard ist 3389).
- Die ACL für den Remotedesktop Datenverkehr Endpunkt auf dem Ziel-VM: ACLs können Sie angeben, gewährten oder verweigerten Datenverkehr aus dem Internet basierend auf der IP-Quelladresse. Falsch konfigurierte ACLs kann Datenverkehr Remotedesktop auf den Endpunkt. Überprüfen Sie die ACLs sicherzustellen, eingehenden Datenverkehr von der öffentlichen IP-Adressen der Proxy oder anderen Edgeserver zulässig. Weitere Informationen finden Sie unter [Was ist ein Netzwerk Zugriffssteuerungsliste (ACL)?](../virtual-network/virtual-networks-acl.md)

Prüfen, ob der Endpunkt die Ursache des Problems ist, entfernen Sie den aktuellen Endpunkt und erstellen Sie eine neue, einen zufälligen Port im Bereich von 49152 bis 65535 für externe Portnummer auswählen. Weitere Informationen finden Sie unter [Endpunkte auf einem virtuellen Computer einrichten](virtual-machines-windows-classic-setup-endpoints.md).

## <a name="source-4-network-security-groups"></a>4 Quelle: Netzwerk-Sicherheitsgruppen

Netzwerk-Sicherheitsgruppen zulassen eine genauere Kontrolle der zulässigen eingehenden und ausgehenden Datenverkehr Sie können mehrere Subnetze Regeln erstellen und Clouddienste in Azure virtual Network. Überprüfen Sie Ihre Netzwerk-Sicherheitsgruppe Regeln sicher, dass Remotedesktop Datenverkehr aus dem Internet:

- Wählen Sie in der Azure-Portal VM
- Klicken Sie auf **Alle** | **Netzwerkschnittstellen** , und wählen Sie die Netzwerkschnittstelle.
- Klicken Sie auf **Alle** | **Netzwerk-Sicherheitsgruppe** , und wählen Sie die Netzwerk-Sicherheitsgruppe.
- Klicken Sie auf **Alle** | **eingehende Sicherheitsregeln** und sicherzustellen, dass eine Regel RDP TCP-Port 3389.
    - Wenn Sie eine Regel nicht verfügen, klicken Sie auf **Hinzufügen** , um eine Regel zu erstellen. Geben Sie für das Protokoll und für den Zielbereich Port **3389** **TCP** .
    - Stellen Sie sicher, dass die Aktion auf **Zulassen** festgelegt ist, und klicken Sie auf OK, um die neue eingehende Regel speichern.


Weitere Informationen finden Sie unter [Neuigkeiten Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)

## <a name="source-5-windows-based-azure-vm"></a>Quelle 5: Windows-basierten Azure VM

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_5.png)

Verwenden Sie das [Diagnosepaket Azure IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) der Fehler aufgrund der Azure virtuelle Maschine ist. Wenn dieses Diagnosepaket das **RDP-Verbindung zum Azure-VM (Neustart erforderlich)** Problem kann, gehen Sie in [diesem Artikel](virtual-machines-windows-reset-rdp.md). Dieser Artikel setzt den Remotedesktopdienst auf dem virtuellen Computer:

- Aktivieren Sie die Standardregel "Remotedesktop" Windows-Firewall (TCP-Port 3389).
- Aktivieren Sie Remotedesktopverbindungen, indem HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections-Registrierungswert auf 0 festlegen.

Wiederholen Sie den Vorgang aus dem Computer. Wenn Sie noch nicht über Remotedesktop herstellen können, überprüfen Sie folgende mögliche Probleme:

- Der Remotedesktopdienst ist nicht auf dem Zielcomputer VM ausgeführt.
- Der Remotedesktopdienst hört TCP-Port 3389 nicht.
- Windows-Firewall oder eine andere lokale Firewall hat eine ausgehende Regel, die Remotedesktop-Datenverkehr verhindern.
- Erkennung von Eindringversuchen oder Netzwerküberwachungssoftware auf Azure Virtual Machine verhindert Remotedesktopverbindungen.

Für virtuelle Computer mit dem klassischen Bereitstellungsmodell erstellt können Sie eine Remotesitzung Azure PowerShell Azure virtuellen Computer verwenden. Zunächst müssen Sie ein Zertifikat für die virtuellen Computer hosting Cloud-Dienst installieren. Zum [Sichern PowerShell RAS konfigurieren, Azure Virtual Machines](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) und die Datei **InstallWinRMCertAzureVM.ps1** auf Ihrem lokalen Computer herunterladen.

Installieren Sie dann Azure PowerShell, wenn nicht bereits geschehen. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

Anschließend öffnen Sie ein Eingabeaufforderungsfenster Azure PowerShell und ändern Sie den aktuellen Ordner auf den Speicherort der Skriptdatei **InstallWinRMCertAzureVM.ps1** . Um eine Azure PowerShell-Skript auszuführen, müssen Sie die richtige Ausführungsrichtlinie festlegen. Führen Sie den Befehl **Get-ExecutionPolicy** der aktuellen Richtlinienebene bestimmt. Informationen zum Festlegen der entsprechenden Ebene finden Sie unter [Set-ExecutionPolicy](https://technet.microsoft.com/library/hh849812.aspx).

Anschließend füllen in Azure Abonnementname Cloud Service Name sowie der Name des virtuellen Computers (entfernt die Zeichen < und >), und führen Sie diese Befehle.

    $subscr="<Name of your Azure subscription>"
    $serviceName="<Name of the cloud service that contains the target virtual machine>"
    $vmName="<Name of the target virtual machine>"
    .\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName

Sie können den richtigen Namen aus der _SubscriptionName_ -Eigenschaft der Ausgabe des Befehls **Get-AzureSubscription** abrufen. Cloud Service Name für den virtuellen Computer können _ServiceName_ Spalte in der Ausgabe des Befehls **Get-AzureVM** abgerufen werden.

Überprüfen Sie, ob das neue Zertifikat. Ein Snap-In Zertifikate für den aktuellen Benutzer öffnen Sie, und suchen Sie im Ordner **Vertrauenswürdige Stamm aufgeführt** . Ein Zertifikat mit dem DNS-Namen des Cloud-Diensts in der Spalte Ausgestellt für sollte angezeigt werden (Beispiel: cloudservice4testing.cloudapp.net).

Als Nächstes Initiieren einer Remotesitzung Azure PowerShell mit diesen Befehlen.

    $uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
    $creds = Get-Credential
    Enter-PSSession -ConnectionUri $uri -Credential $creds

Geben Sie gültige Anmeldeinformationen, sollte ähnlich der folgenden Azure PowerShell Prompt angezeigt werden:

    [cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>

Der erste Teil dieses ist Cloud Dienstnamen, die von "cloudservice4testing.cloudapp.net" sein könnte das Ziel VM enthält. Sie können jetzt Azure PowerShell ausgeben, Befehle für diesen Clouddienst Probleme untersuchen erwähnt und korrigieren Sie die Konfiguration.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>TCP-Port Abhören Remotedesktopdienste manuell korrigieren

Führen Sie diesen Befehl, wenn Sie nicht das [Diagnosepaket Azure IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) für das Problem **RDP-Verbindung zum Azure-VM (Neustart erforderlich)** an den remote Azure PowerShell-Sitzung ausgeführt werden.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

Die Portnummer-Eigenschaft zeigt die aktuelle Anschlussnummer. Ändern Sie bei Bedarf den Remotedesktop Zahl zurück auf den Standardwert (Port 3389) mit diesem Befehl port.

    Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389

Stellen Sie sicher, dass der Port 3389, geändert wurde, mit diesem Befehl.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

Mit diesem Befehl Azure PowerShell-Remotesitzung beenden.

    Exit-PSSession

Überprüfen Sie, ob der Remotedesktop Endpunkt der Azure-VM auch TCP-Port 3398 internen Port. Starten der Azure-VM und Remotedesktop Verbindung.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Azure Diagnosepaket IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)

[Zurücksetzen eines Kennworts oder der Remotedesktopdienst für virtuelle Windows-Maschinen](virtual-machines-windows-reset-rdp.md)

[Zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)

[Problembehandlung bei Secure Shell (SSH) Verbindung zu einer Linux-basierten Azure virtuellen Maschine](virtual-machines-linux-troubleshoot-ssh-connection.md)

[Problembehandlung bei Zugriff auf eine Anwendung auf einer Azure virtuellen Computer](virtual-machines-linux-troubleshoot-app-connection.md)
