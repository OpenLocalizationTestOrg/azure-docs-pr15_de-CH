<properties
   pageTitle="Erstellen einer internen Lastenausgleich mithilfe der Azure-CLI im klassischen Bereitstellungsmodell | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer internen Lastenausgleich mithilfe der Azure-CLI im klassischen Bereitstellungsmodell"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a>Erste Schritte beim Erstellen einer Azure-CLI mit internen Lastenausgleich (klassisch)

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [diese Schritte mit dem Ressourcen-Manager-Modell](load-balancer-get-started-ilb-arm-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a>Erstellen einer internen Lastenausgleich festlegen für virtuelle Maschinen

Erstellen einer internen System zum Lastenausgleich Lastsatz und Servern, die ihren Datenverkehr zu senden, müssen Sie Folgendes ein:

1. Erstellen Sie eine Instanz des internen Lastenausgleich, die den Endpunkt zu Lastenausgleich auf die Server einer Gruppe mit Lastenausgleich von eingehendem.

1. Hinzufügen von Endpunkten für den virtuellen Computer, die den eingehenden Datenverkehr empfangen.

1. Konfigurieren Sie die Server, die Datenverkehr Lastenausgleich um ihren Datenverkehr an die virtuelle IP-Adresse (VIP) Adresse der internen Lastenausgleich Instanz senden zu senden.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Schritt für Schritt Erstellen einer internen Lastenausgleich mit CLI

Dieses Handbuch zeigt eine interne Lastenausgleich basierend auf dem Szenario erstellen.

1. Wenn Azure CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../../articles/xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie den Befehl **Azure Config-Modus** auf zur klassischen Ansicht wechseln, wie unten dargestellt.

        azure config mode asm

    Erwartete Ausgabe:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Erstellen Sie und Laden Sie Lastenausgleichsmodul

Das Szenario geht die virtuellen Computer "DB1" und "DB2" in einem Clouddienst namens "Mytestcloud". Beide virtuelle Computer verwenden ein virtuelles Netzwerk mit Subnetz "Subnetz 1" Meine "Testvnet" bezeichnet.

Dieses Handbuch erstellt einen internen System zum Lastenausgleich Lastsatz mit Port 1433 als privater Port und 1433 als lokalen Port.

Dies ist häufig SQL virtuellen Maschinen im Back-End mit einer internen Lastenausgleich kommen sichergestellt, dass der Datenbankserver gelegt wird nicht direkt mit einer öffentlichen IP-Adresse.


### <a name="step-1"></a>Schritt 1

Erstellen einer internen Lastenausgleich mit festgelegt `azure network service internal-load-balancer add`.

     azure service internal-load-balancer add -r mytestcloud -n ilbset -t subnet-1 -a 192.168.2.7

Parameter:

**-r** - Cloud-Dienstname<BR>
**-n** - interne Load Balancer name<BR>
**-t** - Teilnetznamen (durch interne Lastenausgleich hinzugefügten virtuellen Computer im selben Subnetz)<BR>
**-a** - (optional) fügen Sie eine statische private IP-Adresse<BR>

Auschecken `azure service internal-load-balancer --help` Weitere Informationen.

Überprüfen Sie interne Load Balancer Eigenschaften mit dem Befehl `azure service internal-load-balancer list` *Dienstname Cloud*.

Hier folgt ein Beispiel der Ausgabe:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


## <a name="step-2"></a>Schritt 2

Interne Load Balancer Gruppe wird beim Hinzufügen des ersten Endpunkts konfigurieren. Endpunkt, virtuellen und Sonde Anschluss auf internen Load Balancer in diesem Schritt können zugeordnet werden.

    azure vm endpoint create db1 1433 -k 1433 tcp -t 1433 -r tcp -e 300 -f 600 -i ilbset

Parameter:

**-k** - virtuellen lokalen port<BR>
**-t** - Prüfpunkt port<BR>
**-r** - Prüfpunkt-Protokoll<BR>
**-e** - Prüfpunkt Intervall in Sekunden<BR>
**-f** - Timeout-Intervall in Sekunden <BR>
**-i** - interne Load Balancer name <BR>


## <a name="step-3"></a>Schritt 3

Überprüfen, ob Load Balancer Konfiguration mit `azure vm show` *Name des virtuellen Computers*

    azure vm show DB1

Die Ausgabe ist:

        azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK


## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Erstellen Sie eine remote desktop für einen virtuellen Computer

Erstellen Sie einen remote desktop Endpunkt zum Weiterleiten von Netzwerkdatenverkehr von einem öffentlichen Port an einen lokalen Anschluss für einen bestimmten virtuellen Computer mit `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Lastenausgleich virtuellen Computer entfernen

Sie können einen virtuellen Computer aus einer internen Lastenausgleich durch Löschen des zugeordneten Endpunkts festlegen entfernen. Endpunkt entfernt, wird nicht der virtuelle Computer legen mehr Lastenausgleich angehören.

 Im Beispiel oben entnehmen Sie den Endpunkt für Virtual Machine "DB1" von internen Lastenausgleich "Ilbset" erstellt, mit dem Befehl `azure vm endpoint delete`.

    azure vm endpoint delete DB1 tcp-1433-1433


Auschecken `azure vm endpoint --help` Weitere Informationen.


## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren Sie einen Quell-IP-Affinität mit Load Balancer Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)