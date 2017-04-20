<properties
   pageTitle="Anforderungen StorSimple | Microsoft Azure"
   description="Beschreibt, Software, Netzwerken und hohe Verfügbarkeit und best Practices für Microsoft Azure StorSimple-Lösung."
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
   ms.workload="TBD"
   ms.date="08/31/2016"
   ms.author="alkohli"/>

# <a name="storsimple-software-high-availability-and-networking-requirements"></a>StorSimple Software, hohe Verfügbarkeit und netzwerkanforderungen

## <a name="overview"></a>Übersicht

Willkommen bei Microsoft Azure StorSimple. Dieser Artikel beschreibt wichtige Vorschriften und best Practices für das StorSimple-Gerät und Speicher Clients Zugriff auf das Gerät. Wir empfehlen die Informationen sorgfältig überprüfen, bevor Sie Ihr System StorSimple bereitstellen und dann, nach Bedarf während der Bereitstellung und nachfolgenden Vorgang verweisen.

Systemvoraussetzungen:

- **Software an Clients Storage** - beschreibt die unterstützten Betriebssysteme und zusätzlichen Anforderungen für diese Betriebssysteme.
- **Netzwerk an das Gerät StorSimple** - Informationen zu den Ports, die in der Firewall zu iSCSI, Cloud oder Verwaltungsdatenverkehr geöffnet werden.
- **Hohe Verfügbarkeit an StorSimple** - beschreibt die hohe Verfügbarkeit und best Practices für Ihr StorSimple Gerät und Host-Computer. 


## <a name="software-requirements-for-storage-clients"></a>Für Software für Speicher-clients

Folgende Software sind für Speicher-Clients, die StorSimple Gerät.

| Unterstützte Betriebssysteme | Erforderliche Version | Notizen Vorschriften |
| --------------------------- | ---------------- | ------------- |
| WindowsServer              | 2008 R2 SP1, 2012 2012R2 |StorSimple iSCSI-Volumes werden auf nur Windows Disk folgende unterstützt:<ul><li>Volumes auf Basisdatenträger</li><li>Einfache und gespiegelte Volumes auf einem dynamischen Datenträger</li></ul>Windows Server 2012 thin provisioning und ODX-Funktionen werden bei Verwendung ein StorSimple iSCSI-Volumes unterstützt.<br><br>StorSimple können Thin Provisioning und vollständig bereitgestellte Volumes. Teilweise bereitgestellte Datenträger kann nicht erstellt werden.<br><br>Thin Provisioning Volume formatieren kann lange dauern. Wir empfehlen das Volume löschen und erstellen eine neue statt formatieren. Wenn Sie immer noch dagegen Sie ein Volume zu formatieren:<ul><li>Führen Sie den folgenden Befehl vor dem Formatieren Speicherplatz freigeben Verzögerung: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Nachdem die Formatierung abgeschlossen ist, verwenden Sie folgenden Befehl Wiedergewinnung des Speicherplatzes wieder zu aktivieren:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>Deshalb sollten Windows Server 2012 unter [KB 2878635](https://support.microsoft.com/kb/2870270) zu Ihrem Windows-Server.</li></ul></li></ul></ul> Wenn Sie StorSimple Snapshot-Manager oder StorSimple Adapter für SharePoint konfigurieren, fahren Sie [Softwaremindestanforderungen für optionale Komponenten](#software-requirements-for-optional-components).|
| VMWare ESX | 5.5 und 6.0 | Mit VMWare vSphere unterstützt als iSCSI-Client. VAAI Block-Funktion wird mit VMware vSphere StorSimple Geräte unterstützt.
| Linux RHEL/CentOS | 5, 6 und 7 | Unterstützung für Linux iSCSI-Clients mit Open-iSCSI-Initiator Versionen 5, 6 und 7. |
| Linux | SUSE Linux 11 | |
 > [AZURE.NOTE] IBM AIX wird mit StorSimple derzeit nicht unterstützt.

## <a name="software-requirements-for-optional-components"></a>Software-Anforderungen für optionale Komponenten

Folgende Software sind für die optionalen Komponenten der StorSimple (StorSimple Snapshot-Manager und StorSimple Adapter für SharePoint).

| Komponente | Host-Plattform | Notizen Vorschriften |
| --------------------------- | ---------------- | ------------- |
| StorSimple Snapshot-Manager | Windows Server 2008 R2 SP1 2012 2012R2 | Verwenden der StorSimple-Snapshot-Manager unter Windows Server ist für Backup/Wiederherstellung von gespiegelten dynamischen Datenträgern und anwendungskonsistente Sicherungen erforderlich.<br> StorSimple Snapshot-Manager wird nur unter Windows Server 2008 R2 SP1 (64-Bit), Windows 2012 R2 und Windows Server 2012 unterstützt.<ul><li>Wenn Sie Fenster Server 2012 verwenden, installieren Sie .NET 3.5-4.5 vor StorSimple Snapshot-Manager installieren.</li><li>Wenn Windows Server 2008 R2 SP1 müssen Sie Windows Management Framework 3.0 installieren, bevor Sie StorSimple Snapshot-Manager installieren.</li></ul> |
| StorSimple Adapter für SharePoint | Windows Server 2008 R2 SP1 2012 2012R2 |<ul><li>StorSimple Adapter für SharePoint wird nur auf SharePoint 2010 und SharePoint 2013 unterstützt.</li><li>RSP erfordert SQL Server Enterprise Edition, Version 2008 R2 oder 2012.</li></ul>|

## <a name="networking-requirements-for-your-storsimple-device"></a>Netzwerk an das StorSimple-Gerät

Ihr StorSimple Gerät ist Sperrung. Jedoch Ports in der Firewall zu iSCSI, Cloud und Verwaltungsdatenverkehr geöffnet werden. Die folgende Tabelle enthält die Ports, die in der Firewall geöffnet werden müssen. In dieser Tabelle bezieht sich *in* oder *eingehende* Richtung der eingehende Clientanforderungen das Gerät zugreifen. ** Oder *ausgehende* bezieht sich auf die Richtung, in der StorSimple-Gerät Daten, über die Bereitstellung sendet: z. B. ausgehende Internet.

| Anschluss Nr.<sup>1,2</sup> | Oder | Port-Bereich | Erforderlich | Notizen |
|------------------------|-----------|------------|----------|-------|
|TCP 80 (HTTP)<sup>3</sup>|  Out |  WAN | Nein |<ul><li>Ausgehender Anschluss wird zum Abrufen von Updates für den Internetzugriff verwendet.</li><li>Der ausgehende Webproxy kann vom Benutzer konfiguriert.</li><li>Dieser Port muss Systemupdates zu auch für feste IPs Controller geöffnet sein.</li></ul> |
|TCP 443 (HTTPS)<sup>3</sup>| Out | WAN | Ja |<ul><li>Ausgehender Anschluss dient für den Zugriff auf Daten in der Cloud.</li><li>Der ausgehende Webproxy kann vom Benutzer konfiguriert.</li><li>Dieser Port muss Systemupdates zu auch für feste IPs Controller geöffnet sein.</li><li>Dieser Anschluss wird auch für die Garbagecollection auf den Domänencontrollern verwendet.</li></ul>|
|UDP 53 (DNS) | Out | WAN | In einigen Fällen; Siehe Hinweise. |Dieser Port ist nur bei Verwendung ein Internet-basierten DNS-Servers erforderlich. |
| UDP 123 (NTP) | Out | WAN | In einigen Fällen; Siehe Hinweise. |Dieser Port ist nur bei Verwendung einen internetbasierten NTP-Server erforderlich. |
| TCP-9354 | Out | WAN | Ja |Ausgehende wird vom StorSimple-Gerät zur Kommunikation mit der StorSimple Manager-Dienst. |
| 3260 (iSCSI) | In | LAN | Nein | Dieser Port wird zum Datenzugriff über iSCSI.|
| 5985 | In | LAN | Nein | Eingehender Port wird mit dem StorSimple-Gerät kommunizieren von StorSimple Snapshot-Manager verwendet.<br>Dieser Anschluss wird auch verwendet, wenn Sie eine Remoteverbindung zu Windows PowerShell für StorSimple über HTTP herstellen. |
| 5986 | In | LAN | Nein | Dieser Port wird verwendet, wenn Sie eine Remoteverbindung zu Windows PowerShell für StorSimple über HTTPS herstellen. |

<sup>1</sup> keine eingehenden Ports müssen im öffentlichen Internet geöffnet werden.

<sup>2</sup> Wenn mehrere Ports eine Gateway-Konfiguration ausführen, wird die ausgehenden weitergeleiteten Datenverkehr Reihenfolge bestimmt Reihefolge Port routing [Port routing](#routing-metric)beschrieben.

<sup>3</sup> feste IP-Adressen auf dem Gerät StorSimple Controller muss routingfähig und Verbindung mit dem Internet herstellen. Die festen IP-Adressen werden für die Wartung der Updates für das Gerät verwendet. Wenn Gerätecontroller über die feste IP-Adressen mit dem Internet verbinden können, wird eine Aktualisierung Ihres Geräts StorSimple nicht können.

> [AZURE.IMPORTANT] Stellen Sie sicher, dass die Firewall nicht ändern oder SSL-Datenverkehr zwischen dem StorSimple-Gerät und Azure entschlüsseln.

### <a name="url-patterns-for-firewall-rules"></a>URL-Muster für Firewall-Regeln

Netzwerkadministratoren können erweiterte Firewall-Regeln basierend auf URL-Muster zum Filtern der eingehenden und ausgehenden Datenverkehr konfigurieren. StorSimple-Gerät und der StorSimple Manager-Dienst hängen von anderen Microsoft-Programmen wie Azure Service Bus, Azure Active Directory Access Control, Speicherkonten und Microsoft Update-Servern. Diesen zugeordneten URL-Muster können zum Konfigurieren von Firewallregeln verwendet werden. Es ist wichtig zu verstehen, dass diesen zugeordneten URL-Muster ändern können. Dies erfordert wiederum den Netzwerkadministrator überwachen und Firewall-Regeln für die StorSimple als und bei Bedarf aktualisieren.

Wir empfehlen die Firewall-Regeln für ausgehenden Datenverkehr basierend auf IP-Adressen in den meisten Fällen zwischenzeitlich behoben StorSimple festgelegt. Allerdings können Sie die Informationen unten erweiterte Firewall-Regeln, die zum Erstellen von sicheren Umgebung benötigt werden.

> [AZURE.NOTE] Das Gerät (Quelle) IPs sollte immer auf alle Netzwerk-Schnittstellen festgelegt werden. Das Ziel IPs [Azure-Rechenzentrum IP-](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653)Bereiche fest.

#### <a name="url-patterns-for-azure-portal"></a>URL-Muster für Azure-portal
| URL-Muster                                                      | Komponente-Funktionalität                                           | Gerät IP-Adressen                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple-Manager-Dienst<br>Access Control Service<br>Azure Servicebus| Cloud-aktivierte Netzwerkschnittstellen        |
|`https://*.backup.windowsazure.com`|Registrierung des Geräts| Nur Daten 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Das Sperren von Zertifikaten |Cloud-aktivierte Netzwerkschnittstellen |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure-Speicherkonten und Überwachung | Cloud-aktivierte Netzwerkschnittstellen        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-Servern<br>                             | Feste IP-Adressen nur Controller               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Feste IP-Adressen nur Controller   |
| `https://*.partners.extranet.microsoft.com/*`                    | Support-Paket                                                  | Cloud-aktivierte Netzwerkschnittstellen        |

#### <a name="url-patterns-for-azure-government-portal"></a>URL-Muster für Azure Government portal
| URL-Muster                                                      | Komponente-Funktionalität                                           | Gerät IP-Adressen                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`   | StorSimple-Manager-Dienst<br>Access Control Service<br>Azure Servicebus| Cloud-aktivierte Netzwerkschnittstellen        |
|`https://*.backup.windowsazure.us`|Registrierung des Geräts| Nur Daten 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Das Sperren von Zertifikaten |Cloud-aktivierte Netzwerkschnittstellen |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure-Speicherkonten und Überwachung | Cloud-aktivierte Netzwerkschnittstellen        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-Servern<br>                             | Feste IP-Adressen nur Controller               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Feste IP-Adressen nur Controller   |
| `https://*.partners.extranet.microsoft.com/*`                    | Support-Paket                                                  | Cloud-aktivierte Netzwerkschnittstellen        |

### <a name="routing-metric"></a>Routingmetrik

Eine Routingmetrik ist Schnittstellen und Gateways, die Daten zu den angegebenen Netzwerken weiterleiten zugeordnet. Routingmetrik das Routingprotokoll dient die beste Route für ein bestimmtes Ziel zu berechnen, wenn es erfährt, dass mehrere Pfade zum gleichen Ziel vorhanden sind. Je niedriger die Routingmetrik, je höher die Einstellung.

Im Kontext des StorSimple mehrere Netzwerkschnittstellen und Gateways Channel-Datenverkehr konfiguriert wird die Routingmetrik kommen die relative Reihenfolge bestimmt, in der Schnittstellen verwendet werden. Die Routingmetrik kann vom Benutzer geändert werden. Sie können jedoch die `Get-HcsRoutingTable` Cmdlet routing Tabelle (und Metriken) auf dem Gerät StorSimple ausdrucken. Weitere Informationen zum Cmdlet Get-HcsRoutingTable [StorSimple Problembehandlung bei](storsimple-troubleshoot-deployment.md)Bereitstellung.

Metrischen Routingalgorithmen sind die Softwareversion auf Ihrem StorSimple Gerät abhängig.

**Versionen vor Update 1**

Dazu gehören Software-Versionen vor Update 1 wie GA, 0,1, 0,2 und 0,3 Version. Grundlage Routingmetrik lautet wie folgt:

   *Letzte 10 GbE-Netzwerkschnittstelle konfiguriert > Weitere 10 GbE-Schnittstelle > zuletzt konfigurierten 1 Gigabit-Netzwerkschnittstelle > andere 1-Gigabit-Netzwerkschnittstelle*


**Versionen ab Update 1 vor Update 2**

Dazu gehören Software-Versionen 1, 1.1 oder 1.2. Grundlage Routingmetrik wird Folgendes beschlossen:

   *Daten 0 > letzten 10 GbE-Netzwerkschnittstelle konfiguriert > Weitere 10 GbE-Schnittstelle > zuletzt konfigurierten 1 Gigabit-Netzwerkschnittstelle > andere 1-Gigabit-Netzwerkschnittstelle*

   Update 1 erfolgt die Routingmetrik Daten 0 die niedrigste; Daher wird der gesamten Cloud-Datenverkehr durch Daten 0 weitergeleitet. Machen Sie dies gibt es mehr als eine Cloud-fähige Netzwerkschnittstelle auf dem Gerät StorSimple


**Versionen von Update 2**

Update 2 ist netzwerkbezogene verbessert und die Routingmetrik wurde geändert. Das Verhalten kann wie folgt erklärt werden.

- Eine Reihe von vordefinierten Werten Netzwerkschnittstellen zugewiesen wurden.   

- Sollten Sie eine Beispieltabelle mit Werten, die verschiedene Netzwerkschnittstellen zugewiesen, wenn sie Cloud-aktiviert sind oder Cloud deaktiviert jedoch mit einem konfigurierten Gateway unten. Hinweis: die Werte hier nur Beispielwerte sind.


  	| Netzwerkschnittstelle | Cloud aktiviert | Cloud-Gateway deaktiviert |
  	|-----|---------------|---------------------------|
  	| Daten 0  | 1            | -                        |
  	| Daten 1  | 2            | 20                       |
  	| Daten 2  | 3            | 30                       |
  	| Daten 3  | 4            | 40                       |
  	| Daten 4  | 5            | 50                       |
  	| Daten 5  | 6            | 60                       |


- Ist die Reihenfolge, in der clouddatenverkehr über Netzwerkschnittstellen weitergeleitet werden:

    *Daten 0 > Daten 1 > Datum 2 > Daten 3 > Daten 4 > Daten 5*

    Dies kann durch das folgende Beispiel erläutert.

    Sollten Sie ein StorSimple mit zwei Cloud-aktivierte Netzwerkschnittstellen, Daten 0 und 5 Daten. Daten 1 bis 4 Daten sind Cloud deaktiviert u. konfigurierten Gateway. Die Reihenfolge, in der Datenverkehr für dieses Gerät weitergeleitet werden, werden:

    *Daten 0 (1) > Daten 5 (6) > Daten 1 (20) > Daten 2 (30) > Daten 3 (40) > Daten 4 (50)*

    *die Nummern in Klammern angeben, der jeweiligen Routingmetrik.*

    Daten 0 andernfalls clouddatenverkehr wird durch Daten 5 weitergeleitet. Da bei Daten 0 und Daten 5 Gateway alle Netzwerk konfiguriert ist, gehen clouddatenverkehr durch Daten 1.


- Schlägt eine Cloud-fähige Netzwerkschnittstelle sind 3 Versuche mit 30 Sekunden für die Verbindung zur Schnittstelle. Wenn alle Versuche fehlschlagen, wird der Datenverkehr an die nächste verfügbare Cloud-fähigen Schnittstelle bestimmt die routing-Tabelle weitergeleitet. Wenn das Cloud-fähigen Netzwerk fehlgeschlagen Schnittstellen, fehl des Geräts, der andere Controller (in diesem Fall kein Neustart) über.

- Ein VIP-Fehler für ein iSCSI-Netzwerk-Schnittstelle vorliegt, werden 3 Versuche mit einer Verzögerung von 2 Sekunden. Dieses Verhalten ist von früheren Versionen gleich geblieben. Wenn nicht alle iSCSI-Netzwerkschnittstellen werden Controller-Failover (mit einem Neustart) auftreten.


- Eine Warnung wird auch auf dem Gerät StorSimple ausgelöst, wenn ein VIP-Fehler. Weitere Informationen finden Sie im [alert Kurzübersicht](storsimple-manage-alerts.md).

- In wiederholten versuchen wird iSCSI Cloud Vorrang.

    Beispiel: ein StorSimple Gerät verfügt über zwei Netzwerkschnittstellen aktiviert, Daten 0 und 1 Daten. Daten 0 ist Cloud-Daten 1 sowohl Cloud ist und iSCSI aktiviert. Keine andere Netzwerkschnittstellen auf diesem Gerät sind für Cloud oder iSCSI aktiviert.

    Data 1 ausfällt, gegeben ist der letzte iSCSI-Schnittstelle wird ein Controller-Failover in 1 auf dem Domänencontroller erstellt.


### <a name="networking-best-practices"></a>Bewährte Netzwerk

Zusätzlich zu den Vorschriften über die Netzwerken, die optimale Leistung Ihrer Lösung StorSimple Bitte beachten Sie die folgenden best Practices:

- Hat Ihr StorSimple Gerät eine dedizierte 40 Mbit/s Bandbreite (oder mehr) jederzeit zur Verfügung stehen. Diese Bandbreite sollte nicht freigegeben sein (oder über QoS-Richtlinien gewährleistet werden soll) mit einer anderen Anwendung.

- Sicherstellen Sie, dass die Netzwerkkonnektivität zum Internet jederzeit verfügbar ist. Sporadisch oder unzuverlässige Internet-Verbindung, die Geräte keine Internet-Verbindung machen, führt eine nicht unterstützte Konfiguration.

- Isolieren des iSCSI- und Cloud-Datenverkehrs Netzwerkschnittstellen auf dem Gerät für iSCSI und Cloud gewidmet. Weitere Informationen finden Sie auf Ihrem Gerät StorSimple [Netzwerkschnittstellen ändern](storsimple-modify-device-config.md#modify-network-interfaces) .

- Verwenden Sie keine Link Aggregation Control Protocol (LACP) Konfiguration für die Netzwerkschnittstellen. Dies ist eine nicht unterstützte Konfiguration.


## <a name="high-availability-requirements-for-storsimple"></a>Hohe Verfügbarkeit an StorSimple

Hardware-Plattform, die zusammen mit der StorSimple-Lösung bietet Verfügbarkeit und Zuverlässigkeit, die Grundlage für eine hochverfügbare, fehlertolerante Storage-Infrastruktur im Rechenzentrum bereitstellen. Es gibt jedoch Auflagen und best Practices, denen Sie zur Gewährleistung die Verfügbarkeit der Projektmappe StorSimple entsprechen soll. Vor der Bereitstellung von StorSimple überprüfen Sie sorgfältig die folgende und best Practices für StorSimple-Gerät und Computer angeschlossenen Host.

Weitere Informationen zum Überwachen und Verwalten von Hardware-Komponenten des Geräts StorSimple finden Sie [StorSimple Manager-Dienst Hardwarekomponenten und Status überwachen](storsimple-monitor-hardware-status.md) und [Austausch von StorSimple Hardware Komponenten](storsimple-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Hohe Verfügbarkeit und Verfahren für das StorSimple-Gerät

Lesen Sie die folgenden Informationen sorgfältig durch, um die hohe Verfügbarkeit des Geräts StorSimple sicherzustellen.

#### <a name="pcms"></a>PCM

StorSimple gehören redundante, hot-Swap- und Kühlmodule (PCM). Jede PCM verfügt über ausreichend Kapazität für das gesamte Gehäuse Dienstleistung. Um hohe Verfügbarkeit zu gewährleisten, müssen beide PCM installiert werden.

- Der PCM an verschiedene Quellen Verfügbarkeit fällt eine Stromquelle anschließen
- Fällt ein PCM Kundenbetreuung sofort.
- Entfernen Sie fehlerhafte PCM nur, wenn Sie den Ersatz und installieren.
- Entfernen Sie beide PCM gleichzeitig. PCM-Modul enthält das backup-Batterie-Modul. Entfernen beide der PCM Herunterfahren ohne Akku führt und des Geräts nicht gespeichert. Weitere Informationen über den Akku wechseln Sie [verwalten die Sicherungsbatterie Modul](storsimple-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Controllermodule

StorSimple Geräten gehören redundante, hot-Swap controllermodule. Controllermodule werden aktiv/passiv. Zu jedem Zeitpunkt eine Controller-Modul ist aktiv und bietet Service, während das andere Controller-Modul Passiv ist. Passive Controller-Modul eingeschaltet und funktionsfähig, wenn aktive Controller-Modul ausfällt oder entfernt. Jeder Controller-Modul verfügt über ausreichend Kapazität für das gesamte Gehäuse Dienstleistung. Beide controllermodule müssen installiert sein, um hohe Verfügbarkeit zu gewährleisten.

- Stellen Sie sicher, dass beide controllermodule jederzeit installiert werden.

- Schlägt eine Controller-Modul Kundenbetreuung sofort.

- Entfernen Sie ausgefallene Controller-Modul nur, wenn Sie den Ersatz und installieren. Entfernen ein Modul für längere den Luftstrom beeinträchtigen und die Kühlung des Systems.

- Sicherstellen Sie, dass die Netzwerkanschlüsse beide controllermodule identisch sind und verbundenen Netzwerkschnittstellen verfügen über eine identische Netzwerkkonfiguration.

- Wenn ein Controller-Modul ausfällt oder Ersatz, stellen Sie sicher, dass das Controller-Modul vor dem fehlgeschlagenen Controller-Modul aktiv ist. Um sicherzustellen, dass ein Controller aktiv ist, zum Wechseln Sie [identifizieren den aktiven Controller auf Ihrem Gerät](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device)

- Entfernen Sie beide controllermodule gleichzeitig. Wenn ein Controller-Failover ausgeführt wird, nicht herunterfahren Sie Standby-Controller-Modul oder aus dem Gehäuse zu entfernen.

- Warten Sie mindestens fünf Minuten oder Controller-Modul nach einem Failover Controller.

#### <a name="network-interfaces"></a>Netzwerk-Schnittstellen

StorSimple Gerät controllermodule jedes haben vier 1 Gigabit und zwei 10 Gigabit-Ethernet-Netzwerk-Schnittstellen.

- Sicherstellen Sie, dass die Netzwerkanschlüsse beide controllermodule identisch sind und Netzwerkschnittstellen, Controller-Modulschnittstellen verbunden sind, um eine identische Netzwerkkonfiguration.

- Wenn möglich, Bereitstellen Sie Netzwerkanschlüsse anderen Switches Service-Verfügbarkeit bei einem Gerät.

- Beim Ziehen nur oder die letzte verbleibende iSCSI aktiviert (mit zugewiesen) deaktivieren Sie die Schnittstelle zunächst, und ziehen Sie die Kabel. Wenn die Schnittstelle zunächst entfernt wird, wird es aktive Domänencontroller Failover zum passiven Controller verursacht. Wenn der passive Controller auch die entsprechenden Schnittstellen entfernt hat werden dann beide Controller mehrmals neu starten, bevor auf einem Domänencontroller.

- Jeder Controller-Modul verbunden Sie mindestens zwei Schnittstellen mit dem Netzwerk.

- Wenn Sie die beiden aktiviert haben 10 GbE-Schnittstellen bereitstellen, die andere Switches.

- Verwenden Sie nach Möglichkeit MPIO auf Servern, um sicherzustellen, dass der Server einen Link, Netzwerk oder Schnittstellenfehler tolerieren können.

Weitere Informationen zu Netzwerken Geräts für hohe Verfügbarkeit und Leistung werden [Installieren Sie das Gerät StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) oder [Geräts StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSDs und Festplatten

StorSimple Geräte sind solid-State-Laufwerke (SSDs) und Festplattenlaufwerken (HDDs), die geschützt sind mit Leerzeichen gespiegelt. Verwenden von gespiegelten Leerzeichen wird sichergestellt, dass das Gerät Fehler SSDs oder Festplatten tolerieren.

- Stellen Sie sicher, dass alle SSD und HDD-Module installiert sind.

- Wenn eine SSD oder eine Festplatte ausfällt, Kundenbetreuung sofort.

- Eine SSD Festplatte ausfällt oder muss ausgetauscht, nehmen Sie nur SSD oder Festplatte, die ersetzt werden muss.

- Entfernen Sie mehrere SSD oder Festplatte aus dem System jederzeit nicht rechtzeitig.
Fehler 2 oder mehr Festplatten (HDD, SSD) bestimmten oder aufeinander folgende Fehler innerhalb kurzer Zeit führen System Fehlfunktion und potenziellen Datenverlust. In diesem Fall [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md) , um Hilfe.

- Während des Austauschs überwachen Sie den **Hardwarestatus** auf der Seite **Wartung** für die Laufwerke in SSDs und Festplatten. Ein grünes Häkchen Status zeigt an, dass Datenträger fehlerfrei oder OK, während eine rote Ausrufezeichen zeigt einen fehlgeschlagenen SSD oder Festplatte.

- Wir empfehlen Cloud Snapshots für alle Volumes konfigurieren, die bei einem Systemausfall geschützt werden müssen.

#### <a name="ebod-enclosure"></a>EBOD-Gehäuse

StorSimple Modell 8600 enthält eine erweiterte Reihe von Festplatten (EBOD) Anlage neben der primären Einheit. Enthält ein EBOD EBOD-Controller und Festplatten (Festplatten), die geschützt sind mit Leerzeichen gespiegelt. Verwenden von gespiegelten Leerzeichen wird sichergestellt, dass das Gerät eine oder mehrere Festplatten Fehler tolerieren kann. EBOD-Gehäuse ist die primäre Einheit durch redundante SAS-Kabel verbunden.

- Stellen Sie sicher, dass beide EBOD Gehäuse controllermodule sowohl SAS-Kabel und alle Festplattenlaufwerke werden immer installiert.

- Fällt ein EBOD Gehäuse-Controller-Modul Kundenbetreuung sofort.

- Fällt ein EBOD Gehäuse-Controller-Modul stellen Sie sicher, dass das Controller-Modul aktiv ist, bevor Sie das fehlerhafte Modul ersetzen. Um sicherzustellen, dass ein Controller aktiv ist, zum Wechseln Sie [identifizieren den aktiven Controller auf Ihrem Gerät](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device)

- Während ein EBOD controlleraustausch, kontinuierlich den Status der Komponente in der StorSimple Manager-Dienst auf **Wartung** > **Hardware-Status**.

- Fällt ein SAS-Kabel oder Austausch (Microsoft Support einbezogen werden, diese Bestimmung) erfordert, stellen Sie sicher, dass das SAS-Kabel zu entfernen, das ersetzt werden muss.

- Gleichzeitig entfernen Sie nicht beide SAS-Kabel vom System jederzeit rechtzeitig.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Hohe Verfügbarkeit Vorschläge für Host-Computer

Überprüfen Sie diese best Practices, um die hohe Verfügbarkeit der Server StorSimple verbunden.

- Konfigurieren von StorSimple mit [zwei Knoten Cluster Dateiserverkonfigurationen][1]. Entfernen Sie einzelne Punkte und Redundanz auf Host erstellen, wird die gesamte Lösung hoher Verfügbarkeit.

- Bei einem Failover der Speichercontroller teilt verwenden ständig verfügbar (CA) für hohe Verfügbarkeit mit Windows Server 2012 (SMB 3.0) verfügbar. Weitere Informationen zum Konfigurieren von Serverclustern Datei und ständig verfügbaren Freigaben mit Windows Server 2012 finden Sie in dieser [Demo-video](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über Systemgrenzen StorSimple](storsimple-limits.md).
- [Informationen zum Bereitstellen der Projektmappe StorSimple](storsimple-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
