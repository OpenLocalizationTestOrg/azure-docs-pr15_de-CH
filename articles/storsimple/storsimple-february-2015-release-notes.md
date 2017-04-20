<properties 
   pageTitle="0,3 Versionshinweise StorSimple 8000 aktualisieren | Microsoft Azure"
   description="Beschreibt neue Features und Fixes, offene Probleme und Abhilfen verfügbar für Februar 2015 Microsoft Azure StorSimple Release (Update 0,3)."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>StorSimple 8000-Serie Update 0,3 Release Notes - Februar 2015

## <a name="overview"></a>Übersicht

Die folgenden Versionshinweise identifizieren öffnen kritisch StorSimple 8000-Serie Update veröffentlicht im Februar 2015 0,3. Sie enthalten auch eine Liste der StorSimple Software und Firmware-Updates in dieser Version enthalten. Dies ist das dritte Release nach StorSimple 8000-Serie Freigabeversion Juli 2014 allgemein verfügbar gemacht wurde.
  
Dieses Update ändert nicht die Version der Gerätesoftware Januar Update. Version 6.3.9600.17312 weiterhin. Sie können überprüfen, ob das Update installiert wurde, überprüfen Sie das Datum der **Letzten Aktualisierung** . Wenn das Datum 10/2/2015 oder höher ist, wurde das Update erfolgreich installiert.  

Wir empfehlen Sie suchen und verfügbare Updates sofort nach der Installation des Geräts StorSimple. Sie können auch aktivieren automatische Updates herunterladen und Installieren von Updates mit hoher Priorität von Microsoft, sobald sie freigegeben werden. Weitere Informationen finden Sie unter [StorSimple-Gerät zu aktualisieren](storsimple-update-device.md).  

Überprüfen Sie die Informationen in den Release Notes vor die Aktualisierung in der StorSimple-Lösung bereitstellen.  

>[AZURE.IMPORTANT]   
>
> - Verwenden Sie StorSimple Manager-Dienst und nicht Windows PowerShell für StorSimple, um Februar-Update zu installieren.   
> - Es dauert ungefähr eine Stunde installieren. Jedoch wenn Sie kumulative Updates installieren, kann dieser Vorgang ungefähr 3 Stunden dauern abgeschlossen.  
> - Der Februar-Ausgabe von StorSimple enthält keine Updates für das virtuelle Gerät StorSimple. Sie können noch verfügbare Windows Updates auf Virtuelles Gerät, einschließlich der neuesten Sicherheitsupdates anwenden, aber Sie sehen keine Änderung der Version für das virtuelle Gerät.  

Stellen Sie sicher, dass die folgenden erforderlichen Komponenten vor der Aktualisierung des Geräts StorSimple eingehalten werden.  

- Stellen Sie sicher, dass beide Gerätecontroller ausgeführt werden, bevor Sie nach Updates suchen. Beide Controller nicht ausgeführt wird, schlägt die Überprüfung fehl. Um sicherzustellen, dass der Domänencontroller ordnungsgemäß, navigieren Sie auf der Seite **Wartung** zu **Hardwarestatus** . Falls Komponenten, **müssen Ihre Aufmerksamkeit**, Kontakt Microsoft Support vor dem fortfahren.
- Sicherstellen Sie, dass feste IP-Adressen für Domänencontroller 0 und Controller 1 routingfähig und mit dem Internet verbinden können, wie diese verwendet werden, für die Updates für das Gerät warten. [Verbindung prüfen-Cmdlet](https://technet.microsoft.com/library/hh849808.aspx) können Sie ping eine bekannte Adresse außerhalb des Netzwerks wie outlook.com, um sicherzustellen, dass der Controller mit dem externen Netzwerk verbunden ist.
- Sicherstellen Sie, dass die Ports 80 und 443 auf dem StorSimple-Gerät für die ausgehende Kommunikation. Weitere Informationen finden Sie unter [Netzwerk an das Gerät StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
- Deaktivieren Sie die Version der Gerätesoftware älter als 6.3.9600.17312 (Oktober 2014 Update) Daten 2 und Daten 3 Ports aktivierter vor der Aktualisierung. Daten 2 und Daten 3 können Anschlüsse aktiviert, wenn Sie das Update anwenden Ihre Gerätecontroller Wiederherstellungsmodus zu führen. Beachten Sie, dass beim Deaktivieren der Netzwerkschnittstellen zugeordneten Volumes offline geschaltet und die Ausgaben für die Dauer der Aktualisierung unterbrochen.  
  
## <a name="whats-new-in-the-february-release"></a>Neuigkeiten in der Februar-Ausgabe

Dieses Update behebt für die Factory Problem zurückgesetzt, die auf Geräten aus GA aktualisiert Oktober 2014 Version freigeben. Weitere Informationen finden Sie unter [Probleme in dieser Version behoben](#issues-fixed-in-the-february-release).   

Dieses Update enthält keine neuen Features oder Funktionen.  

## <a name="issues-fixed-in-the-february-release"></a>Probleme in der Februar-Ausgabe

Die folgende Tabelle beschreibt das Problem, das in diesem Update behoben.  
 
| Nein. | Funktion | Problem | Gilt für Gerät | Virtuelles Gerät gilt |
|-----|---------|-------|---------------------------------|-------------------------------|
| 1 | Funktion | Sie versuchen, eine Factory auf ein Gerät, das ursprünglich mussten die GA-Release (Version 6.3.9600.17215) installiert aber vom Oktober wurden zurückgesetzt Version (Version 6.3.9600.17312). Factory zurücksetzen fehlschlägt und das Gerät instabil. | Ja | Nein |


## <a name="known-issues-in-the-february-release"></a>Bekannte Probleme in der Februar-Ausgabe

Die folgende Tabelle enthält eine Zusammenfassung der bekannten Probleme in dieser Version.
 
| Nein. | Funktion | Problem | Kommentare-Lösung | Gilt für Gerät  | Virtuelles Gerät gilt |
|-----|---------|-------|----------------------------|-----------------------------|--------------------------|
| 1 | Funktion | In einigen Fällen beim Ausführen einer Factory zurücksetzen StorSimple-Gerät möglicherweise und diese Meldung angezeigt: **auf Factory zurückgesetzt wird (Phase 8)**. Dies geschieht, wenn Sie STRG + C drücken, während das Cmdlet ausgeführt wird. | Drücken Sie STRG + C nicht nach dem Initiieren einer Factory zurücksetzen. Wenn Sie bereits in diesem Zustand sind, wenden Sie sich Microsoft Support für nächste Schritte. | Ja | Nein |
| 2 | Datenträger-quorum | In seltenen Fällen Wenn die Mehrzahl der Datenträger in das EBOD-Gehäuse ein 8600device getrennt werden wiederum kein Quorum Datenträger werden dann der Speicherpool offline. Offline bleibt, auch wenn die Datenträger wiederhergestellt werden. | Sie müssen das Gerät neu starten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft Support, für nächste Schritte. | Ja | Nein |
| 3 | Cloud-Snapshot-Fehler | In seltenen Fällen möglicherweise Cloud Snapshot mit dem Fehler **maximale backup Limit erreicht**. Dies tritt auf, wenn 255 online Klone auf demselben Gerät aus dem gleichen Originaldatenträger überschreiten die gelöscht wurde. |  | Ja | Ja |
| 4 | Falsche Controller-ID | Wenn ein Controller ausgetauscht kann Controller 0 als Domänencontroller 1 angezeigt. Während des controlleraustauschs beim Laden des Bildes aus der Peerknoten kann die Controller-ID zunächst Peer Controller ID angezeigt In seltenen Fällen kann dieses Verhalten auch nach einem Neustart des Systems angezeigt. | Es ist keine Benutzeraktion erforderlich. Dies löst sich nach Abschluss der Controller ersetzt. | Ja | Nein |
| 5 | Device-monitoring-Diagrammen | In der StorSimple Manager-Dienst Überwachung Diagramme Geräte funktionieren nicht, wenn grundlegende oder NTLM-Authentifizierung in der Konfiguration des Proxyservers für das Gerät aktiviert ist. | Ändern der Web Proxy-Konfigurations für das Gerät StorSimple Manager-Dienst registriert werden, damit die Authentifizierung auf NONE festgelegt ist. Führen Sie dazu die Windows PowerShell für StorSimple Set-HcsWebProxy-Cmdlet. | Ja | Ja |
| 6 | Speicherkonten | Das Speicherkonto löschen mit der Speicherdienst ist ein nicht unterstütztes Szenario. Dies führt zu einer Situation in dem Daten abgerufen werden können. |  | Ja | Ja |
| 7 | Geräte-failover | Mehrere Failovers eines Containers Volume vom selben Quellgerät für verschiedene Geräte wird nicht unterstützt.  Failover von einem inaktiven Gerät auf mehreren Geräten machen volumecontainer für das erste Gerät Failover Besitzrechte Daten verlieren. Nach einem Failover dieser volumecontainer angezeigt oder Verhalten sich anders, wenn Sie im klassischen Azure-Portal anzeigen. |   | Ja | Nein |
| 8 | Installation | Während StorSimple Adapter für SharePoint-Installation müssen Sie zu einem Gerät IP in Reihenfolge für die Installation erfolgreich abgeschlossen. |  | Ja | Nein |
| 9 | Web-proxy | Verfügt der Web Proxy-Konfiguration angegebene Protokoll HTTPS, Ihr Gerät Service-Kommunikation beeinflusst werden und das Gerät offline gehen. Supportpakete werden auch dabei erhebliche Ressourcen auf Ihrem Gerät generiert werden. | Stellen Sie sicher, dass die Web-Proxy-URL angegebene Protokoll HTTP. Weitere Informationen zum [Konfigurieren der Webproxy für Ihr Gerät](storsimple-configure-web-proxy.md). | Ja | Nein |
| 10 | Web-proxy | Konfigurieren und aktivieren Webproxy auf einem registrierten Gerät müssen Sie aktiven Controller auf Ihrem Gerät zu starten. |  | Ja | Nein |
| 11 | Hohe Cloud Latenz und hoher i/o-Arbeitslast | Stößt Ihr StorSimple Gerät aus Cloud hohe Wartezeiten (Reihenfolge von Sekunden) und hoher i/o-Arbeitslast, Gerät Volumes in eine herabgesetzte und e / schlägt mit einer Fehlermeldung "Gerät nicht bereit". | Sie müssen manuell Gerätecontroller oder ein Gerät Failover zum Beheben des Problems durchführen. | Ja | Nein |

## <a name="physical-device-updates-in-the-february-release"></a>Gerät Veröffentlichen im Februar

Dieses Update behebt das Factory Reset-Problem, das auf Geräten aus GA aktualisiert Oktober 2014 Version freigeben. Es enthält keine anderen Updates auf dem Gerät StorSimple.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Serial attached SCSI (SAS) Controller und Firmware-Updates im Februar freigeben

Diese Version enthält keine Updates Serial attached SCSI (SAS) Controller oder der Firmware. Treiber-Update wurde im Oktober 2014 Release.  

## <a name="virtual-device-updates-in-the-february-release"></a>Virtuelles Gerät Veröffentlichen im Februar

Diese Version enthält keine Updates für das virtuelle Gerät. Installation dieses Updates wird die Softwareversion eines virtuellen Geräts nicht geändert.
 
