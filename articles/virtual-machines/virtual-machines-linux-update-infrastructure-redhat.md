<properties
   pageTitle="Red Hat-Infrastruktur (RHUI) | Microsoft Azure"
   description="Weitere Informationen Sie über Red Hat Update Infrastruktur (RHUI) für bei Bedarf Red Hat Enterprise Linux-Instanzen im Microsoft Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="BorisB2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="borisb"/>

# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat-Infrastruktur (RHUI) für bei Bedarf unter Red Hat Enterprise Linux VMs in Azure

Zugriff auf die in Azure bereitgestellt Red Hat Update Infrastruktur (RHUI) werden von bei Bedarf Red Hat Enterprise Linux (RHEL) verfügbaren Abbilder Azure Marketplace erstellte virtuelle Maschinen registriert.  Bei Bedarf RHEL Instanzen haben Zugriff zu einem regionalen Yum Repository und inkrementelle Updates empfangen.

Yum-Repository-Liste, der vom RHUI verwaltet wird, wird während der Bereitstellung in RHEL-Instanz konfiguriert. Sie müssen zusätzliche Konfiguration - ausführen `yum update` bereit, um die neuesten Updates Ihrer RHEL-Instanz ist.

> [AZURE.NOTE] Azure RHUI Infrastruktur wurde kürzlich aktualisiert (September 2016) und ändert die Konfiguration der vorhandenen RHEL Instanzen für den ununterbrochenen Zugriff auf Azure RHUI erfordert. Siehe Abschnitt RHUI Azure Infrastrukturupdate für Details.


## <a name="rhui-azure-infrastructure-update"></a>RHUI Azure-Infrastruktur aktualisieren
Ab September 2016 hat Azure einen neuen Satz von Red Hat Update Infrastruktur (RHUI) Server. Diese Server sind mit [Azure Traffic Manager]( https://azure.microsoft.com/services/traffic-manager/) bereitgestellt, sodass jeder VM unabhängig von Region ein einzelnen Endpunkt (Rhui 1.micrsoft.com) verwendet werden kann. Sie verwenden Sie ein SSL-Zertifikat, das mit einer bekannten Zertifizierungsstelle (Baltimore Root) verkettet ist. Machen das Update automatisch wäre für einige Kunden, die ACLs oder benutzerdefinierte routing-Tabellen für RHUI Update-Servern haben, so ist dieses Update "opt-in." Manuelle Schritte zum Onboarding auf diese neuen Server stehen auf dieser Seite und ein vollständiges Skript Onboarding automatisiert (nach Überprüfung der einzelnen Schritte). Neue RHEL PAYG Bilder Azure Marketplace (Versionen vom September 2016 oder höher) werden automatisch auf die neuen Azure RHUI Server zeigen und keine weiteren Maßnahmen erforderlich.

### <a name="the-new-azure-rhui-infrastructure-onboarding-timeline"></a>Neue Azure RHUI Infrastruktur Onboarding-Zeitachse

| Datum | Hinweis |
| --- | --- |
|22 September 2016 | RHUI Server und installieren Richtung zur Verfügung. VMs mit neuen (vom September 2016) bereitgestellte RHEL PAYG Marketplace Bilder verwendet automatisch die neuen RHUI Server vorhandenen VMs sind jedoch "Anmelden"
|1 November 2016 | RHEL PAYG VM Legacyabbilder (mit der alten Azure RHUI Server) werden aus der Galerie Azure Marketplace entfernt
|16 Januar 2017 | Die alten Azure RHUI Server außer Betrieb genommen werden. Aktualisieren Sie aller der betroffenen PAYG RHEL VMs bis zu diesem Zeitpunkt Zugriff auf Azure-RHUI verwalten

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a>Die IP-Adressen für die neue RHUI Content Server sind

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193
```

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a>Manuelle Aktualisierung so die neue Azure RHUI Server

Die öffentlichen Schlüssel Signatur (über Curl) herunterladen

```
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Überprüfen Sie die heruntergeladene key

```
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Überprüfen Sie die Ausgabe, überprüfen Sie `keyid` und `user ID packet`:

```
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

Installieren Sie den öffentlichen Schlüssel

```
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Herunterladen Sie, überprüfen Sie und installieren Sie Client u/Min

Herunterladen: Für RHEL 6

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

Für RHEL 7

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Überprüfen:

```
rpm -Kv azureclient.rpm
```

Überprüfen Sie in der Ausgabe die Signatur des Pakets ist OK

```
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

Installieren Sie das RPM

```
sudo rpm -U azureclient.rpm
```

Überprüfen Sie nach Abschluss des Azure RHUI Form der VM zugreifen können

### <a name="all-in-one-script-for-automating-the-above-task"></a>All-in-One-Skript für die oben stehende Aufgabe automatisieren
Verwenden Sie das folgende Skript als die Aufgabe betroffenen VMs auf die neuen Azure RHUI Server automatisieren.

```
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>RHUI (Übersicht)
[Red Hat-Infrastruktur](https://access.redhat.com/products/red-hat-update-infrastructure) bietet eine hochgradig skalierbare Lösung zum Verwalten der Yum Repository Content für Red Hat Enterprise Linux Cloud Instanzen von Red Hat-Zertifizierung Cloudanbieter gehostet werden. Grundlage der vorgelagerten Zellstoff Projekt kann RHUI Cloudanbieter lokal spiegeln Repository Content gehosteten Red Hat benutzerdefinierte Repositories mit eigenen Inhalten erstellen und diesen Repositories für einer großen Gruppe von Endbenutzern über einen Lastenausgleich Content Delivery Systems verfügbar machen.

## <a name="regions-where-rhui-is-available"></a>Regionen RHUI verfügbar
RHUI ist in allen Regionen RHEL bei Bedarf Bilder verfügbar. Es enthält derzeit alle Öffentliche Bereichen [Azure Status](https://azure.microsoft.com/status/) Dashboardseite und Azure Regierung Regionen aufgeführt. RHUI Zugriff auf VMs RHEL bei Bedarf Bilder bereitgestellt ist im Preis enthalten. Verfügbarkeit zusätzlicher regionaler, nationaler Cloud aktualisiert in Zukunft Erweiterung RHEL bei Bedarf verfügbar.

> [AZURE.NOTE] Zugriff auf RHUI Azure gehostet ist auf die VMs in [Microsoft Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="get-updates-from-another-update-repository"></a>Ein anderes Updaterepository Updates erhalten

Wenn Sie Updates aus einem anderen Updaterepository (statt RHUI Azure gehostet müssen) müssen Sie Ihre Instanzen von RHUI abmelden und erneut mit der gewünschten Infrastruktur (oder Red Hat Satellite Red Hat Customer Portal CDN) registrieren. Sie benötigen die entsprechenden Red Hat Abonnements für diese Dienste und Registrierung auf [Red Hat Cloud in Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).

Aufheben von RHUI und registrieren Sie Ihre Update-Infrastruktur folgen die unten beschriebenen Schritte aus.

1.  Bearbeiten von /etc/yum.repos.d/rh-cloud.repo und alle `enabled=1` , `enabled=0`. Zum Beispiel:

        # sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo

2.  /Etc/yum/pluginconf.d/rhnplugin.conf bearbeiten und ändern `enabled=0` , `enabled=1`.
3.  Gewünschte Infrastruktur wie Red Hat Customer Portal registrieren. Folgen Sie Red Hat-Lösungshandbuch zum [registrieren und Abonnieren eines Red Hat Customer Portal](https://access.redhat.com/solutions/253273)

> [AZURE.NOTE] Zugriff auf RHUI Azure gehostet ist im Preis Bild RHEL nutzungsbasierte (PAYG) enthalten. Aufheben der Registrierung einer VM PAYG RHEL Azure gehostet RHUI konvertiert nicht den virtuellen Computer zu bringen-Your-eigenen-Lizenz (BYOL) VM und damit Sie möglicherweise werden kündigen double melden Sie den gleichen virtuellen Computer mit einem anderen Updates. 
> 
> Müssen Sie stets eine Infrastruktur verwenden Azure gehostet RHUI sollten erstellen und eigene Bilder (BYOL Typ) gemäß Artikel [Erstellen und Hochladen Red Hat-basierten virtuellen Computer für Azure](virtual-machines-linux-redhat-create-upload-vhd.md) bereitstellen.

## <a name="next-steps"></a>Nächste Schritte
Erstellen einer Red Hat Enterprise Linux VM von Azure Marketplace nutzungsbasierte Bild und nutzen Azure gehostet RHUI gehen [Azure](https://azure.microsoft.com/marketplace/partners/redhat/)Marketplace. Sie werden mit `yum update` in der RHEL-Instanz ohne zusätzliche Konfiguration.