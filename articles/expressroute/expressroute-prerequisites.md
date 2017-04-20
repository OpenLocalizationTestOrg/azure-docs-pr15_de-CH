<properties
   pageTitle="Erforderliche Komponenten für die Annahme von ExpressRoute | Microsoft Azure"
   description="Diese Seite enthält eine Liste Mindestanforderungen erfüllt sein, damit einen Schaltkreis Azure ExpressRoute bestellen können."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute Komponenten & Checkliste  

Verbindung mit Microsoft-Cloud-Diensten mit ExpressRoute müssen Sie überprüfen, ob in den folgenden Abschnitten aufgeführten Folgendes erfüllt sind.

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Ein Azure-Konto

- Ein gültiges und aktives Microsoft Azure-Konto. Dies ist erforderlich, die ExpressRoute-Verbindung einrichten. ExpressRoute-Schaltkreise sind Ressourcen in Azure-Abonnements. Ein Azure-Abonnement ist eine Anforderung auch Konnektivität Clouddienste Azure Microsoft Office 365 und CRM online auf.
- Ein aktives Office 365 Abonnement (bei Office 365-Diensten). Siehe Abschnitt [Office 365 bestimmte Vorschriften](#office-365-specific-requirements) dieses Artikels Weitere Informationen.

## <a name="connectivity-provider"></a>Konnektivitätsanbieter
- Sie können mit einem [ExpressRoute Konnektivität Partner](expressroute-locations.md#partners) für die Verbindung zur Microsoft-Cloud arbeiten. Sie können eine Verbindung zwischen dem lokalen Netzwerk und Microsoft auf [drei Arten](expressroute-introduction.md#howtoconnect)einrichten. 
- Ist Ihr Anbieter keine ExpressRoute Verbindung Partner können Sie weiterhin die Cloud von Microsoft [Exchange Cloudanbieter](expressroute-locations.md#nonpartners)verbinden.

## <a name="network-requirements"></a>Netzwerk
- **Redundante Verbindung**: Es ist keine Redundanz erforderlich physische Konnektivität zwischen Ihnen und Ihren Anbieter. Microsoft verlangt redundante BGP-Sitzung eingerichtet werden zwischen Microsoft Router und den Routern peering auch wenn Sie nur [eine einzige physische Verbindung zu einem Cloud](expressroute-faqs.md#onep2plink). 
- **Routing**: je nachdem, wie die Microsoft-Cloud herstellen, müssen Sie oder Dienstanbieter einrichten und Verwalten von BGP-Sessions für [routing-Domänen](expressroute-circuit-peerings.md). Einige Anbieter von Ethernet-Konnektivität oder Exchange Cloudanbieter möglicherweise BGP Management bieten einen Mehrwert-Service.
- **NAT**: Microsoft akzeptiert nur öffentliche IP-Adressen durch Microsoft peering. Bei Verwendung von privaten IP-Adressen im lokalen Netzwerk Sie oder Adressen übersetzen privaten IP-Adressen der öffentlichen IP-Anbieter müssen [NAT verwenden](expressroute-nat.md).
- **QoS**: Skype für Unternehmen verschiedene Dienste (z. B. Stimme, video, Text) hat, die erfordern differenzierte QoS-Behandlung. Sie und Ihre Anbieter sollten [QoS-Anforderungen](expressroute-qos.md)folgen.
- **Sicherheit**: [Sicherheit](../best-practices-network-security.md) sollten beim Verbinden mit der Microsoft Cloud über ExpressRoute.
 
## <a name="office-365"></a>Office 365

Wenn Sie Office 365 auf ExpressRoute aktivieren möchten, überprüfen Sie die folgenden Dokumente Weitere Informationen zu Office 365.


- [Übersicht über ExpressRoute für Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Routing mit ExpressRoute für Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Office 365-URLs und IP-Adressbereiche](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Netzwerk und Performance-Optimierung für Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Netzwerk-Bandbreite Rechner und tools](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Office 365-Integration mit lokalen](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## <a name="crm-online"></a>CRM Online 
Wenn Sie CRM Online auf ExpressRoute aktivieren möchten, lesen Sie die folgenden Dokumente Weitere Informationen zu CRM Online

- [CRM Online-URLs](https://support.microsoft.com/kb/2655102) und [IP-Adressbereiche](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu ExpressRoute finden Sie im [ExpressRoute häufig gestellte Fragen](expressroute-faqs.md).
- Suchen Sie einen ExpressRoute Verbindung Anbieter. [ExpressRoute-Partner und peering Positionen](expressroute-locations.md)anzeigen
- Siehe Vorschriften für [Routing](expressroute-routing.md), [NAT](expressroute-nat.md) und [QoS](expressroute-qos.md).
- Konfigurieren Sie die ExpressRoute-Verbindung.
    - [Erstellen Sie eine ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md)
    - [Konfigurieren von routing](expressroute-howto-routing-classic.md)
    - [Verknüpfen Sie ein VNet mit ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md)

