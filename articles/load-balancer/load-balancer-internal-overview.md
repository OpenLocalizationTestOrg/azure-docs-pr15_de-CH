
<properties
   pageTitle="Interne Lastenausgleich Übersicht | Microsoft Azure"
   description="Übersicht für interne Lastenausgleich und seine Funktionen. Wie funktioniert ein Lastenausgleich für Azure und mögliche interne Endpunkte konfigurieren"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="internal-load-balancer-overview"></a>Interne Load Balancer (Übersicht)

Im Gegensatz zu dann Internet mit Lastenausgleich leitet internen Lastenausgleich (ILB) Datenverkehr nur auf Ressourcen in der Cloud-Dienst oder über VPN auf der Azure-Infrastruktur. Die Infrastruktur schränkt den Zugriff auf die Lastenausgleich virtuellen IP-Adressen (VIPs) einen Cloud-Dienst oder ein virtuelles Netzwerk, damit sie nie direkt auf einen Internet-Endpunkt verfügbar gemacht werden. Interne Linie branchenspezifische Anwendung in Azure und die Cloud oder von lokalen Ressourcen aus zugegriffen werden können.

## <a name="why-you-may-need-an-internal-load-balancer"></a>Warum kann ein interner Lastenausgleich erforderlich

Azure interne laden Netzwerklastenausgleich (ILB) bietet einen Lastenausgleich zwischen virtuellen Maschinen, die in einer Cloud-Dienst oder ein virtuelles Netzwerk mit einem regionalen Bereich befinden. Finden Sie Informationen zur Verwendung und Konfiguration von virtuellen Netzwerken mit einem regionalen [Regionale virtuelle Netzwerke](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) in Azure-Blog. ILB können keine vorhandene virtuelle Netzwerke, die für eine Gruppe konfiguriert wurden.

ILB kann folgende Lastenausgleich:

- In einem Clouddienst von virtuellen Maschinen auf virtuellen Maschinen, die sich im gleichen Clouddienst befinden (siehe Abbildung 1).
- In einem virtuellen Netzwerk von virtuellen Maschinen in das virtuelle Netzwerk auf eine Gruppe von virtuellen Computern, die sich im gleichen Clouddienst virtuellen Netzwerk (siehe Abbildung 2).
- Für eine standortübergreifende virtuelle Netzwerk aus lokalen Computer eine Gruppe von virtuellen Computern, die sich im gleichen Clouddienst virtuellen Netzwerk (siehe Abbildung 3).
- Internetzugriff, Multi-Tier-Applikationen in die Back-End-Ebenen nicht Internetzugriff aber Lastenausgleich für Datenverkehr aus der Internetschnittstelle Ebene erforderlich.
- Lastenausgleich für LOB Anwendung in Azure ohne zusätzliche Load Balancer Hardware oder Software. Einschließlich lokalen Server in der Gruppe von Computern, deren Datenverkehr ist, ausgeglichen.

## <a name="internet-facing-multi-tier-applications"></a>Multi-Tier-Anwendung mit Internetzugriff


Die Webebene wurde vor Internet-Endpunkte für Internetclients und Teil ein Lastenausgleich. Lastenausgleich verteilt eingehenden Verkehr von Webclients für TCP-Port 443 (HTTPS) zum Webserver.

Die Datenbankserver sind hinter anliegenden ILB die Webserver für Speicher verwenden. Diese Datenbank Service Lastenausgleich Endpunkt, welcher Datenverkehr Lastenausgleich auf Datenbankservern in den ILB.

Die folgende Abbildung zeigt die Anwendung mit mehreren Ebenen innerhalb derselben Cloud-Dienst mit Internetzugriff.

Abbildung 1: Anwendung mit mehreren Ebenen mit Internetzugriff

![Interne Lastenausgleich einzelne Cloud-Dienst](./media/load-balancer-internal-overview/IC736321.png)

Eine weitere Anwendungsmöglichkeit für eine Anwendung mit mehreren Ebenen wird die ILB auf eine andere Cloud-Dienst als die Nutzung des Diensts für den ILB bereitgestellt.

Cloud-Diensten mit demselben virtuellen Netzwerk haben Zugriff auf den ILB-Endpunkt.

Abbildung 2 zeigt die Web-Front-End-Server in einem anderen Clouddienst Datenbank-Backend und ILB Endpunkt im gleichen virtuellen Netzwerk befinden.

![Interne Lastenausgleich zwischen Cloud-Dienste](./media/load-balancer-internal-overview/IC744147.png)

Abbildung 2: Front-End-Server in eine andere Cloud-Dienst

## <a name="intranet-line-of-business-applications"></a>Intranet branchenanwendungen

Datenverkehr von Clients im lokalen Netzwerk vermitteln Lastenausgleich von VPN-Verbindung mit Azure Netzwerk LOB-Servern.

Der Clientcomputer wird eine IP-Adresse von Azure VPN-Dienst mit Website VPN zugreifen. Es ermöglicht die Verwendung die LOB-Anwendung hinter den ILB Endpunkt gehostet.

![Interne Lastenausgleich Standort VPN mit Punkt](./media/load-balancer-internal-overview/IC744148.png)

Abbildung 3: LOB-Anwendung hinter den Endpunkt Pfd

Ein anderes Szenario für das LOB soll eine Standort-zu-Standort-VPN an das virtuelle Netzwerk, in dem der ILB Endpunkt konfiguriert ist. Dadurch werden Netzwerkverkehr lokalen Endpunkt ILB weitergeleitet werden.

![Interne Lastenausgleich mithilfe von Standort zu Standort VPN](./media/load-balancer-internal-overview/IC744150.png)

Abbildung 4: lokale Netzwerkverkehr ILB Endpunkt weitergeleitet


## <a name="next-steps"></a>Nächste Schritte

[Azure Resource Manager Unterstützung für Lastenausgleich Azure](load-balancer-arm.md)

[Erste Schritte mit Lastenausgleich ein Internetzugriff konfigurieren](load-balancer-get-started-internet-arm-ps.md)

[Beginnen Sie einen internen Lastenausgleich konfigurieren](load-balancer-get-started-ilb-arm-ps.md)

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)

