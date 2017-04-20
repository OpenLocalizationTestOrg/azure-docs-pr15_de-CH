<properties
    pageTitle="Konfigurieren von DHCPv6 für Linux VMs | Microsoft Azure"
    description="Konfigurieren von DHCPv6 für Linux VMs"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6 Azure Lastenausgleich dual-Stack, öffentliche IP-Adresse, nativen IPv6-, Mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="configuring-dhcpv6-for-linux-vms"></a>Konfigurieren von DHCPv6 für Linux VMs

Linux VM Bilder in Azure Marketplace DHCPv6 standardmäßig keinen. Zur Unterstützung von IPv6 müssen DHCPv6 in innerhalb der Linux-Betriebssystem konfiguriert werden, die Sie verwenden. Andere Linux-Distributionen haben DHCPv6 konfigurieren, da sie verschiedene Pakete verwenden.

>[AZURE.NOTE] Aktuelle SUSE Linux und CoreOS Bilder in Azure Marketplace wurden DHCPv6 vorkonfiguriert. Diese Bilder sind keine zusätzlichen Änderungen erforderlich.

Dieses Dokument beschreibt die DHCPv6 aktivieren, damit die virtuellen Linux-Maschine eine IPv6-Adresse erhält.

>[AZURE.WARNING] Netzwerk-Konfigurationsdateien nicht ordnungsgemäß bearbeiten kann Sie Netzwerkzugriff auf die VM. Sie sollten die geänderte Konfiguration auf nicht produktiven Systemen testen. Diese Artikel wurden auf die neuesten Versionen der Linux Bilder in Azure Marketplace getestet. Dokumentation für Ihre spezielle Version von Linux Nähere Informationen.

## <a name="ubuntu"></a>Ubuntu

1. Bearbeiten Sie die Datei `/etc/dhcp/dhclient6.conf` , und fügen Sie die folgende Zeile hinzu:

        timeout 10;

2. Bearbeiten Sie die Konfiguration für die Schnittstelle eth0 müssen mit der folgenden Konfiguration:

    * Bearbeiten Sie die Datei unter **Ubuntu 12.04 und 14.04**`/etc/network/interfaces.d/eth0.cfg`
    * Bearbeiten Sie die Datei auf **Ubuntu 16.04**`/etc/network/interfaces.d/50-cloud-init.cfg`

    ```bash
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. Erneuern Sie IPv6-Adresse:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Bearbeiten Sie die Datei `/etc/dhcp/dhclient6.conf` , und fügen Sie die folgende Zeile hinzu:

        timeout 10;

2. Bearbeiten Sie die Datei `/etc/network/interfaces` und die folgende Konfiguration:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Erneuern Sie IPv6-Adresse:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Bearbeiten Sie die Datei `/etc/sysconfig/network` , und fügen Sie den folgenden Parameter:

        NETWORKING_IPV6=yes

2. Bearbeiten Sie die Datei `/etc/sysconfig/network-scripts/ifcfg-eth0` , und fügen Sie die folgenden zwei Parameter:

        IPV6INIT=yes
        DHCPV6C=yes

3. Erneuern Sie IPv6-Adresse:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 und 13 openSUSE

Aktuelle SLES und OpenSUSE Abbilder in Azure wurden DHCPv6 vorkonfiguriert. Diese Bilder sind keine zusätzlichen Änderungen erforderlich. Haben Sie eine VM basierend auf einem älteren oder benutzerdefinierte SUSE, gehen Sie folgendermaßen vor:

1. Installieren Sie die `dhcp-client` Paket bei Bedarf:

    ```bash
    # sudo zypper install dhcp-client
    ```

2. Bearbeiten Sie die Datei `/etc/sysconfig/network/ifcfg-eth0` , und fügen Sie den folgenden Parameter:

        DHCLIENT6_MODE='managed'

3. Erneuern Sie die IPv6-Adresse:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 und OpenSUSE Sprung

Aktuelle SLES und OpenSUSE Abbilder in Azure wurden DHCPv6 vorkonfiguriert. Diese Bilder sind keine zusätzlichen Änderungen erforderlich. Haben Sie eine VM basierend auf einem älteren oder benutzerdefinierte SUSE, gehen Sie folgendermaßen vor:

1. Bearbeiten Sie die Datei `/etc/sysconfig/network/ifcfg-eth0` , und Ersetzen Sie dabei

        #BOOTPROTO='dhcp4'

    mit dem folgenden Wert:

        BOOTPROTO='dhcp'

2. Den folgenden Parameter hinzufügen `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Erneuern Sie die IPv6-Adresse:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Aktuelle CoreOS Abbilder in Azure wurden DHCPv6 vorkonfiguriert. Diese Bilder sind keine zusätzlichen Änderungen erforderlich. Haben Sie eine VM basierend auf einem älteren oder benutzerdefinierte CoreOS, gehen Sie folgendermaßen vor:

1. Bearbeiten Sie die Datei`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Erneuern Sie die IPv6-Adresse:

    ```bash
    # sudo systemctl restart systemd-networkd
    ```
