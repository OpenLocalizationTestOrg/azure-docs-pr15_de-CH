<properties 
   pageTitle="NSGs im ARM-Modus mit der Azure-Portal erstellen | Microsoft Azure"
   description="Informationen Sie zum Erstellen und Bereitstellen von NSGs in Azure-Portal mit ARM"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-manage-nsgs-using-the-azure-portal"></a>NSGs mit der Azure-Portal verwalten

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell Ressourcenmanager. Sie können auch [NSGs im klassischen Bereitstellungsmodell erstellen](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Im Beispiel unten aufgeführten Befehle eine einfache Umgebung bereits erwarten PowerShell basierend auf das Szenario. Wenn die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, zuerst die Umgebung durch Bereitstellung [dieser Vorlage](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd)erstellen **in Azure bereitstellen**, Standardparameterwerte ggf. ersetzen und auf gehen in das Portal. Die folgenden Schritte verwenden **RG NSG** als Namen für die Ressourcengruppe, der die Vorlage bereitgestellt wurde.

## <a name="create-the-nsg-frontend-nsg"></a>NSG FrontEnd NSG erstellen

Gehen Sie wie folgt vor Erstellen **NSG FrontEnd** NSG wie im oben beschriebenen Szenario dargestellt.

1. Ein Browser, navigieren Sie zu http://portal.azure.com und melden Sie sich ggf. mit Ihrem Azure-Konto.
2. Klicken Sie auf **Durchsuchen >** > **Netzwerk-Sicherheitsgruppen**.

    ![Azure-Portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)

3. Klicken Sie in der Blade- **Netzwerk-Sicherheitsgruppen** **Hinzufügen**.
  
    ![Azure-Portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)

4. **Erstellen Netzwerk-Sicherheitsgruppe** Blatt erstellen Sie eine NSG namens *NSG FrontEnd* *RG NSG* Ressourcengruppe und dann auf **Erstellen**.

    ![Azure-Portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Erstellen Sie Regeln in eine vorhandene NSG

Gehen Sie folgendermaßen vor um Regeln in eine vorhandene NSG von Azure-Portal erstellen.

2. Klicken Sie auf **Durchsuchen >** > **Netzwerk-Sicherheitsgruppen**.

3. Klicken Sie in der Liste NSGs **NSG FrontEnd** > **eingehende Sicherheitsregeln**

    ![Azure-Portal - NSG FrontEnd](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)

4. Klicken Sie in der Liste der **eingehenden Sicherheitsregeln**auf **Hinzufügen**.

    ![Azure-Portal - Regel hinzufügen](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)

5. Blatt **Hinzufügen eingehende Regel** erstellen Sie eine Regel mit dem Namen *Web-Regel* mit Priorität *200* Zugriff über *TCP* Port *80* für jede VM aus jeder Quelle und klicken Sie dann auf **OK**. Beachten Sie, dass die meisten Optionen bereits Standardwerte sind.

    ![Azure-Portal - Einstellungen](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)

6. Nach einigen Sekunden wird die neue Regel in der NSG angezeigt.

    ![Azure-Portal - Neuregelung](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)

7. Wiederholen Sie Schritte 6 eine eingehende Regel namens *Rdp-Regel* mit einer Priorität von *250* Zugriff über *TCP* auf Port *3389* für jede VM aus jeder Quelle erstellen.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>NSG Front-End-Subnetz zuordnen

1. Klicken Sie auf **Durchsuchen >** > **Ressourcengruppen** > **NSG RG**.
2. **NSG RG** -Blatt klicken Sie auf **...**  >  **TestVNet**.

    ![Azure-Portal - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)

3. Klicken Sie im Blatt **Einstellungen** auf **Subnetze** > **FrontEnd** > **Netzwerk-Sicherheitsgruppe** > **NSG FrontEnd**.

    ![Azure-Portal - Subnetz settings](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)

4. **Front-End** -Blatt klicken Sie auf **Speichern**.

    ![Azure-Portal - Subnetz settings](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>NSG NSG Back-End erstellen

Gehen Sie folgendermaßen vor um **NSG-BackEnd** NSG und dem **Back-End-** Subnetz.

1. Wiederholen Sie die Schritte [Erstellen NSG FrontEnd NSG](#Create-the-NSG-FrontEnd-NSG) eine NSG namens *NSG Back-End* erstellen
2. Wiederholen Sie die Schritte [Erstellen Regeln in eine bestehende NSG](#Create-rules-in-an-existing-NSG) für **eingehende** Regeln in der folgenden Tabelle.

  	|Eingehende Regel|Ausgehende Regel|
  	|---|---|
  	|![Azure-Portal - eingehende Regel](./media/virtual-networks-create-nsg-arm-pportal/figure17.png)|![Azure-Portal - ausgehende Regel](./media/virtual-networks-create-nsg-arm-pportal/figure18.png)|

3. Wiederholen Sie die Schritte im [NSG Front-End-Subnetz zuordnen](#Associate-the-NSG-to-the-FrontEnd-subnet) **NSG-Backend** NSG **Back-End-** Subnetz zugeordnet werden soll.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie [vorhandene NSGs verwalten](virtual-network-manage-nsg-arm-portal.md)
- [Aktivieren Sie die Protokollierung](virtual-network-nsg-manage-log.md) für NSGs.