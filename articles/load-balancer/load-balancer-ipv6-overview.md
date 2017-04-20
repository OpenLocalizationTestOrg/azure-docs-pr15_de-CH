<properties
    pageTitle="Übersicht über IPv6 für Azure Lastenausgleich | Microsoft Azure"
    description="Grundlegendes zu IPv6-Unterstützung für Lastenausgleich Azure und VMs mit Lastenausgleich."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6 Azure Lastenausgleich dual-Stack, öffentliche IP-Adresse, nativen IPv6-, Mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Übersicht über IPv6 für Azure Lastenausgleich

Internetzugriff Lastenausgleich können mit einer IPv6-Adresse bereitgestellt werden. Zusätzlich zu IPv4-Konnektivität ermöglicht dies die folgenden Funktionen:

* Systemeigene End-to-End-IPv6-Konnektivität zwischen öffentlichen Internetclients und Azure virtuelle Maschinen (VMs) über den Lastenausgleich.
* Systemeigene End-to-End-IPv6-ausgehende Verbindung zwischen VMs und öffentlichen Internet IPv6-fähige Clients.

Das folgende Bild zeigt die IPv6-Funktionalität für Azure Lastenausgleich.

![Azure Lastenausgleich mit IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Nach der Bereitstellung kann ein IPv4- oder IPv6-konformen Internet Client mit öffentlichen IPv4 oder IPv6-Adressen (oder Hostnamen) des Azure Internetzugriff Lastenausgleich kommunizieren. Lastenausgleich routet IPv6-Pakete zu privaten IPv6-Adressen der VMs Netzwerkadressübersetzung (NAT). Der IPv6-Internet-Client kann nicht direkt mit der IPv6-Adresse der virtuellen Computer kommunizieren.

## <a name="features"></a>Funktionen

Systemeigene unterstützt IPv6-VMs bereitgestellt über Azure-Ressourcen-Manager:

1. Lastenausgleich IPv6-Services für IPv6-Clients im Internet
2. Systemeigene IPv6 und IPv4-Endpunkte auf VMs ("zwei gestapelt")
3. Eingehende und ausgehende initiierte systemeigenen IPv6-Verbindung
4. Unterstützte Protokolle wie TCP, UDP und HTTP(s) aktivieren eine Vielzahl von Service-Architekturen

## <a name="benefits"></a>Vorteile

Mit dieser Funktion können die folgenden Hauptvorteile:

* Vorschriften Sie behördlichen, dass neue Anträge auf reine IPv6-Clients zugänglich
* Mobile aktivieren und das Internet der Dinge (IOT) Entwickler zwei gestapelt (IPv4 + IPv6) Azure Virtual Machines auf wachsenden Mobile & IOT Märkte

## <a name="details-and-limitations"></a>Details und Grenzen

Details

* Azure DNS-Dienst enthält eine IPv4- und IPv6-AAAA Namenseinträge und antwortet mit beiden Datensätze für den Lastenausgleich. Der Client wählt die Adresse (IPv4 oder IPv6) zu kommunizieren.
* Wenn ein virtueller Computer eine Verbindung mit einem öffentlichen Internet IPv6-Gerät herstellt, wird die VM IPv6-Quelladresse Netzwerkadresse übersetzt (NAT) die öffentlichen IPv6-Adresse Lastenausgleich.
* VMs des Linux-Betriebssystems müssen konfiguriert werden, um IPv6-Adresse über DHCP erhalten. Viele Linux Bilder in der Galerie Azure sind bereits konfiguriert zur Unterstützung von IPv6 unverändert. Weitere Informationen finden Sie unter [Konfigurieren von DHCPv6 für Linux VMs](load-balancer-ipv6-for-linux.md)
* Wählt einen Integritätstest mit der Lastenausgleich verwenden IPv4-Prüfpunkt erstellen und mit der IPv4- und IPv6-Endpunkte verwenden. Wenn der VM-Dienst ausfällt, werden die IPv4- und IPv6-Endpunkte aus der Rotation.

Grenzen

* Sie können keine IPv6-Load Balance Regeln Azure-Portal hinzufügen. Die Regeln können nur über die CLI, PowerShell Vorlage erstellt.
* Vorhandene virtuelle Computer, um IPv6-Adressen können nicht aktualisiert werden. Sie müssen neue VMs bereitstellen.
* Ein Netzwerkschnittstelle in jeder VM kann eine IPv6-Adresse zugewiesen.
* Die öffentlichen IPv6-Adressen können ein virtueller Computer zugewiesen werden. Sie können nur ein Lastenausgleich zugewiesen werden.
* Sie können nicht die umgekehrte DNS-Suche für die öffentlichen IPv6-Adressen konfigurieren.
* Die VMs mit IPv6-Adressen können nicht Azure Cloud Service angehören. Sie können ein virtuelles Azure-Netzwerk (VNet) an und kommunizieren miteinander über den IPv4-Adressen.
* Private IPv6-Adressen auf einzelnen VMs in einer Ressourcengruppe bereitgestellt werden, aber können nicht in einer Ressourcengruppe über Maßstab legt bereitgestellt werden.
* Azure VMs können nicht über IPv6 auf andere VMs, Azure Services oder lokalen Geräten herstellen. Sie können nur mit Azure Lastenausgleich über IPv6 kommunizieren. Allerdings können sie diese anderen Ressourcen über IPv4 kommunizieren.
* Schutz für IPv4-Netzwerk-Sicherheitsgruppe (NSG) wird in Dual-Stack (IPv4 + IPv6) Bereitstellung unterstützt. NSGs gelten nicht für IPv6-Endpunkte.
* IPv6-Endpunkt auf dem virtuellen Computer ist nicht direkt mit dem Internet verbunden. Es ist hinter einem Lastenausgleich. Load Balancer Regeln angegebenen Ports stehen über IPv6.
* Ändern des Parameters IdleTimeout für IPv6 ist **derzeit nicht unterstützt**. Der Standardwert ist vier Minuten.

## <a name="next-steps"></a>Nächste Schritte

Informationen Sie zum Lastenausgleich mit IPv6 bereitstellen.

* [Verfügbarkeit von IPv6 nach region](https://go.microsoft.com/fwlink/?linkid=828357)
* [Bereitstellen Sie ein Lastenausgleich mit IPv6-Vorlage](load-balancer-ipv6-internet-template.md)
* [Bereitstellen Sie ein Lastenausgleich mit IPv6 Azure PowerShell](load-balancer-ipv6-internet-ps.md)
* [Bereitstellen Sie ein Lastenausgleich mit IPv6 Azure CLI](load-balancer-ipv6-internet-cli.md)
