<properties 
   pageTitle="Problembehandlung bei Netzwerk-Sicherheitsgruppen - Portal | Microsoft Azure"
   description="Erfahren Sie mehr über das Netzwerk von Sicherheitsgruppen in Azure-Portal mit Azure-Ressourcen-Manager-Bereitstellungsmodell behandeln."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-the-azure-portal"></a>Problembehandlung bei Netzwerksicherheitsgruppen Azure-Portal

> [AZURE.SELECTOR]
- [Azure-Portal](virtual-network-nsg-troubleshoot-portal.md)
- [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

Netzwerk-Sicherheitsgruppen (NSGs) auf dem virtuellen Computer (VM) konfiguriert und VM-Verbindungsprobleme auftreten Überblick dieser Artikel einen Diagnosefunktionen für NSGs zu diagnostizieren.

NSGs können Sie den Datenverkehr steuern, fließen in die virtuellen Computer (VMs). NSGs können Subnetze in einer Azure Virtual Network (VNet) oder Netzwerkschnittstellen (NIC) zugewiesen werden. Die effektiven Regeln zu einer Netzwerkkarte sind eine Aggregation Subnetz verbunden, und die Regeln in der NSGs auf einen Netzwerkadapter angewendet. Regeln über diese NSGs können manchmal Konflikte verursachen und eine VM-Netzwerkkonnektivität auswirken.  

Sie können aus der NSGs auf die VM-Netzwerkkarten die effektive Regeln anzeigen. Dieser Artikel veranschaulicht das VM Verbindungsproblemen verwenden diese Regeln in der Azure-Ressourcen-Manager-Bereitstellungsmodell. Wenn Sie nicht mit VNet und NSG Konzepten vertraut sind, lesen Sie Artikel Übersicht über [virtuelle Netzwerk](virtual-networks-overview.md) und [Netzwerk-Sicherheitsgruppen](virtual-networks-nsg.md) .

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Verwendung von effektiven Sicherheitsregeln VM-Datenverkehr

Das folgende Szenario ist ein Beispiel für eine allgemeine Verbindung:

Ein virtueller Computer mit dem Namen *VM1* gehört ein Subnetz mit dem Namen *Subnet1* in einer VNet mit dem Namen *WestUS-VNet1*. Ein Verbindungsversuch an TCP-Port 3389 RDP über VM schlägt fehl. NSGs werden sowohl NIC *VM1 NIC1* Subnetz *Subnet1*angewendet. Datenverkehr an TCP-Port 3389 darf zugeordneten Netzwerkschnittstelle *VM1 NIC1*NSG jedoch TCP ping auf VM1s Port 3389 fehlschlägt.

Während dieses Beispiel TCP-Port 3389 verwendet, können folgendermaßen verwendet werden, eingehenden und ausgehenden Verbindungsfehlern über einen beliebigen Port bestimmen.

### <a name="view-effective-security-rules-for-a-virtual-machine"></a>Regeln Sie effektive Sicherheit für eine virtuelle Maschine

Gehen Sie folgendermaßen vor um NSGs für eine VM zu beheben:

Sie können vollständige effektive Sicherheitsregeln auf einer NIC VM selbst anzeigen. Sie können auch hinzufügen, ändern und Löschregeln NIC und Subnetz NSG Blatt effektive Regeln haben Berechtigungen, um diese Vorgänge auszuführen.

1. Melden Sie das Azure-Portal unter https://portal.azure.com.
2. Klicken Sie unter **Weitere Dienste**auf **virtuelle Computer** in der angezeigten Liste.
3. Wählen Sie VM Problembehandlung aus, das angezeigt wird und eine VM-Blade mit Optionen wird angezeigt.
4. Klicken Sie auf **diagnostizieren und lösen** und wählen Sie dann ein häufig auftretendes Problem. **Ich kann keine Verbindung mit meinem Windows VM** ist beispielsweise ausgewählt. 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)

5. Schritte werden unter das Problem wie in der folgenden Abbildung dargestellt: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)

    Klicken Sie auf *effektive Regeln* in der Liste der empfohlenen Schritte.

6. **Effektive Sicherheitsregeln erhalten** Blatt angezeigt wird, wie in der folgenden Abbildung dargestellt:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)

    Beachten Sie die folgenden Abschnitte des Bildes:

    - **Bereich:** *VM1*festgelegt, die VM in Schritt 3 ausgewählt.
    - **-Schnittstelle:** *VM1 NIC1* ausgewählt ist. Eine VM können mehrere Netzwerkschnittstellen (NIC). Jede Netzwerkkarte kann eindeutige effektive Sicherheit haben. Bei der Problembehandlung müssen Sie effektive Regeln für jede Netzwerkkarte anzeigen
    - **NSGs verknüpft:** NSGs können angewendet werden, um die Netzwerkkarte und das Subnetz, mit dem die Netzwerkkarte verbunden ist. In der Abbildung wurde eine NSG auf der Netzwerkkarte und damit verbundenen Subnetz angewendet. Sie können Namen NSG Regeln in der NSGs ändern klicken.
    - **VM1 Nsg Registerkarte:** Die Liste der im Bild angezeigten ist NSG Netzwerkkarte zugewiesen Mehrere Regeln werden von Azure erstellt, wenn eine NSG erstellt wird. Die Regeln können nicht entfernt, jedoch mit höherer Priorität überschreiben können. Erfahren Sie mehr über Regeln der Artikel [NSG Übersicht](virtual-networks-nsg.md#default-rules) aus.
    - **Zielspalte:** Einige der Regeln haben und andere Adresspräfixe Text in der Spalte. Text ist der Name des Standard-Tags auf der Regel angewendet, wenn es erstellt wurde. Die Tags werden vom System bereitgestellten Bezeichner, die mehrere Präfixe darstellen. Wählen eine Regel mit einem Tag wie *AllowInternetOutBound*zeigt die Präfixe **Adresspräfixe** Blatt.
    - **Herunterladen:** Die Liste der Regeln kann lang sein. Sie können eine CSV-Datei für die Offlineanalyse Regeln klicken **herunterladen** und speichern.
    - **AllowRDP** Eingehende Regel: Diese Regel ermöglicht RDP-Verbindung zur VM.
7. Klicken auf die Registerkarte **Subnet1 NSG** effektiven Regeln angewendet auf das Subnetz NSG anzeigen, wie in der folgenden Abbildung dargestellt: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)

    Beachten Sie die **eingehende** Regel *DenyRDP* . Eingehende Regeln an das Subnetz werden vor Regeln an die Netzwerkschnittstelle ausgewertet. Da die Deny-Regel an das Subnetz angewendet wird, schlägt die Anforderung zum TCP-Port 3389 herstellen, da die Zulassungsregel an die NIC nicht ausgewertet wird. 

    *DenyRDP* -Regel ist der Grund, warum RDP-Verbindung fehlschlägt. Dabei sollte das Problem lösen.

    >[AZURE.NOTE]Wenn die NIC zugeordnete VM nicht ausgeführt, oder NSGs noch nicht auf die Netzwerkkarte oder Subnetz angewendet wurde, werden keine Regeln angezeigt.

8. Klicken Sie NSG Regeln *Subnet1 NSG* in Abschnitt **NSGs verknüpft ist** .
   **NSG Subnet1** Blade wird geöffnet. Sie können Regeln direkt **eingehende Sicherheitsregeln**auf Bearbeiten.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)

9. *DenyRDP* eingehende Regel in **Subnet1 NSG** entfernen und Hinzufügen einer Regel *AllowRDP* sieht die effektiven Liste wie im folgenden Bild:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)

    Bestätigen Sie, dass TCP-Port 3389 durch Öffnen einer RDP-Verbindungs zur VM oder PsPing Werkzeug geöffnet ist. Sie können mehr über PsPing erfahren [PsPing download-Seite](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="view-effective-security-rules-for-a-network-interface"></a>Regeln Sie effektive Sicherheit für eine Netzwerkschnittstelle

Wenn die VM-Datenverkehr für eine bestimmte Netzwerkkarte beeinträchtigt wird, können Sie eine vollständige Liste der effektiven Regeln für die Netzwerkkarte aus dem Netzwerk-Schnittstellen anzeigen, die folgenden Schritte:

1. Melden Sie das Azure-Portal unter https://portal.azure.com.
2. **Weitere Dienste**klicken **Netzwerkschnittstellen** in der angezeigten Liste.
3. Wählen Sie eine Netzwerkschnittstelle. In der folgenden Abbildung wird eine Netzwerkkarte namens *VM1 NIC1* ausgewählt.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)

    Beachten Sie, dass der **Bereich** der Netzwerkschnittstelle ausgewählt. Über zusätzliche Angaben finden Sie in Schritt 6 des Abschnitts **Beheben NSGs für eine VM** .

    >[AZURE.NOTE] Wenn eine NSG aus einer Schnittstelle entfernt wird, gilt das Subnetz NSG noch bestimmten Netzwerkkarte In diesem Fall würde die Ausgabe nur Regeln aus dem Subnetz NSG anzeigen Regeln werden nur angezeigt, wenn die NIC eine VM zugeordnet ist.

4. Sie können Regeln für eine Netzwerkkarte und ein Subnetz zugeordnet NSGs direkt bearbeiten. Um zu erfahren, lesen Sie Schritt 8 des Abschnitts **Anzeigen effektiver Sicherheitsregeln für einen virtuellen Computer** .

## <a name="view-effective-security-rules-for-a-network-security-group-nsg"></a>Regeln Sie effektive Sicherheit für eine Netzwerk-Sicherheitsgruppe (NSG)

Wenn NSG Regeln ändern, sollten Sie die Auswirkung auf einen bestimmten virtuellen Computer hinzugefügten Regeln überprüfen. Eine vollständige Liste der effektiven Sicherheitsregeln für alle angegebene NSG zugewiesen Netzwerkkarten anzeigen ohne Kontext aus dem angegebenen NSG Blade. Gehen Sie folgendermaßen vor, um effektive Regeln innerhalb einer NSG zu beheben:

1. Melden Sie das Azure-Portal unter https://portal.azure.com.
2. **Weitere Dienste**, klicken Sie auf **Netzwerk-Sicherheitsgruppen** in der angezeigten Liste.
3. Wählen Sie eine NSG. In der folgenden Abbildung wurde eine NSG VM1 Nsg namens ausgewählt.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)

    Beachten Sie folgende Abschnitte von vorherigen Bild:

    - **Bereich:** NSG ausgewählt werden soll.
    - **Virtual Machine:** Wenn eine NSG Subnetz angewendet wird, gilt es alle Netzwerkschnittstellen an der VMs mit dem Subnetz verbunden. Diese Liste zeigt alle VMs diese NSG zugewiesen ist. Sie können jede VM aus der Liste auswählen.

    >[AZURE.NOTE] Eine leere Subnetz eine NSG zugewiesen ist, werden virtuelle Computer nicht aufgeführt. Wenn eine NSG zu einer Netzwerkkarte ist nicht mit einer VM angewendet wird, werden die NICs auch nicht aufgeführt. 
    - **Netzwerkschnittstelle:** Eine VM können mehrere Netzwerkschnittstellen. Sie können eine Netzwerkschnittstelle an der ausgewählten virtuellen Computer auswählen.
    - **AssociatedNSGs:** Jederzeit können eine NIC zwei effektive NSGs, eine der Netzwerkkarte und dem Subnetz angewendet. Obwohl die NIC eine effektive Subnetz NSG hat der Bereich als VM1 Nsg, ausgewählt ist, zeigt die Ausgabe sowohl NSGs.
4. Sie können Regeln für NSGs einer Netzwerkkarte oder Subnetz direkt bearbeiten. Um zu erfahren, lesen Sie Schritt 8 des Abschnitts **Anzeigen effektiver Sicherheitsregeln für einen virtuellen Computer** .

Über zusätzliche Angaben finden Sie in Schritt 6 des Abschnitts **Anzeigen effektiver Sicherheitsregeln für einen virtuellen Computer** .

>[AZURE.NOTE] Aber ein Subnetz und NIC jeweils nur eine kann NSG angewendet, kann eine NSG mehrere Netzwerkkarten und mehreren Subnetzen zugeordnet sein.

## <a name="considerations"></a>Hinweise

Berücksichtigen Sie Behandlung von Konnektivitätsproblemen sollten Sie folgende Punkte:

- NSG Standardregeln eingehenden Zugriff aus dem Internet blockiert und VNet nur zulassen eingehenden Datenverkehr. Regeln sollten eingehende Internet Bedarf Zugriff explizit hinzugefügt werden.
- Sind keine NSG Sicherheitsregeln verursacht eine VM-Netzwerkkonnektivität fehlschlägt, kann das Problem Ursachen haben:
    - Firewallsoftware Betriebssystem des virtuellen Computers
    - Routen für virtuelle Appliances oder lokalen Datenverkehr konfiguriert. Internet-Datenverkehr kann lokal über das erzwungene Tunneln umgeleitet werden. Eine RDP/SSH-Verbindung aus dem Internet die VM funktioniert möglicherweise nicht mit dieser Einstellung je nach die Netzwerkhardware lokalen Datenverkehr Behandlung. Die [Problembehandlung Routen](virtual-network-routes-troubleshoot-powershell.md) Artikel Informationen zum Arbeitsplan Probleme diagnostizieren, die den Fluss des Datenverkehrs und die VM behindern kann. 
- Wenn VNets, standardmäßig dies haben, wird das Tag VIRTUAL_NETWORK automatisch erweitert Präfixe bei hervorragendem VNets enthalten. Sie können diese Präfixe in der Liste **ExpandedAddressPrefix** beheben Sie alle Probleme im Zusammenhang mit VNet peering Konnektivität anzeigen. 
- Effektive Sicherheitsregeln werden nur angezeigt, wenn eine NSG der VMs NIC oder Subnetz zugeordnet ist. 
- Keine NSGs mit der Netzwerkkarte verknüpft oder Subnetz und eine öffentliche IP-Adresse der VM zugewiesen haben, werden alle Anschlüsse für eingehenden und ausgehenden Zugriff geöffnet. Hat die VM eine öffentliche IP-Adresse der Netzwerkkarte oder dem Subnetz NSGs zuweisen empfiehlt.