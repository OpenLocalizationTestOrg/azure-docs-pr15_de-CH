<properties 
   pageTitle="Anzeigen und Ändern von Hostnamen | Microsoft Azure"
   description="Zum Anzeigen und Ändern von Hostnamen für Azure virtuelle Maschinen, web und Workerrollen für Auflösung"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="viewing-and-modifying-hostnames"></a>Anzeigen und Ändern von Hostnamen

Damit die Rolleninstanzen von Host-Namen verwiesen werden kann, müssen Sie den Wert für den Hostnamen in der Service-Konfigurationsdatei für die einzelnen Rollen festlegen. Hierzu den gewünschten Hostnamen **VmName** -Attribut des **Role** -Elements hinzufügen. Der Wert des Attributs **VmName** dient als Basis für den Hostnamen jeder Instanz der Rolle. Beispielsweise wenn **VmName** *Webrole* und drei Instanzen dieser Rolle vorhanden sind, werden die Hostnamen der Instanzen *webrole0*, *webrole1*und *webrole2*. Sie müssen keinen Hostnamen für virtuelle Computer in der Konfigurationsdatei angeben, da Hostnamen für einen virtuellen Computer basierend auf dem virtuellen Computernamen aufgefüllt wird. Weitere Informationen zum Konfigurieren von Microsoft Azure Service finden Sie unter [Azure Service Configuration Schema (.cscfg Datei)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Anzeigen von Hostnamen

Mithilfe der folgenden Tools können Sie die Hostnamen von virtuellen Maschinen und Instanzen in einem Clouddienst anzeigen.

### <a name="azure-portal"></a>Azure-Portal

[Azure-Portal](http://portal.azure.com) können Sie die Hostnamen für virtuelle Maschinen für einen virtuellen Computer auf die Übersicht anzeigen. Denken Sie daran, dass das Blade Wert für **Name** und **Hostname**angezeigt wird. Anfänglich identisch sind, wird der Host-Name nicht den Namen des virtuellen Computers oder der Rolleninstanz ändern.

Instanzen können auch im Azure-Portal angezeigt, aber beim Auflisten der Instanzen im Cloud-Dienst der Hostnamen nicht angezeigt. Sie sehen einen Namen für jede Instanz, jedoch namens den Hostnamen nicht darstellen.

### <a name="service-configuration-file"></a>Dienst

Sie können die Dienstkonfigurationsdatei bereitgestellten Dienst Blatt **Konfigurieren** des Diensts in Azure-Portal. Sie können dann das **VmName** -Attribut für das Element **Rollennamen** an den Host-Namen suchen. Denken Sie daran, dass dies als Basis für den Hostnamen der einzelnen Rolleninstanzen verwendet wird. Beispielsweise wenn **VmName** *Webrole* und drei Instanzen dieser Rolle vorhanden sind, werden die Hostnamen der Instanzen *webrole0*, *webrole1*und *webrole2*.

### <a name="remote-desktop"></a>Remote Desktop

Nach dem Aktivieren von Remotedesktop (Windows), Windows PowerShell-Remoting (Windows) oder SSH (Linux und Windows) Verbindung zu Ihrem virtuellen Computer Instanzen können Sie den Hostnamen aus eine Remotedesktop-Verbindung auf verschiedene Arten anzeigen:

- Geben Sie Hostnamen im Eingabeaufforderungsfenster oder SSH-Terminal.

- Geben Sie Ipconfig/zu dem Befehl prompt (nur Windows).

- Zeigt den Computernamen in den Systemeinstellungen (nur Windows).

### <a name="azure-service-management-rest-api"></a>Azure Servicemanagement REST-API

Folgendermaßen Sie von einem anderen Client vor:

1. Sicherstellen Sie, dass Sie ein Clientzertifikat für die Verbindung zum Azure-Portal. Um ein Clientzertifikat zu erhalten, gehen Sie in dargestellten [wie: Download und Import veröffentlichen Settings und Abonnementinformationen](https://msdn.microsoft.com/library/dn385850.aspx). 

1. Legen Sie ein Header namens X-ms-Version mit 2013-11-01.

1. Anfrage im folgenden Format: https://management.core.windows.net/\<Abonnement-Id\>/services/Hostedservices\<Dienstname\>?embed Details = True

1. Suchen Sie nach **HostName** Element für jedes Element **RoleInstance** .

>[AZURE.WARNING] Sie können auch das interne Domänensuffix für den Clouddienst von REST-Aufruf Antwort überprüfen **InternalDnsSuffix** Element oder führen Sie Ipconfig/von einer Befehlszeile aus in einer Remotedesktopsitzung (Windows) oder laufenden Cat /etc/resolv.conf von SSH-Terminal (Linux) anzeigen.

## <a name="modifying-a-hostname"></a>Ändern eines Hostnamens

Der Hostname für alle virtuellen Computer oder die Instanz der Rolle können geänderte Dienstkonfigurationsdatei hochladen oder den Computer von einer Remotedesktopsitzung umbenennen.

## <a name="next-steps"></a>Nächste Schritte

[Auflösung (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Azure Service Configuration Schema (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure Virtual Network Configuration Schema](http://go.microsoft.com/fwlink/?LinkId=248093)

[DNS-Einstellung mithilfe von Netzwerk-Konfigurationsdateien](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
