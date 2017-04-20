<properties
   pageTitle="Mithilfe von dynamischen DNS Hostnamen registrieren"
   description="Diese Seite enthält Informationen wie dynamische DNS Hostnamen in eigene DNS-Server registrieren."
   services="dns"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="" />
<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="using-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Verwenden dynamisches DNS Hostnamen im eigenen DNS-Server registrieren

[Azure bietet eine Auflösung](virtual-networks-name-resolution-for-vms-and-role-instances.md) für virtuelle Maschinen (VMs) und Instanzen. Wenn die Auflösung über die Azure wechseln muss, können Sie eigene DNS-Server bereitstellen. Dies bietet Ihnen die Möglichkeit, Ihre DNS-Lösung Ihre eigenen Bedürfnisse anpassen. Beispielsweise müssen Sie lokale Ressourcen über Active Directory-Domänencontroller zugreifen.

Wenn benutzerdefinierte DNS-Server als Azure VMs befinden können Sie Azure Hostnamen auflösen Hostname Abfragen nach demselben Vnet weiterleiten. Wenn Sie nicht diese Route verwenden möchten, können Sie die VM-Hostnamen in der DNS-Server mit dynamischen DNS registrieren.  Azure kann nicht (z. B. Anmeldeinformationen) Datensätze direkt in Ihrem DNS-Server erstellen, alternativen oft notwendig sind. Hier sind einige häufige Szenarien mit alternativen.

## <a name="windows-clients"></a>Windows-clients

Nicht Domäne Windowsclients versuchen unsicheren Dynamic DNS (DDNS) Updates, wenn sie starten oder ihre IP-Adresse geändert wird. Der DNS-Name ist der Hostname und das primäre DNS-Suffix. Azure lässt das primäre DNS-Suffix leer, jedoch auf dem virtuellen Computer über die [Benutzeroberfläche](https://technet.microsoft.com/library/cc794784.aspx) oder [mithilfe der Automatisierung](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix)können festlegen.

Domäne Windowsclients registrieren ihre IP-Adressen mit dem Domänencontroller mithilfe von sicheren dynamischen DNS. Der Domäne beitreten wird das primäre DNS-Suffix auf dem Client erstellt und verwaltet die Vertrauensstellung.

## <a name="linux-clients"></a>Linux-clients

Linux-Clients im Allgemeinen nicht registrieren sich beim DNS-Server beim Start, angenommen der DHCP-Server nicht. Azure DHCP-Server müssen nicht bzw. Anmeldeinformationen Datensätze in der DNS-Server registrieren.  Ein Tool namens *Nsupdate*, die Bind-Paket enthalten ist, können Sie um dynamische DNS-Updates zu senden. Da das dynamische DNS-Protokoll standardisiert können Sie *Nsupdate* , auch wenn Sie nicht Bind DNS-Server verwenden.

Sie können die Hooks, die vom DHCP-Client zum Erstellen und Verwalten des Hostname-Eintrags in der DNS-Server bereitgestellt werden. Im DHCP-Zyklus führt der Client die Skripts in */etc/dhcp/dhclient-exit-hooks.d/*. Hiermit kann die neue IP-Adresse mit *Nsupdate*registrieren. Zum Beispiel:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on the primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        #done
        exit 0;

Den Befehl *Nsupdate* können Sie sichere dynamische DNS-Updates durchführen. Z. B. wird Bind DNS-Server verwenden, ein öffentliches / privates Schlüsselpaar [erzeugt](http://linux.yyz.us/nsupdate/).  Der DNS-Server ist, können sie die Signatur für die Anforderung mit dem öffentlichen Teil des Schlüssels [konfiguriert](http://linux.yyz.us/dns/ddns-server.html) . Die Option *-k* verwenden zu Schlüsselpaars, *Nsupdate* in der Reihenfolge für das dynamische DNS-Update Anforderung signiert werden.

Wenn Sie einen Windows-DNS-Server verwenden, können Sie Kerberos-Authentifizierung mit Parameters *-g* *Nsupdate* (nicht verfügbar in der Windows-Version von *Nsupdate*). Verwenden Sie hierzu *Kinit* Anmeldeinformationen (z. B. aus einer [Keytab-Datei](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)) laden. Dann übernehmen *Nsupdate g* die Anmeldeinformationen aus dem Cache.

Bei Bedarf können Sie die VMs DNS Search Suffix hinzufügen. Das DNS-Suffix wird in der Datei */etc/resolv.conf* angegeben. Die meisten Linux-Distributionen automatisch verwaltet den Inhalt dieser Datei in der Regel nicht bearbeiten. Allerdings können Sie das Suffix mit der DHCP-Client *Ersetzen* Befehl überschreiben. Klicken Sie dazu im */etc/dhcp/dhclient.conf*hinzufügen:

        supersede domain-name <required-dns-suffix>;

