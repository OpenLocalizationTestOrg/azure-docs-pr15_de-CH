<properties
    pageTitle="Detaillierte SSH Problembehandlung für eine VM Azure | Microsoft Azure"
    description="Detaillierte Schritte zur Fehlerbehebung für Probleme mit einer Azure Virtual Machine SSH"
    keywords="SSH Verbindung zurückgewiesen, ssh Fehler, Azure ssh SSH-Verbindung fehlgeschlagen"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="detailed-ssh-troubleshooting-steps"></a>Detaillierte Problembehandlungsschritte SSH

Es gibt viele Gründe, die SSH-Client möglicherweise nicht zu den SSH-Dienst auf dem virtuellen Computer. Wenn Sie weitere [SSH Problembehebung](virtual-machines-linux-troubleshoot-ssh-connection.md)ausgeführt haben, müssen Sie das Problem diagnostizieren. Dieser Artikel führt Sie durch detaillierte Problembehandlungsschritte, die SSH-Verbindung fehlgeschlagen ist und wie es zu lösen.

## <a name="take-preliminary-steps"></a>Vorbereitende Schritte

Das folgende Diagramm zeigt die Komponenten.

![Diagramm Komponenten des SSH-Dienstes zeigt](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

Die folgenden Schritte unterstützen Sie isolieren der Fehlerquelle und Solutions oder Workarounds.

Überprüfen Sie zunächst den Status des virtuellen Computers im Portal.

In der [Azure-Portal](https://portal.azure.com):

1. Für virtuelle Computer mit dem klassischen Bereitstellungsmodell erstellt, wählen Sie **Durchsuchen** > **virtuellen Maschinen (classic)** > *Name des virtuellen Computers*.

    -ODER-

    Für virtuelle Computer mithilfe des Ressourcen-Manager erstellt, wählen Sie **Durchsuchen** > **virtuellen Computer** > *Name des virtuellen Computers*.

    Im Statusbereich der VM weisen **ausgeführt**. Scrollen Sie Aktivitäten für Compute, Speicher und Netzwerk-Ressourcen anzeigen.

2. **Einstellungen** Endpunkte, IP-Adressen und andere Optionen zu wählen.

    Um Endpunkte auf VMs identifizieren, die mit Ressourcen-Manager erstellt wurden, überprüfen Sie, ob eine [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) definiert wurde. Überprüfen Sie auch die Regeln der Netzwerk-Sicherheitsgruppe angewendet wurden, die sie im Subnetz verwiesen werden.

Im [klassischen Azure-Portal](https://manage.windowsazure.com)für VMs mit klassischen Bereitstellungsmodell erstellt wurden:

1. Wählen Sie die **virtuellen Computer** > *Name des virtuellen Computers*.
2. Wählen Sie die VM **Dashboard** den Status überprüfen.
3. Wählen Sie **Monitor** an Aktivitäten für Compute, Speicher und Netzwerk-Ressourcen.
4. Wählen Sie **Endpunkte** um sicherzustellen, dass es ein Endpunkt für SSH-Verkehr.

Überprüfen Sie Netzwerkkonnektivität überprüfen Sie konfigurierter Endpunkte, erreicht die VM über ein anderes Protokoll wie HTTP oder einem anderen Dienst.

Versuchen Sie nach diesen Schritten die SSH-Verbindung herzustellen.


## <a name="find-the-source-of-the-issue"></a>Die Quelle des Problems

SSH-Client auf Ihrem Computer möglicherweise zu den SSH-Dienst auf Azure VM Probleme oder Konfigurationsfehler im folgenden:

- [SSH-Client-computer](#source-1-ssh-client-computer)
- [Organisation Edge-Gerät](#source-2-organization-edge-device)
- [Cloud-Endpunkt und Zugriffssteuerungsliste (ACL)](#source-3-cloud-service-endpoint-and-acl)
- [Netzwerk-Sicherheitsgruppen](#source-4-network-security-groups)
- [Linux-basierte Azure VM](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>1 Quelle: SSH Clientcomputer

Um Ihren Computer als Ursache des Fehlers zu vermeiden, überprüfen Sie SSH-Verbindung zu einem anderen lokalen, Linux-basierten Computer herstellen können.

![Diagramm, das SSH-Client Computer bearbeiten](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Wenn die Verbindung fehlschlägt, überprüfen Sie Folgendes auf Ihrem Computer:

- Eine lokale Einstellung, Firewall blockiert eingehenden oder ausgehenden SSH-Datenverkehr (TCP 22)
- Proxy-Clientsoftware, die SSH-Verbindungen verhindert lokal installiert
- Lokal installierte Netzwerküberwachungssoftware, die SSH-Verbindung verhindern
- Andere Sicherheitssoftware, die Datenverkehr überwachen oder bestimmte Arten von Datenverkehr zulassen/nicht zulassen

Wenn eine der folgenden Situationen zutrifft, vorübergehend deaktiviert, und versuchen Sie eine SSH-Verbindung zu einem lokalen Computer um den Grund herauszufinden, die Verbindung auf dem Computer blockiert wird. Funktioniert mit Ihrem softwareeinstellungen um SSH Verbindungen korrigieren.

Wenn Sie die Zertifikatauthentifizierung verwenden, überprüfen Sie diese Berechtigungen .ssh Ordner im Basisverzeichnis:

- Chmod 700 ~/.ssh
- Chmod 644 ~/.ssh/\*pub
- Chmod 600 ~/.ssh/id_rsa (oder andere Dateien, die Ihre gespeicherten privaten Schlüssel)
- Chmod 644 ~/.ssh/known_hosts (enthält Hosts, die Sie über SSH verbunden sind)

## <a name="source-2-organization-edge-device"></a>Quelle 2: Organisation Edge-Gerät

Um Ihre Organisation Edge-Gerät als Ursache des Fehlers zu vermeiden, stellen Sie sicher, dass ein Computer direkt mit dem Internet verbunden ist SSH-Verbindung zum Azure-VM. Wenn Sie den virtuellen Computer über ein Standort-zu-Standort-VPN oder eine Azure ExpressRoute-Verbindung zugreifen, fahren Sie mit [Quelle 4: Sicherheitsgruppen Netzwerk](#nsg).

![Diagramm, das Unternehmen peripheres Gerät markiert](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Haben Sie einen Computer, der direkt mit dem Internet verbunden erstellen neue Azure VM in einer eigenen Ressourcengruppe Clouddienst oder verwenden. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Computers in Azure Linux ausgeführt](virtual-machines-linux-quick-create-cli.md). Löschen Sie danach mit der Ressourcengruppe oder VM und Cloud-Dienst.

Wenn Sie eine SSH-Verbindung mit einem Computer erstellen können, die direkt mit dem Internet verbunden ist, überprüfen Sie für Ihre Organisation Edge-Gerät:

- Eine interne Firewall, die SSH-Datenverkehr mit dem Internet blockieren
- Proxyserver, der SSH-Verbindung verhindern
- Erkennung von Eindringversuchen oder Netzwerküberwachungssoftware auf Geräte in Ihrem Netzwerk, die SSH-Verbindung verhindern

Arbeiten Sie mit Ihrem Netzwerkadministrator die Proxyeinstellungen für Ihre Organisation Rand-Devices um SSH-Datenverkehr mit dem Internet.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Quelle 3: Cloud-Endpunkt und ACL

> [AZURE.NOTE] Dabei gilt nur für virtuelle Computer, die mit dem klassischen Bereitstellungsmodell erstellt wurden. Virtuelle Computer, die mit dem Ressourcen-Manager erstellt wurden, fahren Sie mit [4 Quelle: Sicherheitsgruppen Netzwerk](#nsg).

Um Cloud-Endpunkt und ACL als Ursache des Fehlers zu vermeiden, stellen Sie sicher, ein anderes Azure VM im gleichen virtuellen Netzwerk VM SSH-Verbindungen zu können.

![Diagramm, das Cloud-Endpunkt und ACL highlights](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Wenn Sie einen anderen virtuellen Computer im gleichen virtuellen Netzwerk haben, können Sie einfach eine neue erstellen. Weitere Informationen finden Sie unter [Linux VM unter Verwendung der CLI Azure erstellen](virtual-machines-linux-quick-create-cli.md). Löschen Sie zusätzlichen virtuellen Computer mit den Tests erfolgen.

Wenn Sie eine SSH-Verbindung mit einem virtuellen Computer im gleichen virtuellen Netzwerk erstellen können, überprüfen Sie Folgendes:

- **Die Endpunktkonfiguration für SSH-Verkehr auf dem Ziel-VM.** Private TCP-Port des Endpunkts übereinstimmen den TCP-Port der SSH-Dienst auf dem virtuellen Computer überwacht. (Der Standardanschluss ist 22). VMs mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt, überprüfen Sie die SSH TCP-Portnummer im Azure-Portal **Durchsuchen**auswählen > **virtuellen Maschinen (v2)** > *VM-Name* > **Settings** > **Endpunkte**.

- **Die ACL für den Endpunkt SSH Datenverkehr auf dem virtuellen Zielcomputer.** Eine ACL können Sie angeben, gewährten oder verweigerten Datenverkehr aus dem Internet, basierend auf der IP-Quelladresse. Falsch konfigurierte ACLs kann SSH-Datenverkehr an den Endpunkt. Überprüfen Sie Ihre ACLs, um sicherzustellen, dass eingehenden Verkehr von öffentlichen IP-Adressen des Proxies oder anderen Edgeserver zulässig. Weitere Informationen finden Sie unter [über Network Access Control lists (ACLs)](../virtual-network/virtual-networks-acl.md).

Um den Endpunkt als Ursache des Problems beseitigen, entfernen Sie den aktuellen Endpunkt einen neuen Endpunkt zu erstellen und den Namen SSH (TCP-Port 22 für öffentliche und private Portnummer). Weitere Informationen finden Sie in der [Endpunkte auf einem virtuellen Computer in Azure eingerichtet](virtual-machines-windows-classic-setup-endpoints.md).

<a id="nsg"></a>
## <a name="source-4-network-security-groups"></a>4 Quelle: Netzwerk-Sicherheitsgruppen

Netzwerk-Sicherheitsgruppen können Sie genauere Kontrolle der zulässigen eingehenden und ausgehenden Datenverkehr. Sie können Regeln erstellen, die Subnetze und cloud-Dienste in Azure virtual Network. Überprüfen Sie Ihre Gruppe Netzwerksicherheitsregeln um sicherzustellen, dass SSH-Datenverkehr zum und vom Internet zulässig ist.
Weitere Informationen finden Sie unter [Netzwerk-Sicherheitsgruppen](../virtual-network/virtual-networks-nsg.md).

## <a name="source-5-linux-based-azure-virtual-machine"></a>: 5 Linux-basierten Azure virtuellen Quellcomputers

Letzte Quelle über mögliche Probleme ist der Azure virtuelle Maschine.

![Diagramm, das Linux-basierten Azure Virtual Machine highlights](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Wenn Sie dies noch nicht getan haben, führen Sie die Anweisungen [ein Kennwort oder SSH für Linux-basierte virtuelle Computer zurücksetzen](virtual-machines-linux-classic-reset-access.md).

Wiederholen Sie den Verbindungsversuch von Ihrem Computer. Andernfalls noch zählen folgende mögliche Probleme:

- SSH-Dienst wird nicht auf der virtuelle Zielcomputer ausgeführt.
- SSH-Dienst hört TCP-Port 22 nicht. Test einen Telnet-Client auf dem lokalen Computer installieren und Ausführen "Telnet *CloudServiceName*. cloudapp.net 22". Bestimmt, ob der Virtual Machine ermöglicht die ein- und ausgehende Kommunikation SSH-Endpunkt.
- Lokale Firewall auf dem virtuellen Zielcomputer enthält Regeln, die eingehenden oder ausgehenden SSH-Datenverkehr verhindern.
- Erkennung von Eindringversuchen oder Netzwerküberwachungssoftware auf Azure Virtual Machine läuft verhindert SSH-Verbindungen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen
Weitere Informationen zur Problembehandlung bei Zugriff finden Sie unter [Problembehandlung bei Zugriff auf eine Anwendung auf einer Azure virtuellen Computer](virtual-machines-linux-troubleshoot-app-connection.md)