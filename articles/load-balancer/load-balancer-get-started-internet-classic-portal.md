
<properties
   pageTitle="Erste Schritte beim Erstellen einer Internetschnittstelle Lastenausgleich im klassischen Bereitstellungsmodell mit klassischen Azure-Portal | Microsoft Azure"
   description="Erstellen Sie ein System zum Lastenausgleich im klassischen Bereitstellungsmodell mit klassischen Azure-Portal mit Internetzugriff"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Erste Schritte beim Erstellen einer Lastenausgleich (klassisch) im klassischen Azure-Portal mit Internetzugriff

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das klassische Bereitstellungsmodell. Sie können auch [ein Lastenausgleich mithilfe von Azure Resource Manager mit Internetzugriff erstellen](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Richten Sie ein System zum Lastenausgleich Internetzugriff für virtuelle Maschinen

Um Saldo Netzwerkverkehr aus dem Internet auf virtuellen Maschinen eines Cloud-Diensts zu laden, müssen Sie einen Lastenausgleich erstellen können. Es wird vorausgesetzt, dass die virtuellen Computer bereits erstellt haben und dass sie innerhalb derselben Cloud-Dienst.

**So konfigurieren Sie eine Gruppe mit Lastenausgleich für virtuelle Maschinen**

1. In der Azure-Verwaltungsportal auf **virtuelle Computer**und klicken Sie dann auf den Namen eines virtuellen Computers in der Gruppe mit Lastenausgleich.

2. **Endpunkte**auf und klicken Sie dann auf **Hinzufügen**.

3. Klicken Sie auf den Pfeil nach rechts auf der Seite **einen Endpunkt einer virtuellen Maschine hinzufügen** .

4. Auf der Seite **Details des Endpunkts angeben** :

    * Im Feld **Name**einen Namen für den Endpunkt oder wählen aus der Liste der vordefinierten Endpunkte für gängige Protokolle.
    * **Protokoll**aktivieren Sie das Protokoll durch den Typ des Endpunkts, TCP oder UDP Bedarf.
    * Geben Sie **Öffentliche und Private Port**die Port-Nummern des virtuellen Computers verwenden, Bedarf möchten. Können Sie private Port und Firewall-Regeln auf dem virtuellen Computer Datenverkehr so umgeleitet, die für die Anwendung geeignet ist. Private Port kann öffentliche Ports identisch sein. Beispielsweise können für einen Endpunkt für Webdatenverkehr (HTTP) Sie Port 80 öffentlicher und privater Port zuweisen.

5. Erstellen Sie **einer Gruppe mit Lastenausgleich**und klicken Sie dann auf den Pfeil nach rechts.

6. Geben Sie auf der Seite **Konfigurieren festgelegten Lastenausgleich** einen Namen für die Gruppe mit Lastenausgleich und weisen Sie anschließend Werte für Prüfpunkt Verhalten der Azure-Lastenausgleich. Der Lastenausgleich verwendet Prüfpunkte bestimmt die virtuellen Computer in den Lastenausgleich für eingehenden Datenverkehr empfangen werden.

7. Klicken Sie zum Erstellen des Endpunkts Lastenausgleich. In der Spalte **Lastenausgleich Namen** für den virtuellen Computer auf der Seite **Endpunkte** wird **Ja** angezeigt.

8. Klicken Sie im Portal auf **virtuellen Maschinen**, klicken Sie auf einen weiteren virtuellen Computer in den Lastenausgleich klicken Sie **Endpunkte**und klicken Sie dann auf **Hinzufügen**.

9. Klicken Sie auf **einen Endpunkt einer virtuellen Maschine hinzufügen** auf **Add Endpunkt auf vorhandene Lastenausgleich**, wählen Sie den Namen der Gruppe mit Lastenausgleich und klicken Sie auf den Pfeil nach rechts.

10. Geben Sie auf der Seite **Geben Sie die Details des Endpunkts** einen Namen für den Endpunkt, und klicken Sie auf das Häkchen.

Wiederholen Sie die Schritte 8 bis 10 für die zusätzlichen virtuellen Computer in den Lastenausgleich.



## <a name="next-steps"></a>Nächste Schritte

[Beginnen Sie einen internen Lastenausgleich konfigurieren](load-balancer-get-started-ilb-arm-ps.md)

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)

