<properties 
    pageTitle="Konfigurieren eines virtuellen Netzwerks mit einer Netzwerk-Konfigurationsdatei" 
    description="Informationen zum Exportieren und Importieren einer Konfigurationsdatei Netzwerk zum Azure-Verwaltungsportal oder ändern virtueller Netzwerke. " 
    services="virtual-network" 
    documentationCenter="" 
    authors="jimdial" 
    manager="carmonm" 
    editor="tysonn"/>

<tags
    ms.service="virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services" 
    ms.date="03/15/2016"
    ms.author="jdial"/>

# <a name="configure-a-virtual-network-using-a-network-configuration-file"></a>Konfigurieren eines virtuellen Netzwerks mit einer Netzwerk-Konfigurationsdatei

Sie können ein virtuelles Netzwerk (VNet) mit der Azure-Verwaltungsportal oder mithilfe einer Netzwerk-Konfigurationsdatei konfigurieren.

## <a name="creating-and-modifying-a-network-configuration-file"></a>Erstellen und Ändern einer Netzwerk-Konfigurationsdatei 
Die einfachste Möglichkeit zum Erstellen einer Konfigurationsdatei Netzwerk wird eine vorhandene virtuelle Netzwerkkonfiguration die Einstellungen exportieren ändern Sie die Datei, die Einstellungen enthält, die für virtuelle Netzwerke konfigurieren möchten.

Bearbeiten Sie die Konfigurationsdatei Netzwerk kann einfach die Datei öffnen, ändern Sie speichern Sie die Datei. Jedem *XML-* Editor können Sie die Netzwerk-Konfiguration ändern. 

Genau befolgen Sie die Anleitung für [Netzwerk-Konfiguration Schema Einstellungen](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

Azure betrachtet ein Subnetz, das als **verwendet**bereitgestellt hat. Wenn ein Subnetz verwendet wird, kann es nicht geändert werden. Vor dem Ändern von bewegen Sie etwas an das Subnetz in ein anderes Subnetz bereitgestellt haben, die ist nicht geändert wird   [Verschieben eines virtuellen Computers oder der Rolleninstanz in ein anderes Subnetz](virtual-networks-move-vm-role-to-subnet.md)anzeigen

## <a name="export-and-import-virtual-network-settings-using-the-management-portal"></a>Exportieren und Importieren von virtuellen Netzwerkeinstellungen Verwaltungsportal  
Sie können importieren und Exportieren von Netzwerk-Konfigurationen in der Netzwerk-Konfigurationsdatei mithilfe von PowerShell oder Verwaltungsportal enthalten. Beschrieben hilft Sie exportieren und Importieren mit Verwaltungsportal. 

### <a name="to-export-your-network-settings"></a>Netzwerkkonfiguration exportieren
Beim Exportieren werden alle Einstellungen für virtuelle Netzwerke im Rahmen Ihres Abonnements eine XML-Datei geschrieben. 

1. Melden Sie sich im **Verwaltungsportal**.
2. Klicken Sie im Verwaltungsportal am Ende der Seite **Netzwerke** auf **Exportieren**. 
3. Das Fenster **Export-Konfiguration** sicher, dass das Abonnement gewählte Netzwerkkonfiguration exportiert werden soll. Klicken Sie auf das Häkchen unten rechts. 
4. Wenn Sie aufgefordert werden, speichern Sie die Datei *NetworkConfig.xml* an den Speicherort Ihrer Wahl.


### <a name="to-import-your-network-settings"></a>Netzwerkkonfiguration importieren

1. Klicken Sie im **Verwaltungsportal**unten links im Navigationsbereich auf **neu**.
2. Klicken Sie auf **Netzwerkdienste** -> **Virtual Network** -> **Konfiguration importieren**.
3. Auf der Seite **Netzwerk Konfigurationsdatei importieren** nach Konfigurationsdatei Netzwerk und klicken Sie dann auf **den Pfeil** .
4. Auf der Seite **Ihr Netzwerk** finden Sie Informationen auf dem Bildschirm, welche Abschnitte der Netzwerkkonfiguration geändert oder erstellt werden. Die ändert sie korrekt aussehen, klicken Sie auf die Markierung um oder virtuelle Netzwerk. 