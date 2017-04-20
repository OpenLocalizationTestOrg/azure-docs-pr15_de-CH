<properties
    pageTitle="Azure Active Directory-Domänendienste: Netzwerk Richtlinien | Microsoft Azure"
    description="Netzwerk-Aspekte für Azure Active Directory-Domänendienste"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="maheshu"/>

# <a name="networking-considerations-for-azure-ad-domain-services"></a>Netzwerk-Aspekte für Azure Active Directory-Domänendienste

## <a name="how-to-select-an-azure-virtual-network"></a>Ein Azure virtuelles Netzwerk auswählen
Die folgenden Richtlinien können Sie ein virtuelles Netzwerk mit Azure Active Directory-Domänendienste wählen.

### <a name="type-of-azure-virtual-network"></a>Azure virtuelle Netzwerktyp

- Sie können Azure AD-Domänendienste in einem klassischen Azure virtuelle Netzwerk.

- Azure Active Directory-Domänendienste **in virtuelle Netzwerke mit Azure Ressourcenmanager erstellt nicht aktiviert werden**.

- Sie können ein Ressourcen-Manager-virtuelles Netzwerk zu einem klassischen virtuellen Netzwerk verbinden in denen Azure AD-Domänendienste aktiviert ist. Danach können Sie im Ressourcen-Manager-basierten virtuellen Netzwerk Azure Active Directory-Domänendienste. Weitere Informationen finden Sie im Abschnitt [Netzwerkkonnektivität](active-directory-ds-networking.md#network-connectivity) .

- **Regionale virtuelle Netzwerke**: Wenn Sie ein vorhandenes virtuelles Netzwerk verwenden möchten, dass regionale virtuelles Netzwerk ist.

    - Virtuelle Netzwerke, mit denen Gruppen die legacy-Mechanismus können mit Azure AD-Domäne verwendet werden.

    - Azure AD-Domänendienste, [Migration ältere virtuelle Netzwerke auf regionale virtuelle Netzwerke](../virtual-network/virtual-networks-migrate-to-regional-vnet.md)verwenden.


### <a name="azure-region-for-the-virtual-network"></a>Azure-Region für das virtuelle Netzwerk

- Ihre Azure AD-Domänendienste verwalteten Domäne in derselben Azure-Region als virtuelles Netzwerk bereitgestellt wählen Sie den Dienst aktivieren.

- Wählen Sie ein virtuelles Netzwerk Azure Region von Azure Active Directory Domain Services unterstützt.

- Siehe [Azure Services nach Region](https://azure.microsoft.com/regions/#services/) kennen Azure Regionen, die Azure AD-Domänendienste verfügbar ist.


### <a name="requirements-for-the-virtual-network"></a>Vorschriften für das virtuelle Netzwerk

- **Nähe zu Azure Arbeitslasten**: Wählen Sie das virtuelle Netzwerk, die derzeit gehostet/virtuelle Computer, die Zugriff auf Active Directory-Domänendienste Azure gehostet wird.

- **Benutzerdefinierte/bringen eigene DNS-Server**: sicherstellen, dass keine benutzerdefinierte DNS-für das virtuelle Netzwerk konfiguriert Server.

- **Vorhandene Domänen mit demselben Domänennamen**: Stellen Sie sicher, dass Sie keine vorhandene Domäne mit dem gleichen Domänennamen in diesem virtuellen Netzwerk verfügbar. Angenommen Sie beispielsweise, einer Domäne "contoso.com" bereits für das ausgewählte virtuelle Netzwerk aufgerufen. Später versuchen Sie, eine verwaltete Azure Active Directory-Domänendienste-Domäne mit dem gleichen Domänennamen (das "contoso.com") in diesem virtuellen Netzwerk aktivieren. Fehler auftreten beim Azure AD-Domäne aktivieren. Dieser Fehler wird aufgrund von Namenskonflikten für den Domänennamen in diesem virtuellen Netzwerk. In diesem Fall verwenden Sie einen anderen Namen Ihrer verwalteten Azure Active Directory-Domänendienste-Domäne einrichten. Sie können auch Bereitstellung aufheben die vorhandene Domäne und dann Azure AD-Domäne aktivieren.

> [AZURE.WARNING] Sie können nicht Domänendienste nach Aktivierung des Dienstes zu einem anderen virtuellen Netzwerk verschieben.


## <a name="network-security-groups-and-subnet-design"></a>Netzwerksicherheitsgruppen und Subnetz
[Network Security Group (NSG)](../virtual-network/virtual-networks-nsg.md) enthält eine Liste der Zugriffssteuerungsliste (ACL) Regeln, die zulassen oder verweigern auf VM-Instanzen in einem virtuellen Netzwerk. NSGs können Subnets oder einzelne VM-Instanzen innerhalb eines Subnetzes zugeordnet werden. Wenn eine NSG einem Subnetz zugeordnet ist, Regeln die ACL auf alle Instanzen virtueller Computer in diesem Subnetz. Außerdem kann Datenverkehr zu einer einzelnen VM beschränkt werden durch eine NSG direkt an diesen virtuellen Computer zuordnen.

![Empfohlene Subnetz](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)


### <a name="best-practices-for-choosing-a-subnet"></a>Best Practices für ein Subnetz auswählen
- Bereitstellen Sie Azure Active Directory-Domänendienste in einem **separaten dedizierten Subnetz** in Azure virtuelle Netzwerk.

- Dedizierte Subnetzmaske der verwalteten Domäne nicht NSGs zuweisen. Wenn Sie die dedizierte Subnetzmaske NSGs anwenden müssen, stellen Sie sicher, **und Verwalten Ihrer Domäne erforderlichen Ports nicht blockiert**.

- Beschränken Sie die Anzahl der dedizierten Subnetz der verwalteten Domäne verfügbaren IP-Adressen nicht zu. Diese Einschränkung verhindert, dass den Dienst zwei Domänencontroller für die verwalteten Domäne.

- Des virtuellen Netzwerks **Azure AD-Domänendienste im gatewaysubnetz nicht aktivieren** .


> [AZURE.WARNING] Beim Zuordnen eine NSG mit einem Subnetz in der Azure Active Directory-Domänendienste aktiviert ist, Microsoft Fähigkeit und Verwalten der Domäne beeinträchtigen. Darüber hinaus ist die Synchronisierung zwischen Azure AD-Mandanten und der verwalteten Domäne unterbrochen. **Die SLA gilt nicht für Installationen, in denen eine NSG blockiert Azure Active Directory-Domänendienste angewendet wurde aus aktualisieren und Verwalten Ihrer Domäne.**


### <a name="ports-required-for-azure-ad-domain-services"></a>Anschlüsse für Azure AD-Domäne erforderlich
Die folgenden Ports sind erforderlich für Azure AD-Domänendienste Service und der verwalteten Domäne verwalten. Stellen Sie sicher, dass diese Ports in der verwalteten Domäne konnten nicht für das Subnetz blockiert werden.

| Port-Nummer | Zweck |
|---|---|
| 443 | Synchronisierung mit Ihrem Azure AD-Mandanten |
| 3389 | Management Ihrer Domäne |
| 5986 | Management Ihrer Domäne |
| 636 | Sicherer Zugriff auf Ihre verwalteten Domäne LDAP (LDAPS) |



## <a name="network-connectivity"></a>Netzwerkkonnektivität
Eine verwaltete Azure Active Directory-Domänendienste-Domäne kann nur in einem klassischen virtuelles Netzwerk in Azure aktiviert werden. Virtuelle Netzwerke mit Azure-Ressourcen-Manager erstellt werden nicht unterstützt.


### <a name="scenarios-for-connecting-azure-networks"></a>Szenarien für Azure Netzwerke verbinden
Verbinden Sie Verwendung die verwaltete Domäne in einem der folgenden Bereitstellungsszenarien Azure virtuelle Netzwerke:

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>Verwendung der verwalteten Domäne mehrere Azure klassische virtuelles Netzwerk
Andere Azure klassische virtuelle Netzwerke möglich Azure klassische virtuelles Netzwerk in denen Azure AD-Domänendienste aktiviert haben. Diese VPN-Verbindung können Sie Ihre Arbeitslasten in anderen virtuellen Netzwerken bereitgestellt verwalteten Domäne mit.

![Klassische virtuelle Netzwerkkonnektivität](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Verwenden der verwalteten Domäne in einem Ressourcen-Manager-virtuelle Netzwerk
Sie können ein Ressourcen-Manager-virtuelles Netzwerk Azure klassische virtuelles Netzwerk Verbindung in denen Azure Active Directory-Domänendienste konnten. Diese Verbindung ermöglicht der verwalteten Domäne mit der Arbeitslasten in der Ressourcen-Manager-virtuellen Netzwerk bereitgestellt.

![Ressourcen-Manager klassische virtuelle Netzwerkkonnektivität](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)


### <a name="network-connection-options"></a>Netzwerk-Verbindungsoptionen

- **Standort-zu-Standort-VPN-Verbindungen VNet VNet Verbindung**: ein virtuelles Netzwerk mit einem anderen virtuellen Netzwerk (VNet VNet) ist ein virtuelles Netzwerk mit einem lokalen Standort ähnlich. Beide Konnektivität verwenden eine VPN-Gateway für einen sicheren Tunnel mit IPsec-IKE.

    ![Virtuelle Netzwerkkonnektivität mit VPN-Gateway](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Weitere Informationen – virtuelle Netzwerke mit VPN-Gateway herstellen](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)


- **VNet VNet Verbindung mit virtuellen Netzwerk peering**: virtuelles Netzwerk peering ist ein Mechanismus, der zwei virtuelle Netzwerke in derselben Region Azure Backbone-Netzwerk verbunden. Sobald dies, werden zwei virtuelle Netzwerke als Gründen Konnektivität angezeigt. Sie werden weiterhin als separate Ressourcen verwaltet, aber virtuellen Computer in dieser virtuellen Netzwerke direkt mithilfe von privaten IP-Adressen miteinander kommunizieren können.

    ![Virtuelle Netzwerkkonnektivität mit peering](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Weitere Informationen – virtuelle Netzwerk peering](../virtual-network/virtual-network-peering-overview.md)



<br>

## <a name="related-content"></a>Verwandte Inhalte

- [Azure virtual Network peering](../virtual-network/virtual-network-peering-overview.md)

- [Konfigurieren einer Verbindung VNet VNet für das klassische Bereitstellungsmodell](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

- [Azure-Netzwerk von Sicherheitsgruppen](../virtual-network/virtual-networks-nsg.md)
