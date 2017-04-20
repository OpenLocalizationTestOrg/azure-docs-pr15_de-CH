<properties 
   pageTitle="Wie private Adresse im klassischen Modus mithilfe der Azure-Portal | Microsoft Azure"
   description="Statische private IP-Adressen und Verwaltung im klassischen Modus mit Azure-Portal verstehen"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-the-azure-portal"></a>Wie eine statische private IP-Adresse im Azure-Portal (klassisch)

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das klassische Bereitstellungsmodell. Sie können auch [statische private IP-Adresse in der Ressourcen-Manager-Bereitstellungsmodell verwalten](virtual-networks-static-private-ip-arm-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Die Beispiel folgendermaßen erwarten eine einfache Umgebung bereits erstellt. Die Schritte ausführen, wie sie in diesem Dokument angezeigt werden soll, zuerst Erstellen der Umgebung beschrieben in [ein Vnet erstellen](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Statische private IP-Adresse angeben, wenn Sie einen virtuellen Computer erstellen
Erstellen Sie einen virtuellen Computer mit dem Namen *DNS01* im *Front-End-* Subnetz ein VNet namens *TestVNet* mit privaten Adresse des *192.168.1.101*wie folgt:

1. Ein Browser, navigieren Sie zu http://portal.azure.com und melden Sie sich ggf. mit Ihrem Azure-Konto.
2. Klicken Sie auf **neu** > **berechnen** > **Windows Server 2012 R2 Datacenter**, beachten Sie, dass die Liste **Wählen Sie ein Bereitstellungsmodell** bereits zeigt **klassische**und klicken Sie dann auf **Erstellen**.

    ![VM in Azure-Portal erstellen](./media/virtual-networks-static-ip-classic-pportal/figure01.png)

3. Blade **VM erstellen** , geben Sie den Namen der VM (*DNS01* in diesem Szenario) erstellt werden lokale Administratorkonto und Kennwort.

    ![VM in Azure-Portal erstellen](./media/virtual-networks-static-ip-classic-pportal/figure02.png)

4. Klicken Sie auf **Optionale Konfiguration** > **Netzwerk** > **Virtuellen Netzwerk**, und klicken Sie dann auf **TestVNet**. Wenn **TestVNet** nicht verfügbar ist, stellen Sie sicher, *USA* -Speicherort verwenden und die am Anfang dieses Artikels beschriebene Umgebung erstellt haben.

    ![VM in Azure-Portal erstellen](./media/virtual-networks-static-ip-classic-pportal/figure03.png)

5. Blatt **Netzwerk** Achten Sie ausgewählte Subnetz *FrontEnd*, dann klicken Sie auf **IP-Adressen**klicken Sie **IP-Adresszuweisung** **statische**und geben Sie *192.168.1.101* für **IP-Adresse** wie folgt.

    ![VM in Azure-Portal erstellen](./media/virtual-networks-static-ip-classic-pportal/figure04.png)   

6. **Klicken Sie in der Blade- **IP-Adressen** ** dann **Klicken Sie in das **Netzwerk** -Blade** und auf **OK** **optionale Konfiguration** Blatt.
7. Klicken Sie auf Blatt **Erstellen-VM** **Erstellen**. Beachten Sie die Kachel unten im Dashboard angezeigt.

    ![VM in Azure-Portal erstellen](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Statische private IP-Adressinformationen für einen virtuellen Computer abrufen

Um die statische private IP-Informationen für die VM mit den Schritten erstellt anzuzeigen, führen Sie die folgenden Schritte aus.

1. Azure Azure-Portal klicken Sie auf **Alle suchen** > **virtuellen Maschinen (classic)** > **DNS01** > **Alle** > **IP-Adressen** und beachten Sie die IP-Adresszuweisung und IP-Adressen wie folgt.

    ![VM in Azure-Portal erstellen](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Statische private IP-Adresse eines virtuellen Computers entfernen
Gehen Sie folgendermaßen vor um statische private IP-Adresse von der VM oben erstellten entfernen.
    
1. Obigen **Adressen** Blatt klicken Sie auf **dynamische** **IP-Adresszuweisung**rechts klicken Sie auf **Speichern**, und klicken Sie auf **Ja**.

    ![VM in Azure-Portal erstellen](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Statische private IP-Adresse zu einem vorhandenen virtuellen Computer hinzufügen
Die VM-Schritten erstellt eine statische private IP-Adresse hinzu, wie folgt:

1. Klicken Sie auf **statische** **IP-Adresszuweisung**rechts oben Blatt **IP-Adressen** .
2. Geben Sie *192.168.1.101* **IP-Adresse**, klicken Sie auf **Speichern**, und klicken Sie auf **Ja**.

## <a name="next-steps"></a>Nächste Schritte

- [Reservierte öffentliche IP-](virtual-networks-reserved-public-ip.md) Adressen erfahren.
- Erfahren Sie mehr über [öffentliche IP (ILPIP) auf Instanzebene](virtual-networks-instance-level-public-ip.md) Adressen.
- Wenden Sie sich an die [reservierte IP-REST-APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).