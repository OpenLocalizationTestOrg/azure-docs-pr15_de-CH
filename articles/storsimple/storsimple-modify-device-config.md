<properties 
   pageTitle="Ändern die Gerätekonfiguration StorSimple | Microsoft Azure" 
   description="Beschreibt, wie mit der StorSimple Manager-Dienst ein StorSimple konfigurieren, die bereits bereitgestellt wurde." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="09/29/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>So ändern Sie die Gerätekonfiguration StorSimple nutzen Sie StorSimple Manager

## <a name="overview"></a>Übersicht 

Der Azure-Verwaltungsportal Seite **Konfigurieren** enthält alle Geräteparameter, die auf einem StorSimple-Gerät konfigurieren können, die von einem StorSimple Manager-Dienst verwaltet. Dieses Tutorial erläutert die Verwendung die Seite **Konfigurieren** auf Folgendes ausführen:

- Gerät ändern 
- Zeit ändern 
- DNS-Einstellung ändern 
- Netzwerkschnittstellen ändern
- Austauschen oder IP-Adressen zuweisen

## <a name="modify-device-settings"></a>Gerät ändern

Die Einstellungen umfassen den Anzeigenamen und die Beschreibung.

Ein StorSimple an der StorSimple Manager-Dienst wird ein Standardname zugewiesen. Der Standardname entspricht in der Regel die Seriennummer des Geräts. Ein Standard-Gerätenamen, der 15 Zeichen, wie 8600-SHX0991003G44HT gibt z. B. die folgenden:

- **8600** – zeigt das Modell an.
- **SHX** -gibt die Produktionsstätte.
- **0991003** - zeigt ein bestimmtes Produkt an.
- **G44HT**- die letzten 5 Ziffern erhöht, um eindeutige Seriennummern erstellen. Sequenzielle Reihe möglicherweise nicht.

Klassische Azure-Portal können Sie den Gerätenamen ändern und Zuweisen eines eindeutigen Namens Ihrer Wahl. Der angezeigte Name beliebige Zeichen enthalten und kann maximal 64 Zeichen lang sein.

Sie können auch eine Beschreibung angeben. Eine Beschreibung identifizieren normalerweise den Besitzer und die physische Position des Geräts an. Das Beschreibungsfeld muss weniger als 256 Zeichen enthalten.
 
## <a name="modify-time-settings"></a>Zeit ändern

Ihr Gerät muss zur Authentifizierung mit der Cloud Service-Anbieter Zeit synchronisieren. Wählen Sie eine Zeitzone aus der Dropdown-Liste, und geben Sie bis zu zwei Network Time Protocol (NTP) Server. Der primäre NTP-Server ist erforderlich und wird angegeben, wenn Sie mithilfe von Windows PowerShell für StorSimple Konfiguration Ihres Geräts. Sie können standardmäßige Windows Zeitserver **time.windows.com** als NTP-Server. Primäre NTP-Server-Konfiguration durch klassischen Azure-Portal anzeigen, aber verwenden Sie die Windows PowerShell-Schnittstelle um zu ändern.

Die sekundäre NTP-Server-Konfiguration ist optional. Das Verwaltungsportal können Sie einen sekundären NTP-Server konfigurieren. 

Beim Konfigurieren des NTP-Servers sicher, dass Ihr Netzwerk den NTP Datenverkehr vom Rechenzentrum mit dem Internet ermöglicht. Wenn öffentlichen NTP-Server angeben, müssen Sie sicherstellen, dass Ihre Netzwerk-Firewalls und andere Sicherheitseinrichtungen für NTP-Datenverkehr zum und vom externen Netzwerk konfiguriert werden. Wenn bidirektionale NTP-Datenverkehr nicht zulässig ist, verwenden Sie einen internen NTP-Server (Windows-Domänencontroller bietet diese Funktion). Wenn Ihr Gerät Uhrzeit synchronisieren kann, es mit Ihr Cloud-Anbieter möglicherweise nicht.

Um eine Liste öffentlicher NTP-Server anzuzeigen, wechseln Sie [NTP Server Web](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Was geschieht, wenn das Gerät in einer anderen Zeitzone bereitgestellt wird?

Wenn das Gerät in einer anderen Zeitzone bereitgestellt wird, wird das Gerät Zeitzone ändern. Da alle backup-Policies Zeitzone des Geräts verwenden, passt backup-Policies automatisch entsprechend der neuen Zeitzone. Es ist kein Benutzereingriff erforderlich.

## <a name="modify-dns-settings"></a>DNS-Einstellung ändern

Ein DNS-Server wird verwendet, wenn das Gerät mit der Cloud Service-Anbieter zu kommunizieren versucht. Für eine hohe Verfügbarkeit müssen Sie die primären und sekundären DNS-Server während der gerätebereitstellung des ersten konfigurieren. Konfigurieren Sie den primären DNS-Server müssen Sie die Windows PowerShell-Schnittstelle auf dem StorSimple-Gerät verwenden.

Ändern den sekundären DNS-Server können Sie den Azure-Verwaltungsportal.



## <a name="modify-network-interfaces"></a>Netzwerkschnittstellen ändern

Das Gerät hat sechs Gerät Netzwerkschnittstellen vier sind 1 GbE und zwei 10 GbE Diese Schnittstellen sind als Daten 0 – Daten 5 gekennzeichnet. Daten 0 Daten 1, Daten 4 und 5 Daten sind 1 GbE Daten 2 und Daten 3 10 GbE-Netzwerkschnittstellen.

Konfigurieren Sie **Network Interface** für Schnittstellen verwendet werden. Um hohe Verfügbarkeit sicherzustellen, wird empfohlen, mindestens zwei iSCSI- und zwei Cloud-aktivierten Schnittstellen auf Ihrem Gerät haben. Wir empfehlen jedoch, dass nicht verwendete Schnittstellen deaktiviert nicht erforderlich.

Wenn Sie Netzwerkschnittstellen konfigurieren, müssen Sie eine VIP (virtual IP) konfigurieren.

0 werden Cloud standardmäßig aktiviert. Wenn Daten 0 konfigurieren, müssen Sie zwei feste IP-Adressen für jeden Controller konfigurieren. Diese festen IP-Adressen wird die Gerätecontroller direkt und sind hilfreich, wenn Sie Updates installieren, auf dem Gerät oder der Domänencontroller zur Problembehandlung zugreifen.

StorSimple 8000-Serie Update 1 Routingmetrik Daten 0 festgelegt ist, den niedrigsten; Daher läuft das Gerät StorSimple 8000-Serie Update 1, der clouddatenverkehr durch Daten 0 weitergeleitet. Machen Sie dies haben Sie mehr als eine Cloud-fähige Netzwerkschnittstelle auf dem Gerät StorSimple

>[AZURE.NOTE] Festen IP-Adressen für den Controller dienen zur Wartung der Updates für das Gerät. Daher muss die feste IP-Adressen routingfähig und Verbindung mit dem Internet herstellen.

Die folgenden Parameter werden für jede Netzwerkschnittstelle angezeigt:

- **Geschwindigkeit** – nicht konfigurierbare Parameter. Daten 0 Daten 1, Daten 4 und 5 Daten sind immer 1 GbE Daten 2 und Daten 3 10 GbE-Schnittstellen.

     >[AZURE.NOTE] Geschwindigkeit und Duplexmodus werden immer automatisch-ausgehandelt. Jumbo-Frames werden nicht unterstützt.
 
- **Schnittstellenstatus** – eine Schnittstelle aktiviert oder deaktiviert werden. Wenn aktiviert, versucht das Gerät der Schnittstelle. Wir empfehlen, aktiviert diese Schnittstellen, die mit dem Netzwerk verbunden und verwendet. Deaktivieren Sie alle Schnittstellen, die Sie nicht verwenden.

- **Schnittstelle** – dieser Parameter können Sie iSCSI-Datenverkehr von Cloud Speicherdatenverkehr isolieren. Dieser Parameter kann eine der folgenden sein:

    - **Cloud aktiviert** – Wenn aktiviert, wird diese Schnittstelle verwenden, kommunizieren mit der Cloud.
    - **iSCSI aktiviert** – Wenn aktiviert, das Gerät diese Schnittstelle verwendet mit iSCSI-Host kommunizieren.

    Isolieren von iSCSI-Datenverkehr von Cloud Speicherdatenverkehr empfohlen. Beachten Sie auch, ist der Host in demselben Subnetz wie Ihr Gerät, müssen Sie nicht Gateway zuweisen; ist der Host in einem anderen Subnetz als Ihr Gerät müssen Sie ein Gateway zuweisen.

- **IP-Adresse** kann IPv4 oder IPv6 oder beides. IPv4 und IPv6-Adressengruppen sind für die Netzwerkschnittstellen Gerät unterstützt. Bei IPv4 angeben einer 32-Bit-IP-Adresse (*xxx.xxx.xxx.xxx*) in Punkt Dezimalschreibweise. Bei IPv6 Geben Sie einfach einen 4-stellige Präfix und eine 128-Bit-Adresse wird automatisch für Ihr Gerät Netzwerkschnittstelle basierend auf das Präfix generiert.

- **Subnetz** – Dies bezieht sich auf die Subnetzmaske und über die Windows PowerShell-Schnittstelle konfiguriert ist.

- **Gateway** – Dies ist das Standard-Gateway, das diese Schnittstelle verwendet werden soll, wenn versucht wird, die Kommunikation mit Knoten, die nicht innerhalb derselben IP-Adresse (Subnetz). Das Standardgateway müssen desselben Adressbereichs (Subnetz) wie die Schnittstelle IP-Adresse die Subnetzmaske bestimmt.

- **Feste IP-Adresse** – dieses Feld steht nur beim Konfigurieren der Daten 0 Schnittstelle. Für Vorgänge wie Updates oder zur Problembehandlung des Geräts müssen Sie direkt mit dem Gerätecontroller hergestellt. Die feste IP-Adresse kann auf den aktiven und passiven Controller auf dem Gerät verwendet werden.

Controller 0 und 1 Controller können über klassischen Azure-Portal neu konfigurieren.

>[AZURE.NOTE] 
>
>- Überprüfen Sie zum ordnungsgemäßen Betrieb zu gewährleisten, Geschwindigkeit und Duplex am Switch Geräteschnittstelle verbunden ist. Switch-Schnittstellen entweder mit aushandeln soll oder für Gigabit Ethernet (1000 Mbit/s) und Vollduplex. Schnittstellen langsamer oder Halbduplex-Betrieb führt zu Performance-Problemen.
>
>- Um Störungen und Ausfallzeiten zu minimieren, sollten Sie Portfast auf den Switch-Ports mit denen die iSCSI-Schnittstelle des Geräts herstellen. Dadurch wird sichergestellt, dass bei einem Failover Netzwerkkonnektivität schnell eingerichtet werden kann.
 
## <a name="swap-or-reassign-ips"></a>Austauschen oder IP-Adressen zuweisen

Derzeit ist jeder Netzwerkschnittstelle auf dem Controller zugewiesen wird VIP (durch dasselbe Gerät oder einem anderen Gerät im Netzwerk), fehl dann die Steuerung über. Daher müssen Sie die richtige Vorgehensweise VIPs für die Netzwerkschnittstelle Gerät austauschen da eine doppelte IP-Situation erstellen.

Die folgenden Schritte austauschen oder VIPs für alle Netzwerkschnittstellen zuweisen:

#### <a name="to-reassign-ips"></a>Zuweisen von IP-Adressen

1. Deaktivieren Sie die IP-Adresse für beide Schnittstellen.

2. Die entsprechenden Schnittstellen nach IP-Adressen deaktiviert werden, weisen Sie die neuen IP-Adressen zu.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie [MPIO für Ihr StorSimple-Gerät](storsimple-configure-mpio-windows-server.md)konfigurieren.

- Informationen zum Verwenden [den StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).
     
