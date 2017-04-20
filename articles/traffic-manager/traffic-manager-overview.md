<properties
    pageTitle="Was ist Traffic Manager | Microsoft Azure"
    description="Dieser Artikel hilft Ihnen zu verstehen, was Traffic Manager ist und kann recht Datenverkehr routing Wahl für die Anwendung"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-traffic-manager"></a>Übersicht der Traffic Manager

Microsoft Azure Traffic Manager ermöglicht die Verteilung von Benutzern für Endpunkte in verschiedenen Rechenzentren zu steuern. Unterstützt von Traffic Manager-Endpunkte enthalten Azure VMs Web Apps und cloud-Services. Traffic Manager verwenden Sie externe, nicht Azure Endpunkte.

Traffic Manager verwendet das Domain Name System (DNS) auf Clientanforderungen an den am besten geeigneten Endpunkt basierend auf einer [Streckenführung Methode](traffic-manager-routing-methods.md) und die Endpunkte weitergeleitet. Traffic Manager bietet eine Reihe von Methoden an andere Anforderungen Endpunkt [Überwachen](traffic-manager-monitoring.md)und automatisches Failover Datenverkehr weiterleiten. Traffic Manager ist anfällig für Fehler, einschließlich der gesamten Azure-Region.

## <a name="traffic-manager-benefits"></a>Traffic Manager Vorteile

Traffic Manager ermöglicht Ihnen:

- **Verbesserung der Verfügbarkeit von wichtigen Applikationen**

    Traffic Manager bietet hohen Verfügbarkeit für Ihre Überwachung Endpunkte und automatisches Failover bei ein Endpunkt.

- **Verbesserung der Reaktionszeiten für Applikationen mit hoher performance**

    Azure ermöglicht Cloud-Dienste oder Websites in Rechenzentren auf der ganzen Welt führen. Traffic Manager verbessert die Reaktionsfähigkeit der Anwendung durch den Verkehr an den Endpunkt mit der niedrigsten Netzwerklatenz für den Client.

- **Wartung ohne Ausfallzeiten durchführen**

    Sie können geplante Wartungsarbeiten Anwendung ohne Ausfallzeiten durchführen. Traffic Manager regelt den Verkehr mit anderen Endpunkten während die Wartung durchgeführt wird.

- **Kombinieren von lokalen und Cloud-basierte**

    Traffic Manager unterstützt externe, nicht Azure Endpunkte ermöglichen mit Hybriden cloud und lokalen Bereitstellung der "Burst-zu-Cloud" "Migration zur Cloud," und "Failover-Cloud" Szenarien.

- **Datenverkehr für große und komplexe Bereitstellung verteilen**

    [Geschachtelte Traffic Manager Profile](traffic-manager-nested-profiles.md)verwenden, können Streckenführung Methoden kombiniert werden, um anspruchsvolle und flexible Regeln, um die Bedürfnisse größerer, komplexerer Bereitstellung erstellen.

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md).

- Informationen Sie zum Anwendungsentwicklung Hochverfügbarkeits- [Traffic Manager Endpunkt](traffic-manager-monitoring.md)Überwachung.

- Erfahren Sie mehr über die [Methoden Datenverkehr weiterleiten](traffic-manager-routing-methods.md) von Traffic Manager unterstützt.

- [Traffic Manager-Profil erstellen](traffic-manager-manage-profiles.md).

