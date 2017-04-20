<properties
   pageTitle="Netzwerk-Sicherheitsgruppen in Azure Security Center aktivieren | Microsoft Azure"
   description="Dieses Dokument veranschaulicht die Empfehlung Azure Security Center **Können Netzwerk-Sicherheitsgruppen**implementieren."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-network-security-groups-in-azure-security-center"></a>Sicherheitsgruppen in Azure Security Center Netzwerk aktivieren

Azure Security Center wird empfohlen, eine Netzwerk-Sicherheitsgruppe (NSG) aktivieren, wenn noch nicht aktiviert ist. NSGs enthalten eine Liste der Zugriffssteuerungsliste (ACL) Regeln, die zulassen oder verweigern auf VM-Instanzen in einem virtuellen Netzwerk. NSGs können Subnets oder einzelne VM-Instanzen innerhalb eines Subnetzes zugeordnet werden. Wenn eine NSG einem Subnetz zugeordnet ist, Regeln die ACL auf alle Instanzen virtueller Computer in diesem Subnetz. Außerdem kann Datenverkehr zu einer einzelnen VM beschränkt werden durch eine NSG direkt an diesen virtuellen Computer zuordnen. Um weitere finden Sie unter [Neuigkeiten Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)

Wenn Sie keinen NSGs aktiviert vorhanden Sicherheitscenter zwei Vorschläge für Sie: Netzwerk-Sicherheitsgruppen Subnetze und Netzwerk-Sicherheitsgruppen auf virtuellen Computern aktivieren aktivieren. Wählen Sie die Ebene, Subnetz oder VM NSGs anwenden.


> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung.  Dies ist nicht Schritt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. Blatt **Recommendations** **Aktivieren Netzwerksicherheitsgruppen** in Subnetzen oder wählen Sie auf virtuellen Computern aus
![Netzwerksicherheitsgruppen aktivieren][1]

2. Das Blade **Konfigurieren fehlt Netzwerksicherheitsgruppen** für Subnetze oder virtuelle Computer je nach der ausgewählten Empfehlung wird geöffnet. Wählen Sie ein Subnetz oder einen virtuellen Computer eine NSG auf konfigurieren.

  ![NSG Subnetzes konfigurieren][2]

  ![NSG für VM konfigurieren][3]
3. Auf der **Netzwerk-Sicherheitsgruppe auswählen** wählen Sie eine vorhandene NSG oder neue NSG erstellen.

  ![Wählen Sie Netzwerk-Sicherheitsgruppe][4]

Beim Erstellen einer neuen NSG führen Sie die Schritte [NSGs mit Azure-Portal verwalten](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) erstellt eine NSG und Sicherheitsregeln.

## <a name="see-also"></a>Siehe auch

Dieser Artikel wurde gezeigt, wie das Sicherheitscenter Empfehlung "Network Security Groups aktivieren" für Subnetze oder virtuellen Computern. Erfahren Sie mehr über das Aktivieren der NSGs finden Sie hier:

- [Was ist Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [NSGs mit der Azure-Portal verwalten](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md) – erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Security Health monitoring in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Überwachung Partner Solutions mit Azure Security Center](security-center-partner-solutions.md) erfahren Sie den Status Ihrer Lösungen Partner überwachen.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) - Sicherheit von Azure-Neuigkeiten und Informationen.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
