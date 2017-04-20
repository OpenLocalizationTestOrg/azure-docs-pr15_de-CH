<properties
   pageTitle="StorSimple Virtual Array Anforderungen"
   description="Erfahren Sie mehr über die Software und Netzwerk für virtuelle StorSimple Array"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/17/2016"
   ms.author="alkohli"/>

# <a name="storsimple-virtual-array-system-requirements"></a>StorSimple Virtual Array Anforderungen

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt wichtige Vorschriften für Ihre Microsoft Azure StorSimple Virtual Array (auch lokal StorSimple virtuelles Gerät bzw. virtuellen Gerät StorSimple) und Storage Clients den Zugriff auf das Array. Wir empfehlen die Informationen sorgfältig überprüfen, bevor Sie Ihr System StorSimple bereitstellen und dann, nach Bedarf während der Bereitstellung und nachfolgenden Vorgang verweisen.

Systemvoraussetzungen:

-   **Software an Clients Storage** - beschreibt Unterstützte Virtualisierungsplattformen, Webbrowsern iSCSI-Initiatoren, SMB-Clients, minimale virtuelles Gerät Vorschriften und zusätzlichen Anforderungen für diese Betriebssysteme.

-   **Netzwerk an das Gerät StorSimple** - Informationen zu den Ports, die in der Firewall zu iSCSI, Cloud oder Verwaltungsdatenverkehr geöffnet werden.

StorSimple Vorschriften Systeminformationen veröffentlicht in diesem Artikel gilt nur für virtuelle StorSimple-Arrays.

- 8000-Serie Geräte gehen Sie zu [System für Ihr Gerät StorSimple 8000-Serie](storsimple-system-requirements.md).
 
- 7000 Serie Geräte finden Sie unter [System Requirements für Ihr Gerät StorSimple 5000-7000-Serie](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).


## <a name="software-requirements"></a>Für Software

Die Software umfassen Informationen zu unterstützten Webbrowsern, SMB-Versionen, Virtualisierungsplattformen und die minimale virtuelles Gerät.

### <a name="supported-virtualization-platforms"></a>Unterstützte Plattformen


| **Hypervisor** | **Version**                          |
|----------------|--------------------------------------|
| Hyper-V        | Windows Server 2008 R2 SP1 und höher |
| VMware ESXi    | 5.5 und höher                        |

### <a name="virtual-device-requirements"></a>Virtuelles Gerät Vorschriften

| **Komponente**                                | **Anforderung**            |
|----------------------------------------------|----------------------------|
| Minimale Anzahl der virtuellen Prozessoren (Kerne) | 4                          |
| Minimale Arbeitsspeicher (RAM)                         | 8 GB                       |
| Disk Space<sup>1</sup>                       | Betriebssystem-Datenträger - 80 GB <br></br>Datenträger - 500 GB auf 8 TB|
| Minimale Anzahl von Netzwerk-Schnittstellen       | 1                          |
| Minimale Internet Bandbreite<sup>2</sup>       | 5 Mbit/s                     |

<sup>1</sup> - thin bereitgestellt

<sup>2</sup> - möglicherweise die täglichen Datenänderungsrate Netzwerk abhängig. Beispielsweise sollte ein 10 GB oder mehr an einem Tag gesichert, dauerte dann die tägliche Sicherung über 5 Mbit/s-Verbindung 4,25 Stunden (wenn die Daten nicht komprimiert oder dedupliziert werden konnte).

### <a name="supported-web-browsers"></a>Unterstützte Webbrowser

| **Komponente**     | **Version** | **Notizen Vorschriften** |
|-------------------|-----------------|-----------------------------------|
| Microsoft Edge    | Neueste version  |                                   |
| InternetExplorer | Neueste version  | Getestet mit InternetExplorer 11  |
| Google Chrome     | Neueste version  | Getestet mit 46             |

### <a name="supported-storage-clients"></a>Unterstützte Speicher-clients 

Folgende Software sind für die iSCSI-Initiatoren, die Zugriff auf das virtuelle StorSimple Array (konfiguriert als iSCSI-Server).

| **Unterstützte Betriebssysteme** | **Erforderliche Version** | **Notizen Vorschriften** |
| --------------------------- | ---------------- | ------------- |
| WindowsServer              | 2008 R2 SP1, 2012 2012R2 |StorSimple können Thin Provisioning und vollständig bereitgestellte Volumes. Teilweise bereitgestellte Datenträger kann nicht erstellt werden. StorSimple iSCSI-Volumes werden von unterstützt: <ul><li>Einfache Volumes auf Basisdatenträgern Windows.</li><li>Windows NTFS zum Formatieren eines Volumes.</li>|


Folgende Software sind für SMB-Clients, die Zugriff auf das virtuelle StorSimple Array (als Dateiserver konfiguriert).

| **SMB Version** |
|-------------|
| SMB 2.x     |
| SMB 3.0     |
| SMB 3.02    |

> [AZURE.IMPORTANT] Kopieren oder Dateien von Windows Encrypting File System (EFS) StorSimple Virtual Array Dateiserver geschützt; Dies führt zu einer nicht unterstützten Konfiguration. 

## <a name="networking-requirements"></a>Netzwerk-Vorschriften 

Die folgende Tabelle enthält die Ports, die geöffnet werden, Ihre Firewalls für iSCSI, SMB, Cloud oder Verwaltungsdatenverkehr. In dieser Tabelle bezieht sich *in* oder *eingehende* Richtung der eingehende Clientanforderungen das Gerät zugreifen. ** Oder *ausgehende* bezieht sich auf die Richtung, in der StorSimple-Gerät Daten, über die Bereitstellung sendet: z. B. ausgehende Internet.

| **Anschluss Nr.<sup>1</sup>** | **Oder** | **Port-Bereich** | **Erforderlich**              | **Notizen**                                                                                                            |
|--------------------------|---------------|----------------|---------------------------|----------------------------------------------------------------------------------------------------------------------|
| TCP 80 (HTTP)            | Out           | WAN            | Nein                        | Ausgehender Anschluss wird zum Abrufen von Updates für den Internetzugriff verwendet. <br></br>Der ausgehende Webproxy kann vom Benutzer konfiguriert. |
| TCP 443 (HTTPS)          | Out           | WAN            | Ja                       | Ausgehender Anschluss dient für den Zugriff auf Daten in der Cloud. <br></br>Der ausgehende Webproxy kann vom Benutzer konfiguriert. |
| UDP 53 (DNS)             | Out           | WAN            | In einigen Fällen; Siehe Hinweise. | Dieser Port ist nur bei Verwendung ein Internet-basierten DNS-Servers erforderlich. <br></br> **Hinweis**: Wenn einen Dateiserver bereitstellen, empfehlen wir lokalen DNS-Server.|
| UDP 123 (NTP)            | Out           | WAN            | In einigen Fällen; Siehe Hinweise. | Dieser Port ist nur bei Verwendung einen internetbasierten NTP-Server erforderlich.<br></br> **Hinweis:** Bereitstellen ein Dateiservers sollten Zeit mit Active Directory-Domänencontroller synchronisieren.  |
| TCP 80 (HTTP)           | In            | LAN            | Ja                       | Dies ist der eingehende Port für lokale Benutzeroberfläche auf dem StorSimple-Gerät für die lokale Verwaltung. <br></br> **Hinweis**: Zugriff auf lokale Benutzeroberfläche über HTTP zu HTTPS automatisch umgeleitet wird.|
| TCP 443 (HTTPS)          | In            | LAN            | Ja                       | Dies ist der eingehende Port für lokale Benutzeroberfläche auf dem StorSimple-Gerät für die lokale Verwaltung.|
| TCP-3260 (iSCSI)         | In            | LAN            | Nein                        | Dieser Port wird zum Datenzugriff über iSCSI.|

<sup>1</sup> keine eingehenden Ports müssen im öffentlichen Internet geöffnet werden.

> [AZURE.IMPORTANT] Stellen Sie sicher, dass die Firewall nicht ändern oder SSL-Datenverkehr zwischen dem StorSimple-Gerät und Azure entschlüsseln.


### <a name="url-patterns-for-firewall-rules"></a>URL-Muster für Firewall-Regeln 

Netzwerkadministratoren können erweiterte Firewall-Regeln basierend auf URL-Muster zum Filtern der eingehenden und ausgehenden Datenverkehr konfigurieren. Virtuelles Array und der StorSimple Manager-Dienst hängen von anderen Microsoft-Programmen wie Azure Service Bus, Azure Active Directory Access Control, Speicherkonten und Microsoft Update-Servern. Diesen zugeordneten URL-Muster können zum Konfigurieren von Firewallregeln verwendet werden. Es ist wichtig zu verstehen, dass diesen zugeordneten URL-Muster ändern können. Dies erfordert wiederum den Netzwerkadministrator überwachen und Firewall-Regeln für die StorSimple als und bei Bedarf aktualisieren. 

Wir empfehlen die Firewall-Regeln für ausgehenden Datenverkehr basierend auf IP-Adressen in den meisten Fällen zwischenzeitlich behoben StorSimple festgelegt. Allerdings können Sie die Informationen unten erweiterte Firewall-Regeln, die zum Erstellen von sicheren Umgebung benötigt werden.

> [AZURE.NOTE] 
> 
> - Gerät (Quelle) IPs sollte immer auf die Cloud-Netzwerk-Schnittstellen festgelegt. 
> - Das Ziel IPs [Azure-Rechenzentrum IP-](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653)Bereiche fest.


| URL-Muster                                                      | Komponente-Funktionalität                                           |
|------------------------------------------------------------------|---------------------------------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple-Manager-Dienst<br>Access Control Service<br>Azure Servicebus|
|`http://*.backup.windowsazure.com`|Registrierung des Geräts|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Das Sperren von Zertifikaten |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure-Speicherkonten und Überwachung |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-Servern<br>                        |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |
| `https://*.partners.extranet.microsoft.com/*`                    | Support-Paket                                                  |
| `http://*.data.microsoft.com `                   | Telemetriedienst in Windows finden Sie unter [Update für Kundenzufriedenheit und Diagnose Telemetrie](https://support.microsoft.com/en-us/kb/3068708)                                                  |

## <a name="next-step"></a>Nächstes

-   [Bereiten Sie das Portal StorSimple Virtual Array bereitstellen vor](storsimple-ova-deploy1-portal-prep.md)
