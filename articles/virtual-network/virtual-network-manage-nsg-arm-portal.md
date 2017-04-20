<properties 
   pageTitle="Verwalten über das vorschauportal im Ressourcen-Manager NSGs | Microsoft Azure"
   description="Erfahren Sie, wie vorhandene NSGs über das vorschauportal im Ressourcen-Manager verwalten"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-the-preview-portal"></a>NSGs mit vorschauportal verwalten

> [AZURE.SELECTOR]
- [Portal](virtual-network-manage-nsg-arm-portal.md)
- [PowerShell](virtual-network-manage-nsg-arm-ps.md)
- [Azure CLI](virtual-network-manage-nsg-arm-cli.md)

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Informationen abrufen

Sie können Ihre vorhandenen NSGs anzeigen, Regeln für eine vorhandene NSG abrufen und herauszufinden, welche Ressourcen eine NSG zugeordnet ist.

### <a name="view-existing-nsgs"></a>Vorhandene NSGs anzeigen
Gehen Sie folgendermaßen vor um alle vorhandenen NSGs in einem Abonnement anzuzeigen.

1. Ein Browser, navigieren Sie zu http://portal.azure.com und melden Sie sich ggf. mit Ihrem Azure-Konto.
2. Klicken Sie auf **Durchsuchen >** > **Netzwerk-Sicherheitsgruppen**.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Überprüfen Sie die Liste der NSGs im **netzwerksicherheitsgruppen** Blade.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

Zum Anzeigen der Liste der NSGs in der Ressourcengruppe **RG NSG** wie folgt. 

1. Klicken Sie auf **Ressourcengruppen >** > **RG NSG** > **...**.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. Suchen Sie in der Liste der Ressourcen Elemente Symbol NSG Siehe Blatts **Ressourcen** anzeigen.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure4.png)
         
### <a name="list-all-rules-for-an-nsg"></a>Liste aller Regeln für eine NSG

Gehen Sie folgendermaßen vor um Regeln eine NSG **NSG FrontEnd**Namens anzuzeigen. 

1. Klicken Sie auf **NSG-FrontEnd-**Blade- **Netzwerk-Sicherheitsgruppen** und **Ressourcen** Blade oben.
2. Klicken Sie auf der Registerkarte **Einstellungen** auf **eingehende Sicherheitsregeln**.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. **Eingehende Sicherheitsregeln** Blatt wird angezeigt, wie unten dargestellt.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. Klicken Sie auf der Registerkarte **Optionen** auf **ausgehende Sicherheitsregeln** ausgehenden Regeln.

>[AZURE.NOTE] Klicken Sie Standardregeln **Standardregeln** Symbol oben Blade, das die Regeln anzeigt.

### <a name="view-nsgs-associations"></a>NSGs Zuordnung anzeigen

Gehen Sie wie folgt vor Anzeige ist **NSG FrontEnd** NSG Ressourcen zuordnen.

1. Klicken Sie auf **NSG-FrontEnd-**Blade- **Netzwerk-Sicherheitsgruppen** und **Ressourcen** Blade oben.
2. Klicken Sie auf der Registerkarte **Optionen** auf **Subnets** um anzuzeigen, welche Subnetze der NSG zugeordnet sind.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. Klicken Sie auf der Registerkarte **Optionen** auf **Netzwerkschnittstellen** , um anzuzeigen, welche Netzwerkkarten der NSG zugeordnet sind.

## <a name="manage-rules"></a>Regeln verwalten

Sie können eine vorhandene NSG Regeln hinzufügen, bestehende Regeln bearbeiten und entfernen.

### <a name="add-a-rule"></a>Hinzufügen einer Regel

Gehen Sie folgendermaßen vor um eine Regel **eingehenden** Datenverkehr auf Port **443** von jedem Computer **NSG FrontEnd** NSG hinzuzufügen.

1. Klicken Sie auf **NSG-FrontEnd-**Blade- **Netzwerk-Sicherheitsgruppen** und **Ressourcen** Blade oben.
2. Klicken Sie auf der Registerkarte **Einstellungen** auf **eingehende Sicherheitsregeln**.
3. **Eingehende Sicherheitsregeln** Blade klicken Sie auf **Hinzufügen**. Dann Blatt **Hinzufügen eingehende Regel** Geben Sie die Werte wie folgt, und klicken Sie auf **OK**.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

4. Beachten Sie nach wenigen Sekunden die neue Regel **eingehende Sicherheitsregeln** Blatt.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Ändern einer Regel

Gehen Sie folgendermaßen vor um eingehenden Datenverkehr aus dem **Internet** nur oben erstellte Regel ändern.

1. Klicken Sie auf **NSG-FrontEnd-**Blade- **Netzwerk-Sicherheitsgruppen** und **Ressourcen** Blade oben.
2. Klicken Sie auf die oben erstellte Regel auf der Registerkarte **Einstellungen** .
3. Blatt **können Https** ändern Sie **Quelleigenschaft** wie folgt und dann auf **Speichern**.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Löschen einer Regel

Gehen Sie folgendermaßen vor um oben erstellte Regel löschen.

1. Klicken Sie auf **NSG-FrontEnd-**Blade- **Netzwerk-Sicherheitsgruppen** und **Ressourcen** Blade oben.
2. Klicken Sie auf die oben erstellte Regel auf der Registerkarte **Einstellungen** .
3. Blades **können Https** auf **Löschen**und klicken Sie auf **Ja**.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Verwalten von Assoziationen

Sie können eine NSG, Subnetze und Netzwerkkarten zuordnen. Sie können auch eine NSG aus beliebigen Ressourcen trennen, zugeordnet ist.

### <a name="associate-an-nsg-to-a-nic"></a>NSG zu einer Netzwerkkarte zuordnen

Gehen Sie folgendermaßen vor um **NSG FrontEnd** NSG, **TestNICWeb1** NIC zuzuordnen.

1. Klicken Sie auf **NSG-FrontEnd-**Blade- **Netzwerk-Sicherheitsgruppen** und **Ressourcen** Blade oben.
2. Klicken Sie auf der Registerkarte **Einstellungen** auf **Netzwerkschnittstellen** > **Ordnen Sie** > **TestNICWeb1**.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>NSG aus einer Netzwerkkarte trennen

Gehen Sie folgendermaßen vor um **NSG FrontEnd** NSG aus **TestNICWeb1** NIC trennen.

1. Wählen Sie Azure-Portal **Ressourcengruppen >** > **RG NSG** > **...**  >  **TestNICWeb1**.
2. Blatt **TestNICWeb1** klicken Sie auf **Sicherheit ändern...**  > **None**.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

>[AZURE.NOTE] Dieses Blatt können Sie die NIC vorhandenen NSG zuordnen.

### <a name="dissociate-an-nsg-from-a-subnet"></a>NSG in einem Subnetz trennen

Gehen Sie folgendermaßen vor um **NSG FrontEnd** NSG vom **Front-End** -Subnetz zu trennen.

1. Wählen Sie Azure-Portal **Ressourcengruppen >** > **RG NSG** > **...**  >  **TestVNet**.
2. Klicken Sie im Blatt **Einstellungen** auf **Subnetze** > **FrontEnd** > **Netzwerk-Sicherheitsgruppe** > **keine**.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. **Front-End** -Blatt klicken Sie auf **Speichern**.

![Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>NSG Subnetz zuordnen

Gehen Sie folgendermaßen vor um **NSG FrontEnd** NSG Subnetz **FronEnd** zuzuordnen.

1. Wählen Sie Azure-Portal **Ressourcengruppen >** > **RG NSG** > **...**  >  **TestVNet**.
2. Klicken Sie im Blatt **Einstellungen** auf **Subnetze** > **FrontEnd** > **Netzwerk-Sicherheitsgruppe** > **NSG FrontEnd**.
3. **Front-End** -Blatt klicken Sie auf **Speichern**.

>[AZURE.NOTE] Sie können auch eine NSG Subnetz Thh NSG **Einstellungen** Blatt zuordnen.

## <a name="delete-an-nsg"></a>Eine NSG löschen

Wenn keine Ressourcen zugeordnet sind, können Sie nur eine NSG löschen. Gehen Sie folgendermaßen vor um eine NSG zu löschen.

1. Wählen Sie Azure-Portal **Ressourcengruppen >** > **RG NSG** > **...**  >  **NSG FrontEnd**.
2. Klicken Sie im Blatt **Einstellungen** **Netzwerkschnittstellen**.
3. Sind alle aufgeführten NICs, klicken Sie auf die Netzwerkkarte und führen Sie Schritt 2 im [lg NSG aus einer Netzwerkkarte](#Dissociate-an-NSG-from-a-NIC).
4. Wiederholen Sie Schritt 3 für jede Netzwerkkarte.
5. Klicken Sie im Blatt **Einstellungen** auf **Subnetze**.
6. Sind Subnetze aufgeführt, klicken Sie auf das Subnetz und führen Sie die Schritte 2 und 3 [lg NSG in einem Subnetz](#Dissociate-an-NSG-from-a-subnet).
7. ** **NSG FrontEnd** -Blade links Rollen klicken** > **Ja**.

[Azure-Portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren Sie die Protokollierung](virtual-network-nsg-manage-log.md) für NSGs.
