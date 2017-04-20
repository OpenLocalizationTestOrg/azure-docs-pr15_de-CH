<properties
   pageTitle="Erste Schritte beim Erstellen ein Lastenausgleich im klassischen Bereitstellungsmodell Verwendung der Azure-CLI mit Internetzugriff | Microsoft Azure"
   description="Erstellen Sie ein System zum Lastenausgleich im klassischen Bereitstellungsmodell Verwendung der Azure-CLI mit Internetzugriff"
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

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-cli"></a>Erste Schritte beim Erstellen einer Internetschnittstelle Lastenausgleich (klassisch) in der Azure-CLI

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das klassische Bereitstellungsmodell. Sie können auch [ein Lastenausgleich mithilfe von Azure Resource Manager mit Internetzugriff erstellen](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Schritt für Schritt erstellen ein Lastenausgleich über die Befehlszeilenschnittstelle mit Internetzugriff

Dieses Handbuch zeigt ein Internet-Lastenausgleich basierend auf dem Szenario erstellen.

1. Wenn Azure CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../../articles/xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie den Befehl **Azure Config-Modus** auf zur klassischen Ansicht wechseln, wie unten dargestellt.

        azure config mode asm

    Erwartete Ausgabe:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Erstellen Sie und Laden Sie Lastenausgleichsmodul

Das Szenario geht der virtuelle Computer "web1" und "web2" erstellt wurden.
Dieses Handbuch wird Load Balancer verwenden Port 80 als öffentliche Port und Port 80 als lokalen Port erstellen. Prüfpunkt-Port ist Port 80 konfiguriert und Load Balancer Satz "Lbset" benannt.


### <a name="step-1"></a>Schritt 1

Erstellen Sie den ersten Endpunkt und Lastenausgleich mit festgelegt `azure network vm endpoint create` für Virtual Machine "web1".

    azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset

Parameter:

**-k** - virtuellen lokalen port<br>
**-o** - Protokoll<BR>
**-t** - Prüfpunkt port<BR>
**-b** - Load-Balancer name<BR>

## <a name="step-2"></a>Schritt 2

Hinzufügen einer zweiten VM "web2" Load Balancer Satz.

    azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## <a name="step-3"></a>Schritt 3

Überprüfen, ob Load Balancer Konfiguration mit `azure vm show` .

    azure vm show web1

Die Ausgabe ist:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Erstellen Sie eine remote desktop für einen virtuellen Computer

Erstellen Sie einen remote desktop Endpunkt zum Weiterleiten von Netzwerkdatenverkehr von einem öffentlichen Port an einen lokalen Anschluss für einen bestimmten virtuellen Computer mit `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Lastenausgleich virtuellen Computer entfernen

Lastenausgleich vom virtuellen Computer zugeordneten Endpunkt löschen muss. Entfernt der Endpunkt, gehört nicht der virtuelle Computer Lastenausgleich mehr festgelegt.

 Im Beispiel oben entnehmen Sie den Endpunkt für den virtuellen Computer "web1" erstellt von "Lbset" mit dem Befehl zum Lastenausgleich `azure vm endpoint delete`.

    azure vm endpoint delete web1 tcp-80-80


>[AZURE.NOTE] Entdecken Sie mehr Optionen zum Verwalten der Endpunkte des Befehls`azure vm endpoint --help`


## <a name="next-steps"></a>Nächste Schritte

[Beginnen Sie einen internen Lastenausgleich konfigurieren](load-balancer-get-started-ilb-arm-ps.md)

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)

