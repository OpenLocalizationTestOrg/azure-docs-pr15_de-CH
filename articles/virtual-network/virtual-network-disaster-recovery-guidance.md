<properties
    pageTitle="Was bei einer Azure langwierige beeinträchtigen Azure Virtual Networks | Microsoft Azure"
    description="Vorgehensweise bei einer Azure langwierige Azure virtuelle Netzwerke beeinträchtigen."
    services="virtual-network"
    documentationCenter=""
    authors="NarayanAnnamalai"
    manager="jefco"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="virtual-network"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="narayan;aglick"/>

#<a name="virtual-network--business-continuity"></a>Virtuelles Netzwerk – Business Continuity

##<a name="overview"></a>Übersicht

Ein virtuelles Netzwerk (VNet) ist eine logische Darstellung des Netzwerks in der Cloud. Sie können Ihre eigenen privaten IP-Adressraum und Segmentieren des Netzwerks in Subnets. VNets ist eine Vertrauensgrenze die Serverressourcen Azure Virtual Machines wie Cloud-Dienste (Web-Worker-Rollen) hosten. Eine VNet ermöglicht direkte private IP-Kommunikation zwischen den Ressourcen darin. Ein virtuelles Netzwerk kann auch mit einem lokalen Netzwerk über einen verfügbaren Hybrid oder ein VPN-Gateway ExpressRoute verknüpft werden.
 
Im Rahmen eines Bereichs wird ein VNet erstellt. VNets mit demselben Adressraum in zwei unterschiedlichen Regionen erstellen (d. h. uns Ost und West uns sie einander direkt Verbindung herstellen). 

##<a name="business-continuity"></a>Business Continuity

Es kann verschiedene Arten die Anwendung unterbrochen werden kann. Eine bestimmte Region konnte aufgrund einer Naturkatastrophe oder teilweise Notfall Ausfall mehrerer Geräte-Dienste vollständig abgeschnitten. Die Auswirkung auf den VNet-Dienst unterscheidet sich in allen Situationen.

**Was tun Sie bei einem Ausfall einer ganzen Region? d. h., wenn ein Bereich vollständig cutoff aufgrund einer Naturkatastrophe ist? Was geschieht mit virtuellen Netzwerke im Bereich gehostet?**

A: das virtuelle Netzwerk und Ressourcen in der betroffenen Region bleibt während der Dauer der Unterbrechung des Dienstes nicht zugegriffen.

![Einfache virtuelles Netzwerkdiagramm](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**Was kann ich im gleichen virtuellen Netzwerk in einem anderen Bereich neu erstellen?**

A: Virtual Network (VNet) ist ziemlich einfachen Ressource. Sie können Azure-APIs, um eine VNet mit dem gleichen Adressraum in einer anderen Region erstellen aufrufen. Erstellen einer Umgebung wieder, die in der betroffenen Region müssen Sie API-Aufrufe für Ihre Cloud-Dienste (Web-Worker-Rollen) und virtuelle Computer, die Sie erneut bereitstellen. Außerdem müssen Sie ein VPN-Gateway und Verbinden mit Ihrem lokalen Netzwerk wäre der lokalen Verbindung (wie in einer hybridbereitstellung).

Die Schritte zum Erstellen einer VNet finden Sie [hier](./virtual-networks-create-vnet-arm-pportal.md). 

**F: sein kann ein Replikat des VNet in einem bestimmten Bereich neu erstellt in anderen voraus?**

A: Ja, können Sie zwei VNets mit demselben privaten IP-Adressraum und Ressourcen in zwei verschiedenen Bereichen voraus erstellen. Wenn Kunden Dienste in der VNet mit Internetzugriff gehostet wurde, konnte sie von Traffic Manager Geo-Datenverkehr der Region festgelegt haben, die aktiv ist. Jedoch kann kein Kunde zwei VNets mit dem gleichen Adressraum mit ihrem lokalen Netzwerk verbinden, Routingproblemen führen würde. Von einem Ausfall und Verlust von VNet in einer Region können ein Kunde das VNet in den verfügbaren Bereich mit übereinstimmenden Adressraum mit ihrem lokalen Netzwerk.

Die Schritte zum Erstellen einer VNet finden Sie [hier](./virtual-networks-create-vnet-arm-pportal.md).
