<properties 
   pageTitle="Wie private Adresse in Azure-Portal mit ARM-Modus | Microsoft Azure"
   description="Grundlegendes zu privaten IP-Adressen (DIPs) und Verwaltung in Azure-Portal mit ARM-Modus"
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

# <a name="how-to-set-a-static-private-ip-address-in-the-azure-portal"></a>Wie eine statische private IP-Adresse in Azure-portal

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell Ressourcenmanager. Sie können auch [statische private IP-Adresse im klassischen Bereitstellungsmodell verwalten](virtual-networks-static-private-ip-classic-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Die Beispiel folgendermaßen erwarten eine einfache Umgebung bereits erstellt. Die Schritte ausführen, wie sie in diesem Dokument angezeigt werden soll, zuerst Erstellen der Umgebung beschrieben in [ein Vnet erstellen](virtual-networks-create-vnet-arm-pportal.md).

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Erstellen eine VM zu Testzwecken statische private IP-Adressen

Sie können keine statische private IP-Adresse während der Erstellung eines virtuellen Computers in der Ressourcen-Manager-Bereitstellungsmodus festlegen Azure-Portal. Sie müssen den virtuellen Computer erstellen, Textauswahl seine private IP statisch festlegen

Gehen Sie folgendermaßen vor um einen virtuellen Computer mit dem Namen *DNS01* im *Front-End* -Subnetz ein VNet namens *TestVNet*zu erstellen.

1. Ein Browser, navigieren Sie zu http://portal.azure.com und melden Sie sich ggf. mit Ihrem Azure-Konto.
2. Klicken Sie auf **neu** > **berechnen** > **Windows Server 2012 R2 Datacenter**, beachten Sie, dass die Liste **Wählen Sie ein Bereitstellungsmodell** bereits zeigt **Ressourcen-Manager**und klicken Sie auf **Erstellen**, wie in der folgenden Abbildung dargestellt.

    ![VM in Azure-Portal erstellen](./media/virtual-networks-static-ip-arm-pportal/figure01.png)

3. Blatt **Grundlagen** Name der VM (*DNS01* in diesem Szenario) erstellt lokale Administratorkonto und Kennwort wie in der folgenden Abbildung dargestellt.

    ![Grundlagen blade](./media/virtual-networks-static-ip-arm-pportal/figure02.png)

4. Sicherstellen Sie der ausgewählten **Speicherort** *USA*und klicken Sie auf **Wählen Sie vorhandene** **Ressourcengruppe**klicken Sie erneut auf **Ressourcengruppe** und klicken Sie auf *TestRG*, und klicken Sie auf **OK**.

    ![Grundlagen blade](./media/virtual-networks-static-ip-arm-pportal/figure03.png)

5. Blatt **Wählen Sie eine Größe** wählen Sie **A1 Standard**und dann auf **auswählen**.

    ![Wählen Sie eine Größe blade](./media/virtual-networks-static-ip-arm-pportal/figure04.png) 

6. Blatt **Einstellungen** sicherstellen, dass die folgenden Eigenschaften festgelegt werden mit den folgenden Werten festgelegt und dann auf **OK**.

    -**Konto**: *Vnetstorage*
    - **Netzwerk**: *TestVNet*
    - **Subnetzmaske**: *Front-End*

    ![Wählen Sie eine Größe blade](./media/virtual-networks-static-ip-arm-pportal/figure05.png)  

7. **Zusammenfassung** -Blade klicken Sie auf **OK**. Beachten Sie die Kachel unten im Dashboard angezeigt.

    ![VM in Azure-Portal erstellen](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Statische private IP-Adressinformationen für einen virtuellen Computer abrufen

Um die statische private IP-Informationen für die VM mit den Schritten erstellt anzuzeigen, führen Sie die folgenden Schritte aus.

1. Azure Azure-Portal klicken Sie auf **Alle suchen** > **virtuellen Computer** > **DNS01** > **Alle** > **Netzwerkschnittstellen** und klicken Sie dann auf die einzige Schnittstelle aufgeführt.

    ![Bereitstellen von VM-Kachel](./media/virtual-networks-static-ip-arm-pportal/figure07.png)

2. Blatt **Netzwerkschnittstelle** klicken **Alle** > **IP-Adressen** und Werte **Zuordnung** und **IP-Adresse** .

    ![Bereitstellen von VM-Kachel](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Statische private IP-Adresse zu einem vorhandenen virtuellen Computer hinzufügen
Die VM-Schritten erstellt eine statische private IP-Adresse hinzu, wie folgt:

1. Klicken Sie oben Blatt **IP-Adressen** unter **Zuweisung** **statische** .
2. Geben Sie *192.168.1.101* **IP-**Adresse, und klicken Sie auf **Speichern**.

    ![VM in Azure-Portal erstellen](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

>[AZURE.NOTE] Wenn nach dem Klicken auf **Speichern** Sie feststellen, dass die Zuordnung noch auf **dynamische**gesetzt ist, bedeutet dies, dass die eingegebene IP-Adresse bereits verwendet wird. Versuchen Sie eine andere IP-Adresse.

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Statische private IP-Adresse eines virtuellen Computers entfernen
Führen Sie zum Entfernen der statischen privaten IP-Adresse von oben erstellten VM die folgenden Schritte aus.
    
1. Obigen **Adressen** Blatt klicken Sie unter **Aufgaben**auf **dynamische** und klicken Sie dann auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

- [Reservierte öffentliche IP-](virtual-networks-reserved-public-ip.md) Adressen erfahren.
- Erfahren Sie mehr über [öffentliche IP (ILPIP) auf Instanzebene](virtual-networks-instance-level-public-ip.md) Adressen.
- Wenden Sie sich an die [reservierte IP-REST-APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).